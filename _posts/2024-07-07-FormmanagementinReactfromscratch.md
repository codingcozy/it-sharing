---
title: "React에서 폼 관리 처음부터 완벽 가이드"
description: ""
coverImage: "/assets/img/2024-07-07-FormmanagementinReactfromscratch_0.png"
date: 2024-07-07 12:31
ogImage:
  url: /assets/img/2024-07-07-FormmanagementinReactfromscratch_0.png
tag: Tech
originalTitle: "Form management in React from scratch"
link: "https://medium.com/@marchenko.dmitriy.00/form-management-in-react-from-scratch-82d2ea5d88a5"
---

# 맥락

## React에서 양식을 관리하는 전형적인 방법

React 생태계에는 react-hook-form, formik, tanstack form 등 다양한 양식 관리 라이브러리가 있습니다. 이들의 공통점은 모두 훅을 사용하여 양식 데이터를 관리한다는 것입니다.

```js
const form = useForm({
  initialValues: {
    firstName: "",
    secondName: "",
    phone: "",
    email: "",
  },
  onSubmit: (values) => {},
});
```

<div class="content-ad"></div>

작은 필드를 가진 작은 양식에는 적합하지만, 더 복잡한 시나리오를 처리할 때는 잘 작동하지 않습니다.

# 문제점

## 복잡한 양식

AWS ECS 작업 정의 양식을 만들어 보겠습니다. 많은 필드, 섹션, 폼 배열이 있으며, 더 큰 양식의 일부에 불과합니다.

<div class="content-ad"></div>

![Form management in React from scratch](/assets/img/2024-07-07-FormmanagementinReactfromscratch_0.png)

## 하위 폼 관리에 훅 사용하기

이러한 양식을 구현하려면 대부분 섹션으로 나눌 가능성이 높습니다(컨테이너 세부정보, 포트 매핑, 로깅 등). UI뿐만 아니라 양식 로직도 분할해야 합니다. 따라서 각 섹션에는 양식 유효성 검사 로직, 필드 구조 캡슐화, 필드 A를 선택하면 필드 B를 표시하는 것과 같은 섹션별 로직이 있습니다.

훅 기반 접근 방식으로 구현하려고 하면 몇 가지 함정이 있습니다.

<div class="content-ad"></div>

부모 폼에서 제어할 수 있는 능력을 잃지 않도록 이러한 후크들을 구성 요소에 넣는 경우:

```js
import * as yup from "yup";

import { useForm } from "@/utils/useForm";

import { ContainerDetailsInitialValues } from "./types";

const containerDetailsValidationSchema = yup.object().shape({
  containerName: yup.string().required().label("Container Name"),
});

type Props = {
  initialValues: ContainerDetailsInitialValues,
};

export const useContainerDetailsForm = ({ initialValues }: Props) => {
  const form = useForm({
    initialValues,
    validationSchema: containerDetailsValidationSchema,
  });

  return form;
};
```

```js
import { useContainerDetailsForm } from "./useContainerDetailsForm";

export const ContainerDetailsForm = () => {
  const containerDetailsForm = useContainerDetailsForm({
    initialValues: {
      containerName: "",
    },
  });

  return (
    <fieldset>
      <input {...containerDetailsForm.register("containerName")} />
    </fieldset>
  );
};
```

```js
import { ContainerDetailsForm } from "./widgets/containerDetails";
import { PortMappingsForm } from "./widgets/portMappings";

export const ContainerForm = () => {
  return (
    <fieldset>
      <ContainerDetailsForm />
      <PortMappingsForm />
    </fieldset>
  );
};
```

<div class="content-ad"></div>

양식을 제출하면 유효성 검사가 실행되며, 양식이 유효하면 데이터가 서버로 전송됩니다. 이러한 방식으로는 메인 양식에서 서브 양식의 유효성 검사를 실행할 수 없습니다. 왜냐하면 서브 양식의 후크는 외부에서 접근할 수 없기 때문입니다.

