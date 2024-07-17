---
title: "Azure Functions와 TypeScript로 규정 준수 정책 할당 자동화하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-AutomatingCompliancePolicyAssignmentswithAzureFunctionsandTypeScript_0.png"
date: 2024-07-09 12:58
ogImage:
  url: /assets/img/2024-07-09-AutomatingCompliancePolicyAssignmentswithAzureFunctionsandTypeScript_0.png
tag: Tech
originalTitle: "Automating Compliance Policy Assignments with Azure Functions and TypeScript"
link: "https://medium.com/stackademic/automating-compliance-policy-assignments-with-azure-functions-and-typescript-2ef3bcad5264"
---

<img src="/assets/img/2024-07-09-AutomatingCompliancePolicyAssignmentswithAzureFunctionsandTypeScript_0.png" />

# 소개

현재의 비즈니스 환경에서 보안 정책을 준수하는 것은 중요합니다. 이 프로세스를 자동화하면 준수 관리를 크게 간소화할 수 있습니다. 이 문서에서는 TypeScript와 Microsoft Graph API를 사용하여 Azure 함수를 구현하여 컴플라이언스 정책을 자동으로 생성하고 할당하는 방법을 탐색합니다. 클라우드 나 크라다의 Marcelo 콘텐츠를 기반으로 자동화 및 Azure 함수를 사용한 컴플라이언스 정책 관리가 쉬워졌다고 할 수 있습니다.

# 환경 설정

<div class="content-ad"></div>

시작하기 전에 Azure Portal에서 몇 가지 환경 변수를 설정해야 합니다:

![이미지1](/assets/img/2024-07-09-AutomatingCompliancePolicyAssignmentswithAzureFunctionsandTypeScript_1.png)

![이미지2](/assets/img/2024-07-09-AutomatingCompliancePolicyAssignmentswithAzureFunctionsandTypeScript_2.png)

- CLIENT_ID: 인증용 Azure AD 클라이언트 ID.
- CLIENT_SECRET: Azure AD 클라이언트 비밀.
- TENANT_ID: Azure AD 테넌트 ID.

<div class="content-ad"></div>

Azure Function 구현

```js
import { AzureFunction, Context, HttpRequest } from "@azure/functions";
import { ServicePrincipal } from "../src/service/ServicePrincipal";
import { CompliancePolicies } from "../src/service/CompliancePolicies";

/**
 * 컴플라이언스 정책 할당을 처리하는 HTTP 트리거 함수입니다.
 *
 * @param {Context} context - Azure Function을 위한 컨텍스트 객체입니다.
 * @param {HttpRequest} req - HTTP 요청 객체입니다.
 * @returns {Promise<void>} - 함수가 완료되면 해결되는 프로미스입니다.
 */
const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
  try {
    context.log("HTTP 트리거 함수가 요청을 처리했습니다.");

    // 요청 메서드 유효성 검사
    if (req.method !== "POST") {
      context.res = {
        status: 405,
        body: "Method Not Allowed",
      };
      return;
    }

    // 환경 변수 추출
    const clientId: string = String(process.env.CLIENT_ID);
    const clientSecret: string = String(process.env.CLIENT_SECRET);
    const tenantId: string = String(process.env.TENANT_ID);

    // Azure AD 자격 증명을 사용하여 ServicePrincipal 초기화
    context.log("tenantId로 ServicePrincipal을 초기화합니다:", tenantId);
    const servicePrincipal = new ServicePrincipal(tenantId, clientId, clientSecret);

    // Azure AD에서 액세스 토큰 요청
    context.log("액세스 토큰 요청 중");
    const accessToken: string = await servicePrincipal.getAccessToken();
    context.log("액세스 토큰을 받았습니다.");

    // 요청 매개변수로 CompliancePolicies 인스턴스 초기화
    const compliancePolicies = new CompliancePolicies(req.body.name, req.body.description);
    context.log("명칭:", req.body.name, "설명:", req.body.description, "로 CompliancePolicies을 초기화했습니다.");

    // complianceid가 제공되었는지 확인; 제공되지 않았다면 새로운 컴플라이언스 정책 생성
    let complianceid: string;
    if (!req.body.complianceid) {
      context.log("complianceid가 제공되지 않았습니다. 새로운 컴플라이언스 정책을 생성합니다.");
      const compliancePolicy = await compliancePolicies.postCompliancePolicy(accessToken);
      complianceid = compliancePolicy.body.id;
      context.log("id:", complianceid, "를 가진 컴플라이언스 정책을 생성했습니다.");
    } else {
      complianceid = req.body.complianceid;
      context.log("제공된 complianceid를 사용합니다:", complianceid);
    }

    // 정책을 지정된 그룹에 할당
    context.log("complianceid:", complianceid, "를 groupId:", req.body.groupId, "에 할당합니다.");
    const response = await compliancePolicies.assignPolicy(complianceid, req.body.groupId, accessToken);

    // HTTP 응답을 준비하고 전송
    context.res = {
      status: 201,
      body: response.body,
    };
    context.log("응답을 전송했습니다.");
  } catch (error) {
    context.log.error("요청 처리 중 오류 발생:", error.message);
    context.res = {
      status: 500,
      body: { message: "내부 서버 오류" },
    };
  }
};

export default httpTrigger;
```

