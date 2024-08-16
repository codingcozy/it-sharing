---
title: "K8S에서 Spring Batch 사용하기 잡Jobs와 크론잡CronJobs을 활용한 Spring Boot 배치 작업 방법"
description: ""
coverImage: "/assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_0.png"
date: 2024-07-20 11:31
ogImage: 
  url: /assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_0.png
tag: Tech
originalTitle: "Lab7 Spring Boot K8S Spring Batch on Kubernetes  Jobs and CronJobs"
link: "https://medium.com/@boottechnologies-ci/lab7-spring-boot-k8s-spring-batch-on-kubernetes-jobs-and-cronjobs-d75344fec5af"
isUpdated: true
updatedAt: 1723816740939
---



우리의 k8s 시리즈에 다시 오신 걸 환영합니다. 이 이야기에서는 쿠버네티스 Jobs와 CronJobs가 실제로 어떻게 작동하는지 배워보겠습니다.

![이미지](/assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_0.png)

- 쿠버네티스에서 Jobs 이해하기
  - 쿠버네티스 Job 유형
  - Job 리소스 정의
- 쿠버네티스의 CronJob
  - 쿠버네티스 CronJob이란
  - CronJob 스펙 작성
  - 쿠버네티스에서 CronJob 생성하는 방법
- 쿠버네티스에서 Spring Batch
  - 코드 작성
  - 테스트
- 결론
- 참고 자료

이 시리즈에서는 스프링 생태계에서 쿠버네티스를 사용하는 방법을 보여줍니다. 우리는 스프링 부트 API와 Minikube를 사용하여 가벼우면서 빠른 개발 환경을 구성하여 프로덕션과 유사한 환경을 제공합니다.

<div class="content-ad"></div>

- Lab1 (Spring Boot/K8S): Kubernetes에 Spring Boot 애플리케이션 배포
- Lab2 (Spring Boot/K8S): Spring Boot로 Kubernetes 건강 검사 설정
- Lab3 (Spring Boot/K8S): Kubernetes에서 ConfigMaps 마스터링
- Lab4 (Spring Boot/K8S): Spring Boot에서 Kubernetes Secrets 사용
- Lab5 (Spring Boot/K8S): Kubernetes 리소스 관리 이해
- Lab6 (Spring Boot/K8S): Kubernetes에서 지속적인 볼륨
- 👉 Lab7 (Spring Boot/K8S): Kubernetes에서 Spring Batch - Jobs 및 CronJobs

Kubernetes job 또는 CronJob은 특정 작업 (Job이라고 함)을 정의하고 해당 작업이 완료된 후 일정 수의 파드가 중지되도록 보장합니다. 이 두 가지 자원은 Kubernetes 클러스터 내에서 효율적인 작업 스케줄링을 가능하게 합니다.

# Kubernetes에서 Jobs 이해

Jobs는 완전히 실행되어야 하는 일회성 작업을 수행하는 Kubernetes 자원입니다. Job은 템플릿에서 하나 이상의 파드를 생성하고 적어도 지정된 수의 파드가 성공적으로 종료될 때까지 기다립니다. 그런 다음 Job은 완료된 것으로 표시됩니다.

<div class="content-ad"></div>

간단한 경우는 하나의 Job 객체를 만들어 하나의 Pod가 완료될 수 있도록 신뢰성 있게 실행하는 것입니다. 예를 들어, 첫 번째 Pod가 실패하거나 삭제될 경우(예: 노드 하드웨어 장애 또는 노드 재부팅 등) Job 객체는 새로운 Pod를 시작합니다.

## 쿠버네티스 Job 유형

쿠버네티스는 다음과 같은 세 가지 주요 유형의 작업(태스크)을 Job으로 실행할 수 있습니다:

- 병렬하지 않은 Job — 오직 하나의 Pod만 실행하는 Job입니다. Pod가 성공적으로 완료되면 Job은 완료된 것으로 간주됩니다. 병렬성은 관련이 없습니다.
- 고정된 완료 횟수를 가진 병렬 Job — 병렬로 여러 Pod를 시작하고 실행하는 작업입니다. 지정된 성공적인 Pod .spec.completions 횟수가 발생할 때까지 Job이 계속 실행됩니다. .spec.completionMode="Indexed"를 사용할 때 각 Pod는 0부터 .spec.completions-1까지의 범위 내에서 다른 색인을 받습니다. 다른 옵션으로는 spec.parallelism 필드를 설정하여 병렬 Job의 수를 정의할 수 있습니다.
- Work Queue를 사용하는 병렬 Job — 병렬로 여러 작업을 실행하는 작업입니다. 해당 프로세스가 여러 작업으로 이뤄진 경우에 사용됩니다만 작업들 간에 의존성이 없습니다. Pod들은 자체 간이 조정하거나 외부 서비스를 통해 작업을 결정해야 합니다. 예를 들어, Pod는 작업 큐에서 최대 N개의 항목 배치를 가져올 수 있습니다. 하나 이상의 Pod가 성공적으로 종료되고 모든 Pod가 종료된 후에 Job이 성공적으로 완료됩니다.

<div class="content-ad"></div>

## 작업 리소스 정의

아래는 Job manifest의 예시입니다:

```js
apiVersion: batch/v1
kind: Job
metadata:
  name: batch-job-pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```

모든 쿠버네티스 구성과 마찬가지로 Job에는 apiVersion, kind 및 metadata 필드가 필요합니다. Job은 batch API 그룹 및 v1 API 버전의 일부입니다.

<div class="content-ad"></div>

YAML 파일은 perl:5.34.0 이미지를 실행하는 Job 유형의 리소스를 정의합니다. 해당 Job은 2000자리의 π 값을 계산하고 출력합니다. 완료하는 데 약 10초가 걸립니다.

```js
$ kubectl apply -f batch-job-pi.yml

job.batch/batch-job-pi created
```

kubectl을 사용하여 job 상태를 확인하려면:

![이미지](/assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_1.png)

<div class="content-ad"></div>

작업 상태는 대시보드를 통해 확인할 수도 있습니다:

두 경우 모두 .spec.completions 및 .spec.parallelism을 설정하지 않으면 기본적으로 1로 설정됩니다.

일정한 완료 횟수를 갖는 작업의 경우, .spec.completions를 필요한 완료 횟수로 설정해야 합니다. .spec.parallelism을 설정하거나 설정하지 않아도 기본값은 1입니다.

작업 대기열의 경우, .spec.completions를 설정하지 말고 .spec.parallelism을 음이 아닌 정수로 설정해야 합니다.

<div class="content-ad"></div>

# Kubernetes에서의 CronJob

## Kubernetes CronJob이란

Kubernetes CronJob은 시간 기반 일정에 따라 작업을 실행하는 방법으로, Linux 및 UNIX 시스템에서 오랫동안 사용되어 왔습니다. 백업 작업, 이메일 트리거, 보고서 생성 또는 컨테이너 재시작 자동화와 같은 반복 작업을 실행하는 데 사용할 수 있습니다. 이들은 Cron 형식으로 작성된 주기적으로 Job을 특정 일정에 따라 실행합니다.

## CronJob 스펙 작성

<div class="content-ad"></div>

.spec.schedule 필드는 필수입니다. 해당 필드의 값은 Cron 구문을 따릅니다:

```js
# ┌───────────── 분 (0 - 59)
# │ ┌───────────── 시 (0 - 23)
# │ │ ┌───────────── 월의 몇 번째 날 (1 - 31)
# │ │ │ ┌───────────── 월 (1 - 12)
# │ │ │ │ ┌───────────── 주의 몇 번째 요일 (0 - 6) (일요일부터 토요일까지)
# │ │ │ │ │                                   또는 sun, mon, tue, wed, thu, fri, sat
# │ │ │ │ │ 
# │ │ │ │ │
# * * * * *
```