그러면 ContainerForm 컴포넌트로 이들을 끌어올리는 것을 시도해 볼 수 있습니다:

```js
import * as yup from "yup";

import { useForm } from "@/utils/useForm";

import { ContainerDetailsInitialValues } from "./types";

const containerDetailsValidationSchema = yup.object().shape({
  containerName: yup.string().required().label("Container Name"),
});

type Props = {
  initialValues: ContainerDetailsInitialValues,
};

export const useContainerDetailsForm = ({ initialValues }: Props) => {
  const form = useForm({
    initialValues,
    validationSchema: containerDetailsValidationSchema,
  });

  return form;
};
```

```js
import { Form } from "@/utils/useForm";

import { ContainerDetailsInitialValues } from "./types";

type Props = {
  form: Form<ContainerDetailsInitialValues>,
};

export const ContainerDetailsForm = ({ form }: Props) => {
  return (
    <fieldset>
      <input {...form.register("containerName")} />
    </fieldset>
  );
};
```

<div class="content-ad"></div>

```js
import { ContainerDetailsForm, useContainerDetailsForm } from "./widgets/containerDetails";
import { PortMappingsForm, usePortMappingsForm } from "./widgets/portMappings";

export const ContainerForm = () => {
  const containerDetailsForm = useContainerDetailsForm({
    initialValues: {
      containerName: "",
    },
  });
  const portMappingsForm = usePortMappingsForm({
    initialValues: {
      ports: [{ containerPort: "", hostPort: "", protocol: "" }],
    },
  });

  return (
    <fieldset>
      <ContainerDetailsForm form={containerDetailsForm} />
      <PortMappingsForm form={portMappingsForm} />
    </fieldset>
  );
};
```

위에서 언급한대로 Container Form은 더 큰 폼(작업 정의 폼)의 일부입니다. 더 정확히 말하면 작업 정의 폼 내에 컨테이너 폼 배열이 있습니다.

```js
import { ContainerForm } from "./widgets/container/ContainerForm";
import {
  InfrastructureRequirementsForm,
  useInfrastructureRequirementsForm,
} from "./widgets/infrastructureRequirements";

export const TaskDefinitionForm = () => {
  const infrastructureRequirementsForm = useInfrastructureRequirementsForm({
    initialValues: {
      taskSize: {
        cpu: "",
        memory: "",
      },
    },
  });

  return (
    <form>
      {/* 이 부분은 목록이어야 함 */}
      <ContainerForm />
      <InfrastructureRequirementsForm form={infrastructureRequirementsForm} />
    </form>
  );
};
```

현재 수준에서는 컨테이너 폼 목록이 있고 각 항목마다 동적으로 삭제하거나 새 항목을 추가할 수 있기 때문에 최상위 수준에서 후드를 올릴 수 없습니다. Container Form의 후드를 최상위 수준 폼에서 제어할 수도 없습니다. 왜냐하면 Container Form 내에서 초기화되기 때문입니다.

<div class="content-ad"></div>

저희는 후크를 사용하여 양식을 작은 단위로 나눌 수 없습니다. 남은 방법은 모든 로직을 주 양식의 후크에 넣고 하위 양식으로 전달하는 것뿐입니다.

```js
import * as yup from "yup";

import { useForm } from "@/utils/useForm";

import { TaskDefinitionInitialValues } from "./types";
import { infrastructureRequirementsValidationSchema } from "./widgets/infrastructureRequirements";
import { portMappingsValidationSchema } from "./widgets/container/widgets/portMappings";
import { containerDetailsValidationSchema } from "./widgets/container/widgets/containerDetails";

const taskDefinitionFormValidationSchema = yup.object().shape({
  containers: yup.object().shape({
    containerDetails: containerDetailsValidationSchema,
    portMappings: portMappingsValidationSchema,
  }),
  infrastructureRequirements: infrastructureRequirementsValidationSchema,
});

type Props = {
  initialValues: TaskDefinitionInitialValues,
};

export const useTaskDefinitionForm = ({ initialValues }: Props) => {
  const form = useForm({
    initialValues,
    validationSchema: taskDefinitionFormValidationSchema,
  });

  return form;
};
```

