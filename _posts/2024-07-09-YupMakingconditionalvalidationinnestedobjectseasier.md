---
title: "Yup 중첩 객체에서 조건부 유효성 검사를 쉽게 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-09-YupMakingconditionalvalidationinnestedobjectseasier_0.png"
date: 2024-07-09 17:19
ogImage:
  url: /assets/img/2024-07-09-YupMakingconditionalvalidationinnestedobjectseasier_0.png
tag: Tech
originalTitle: "Yup: Making conditional validation in nested objects easier"
link: "https://medium.com/@ctcaerr/yup-making-conditional-validation-in-nested-objects-easier-e237d0a1a351"
---

![Image](/assets/img/2024-07-09-YupMakingconditionalvalidationinnestedobjectseasier_0.png)

가끔은 yup을 사용하여 크고 중첩된 객체를 유효성 검사할 때 현재 객체의 부모 속성을 기반으로 유효성 검사를 수행하려고 할 때 schema.when() 메서드를 통해 부모 속성에 액세스할 수 없다는 것을 깨닫게 될 수 있습니다.

예시를 살펴봅시다:

```js
import * as yup from "yup";
const yupWrongNestedSchema = yup.object({
  user_type: yup.string().required(),
  user: yup.object({
    name: yup.string().required(),
    password: yup.string().when("user_type", {
      is: (val) => {
        //this will output undefined
        console.log(val);
        return val === "admin";
      },
      then: (s) => s.required(),
      otherwise: (s) => s,
    }),
  }),
});
//this must throw
try {
  const data = {
    user_type: "admin",
    user: {
      name: "admin",
    },
  };
  yupWrongNestedSchema.validateSync(data);
} catch (e) {
  console.log(e);
}
```

<div class="content-ad"></div>

위의 예시에서는 사용자 유형을 알아내기 위해 user_type 속성에 액세스하려고 시도했지만 정의되지 않았습니다.

그래서 이런 종류의 스키마를 만드는 간단한 해결책을 소개해 드리겠습니다.

```js
import * as yup from "yup";
const yupGoodNestedSchema = yup.object({
  user_type: yup.string().required(),
  user: yup.object({
    name: yup.string().required(),
    password: yup.string().when("$user_type", {
      is: (val) => {
        //this will output admin
        console.log(val);
        return val === "admin";
      },
      then: (s) => s.required(),
      otherwise: (s) => s,
    }),
  }),
});
const data = {
  user_type: "admin",
  user: {
    name: "admin",
  },
};
//이것은 작동해야 합니다.

try {
  yupGoodNestedSchema.validateSync(data, {
    context: data,
  });
} catch (e) {
  console.log(e);
}
```

그러면 이게 어떻게 동작하는 걸까요?

<div class="content-ad"></div>

매우 간단해요. 우리가 액세스하려는 속성의 키에 " $" 접두사를 추가하면, 해당 속성이 context에서 가져올 것임을 yup에게 알리게 됩니다. "context"는 validate 메소드에서 yup 구성을 통해 보내는 객체입니다.

아래 링크를 통해 직접 시도해볼 수 있는 codesandbox가 있습니다:

[https://codesandbox.io/p/sandbox/yup-nested-validation-gmw39j](https://codesandbox.io/p/sandbox/yup-nested-validation-gmw39j)
