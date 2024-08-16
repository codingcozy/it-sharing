---
title: "Angular에서 빌더 디자인 패턴 사용 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-30-DesignPatternsBuilderinAngular_0.png"
date: 2024-07-30 17:00
ogImage: 
  url: /assets/img/2024-07-30-DesignPatternsBuilderinAngular_0.png
tag: Tech
originalTitle: "Design Patterns  Builder in Angular"
link: "https://medium.com/design-patterns-builder-in-angular/design-patterns-builder-in-angular-c9a05b91385f"
isUpdated: true
updatedAt: 1723816468153
---




![이미지](/assets/img/2024-07-30-DesignPatternsBuilderinAngular_0.png)

창조적 디자인 패턴은 객체를 만드는 방법에 관심이 있습니다.

JavaScript, TypeScript 등에서 객체를 만드는 방법은 new 연산자를 사용하는 것입니다.

창조적 패턴으로는 다음이 있습니다:
1. Factory
2. Factory Method
3. Abstract Factory
4. Singleton
5. Prototype
6. Builder
7. Object Pool


<div class="content-ad"></div>

이 글에서는 빌더 패턴이 어떻게 작동하는지 설명하겠습니다. 데모 프로젝트에서 빌더 패턴의 예제를 확인하고 실행할 수 있습니다. 다음 링크에서 해당 프로젝트를 찾을 수 있습니다.

빌더 디자인 패턴은 이름에서 알 수 있듯이 구성 프로세스를 모델링하는 데 사용됩니다. 일상 생활에서는 집, 아파트 블록, 성, 구조물 등을 짓습니다. 집을 짓기 위해서는 특정 순서로 특정 제품을 사용해야 하는 특정 작업이 수행되어야 합니다. 웹 시스템에서 구축은 특정 필드가 있는 양식을 생성하는 방식으로 나타낼 수 있습니다. 유효성 검사가 성공하면 양식 데이터가 API로 전송됩니다. 양식은 집처럼 각각 다를 수 있습니다.

Angular는 빌더 패턴의 구현을 제공합니다. 이는 @angular/forms 라이브러리의 FormBuilder 클래스입니다. 이 클래스에는 그룹, 레코드, 컨트롤, 배열과 같은 여러 메서드가 포함되어 있으며 각각 양식 컨트롤을 생성합니다. FormBuilder를 사용하려면 클래스를 생성자에 주입해야 합니다:

```js
@Component({
// ...
})export class BuilderComponent {
   constructor(private formBuilder: FormBuilder) {}
}
```

<div class="content-ad"></div>

테이블 태그를 마크다운 형식으로 변경하거나 inject 함수에 전달하세요:

```js
@Component({
// ...
})
export class BuilderComponent {
     private formBuilder = inject(FormBuilder)
}
```

빌더 디자인 패턴의 구조는 여러 요소로 구성되어 있습니다.

인터페이스 — 모든 유형의 빌더에 공통인 제품 구성 단계를 선언합니다.

<div class="content-ad"></div>

빌더 - 건설 단계의 다양한 구현을 제공합니다. 특정 빌더는 공통 인터페이스를 공유하지 않는 제품을 생성할 수 있습니다. 저희 양식의 예시에서는 이것이 필드 설정 등입니다.

제품 - 이들은 우리 경우 양식을 구축하는 데 사용되는 객체들입니다. 이는 FormControl, FormGroup, FormArray 또는 FormRecord일 수 있습니다. 서로 다른 빌더에 의해 구성된 제품들은 동일한 클래스 계층 구조나 인터페이스에 속할 필요가 없습니다.

디렉터 클래스 - 우리 경우 양식을 생성하기 위해 건설 단계를 호출하는 순서를 정의합니다.

클라이언트 - 클라이언트는 디렉터에 빌더 객체 중 하나를 할당해야 합니다. 우리 경우에는 유효성 검사 또는 옵션 설정을 명시할 수 있을 것입니다. 클라이언트는 양식의 적용과 양식에서 다른 메서드로 데이터를 전달할 수 있습니다.