```js
import { useTaskDefinitionForm } from "./useTaskDefinitionForm";
import { ContainerForm } from "./widgets/container/ContainerForm";
import { InfrastructureRequirementsForm } from "./widgets/infrastructureRequirements";

export const TaskDefinitionForm = () => {
  const taskDefinitionForm = useTaskDefinitionForm({
    initialValues: {
      containers: [
        {
          containerDetails: {
            containerName: "",
          },
          portMappings: [
            {
              containerPort: 0,
              hostPort: 0,
              protocol: "",
            },
          ],
        },
      ],
      infrastructureRequirements: {
        taskSize: {
          cpu: "",
          memory: "",
        },
      },
    },
  });

  return (
    <form>
      {taskDefinitionForm.values.containers.map((_, containerIndex) => (
        <ContainerForm form={taskDefinitionForm} containerIndex={containerIndex} />
      ))}
      <InfrastructureRequirementsForm form={taskDefinitionForm} />
    </form>
  );
};
```

```js
import { Form } from "@/utils/useForm";
import { TaskDefinitionInitialValues } from "@/steps/04-combine-all-in-one-hook/taskDefinitionForm/types";

import { ContainerDetailsForm } from "./widgets/containerDetails";
import { PortMappingsForm } from "./widgets/portMappings";

type Props = {
  form: Form<TaskDefinitionInitialValues>,
  containerIndex: number,
};

export const ContainerForm = ({ form, containerIndex }: Props) => {
  return (
    <fieldset>
      <ContainerDetailsForm form={form} />
      <PortMappingsForm form={form} containerIndex={containerIndex} />
    </fieldset>
  );
};
```

<div class="content-ad"></div>

```js
import { set } from "lodash";

import { Form } from "@/utils/useForm";
import { TaskDefinitionInitialValues } from "@/steps/04-combine-all-in-one-hook/taskDefinitionForm/types";

type Props = {
  form: Form<TaskDefinitionInitialValues>,
  containerIndex: number,
};

export const PortMappingsForm = ({ form, containerIndex }: Props) => {
  const addPort = () => {
    form.setValues((prev) => {
      return set(
        prev,
        `containers.${containerIndex}.portMappings.${prev.containers[containerIndex].portMappings.length}`,
        { containerPort: "", hostPort: "", protocol: "" }
      );
    });
  };

  const removePort = (index: number) => {
    form.setValues((prev) => {
      return set(
        prev,
        `containers.${containerIndex}.portMappings`,
        prev.containers[containerIndex].portMappings.filter((_, i) => i !== index)
      );
    });
  };

  return (
    <fieldset>
      {form.values.containers[containerIndex].portMappings.map((_, index) => (
        <div key={index}>
          <input {...form.register(`containers.${containerIndex}.portMappings.${index}.containerPort`)} />
          <input {...form.register(`containers.${containerIndex}.portMappings.${index}.hostPort`)} />
          <input {...form.register(`containers.${containerIndex}.portMappings.${index}.protocol`)} />
          <button type="button" onClick={() => removePort(index)}>
            Remove port
          </button>
        </div>
      ))}
      <button type="button" onClick={addPort}>
        Add port
      </button>
    </fieldset>
  );
};
```

다음으로 중첩 위젯인 PortMappings를 살펴보겠습니다. 이 위젯은 부모 폼의 구조에 매우 의존하며, 이는 좋지 않습니다. 또한, 가장 간단한 조작을 수행하는 데 필요한 코드 양이 상당히 증가합니다.

## 문제 정의

주요 문제는 훅이 UI(뷰) 로직에 관한 것이며, React 컴포넌트에서만 존재하며 뷰와 관련된 작업을 처리하는 데 도움이 됩니다. 그러나 폼은 뷰 레이어가 아니라 컨트롤러 레이어에 가깝습니다. 폼은 데이터 유효성 검사 및 관리를 담당하며 그 중 일부만이 뷰 레이어에 속합니다(사용자 입력 처리 및 정보 표시). 뷰 레이어에서 폼 로직을 작성할 수는 있지만, 복잡한 시나리오가 충분히 발생할 때까지는 작동합니다.