# 기술적인 세부 정보

## ServicePrincipal 클래스

<div class="content-ad"></div>

Azure AD 인증을 처리하기 위해 @azure/identity 패키지의 ClientSecretCredential을 사용합니다.

```js
import { ClientSecretCredential } from "@azure/identity";

/**
 * Azure AD 인증을 처리하는 ServicePrincipal 클래스입니다.
 */
export class ServicePrincipal {
    /** Azure 관리를 위한 리소스 URL입니다. */
    private readonly resource: string = "https://management.azure.com/.default";

    /**
     * ServicePrincipal 인스턴스를 생성합니다.
     *
     * @param {string} tenantId - Azure AD 테넌트 ID입니다.
     * @param {string} clientId - Azure AD 앱의 클라이언트 ID입니다.
     * @param {string} clientSecret - Azure AD 앱의 클라이언트 시크릿입니다.
     */
    constructor(
        private readonly tenantId: string,
        private readonly clientId: string,
        private readonly clientSecret: string
    ) {}

    /**
     * Azure AD에서 액세스 토큰을 검색합니다.
     *
     * @returns {Promise<string>} - 액세스 토큰으로 해결되는 프로미스입니다.
     * @throws {Error} - 액세스 토큰을 가져 오지 못하는 경우 오류가 발생합니다.
     */
    public async getAccessToken(): Promise<string> {
        try {
            const credential = new ClientSecretCredential(this.tenantId, this.clientId, this.clientSecret);
            const tokenResponse = await credential.getToken(this.resource);

            if (!tokenResponse) {
                throw new Error("액세스 토큰 획득 실패");
            }

            return tokenResponse.token;
        } catch (error) {
            console.error("액세스 토큰을 획득하는 중 오류가 발생했습니다:", error);
            throw new Error("액세스 토큰 획득 실패");
        }
    }
}
```

## CompliancePolicy Interface

데이터 유형이 Graph API로 전송될 때 선언됩니다.

<div class="content-ad"></div>