<div class="content-ad"></div>

사용자 정의 FormBuilder를 구현하는 방법은 다음과 같습니다:

```js
import { NgFor } from '@angular/common';
import { Component, inject } from '@angular/core';
import { AbstractControlOptions, FormArray, FormBuilder, FormGroup, ReactiveFormsModule, 
  ValidatorFn, AsyncValidatorFn, FormRecord, Validators } from '@angular/forms';


class NewDefinitionFormGroup extends FormGroup {
  constructor(controls: any, validatorOrOpts?: ValidatorFn | ValidatorFn[] | AbstractControlOptions | null, asyncValidator?: AsyncValidatorFn | AsyncValidatorFn[] | null) {
    super(controls);
    console.info('새로운 정의 키', Object.keys(controls));
  }
}

export class NewDefinitionFormBuilderPattern extends FormBuilder {
  override group<T extends {}>(controls: T, options?: AbstractControlOptions | null): FormGroup {
    return new NewDefinitionFormGroup(controls);
  }
}

@Component({
  selector: 'app-builder',
  standalone: true,
  imports: [ReactiveFormsModule, NgFor],
  templateUrl: './builder.component.html',
  styleUrl: './builder.component.scss',
  providers: [{
    provide: FormBuilder,
    useClass: NewDefinitionFormBuilderPattern
  }]
})
export class BuilderComponent {
  private formBuilder = inject(FormBuilder)

  radios: string[] = ['Male', 'Female'];
  selects: string[] = ['USA', 'Canada', 'UK', 'Australia', 'Other'];

  formBuilderGroup: FormGroup = this.formBuilder.group({
    name: this.formBuilder.control('', [Validators.maxLength(10)]),
    number: this.formBuilder.control(0, { nonNullable: true, }),
    group: this.formBuilder.group({
      groupName: this.formBuilder.control('', [Validators.required]),
    }),
    groupRadioSelectArrayChcekBox: this.formBuilder.group({
      radio: this.formBuilder.control(''),
      select: this.formBuilder.control(''),
      checkBox: this.formBuilder.array([
        this.createOption('checkBox 1'),
        this.createOption('checkBox 2'),
        this.createOption('checkBox 3')
      ])
    }),
    record: this.formBuilder.array([this.createRecord()])
  });

  createOption(defaultvalue: string, selected = false) {
    return this.formBuilder.group({
      selected: this.formBuilder.control(selected),
      value: this.formBuilder.control(defaultvalue)
    })
  }

  get checkBox(): FormArray {
    return this.formBuilderGroup.get('groupRadioSelectArrayChcekBox.checkBox') as FormArray;
  }
  get record(): FormArray {
    return this.formBuilderGroup.get('record') as FormArray;
  }

  createRecord(): FormRecord {
    return this.formBuilder.record({
      key: this.formBuilder.control(''),
      value: this.formBuilder.control(''),
    });
  }
  addRecord(): void {
    this.record.push(this.createRecord());
  }

  save() {
    console.log(this.formBuilderGroup);
  }
  reset() {
    this.formBuilderGroup.reset()
  }
}
```

새로운 `NewDefinitionFormBuilderPattern` 클래스의 통합은 providers 배열에 위치합니다. 해당 클래스를 인스턴스화할 때, 생성자는 `NewDefinitionFormBuilderPattern` 클래스로 대체되며, `group` 메서드를 재정의하고 새롭게 `NewDefinitionFormGroup` 클래스를 반환합니다. `NewDefinitionFormGroup` 클래스는 폼 컨트롤 키를 콘솔에 표시합니다.

첨부된 컨트롤이 있는 폼 구조의 템플릿은 다음과 같습니다:

<div class="content-ad"></div>