<div class="content-ad"></div>

# 개념 (Angular와 유사한 방식)

## 접근 방식의 재구상

UI의 복잡한 폼 문제를 해결하기 위해 뷰 레이어에서 로직을 추출할 수 있습니다.

나중에 폼 컨트롤러를 만들기 위해 나중에 사용할 수 있는 유틸리티 클래스를 만들어 봅시다.

<div class="content-ad"></div>

- 전체적인 구조는 Angular 폼에서 영감을 받았어요.

```js
import { AbstractControl } from "./AbstractControl";

export class FieldControl<TValue> extends AbstractControl {
  value: TValue;

  constructor(initialValue: TValue) {
    super();
    this.value = initialValue;
  }
}
```

```js
import { AbstractControl } from "./AbstractControl";

export class FormArray extends AbstractControl {
  controls: AbstractControl[];

  constructor(controls: AbstractControl[]) {
    super();
    this.controls = controls;
  }
}
```

```js
import { AbstractControl } from "./AbstractControl";

export class FormGroup<TControl extends {
  [K in keyof TControl]: AbstractControl;
}> extends AbstractControl {
  controls: TControl;

  constructor(controls: TControl) {
    super();
    this.controls = controls;
  }
}
```

<div class="content-ad"></div>

```js
export abstract class AbstractControl {}
```

이와 같은 구조를 가지고 있으면 이러한 클래스를 어떤 형태의 복잡한 양식에도 결합할 수 있습니다. AWS ECS 작업 정의 양식을 위한 양식 구조를 만들어 봅시다.

## 컨트롤러 구현

```js
import { FormControl, FormGroup } from "@/steps/05-extract-controller/form";

export type ContainerDetailsInitialValues = {
  containerName: string,
};

export class ContainerDetailsController extends FormGroup<{
  containerName: FormControl<string>,
}> {
  constructor(initialValues: ContainerDetailsInitialValues) {
    super({
      containerName: new FormControl(initialValues.containerName),
    });
  }
}
```

<div class="content-ad"></div>

```js
import { FormArray, FormControl, FormGroup } from "@/steps/05-extract-controller/form";

export type PortMappingInitialValues = {
  containerPort: number,
  hostPort: number,
  protocol: string,
};

export type PortMappingsInitialValues = PortMappingInitialValues[];

export class PortMappingController extends FormGroup<{
  containerPort: FormControl<number>,
  hostPort: FormControl<number>,
  protocol: FormControl<string>,
}> {
  constructor(initialValues: PortMappingInitialValues) {
    super({
      containerPort: new FormControl(initialValues.containerPort),
      hostPort: new FormControl(initialValues.hostPort),
      protocol: new FormControl(initialValues.protocol),
    });
  }
}

export class PortMappingsController extends FormArray<PortMappingController> {
  constructor(initialValues: PortMappingsInitialValues) {
    super(initialValues.map((portMapping) => new PortMappingController(portMapping)));
  }
}
```

```js
import { FormArray, FormGroup } from "@/steps/05-extract-controller/form";

import { ContainerDetailsController, ContainerDetailsInitialValues } from "./widgets/containerDetails";
import { PortMappingsController, PortMappingsInitialValues } from "./widgets/portMappings";

export type ContainerInitialValues = {
  containerDetails: ContainerDetailsInitialValues,
  portMappings: PortMappingsInitialValues,
};
export type ContainersInitialValues = ContainerInitialValues[];

export class ContainerController extends FormGroup<{
  containerDetails: ContainerDetailsController,
  portMappings: PortMappingsController,
}> {
  constructor(initialValues: ContainerInitialValues) {
    super({
      containerDetails: new ContainerDetailsController(initialValues.containerDetails),
      portMappings: new PortMappingsController(initialValues.portMappings),
    });
  }
}

export class ContainersController extends FormArray<ContainerController> {
  constructor(initialValues: ContainersInitialValues) {
    super(initialValues.map((container) => new ContainerController(container)));
  }
}
```