```js
/**
 * 적합성 정책을 나타냅니다.
 */
export interface CompliancePolicy {
  /** 정책의 OData 유형입니다. */
  "@odata.type": string;
  /** 활성 방화벽이 필요한지 여부를 나타냅니다. */
  activeFirewallRequired: boolean;
  /** 안티-스파이웨어가 필요한지 여부를 나타냅니다. */
  antiSpywareRequired: boolean;
  /** 백신이 필요한지 여부를 나타냅니다. */
  antivirusRequired: boolean;
  /** BitLocker가 활성화되었는지 여부를 나타냅니다. */
  bitLockerEnabled: boolean;
  /** 코드 무결성이 활성화되었는지 여부를 나타냅니다. */
  codeIntegrityEnabled: boolean;
  /** Windows Defender가 활성화되었는지 여부를 나타냅니다. */
  defenderEnabled: boolean;
  /** 적합성 정책의 설명입니다. */
  description: string;
  /** 디바이스 위협 방지가 활성화되었는지 여부를 나타냅니다. */
  deviceThreatProtectionEnabled: boolean;
  /** 디바이스 위협 방지에 대한 필수 보안 수준입니다. */
  deviceThreatProtectionRequiredSecurityLevel: string;
  /** 적합성 정책의 표시 이름입니다. */
  displayName: string;
  /** 필요한 패스워드 유형입니다. */
  passwordRequiredType: string;
  /** 역할 범위 태그 ID 목록입니다. */
  roleScopeTagIds: string[];
  /** 실시간 보호가 활성화되었는지 여부를 나타냅니다. */
  rtpEnabled: boolean;
  /** 규칙에 대한 예약된 작업입니다. */
  scheduledActionsForRule: ScheduledActionForRule[];
  /** Secure Boot가 활성화되었는지 여부를 나타냅니다. */
  secureBootEnabled: boolean;
  /** 서명이 오래되었는지 여부를 나타냅니다. */
  signatureOutOfDate: boolean;
  /** TPM이 필요한지 여부를 나타냅니다. */
  tpmRequired: boolean;
}

/**
 * 규칙을 위한 예약된 작업을 나타냅니다.
 */
interface ScheduledActionForRule {
  /** 규칙의 이름입니다. */
  ruleName: string;
  /** 규칙에 대한 예약된 작업 구성입니다. */
  scheduledActionConfigurations: ScheduledActionConfiguration[];
}

/**
 * 예약된 작업 구성을 나타냅니다.
 */
interface ScheduledActionConfiguration {
  /** 작업의 유형입니다. */
  actionType: string;
  /** 작업의 유예 기간(시간)입니다. */
  gracePeriodHours: number;
  /** 알림 메시지에서 CC될 이메일 주소 목록입니다. */
  notificationMessageCCList: string[];
  /** 알림 템플릿 ID입니다. */
  notificationTemplateId: string;
}
```

## CompliancePolicies Class

Microsoft Graph API를 사용하여 적합성 정책을 생성하고 할당하는 논리를 캡슐화합니다. 새 정책을 게시하고 그룹에 할당하기 위한 메서드를 포함합니다.