예를 들어, 0 0 13 * 5는 매월 13일과 매주 금요일 자정에 작업이 시작되어야 함을 나타냅니다.

표준 구문 이외에도 @monthly와 같은 매크로도 사용할 수 있습니다:

<div class="content-ad"></div>


![Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_2](/assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_2.png)

To generate CronJob schedule expressions, you can also use web tools like crontab.guru.

## 쿠버네티스에서 CronJob 생성 방법

다음의 CronJob 매니페스트 예시는 매분마다 현재 시간과 hello 메시지를 출력합니다.


<div class="content-ad"></div>

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cronjob
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

kind을 CronJob으로 설정하고 .spec.schedule을 매 분마다 작업을 실행하도록 설정했습니다. .spec.schedule은 .spec의 필수 필드입니다.

```yaml
$ kubectl apply -f hello-cronjob.yml

cronjob.batch/hello-cronjob created
```

kubectl을 사용하여 작업 상태를 확인하세요.

<div class="content-ad"></div>

<img src="/assets/img/2024-07-20-Lab7SpringBootK8SSpringBatchonKubernetesJobsandCronJobs_3.png" />

cronjobs은 매 분마다 실행되어 관련 작업 및 포드를 생성합니다. ACTIVE 필드는 진행 중인 작업 수를 보여주며, 0은 이미 완료되었거나 실패했음을 의미합니다.

# Kubernetes에서 Spring Batch

이 섹션에서는 이전에 MongoDB에서 데이터를 읽고 CSV 파일을 생성하는 Spring Batch 스토리를 사용할 것입니다.

<div class="content-ad"></div>

우리는 코드를 업데이트하고 컨테이너화한 후 Kubernetes에 작업으로 배포할 것입니다. 목표는 Kubernetes 작업을 사용하여 Spring batch 애플리케이션을 실행하는 방법을 배우는 것입니다.

우리는 CSV 보고서를 이메일로 보내는 새로운 기능을 추가할 것입니다.

```js
    public void sendNotificationEmailReport(String fileToAttach) {

        MimeMessagePreparator preparator = mimeMessage -> {

            mimeMessage.setFrom(new InternetAddress(MAIL_FROM));
            mimeMessage.setRecipient(Message.RecipientType.TO, new InternetAddress(MAIL_RECIPIENT));
            mimeMessage.setSubject(MessageFormat.format("{0}{1}", MAIL_SUBJECT, fileDateFormat));

            try {
                FileSystemResource file = new FileSystemResource(new File(fileToAttach));
                MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
                helper.addAttachment(Objects.requireNonNull(file.getFilename()), file);
                helper.setText("Hi Team,  <br><br> Please find attached the daily report. <br> Regards.", true);

            } catch (Exception ex) {
                LOGGER.error(">> Unable to send report by email {} ", ex.getMessage());
            }

        };
        mailSender.send(preparator);
    }
```

## 코딩을 시작해 볼까요?

<div class="content-ad"></div>

코드 업데이트의 주요 내용은 스케줄러를 제거하는 것입니다. Kubernetes Cronjob으로 대체될 것입니다.

다음은 완전한 작업 매니페스트 내용입니다:

```js
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  # property-like keys; each key maps to a simple value
  # replace the config values with yours
  host: cluster0.xpkhnmq.mongodb.net
  database: sample_training
  option: retryWrites=true&w=majority
  smtp-host: vps-3904db98.vps.ovh.net
  smtp-port: "1025"
---
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  # replace the secret values with yours
  username: ZGJ1c2VybmFtZQ==
  password: ZGJwYXNzd29yZA==
  smtp-username: YWRtaW4=
  smtp-password: bXkgcGFzc3dvcmQ=
---
apiVersion: batch/v1
kind: Job
metadata:
  name: spring-batch-job
spec:
  template:
    spec:
      restartPolicy: OnFailure
      containers:
        - name: spring-batch5-mongodb
          image: spring-batch5-mongodb:latest
          imagePullPolicy: Never
          env: # array of environment variable definitions
            - name: MONGODB_URL
              valueFrom: # select individual keys in a ConfigMap
                configMapKeyRef:
                  name: app-config
                  key: host
            - name: MONGODB_DATABASE
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: database
            - name: MONGODB_OPTS
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: option
            - name: SMTP_HOST
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: smtp-host
            - name: SMTP_PORT
              valueFrom:
                configMapKeyRef:
                  name: app-config
                  key: smtp-port
            - name: MONGODB_LOGIN
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: username
            - name: MONGODB_PWD
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: password
            - name: SMTP_USERNAME
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: smtp-username
            - name: SMTP_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: smtp-password
```

<div class="content-ad"></div>

이 랩에서는 Atlas의 Mongo 데이터베이스를 사용합니다.

## 테스트

```js
$ kubectl apply -f k8s/spring-batch-job.yml

configmap/app-config가 생성되었습니다
secret/app-secret가 생성되었습니다
job.batch/spring-batch-job가 생성되었습니다
```

작업 상태를 확인하려면하세요.

<div class="content-ad"></div>

테이블 태그를 Markdown 형식으로 변경해주세요.

<div class="content-ad"></div>

작업을 예약하려면 전체 CronJob 매니페스트가 필요합니다. 이 경우, 작업은 매 10분마다 실행됩니다.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: spring-batch-cron-job
spec:
  schedule: '10 * * * *'
  successfulJobsHistoryLimit: 1
  failedJobsHistoryLimit: 5
  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: OnFailure
          containers:
            - name: spring-batch5-mongodb
              image: 'spring-batch5-mongodb:latest'
              imagePullPolicy: Never
              env:
                - name: MONGODB_URL
                  valueFrom:
                    configMapKeyRef:
                      name: app-config
                      key: host
                - name: MONGODB_DATABASE
                  valueFrom:
                    configMapKeyRef:
                      name: app-config
                      key: database
                - name: MONGODB_OPTS
                  valueFrom:
                    configMapKeyRef:
                      name: app-config
                      key: option
                - name: SMTP_HOST
                  valueFrom:
                    configMapKeyRef:
                      name: app-config
                      key: smtp-host
                - name: SMTP_PORT
                  valueFrom:
                    configMapKeyRef:
                      name: app-config
                      key: smtp-port
                - name: MONGODB_LOGIN
                  valueFrom:
                    secretKeyRef:
                      name: app-secret
                      key: username
                - name: MONGODB_PWD
                  valueFrom:
                    secretKeyRef:
                      name: app-secret
                      key: password
                - name: SMTP_USERNAME
                  valueFrom:
                    secretKeyRef:
                      name: app-secret
                      key: smtp-username
                - name: SMTP_PASSWORD
                  valueFrom:
                    secretKeyRef:
                      name: app-secret
                      key: smtp-password
```

## 결론

이 이야기에서는 Kubernetes Jobs 및 CronJob을 살펴보았습니다. 이것들은 Kubernetes 클러스터에서 작업을 자동화하고 예약하는 데 좋은 선택지입니다.

<div class="content-ad"></div>

GitHub에 완벽한 소스 코드가 있습니다.

GitHub Sponsors를 통해 저를 지원해주세요.

읽어 주셔서 감사합니다! 다음 이야기에서 만나요.

# 참고 자료

<div class="content-ad"></div>

- [Kubernetes Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [Spring Batch on Kubernetes](https://spring.io/blog/2021/01/27/spring-batch-on-kubernetes-efficient-batch-processing-at-scale)
- [Kubernetes Cron Job](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)
- [MongoDB Atlas Sample Data](https://www.mongodb.com/docs/atlas/sample-data/sample-training/)