각 컨트롤러가 자체 책임 영역을 가지고 있습니다. 폼 로직은 이제 뷰 레이어 밖에 있습니다. 이 구조는 서로 다른 서브폼을 복합하여 하나의 복잡한 형태를 만들기 쉽게 만들어줍니다.

## 상태 관리자 통합하기

<div class="content-ad"></div>

다음 단계는 React와 통합하는 것입니다. 위의 컨트롤러는 클래스일 뿐이기 때문에 그 값이 변경될 때 React가 뷰를 업데이트하도록 강제하지 않습니다.

간단히 하기 위해 mobx를 form 유틸리티 클래스에 추가하여 값이 변경될 때 뷰가 업데이트되도록 observable하게 만들 수 있습니다.

```js
import { makeObservable, observable } from "mobx";

import { AbstractControl } from "./AbstractControl";

export class FormControl<TValue> extends AbstractControl {
  value: TValue;

  constructor(initialValue: TValue) {
    super();
    this.value = initialValue;
    makeObservable(this, {
      value: observable,
    });
  }
}
```

```js
import { makeObservable, observable } from 'mobx';

import { AbstractControl } from "./AbstractControl";

export class FormArray<TControl extends AbstractControl> extends AbstractControl {
  controls: TControl[];

  constructor(controls: TControl[]) {
    super();
    this.controls = controls;
    makeObservable(this, {
      controls: observable,
    });
  }
}
```

<div class="content-ad"></div>

```js
import { makeObservable, observable } from 'mobx';

import { AbstractControl } from "./AbstractControl";

export class FormGroup<TControl extends {
  [K in keyof TControl]: AbstractControl;
}> extends AbstractControl {
  controls: TControl;

  constructor(controls: TControl) {
    super();
    this.controls = controls;
    makeObservable(this, {
      controls: observable,
    });
  }
}
```

주요 단점은 폼을 사용하는 각 구성 요소를 observer 함수로 감싸야 mobx가 작동한다는 것입니다.

```js
import { observer } from "mobx-react";

import { Input, InputNumber } from "@/steps/05-extract-controller/form";

import { PortMappingController, PortMappingsController } from "./PortMappingsController";

type Props = {
  formController: PortMappingsController,
};

export const PortMappingsForm = observer(({ formController }: Props) => {
  const addPort = () => {
    formController.controls.push(
      new PortMappingController({
        containerPort: 0,
        hostPort: 0,
        protocol: "",
      })
    );
  };

  const removePort = (index: number) => {
    formController.controls.splice(index, 1);
  };

  return (
    <fieldset>
      {formController.controls.map((portMapping, index) => (
        <div key={index}>
          <InputNumber control={portMapping.controls.containerPort} />
          <InputNumber control={portMapping.controls.hostPort} />
          <Input control={portMapping.controls.protocol} />
          <button type="button" onClick={() => removePort(index)}>
            포트 제거
          </button>
        </div>
      ))}
      <button type="button" onClick={addPort}>
        포트 추가
      </button>
    </fieldset>
  );
});
```

```js
import { observer } from "mobx-react";

import { FormControl } from "../utils";

type Props = {
  control: FormControl<string>,
};

export const Input = observer(({ control }: Props) => {
  return <input value={control.value} onChange={(e) => (control.value = e.target.value)} />;
});
```

<div class="content-ad"></div>

## 유효성 검사 추가

이제 양식 데이터를 검증하는 방법과 필드가 상호 작용한 여부를 유지하는 유틸리티 필드를 추가해야 합니다.