```js
import axios, { AxiosRequestConfig } from "axios";
import { CompliancePolicy } from "../interfaces/CompliancePolicy";

/**
 * 추가 프로퍼티를 포함하는 CompliancePolicy를 확장하는 인터페이스입니다.
 */
interface OutputCompliancePolicy extends CompliancePolicy {
    id: string;
    createdDateTime: Date;
    lastModifiedDateTime: Date;
}

/**
 * CompliancePolicies 클래스는 적합성 정책을 생성하고 할당하는 데 사용됩니다.
 */
export class CompliancePolicies {
    /** Microsoft Graph API의 기본 URL입니다. */
    private graphBaseUrl: string = "https://graph.microsoft.com/beta";

    /**
     * CompliancePolicies의 인스턴스를 생성합니다.
     *
     * @param {string} name - 적합성 정책의 표시 이름.
     * @param {string} description - 적합성 정책의 설명.
     */
    constructor(private readonly name: string, private readonly description: string) {}

    /**
     * Microsoft Graph에 새로운 적합성 정책을 생성합니다.
     *
     * @param {string} accessToken - 인증을 위한 액세스 토큰.
     * @returns {Promise<{status: number, body: OutputCompliancePolicy}>} - 생성된 적합성 정책을 해결하는 프로미스.
     * @throws {Error} - HTTP 요청이 실패하면 오류가 발생합니다.
     */
    async postCompliancePolicy(accessToken: string) {
        try {
            const url = `${this.graphBaseUrl}/deviceManagement/deviceCompliancePolicies`;
            const policy: CompliancePolicy = {
                "@odata.type": "#microsoft.graph.windows10CompliancePolicy",
                activeFirewallRequired: true,
                antiSpywareRequired: true,
                antivirusRequired: true,
                bitLockerEnabled: true,
                codeIntegrityEnabled: true,
                defenderEnabled: true,
                description: this.description,
                deviceThreatProtectionEnabled: false,
                deviceThreatProtectionRequiredSecurityLevel: "unavailable",
                displayName: this.name,
                passwordRequiredType: "deviceDefault",
                roleScopeTagIds: ["0"],
                rtpEnabled: true,
                scheduledActionsForRule: [
                    {
                        ruleName: "PasswordRequired",
                        scheduledActionConfigurations: [
                            {
                                actionType: "block",
                                gracePeriodHours: 12,
                                notificationMessageCCList: [],
                                notificationTemplateId: "",
                            },
                            {
                                actionType: "retire",
                                gracePeriodHours: 4320,
                                notificationMessageCCList: [],
                                notificationTemplateId: "",
                            },
                        ],
                    },
                ],
                secureBootEnabled: true,
                signatureOutOfDate: true,
                tpmRequired: true,
            };

            const config: AxiosRequestConfig = {
                headers: {
                    Authorization: `Bearer ${accessToken}`,
                    "Content-Type": "application/json",
                },
            };

            const response = await axios.post<OutputCompliancePolicy>(url, policy, config);

            return {
                status: 201,
                body: response.data,
            };
        } catch (error) {
            console.error("적합성 정책 생성 중 오류 발생:", error);
            throw new Error("적합성 정책 생성 실패");
        }
    }

    /**
     * Microsoft Graph에서 그룹에 적합성 정책을 할당합니다.
     *
     * @param {string} policyId - 적합성 정책의 ID.
     * @param {string} groupId - 정책을 할당할 그룹의 ID.
     * @param {string} accessToken - 인증을 위한 액세스 토큰.
     * @returns {Promise<{status: number, body: any}>} - 할당 작업의 응답을 해결하는 프로미스.
     * @throws {Error} - HTTP 요청이 실패하면 오류가 발생합니다.
     */
    public async assignPolicy(policyId: string, groupId: string, accessToken: string) {
        try {
            const url = `${this.graphBaseUrl}/deviceManagement/deviceCompliancePolicies/${policyId}/assign`;

            const assignment = {
                assignments: [
                    {
                        target: {
                            "@odata.type": "#microsoft.graph.groupAssignmentTarget",
                            groupId: groupId,
                        },
                    },
                ],
            };

            const config: AxiosRequestConfig = {
                headers: {
                    Authorization: `Bearer ${accessToken}`,
                    "Content-Type": "application/json",
                },
            };

            const response = await axios.post(url, assignment, config);

            return {
                status: 201,
                body: response.data,
            };
        } catch (error) {
            console.error("적합성 정책 할당 중 오류 발생:", error);
            throw new Error("적합성 정책 할당 실패");
        }
    }
}
```

<div class="content-ad"></div>

# 결론

Azure Functions 및 TypeScript를 사용하여 규정 준수 정책 관리를 자동화하면 오퍼레이션을 간소화할 뿐만 아니라 조직의 보안과 규정 준수를 강화할 수 있습니다. 이 구현을 통해 기업은 최소한의 노력으로 안전하고 규정 준수를 유지할 수 있습니다.

# 참고 자료

# Stackademic 🎓

<div class="content-ad"></div>

제바기심어주셔서 감사합니다. 이제 가시기 전에:

- 작가를 클로핑하고 팔로우하기를 고려해주세요! 👏
- 저희를 팔로우하기: X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문하기: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기