<div class="container">
    <form [formGroup]="formBuilderGroup" (ngSubmit)="save()">
        <div class="form-row">
            <label class="form-label" for="name">이름:</label>
            <input class="form-input" type="text" id="name" name="name" 
                formControlName="name">
        </div>
        <div class="form-row">
            <label class="form-label" for="number">Number:</label>
            <input class="form-input" type="number" id="number" name="number" 
                formControlName="number">
        </div>

        <div class="form-row" formGroupName="group">
            <label class="form-label" for="groupName">그룹명:</label>
            <input class="form-input" type="text" id="groupName" 
                formControlName="groupName">
        </div>

        <div class="form-group" formGroupName="groupRadioSelectArrayChcekBox">
            <label class="form-label">라디오버튼:</label>
            <div *ngFor="let radio of radios">
                <input class="form-input" type="radio" [value]="radio" 
                    formControlName="radio">{ radio }
            </div>

            <div class="form-row">
                <label class="form-label" for="select">선택:</label>
                <select class="form-input" id="select" 
                    formControlName="select">
                    <option *ngFor="let select of selects" [value]="select">
                        { select }
                    </option>
                </select>
            </div>

            <div class="form-group">
                <label class="form-label">체크박스: </label>
                <div formArrayName="checkBox" *ngFor="let item of checkBox.controls; let i = index">
                    <div [formGroupName]="i">
                        <input class="form-input" type="checkbox" formControlName="selected">
                        { item.value.value }
                    </div>
                </div>
            </div>
        </div>
        <div class="form-group">
            <div formArrayName="record">
                <label class="form-label">기록: </label>
                <button type="submit" (click)="addRecord()">
                    + 다른 기록 추가
                </button>

                @for ( record of record.controls; track record; let i = $index) {
                <div class="form-row" [formGroupName]="i">
                    <label class="form-label" for="key-{ i }">키:</label>
                    <input class="form-input" id="key-{ i }" type="text" 
                        formControlName="key">
                    <label class="form-label" for="value-{ i }">값:</label>
                    <input class="form-input" id="value-{ i }" type="text" 
                        formControlName="value">
                </div>
                }
            </div>
        </div>
        <button type="submit">제출</button>
        <button type="submit" (click)="reset()">초기화</button>
    </form>
</div>


위의 폼 구조는 제안사항입니다. FormBuilder 클래스는 폼 기능에 대한 방대한 메서드와 옵션을 제공합니다. 제안된 유효성 검사 방법은 여러 컨트롤을 결합하고 유효성을 검사할 수 있습니다.

아래는 Flexbox 속성을 사용하여 구축된 SCSS 스타일입니다:

```scss
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

form {
    width: 100%;
    max-width: 700px;
    display: flex;
    flex-direction: column;
}

.form-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    margin-bottom: 20px;

    .form-label {
        flex: 2;
        text-align: right;
        margin-right: 10px;
    }

    .form-input {
        flex: 3;
    }
}
.form-group {
    margin-bottom: 20px;
}
```

<div class="content-ad"></div>

# 요약:

Builder 패턴은 구축 중인 객체의 내부 표현을 변경하는 것을 허용하며 구축 세부 정보를 숨깁니다. 각 빌더는 다른 빌더와 시스템의 나머지 부분과 독립적입니다. 모듈화를 향상시키고 새로운 빌더를 쉽게 추가할 수 있습니다.

자료:

- Java. Wzorce projektowe. 번역: Piotr Badarycz. 원제: Java design patterns. A Tutorial. 저자: James William Cooper
- [Builder 디자인 패턴 참조 링크](https://refactoring.guru/pl/design-patterns/builder)

<div class="content-ad"></div>

다른 디자인 패턴과 관련된 내 Angular 기사:

구조 패턴:
- Angular에서 디자인 패턴 — 프록시
- Angular에서 디자인 패턴 — 컴포지트
- Angular에서 디자인 패턴 — 어댑터

영어 실력은 부족하지만, 영어 실력 향상을 위해 글을 써보고 있어요. 함께 응원해주세요!