```js
import { action, computed, makeObservable, observable } from "mobx";
import { FormControlOptions, ValidationError } from "./types";

export abstract class AbstractControl<TValue = any> {
  public touched: boolean;
  public errors: ValidationError[];
  public name: string;

  constructor(
    options?: FormControlOptions,
  ) {
    this.touched = false;
    this.errors = [];
    this.name = options?.controlName || 'Field';
    makeObservable<AbstractControl>(this, {
      touched: observable,
      errors: observable,
      markAsTouched: action,
      isValid: computed,
    });
  }

  abstract get value(): TValue;

  abstract validate(): boolean;

  public markAsTouched(): void {
    this.touched = true;
  }

  public markAsUntouched(): void {
    this.touched = false;
  }

  public get isValid(): boolean {
    return !this.errors.length;
  }
}
```

```js
import { makeObservable, observable } from 'mobx';

import { AbstractControl } from "./AbstractControl";
import { FormControlOptions, ValidatorFunction } from './types';

export class FormControl<TValue> extends AbstractControl {
  private _value: TValue;
  protected validators: ValidatorFunction<FormControl<TValue>>[] = [];

  constructor(initialValue: TValue, options?: FormControlOptions) {
    super(options);
    this._value = initialValue;
    this.validators = options?.validators || [];
    makeObservable<FormControl<TValue>, '_value'>(this, {
      _value: observable,
    });
  }

  set value(value: TValue) {
    this._value = value;
  }

  get value(): TValue {
    return this._value;
  }

  validate(): boolean {
    this.errors = this.validators
      .map((validator) => validator(this))
      .filter((error): error is string => typeof error === 'string')
      .map((message) => ({ message }));
    return this.isValid;
  }

  addValidator(validator: ValidatorFunction<FormControl<TValue>>): void {
    this.validators.push(validator);
  }
}
```

<div class="content-ad"></div>

```js
import { makeObservable, observable } from 'mobx';

import { AbstractControl } from "./AbstractControl";
import { AbstractControlValue, FormControlOptions, ValidatorFunction } from './types';

type FormGroupValue<T extends {
  [K in keyof T]?: AbstractControl;
}> = {
  [K in keyof T]: AbstractControlValue<T[K]>;
};

export class FormGroup<TControl extends {
  [K in keyof TControl]: AbstractControl;
}> extends AbstractControl {
  controls: TControl;
  protected validators: ValidatorFunction<FormGroup<TControl>>[] = [];

  constructor(controls: TControl, options?: FormControlOptions) {
    super(options);
    this.controls = controls;
    this.validators = options?.validators || [];
    makeObservable(this, {
      controls: observable,
    });
  }

  get value(): FormGroupValue<TControl> {
    return Object.keys(this.controls).reduce((acc, key) => {
      acc[key as keyof TControl] = this.controls[key as keyof TControl].value;
      return acc;
    }, {} as FormGroupValue<TControl>);
  }

  override get isValid(): boolean {
    return super.isValid && !Object.values<AbstractControl>(this.controls).some((control) => !control.isValid);
  }

  validate(): boolean {
    this.errors = this.validators
      .map((validator) => validator(this))
      .filter((error): error is string => typeof error === 'string')
      .map((message) => ({ message }));
    Object.values<AbstractControl>(this.controls).forEach((control) => control.validate());
    return this.isValid;
  }

  addValidator(validator: ValidatorFunction<FormGroup<TControl>>): void {
    this.validators.push(validator);
  }
}
```

```js
import { makeObservable, observable } from 'mobx';

import { AbstractControl } from "./AbstractControl";
import { AbstractControlValue, FormControlOptions, ValidatorFunction } from './types';

type FormArrayValue<T extends AbstractControl> = AbstractControlValue<T>[];

export class FormArray<TControl extends AbstractControl> extends AbstractControl {
  controls: TControl[];
  protected validators: ValidatorFunction<FormArray<TControl>>[] = [];

  constructor(controls: TControl[], options?: FormControlOptions) {
    super(options);
    this.controls = controls;
    this.validators = options?.validators || [];
    makeObservable(this, {
      controls: observable,
    });
  }

  get value(): FormArrayValue<TControl> {
    return this.controls.map((control) => control.value);
  }

  override get isValid(): boolean {
    return super.isValid && !this.controls.some((control) => !control.isValid);
  }

  validate(): boolean {
    this.errors = this.validators
      .map((validator) => validator(this))
      .filter((error): error is string => typeof error === 'string')
      .map((message) => ({ message }));
    this.controls.forEach((control) => control.validate());
    return this.isValid;
  }

  addValidator(validator: ValidatorFunction<FormArray<TControl>>): void {
    this.validators.push(validator);
  }
}
```

양식에 유효성 검사를 추가하는 방법입니다. 유효성 검사기를 만들고, 양식 컨트롤러를 초기화할 때 양식 컨트롤에 추가해야 합니다.

```js
import { ValidatorFunction } from './types';

export class Validators {
  public static required: ValidatorFunction = (control) => {
    return typeof control.value === 'undefined' || control.value === null || control.value === ''
      ? `${control.name} is required`
      : true;
  };

  public static string: ValidatorFunction = (control) => {
    return typeof control.value === 'string' ? true : `${control.name} should be a string`;
  };

  public static number: ValidatorFunction = (control) => {
    return typeof control.value === 'number' ? true : `${control.name} should be a number`;
  };

  public static boolean: ValidatorFunction = (control) => {
    return typeof control.value === 'boolean' ? true : `${control.name} should be a boolean`;
  };
}
```

<div class="content-ad"></div>

```js
import { FormControl, FormGroup, Validators } from "@/steps/06-adding-validation/form";

export type ContainerDetailsInitialValues = {
  containerName: string,
};

export class ContainerDetailsController extends FormGroup<{
  containerName: FormControl<string>,
}> {
  constructor(initialValues: ContainerDetailsInitialValues) {
    super({
      containerName: new FormControl(initialValues.containerName, {
        validators: [Validators.required, Validators.string],
      }),
    });
  }
}
```

초기화 이후에 유효성 검사기를 추가하는 것도 가능합니다.

```js
import { FormControl, FormGroup, ValidatorFunction } from "@/steps/06-adding-validation/form";

export type InfrastructureRequirementsInitialValues = {
  taskSize: {
    cpu: string;
    memory: string;
  };
};

export class InfrastructureRequirementsController extends FormGroup<{
  taskSize: FormGroup<{
    cpu: FormControl<string>;
    memory: FormControl<string>;
  }>;
}> {
  constructor(initialValues: InfrastructureRequirementsInitialValues) {
    super({
      taskSize: new FormGroup({
        cpu: new FormControl(initialValues.taskSize.cpu),
        memory: new FormControl(initialValues.taskSize.memory),
      }),
    });
    this.controls.taskSize.controls.memory.addValidator(this.validateTaskSize);
  }

  private validateTaskSize: ValidatorFunction<FormControl<string>> = () => {
    const cpu = Number(this.controls.taskSize.controls.cpu.value);
    const memory = Number(this.controls.taskSize.controls.memory.value);
    if (cpu === 0.25 && memory >= 1024) {
      return 'For 0.25 CPU, memory should be less than 1024';
    }
    return true;
  }
}
```

Mobx를 사용하면 데이터 변경에 구독할 수도 있습니다. 예를 들어, 특정 조건이 충족될 때만 일부 값이 있는 양식을 생성하거나, 현재 상태에 따라 동적으로 양식을 유효성 검사할 수 있습니다(예: 필드 A에 값 'abc'가 있는 경우에만 필드 A가 필수로 선택됨).

<div class="content-ad"></div>

# 요약

우리는 복잡한 양식을 구현하는 문제에 직면했고, 양식을 관리하기 위한 기본 도구를 사용하는 대신 복잡성을 줄이고 양식 구성 요소 간의 종속성을 제거하기 위해 완전히 다른 아키텍처를 설계했습니다.

모든 예제는 이 저장소에서 찾을 수 있습니다: https://github.com/marchenkodima/forms-article

의견이 있거나 양식을 적절하게 관리하는 더 나은 접근 방법을 알고 계시다면 댓글에서 자유롭게 공유해 주세요.
