---
title: "Angular와 함께 배우는 NGRX 완벽 가이드 간단한 CRUD 작업으로 익히기"
description: ""
coverImage: "/assets/img/2024-07-09-NGRXCompleteGuidewithAngularLearnbyDoingSimpleCRUDOperation_0.png"
date: 2024-07-09 16:55
ogImage:
  url: /assets/img/2024-07-09-NGRXCompleteGuidewithAngularLearnbyDoingSimpleCRUDOperation_0.png
tag: Tech
originalTitle: "NGRX Complete Guide with Angular: Learn by Doing Simple CRUD Operation"
link: "https://medium.com/widle-studio/ngrx-complete-guide-with-angular-learn-by-doing-simple-crud-operation-17be5f7fa6ea"
---

앵귤러는 확장 가능하고 유지보수가 쉬운 웹 애플리케이션을 구축하기 위한 강력한 프레임워크입니다. 애플리케이션이 복잡해지면 상태 관리가 개발의 중요한 측면이 됩니다. NGRX는 앵귤러 애플리케이션을 위한 상태 관리 라이브러리로, 복잡한 애플리케이션 상태를 관리해주고 애플리케이션 성능을 향상시켜줍니다.

![NGRX Complete Guide with Angular](/assets/img/2024-07-09-NGRXCompleteGuidewithAngularLearnbyDoingSimpleCRUDOperation_0.png)

본 글에서는 앵귤러와 함께 NGRX를 사용하는 완벽한 가이드를 제공할 것입니다. 우리는 NGRX를 사용하여 간단한 CRUD (생성, 읽기, 수정, 삭제) 작업에 초점을 맞출 것입니다. 이 가이드를 완료하면, 앵귤러 애플리케이션에서 NGRX를 구현하고 예측 가능하고 효율적인 방식으로 상태를 처리하는 방법에 대해 solide한 이해를 얻을 것입니다.

# 목차

<div class="content-ad"></div>

- NGRX란 무엇이며 왜 사용하는가?
- Angular 프로젝트 설정
- 스토어 생성
- 액션 정의
- 리듀서 구현
- 셀렉터 생성
- 액션 디스패치
- 컴포넌트에서 데이터 표시
- 액션으로 데이터 업데이트
- 액션으로 데이터 삭제
- 결론

# 1. NGRX란 무엇이며 왜 사용하는가?

NGRX는 React 생태계에서 인기 있는 상태 관리 도구인 Redux에서 영감을 받은 상태 관리 라이브러리입니다. 상태를 불변하게 유지하고 순수 함수를 통해 변경을 수행하는 단방향 데이터 흐름의 원칙을 따릅니다. NGRX는 애플리케이션 상태를 중앙 집중화하여 관리와 디버깅을 쉽게 할 수 있도록 도와줍니다.

NGRX 사용의 주요 이점은 다음과 같습니다:

<div class="content-ad"></div>

- 예측 가능한 상태 관리: NGRX는 엄격한 패턴을 따라가므로 애플리케이션의 상태를 이해하고 유지하기 쉽습니다.
- 확장성: 애플리케이션이 성장함에 따라 NGRX는 확장 가능한 아키텍처를 제공하여 상태 관리를 조직화하고 효율적으로 유지합니다.
- 시간 여행 디버깅: NGRX를 사용하면 액션 및 상태 변경을 추적할 수 있어 디버깅 및 테스트가 훨씬 편리해집니다.
- 컴포넌트 간 상태 공유: NGRX를 통해 컴포넌트 간에 상태를 공유할 수 있으며, 이들이 직접적으로 관련이 없어도 가능합니다.
- 성능 향상: NGRX는 중복된 상태 변경의 수를 줄이는 방식으로 성능을 최적화합니다.

NGRX를 사용하는 이점을 이해했으니, Angular 프로젝트를 설정하고 NGRX를 시작해봅시다.

# 2. Angular 프로젝트 설정하기

시작하기 전에 시스템에 Node.js와 Angular CLI가 설치되어 있는지 확인하세요. 만약 없다면, 다음 명령어를 실행하여 설치할 수 있습니다:

<div class="content-ad"></div>

```js
npm install -g @angular/cli
```

새로운 Angular 프로젝트를 만들려면 Angular CLI를 사용하여 보일러플레이트 코드를 생성하세요:

```js
ng new ngrx-crud-example
```

새로 생성된 프로젝트 디렉토리로 이동하세요.

<div class="content-ad"></div>

cd ngrx-crud-example

우리가 Angular 프로젝트를 준비했으니, 이제 NGRX에 필요한 종속성을 설치할 차례입니다.

# 3. 스토어 생성

NGRX에서 스토어는 애플리케이션 상태를 관리하는 곳입니다. 스토어를 만들기 위해서는 @ngrx/store 패키지를 설치해야 합니다.

<div class="content-ad"></div>

```js
npm install @ngrx/store --save
```

그 다음, 우리는 애플리케이션의 상태를 정의해야 합니다. 이 예제에서는 id, 제목, 설명을 가진 간단한 작업 관리 애플리케이션을 구축한다고 가정해 봅시다.

src/app 디렉토리에 task.model.ts라는 새 파일을 만들고 Task 인터페이스를 정의하세요:

```js
// src/app/task.model.ts

export interface Task {
  id: number;
  title: string;
  description: string;
}
```

<div class="content-ad"></div>

이제 src/app 디렉토리에 task.reducer.ts라는 또 다른 파일을 만들어주세요. 이 파일에는 애플리케이션의 초기 상태와 상태 변경을 처리하는 리듀서 함수가 포함됩니다:

```js
// src/app/task.reducer.ts

import { Task } from "./task.model";

export interface AppState {
  tasks: Task[];
}

export const initialState: AppState = {
  tasks: [],
};

export function taskReducer(state = initialState, action): AppState {
  switch (action.type) {
    default:
      return state;
  }
}
```

이 코드 스니펫에서는 빈 작업 배열로 애플리케이션의 초기 상태를 정의했습니다. taskReducer 함수는 순수 함수로, 현재 상태와 액션을 인수로 받아 액션 유형을 기반으로 새 상태를 반환합니다.

# 4. 작업 정의하기

<div class="content-ad"></div>

NGRX에서의 액션은 애플리케이션의 상태 변경을 설명하는 데 사용됩니다. 이들은 수행되는 작업을 설명하는 type 속성을 가진 일반 JavaScript 객체들입니다. 우리의 작업 관리 애플리케이션에는 AddTask, UpdateTask 및 DeleteTask 세 가지 유형의 작업을 정의할 것입니다.

src/app 디렉토리에 task.actions.ts라는 새 파일을 만들고 다음과 같이 작업을 정의하세요:

```js
// src/app/task.actions.ts

import { createAction, props } from '@ngrx/store';
import { Task } from './task.model';

export const addTask = createAction('[Task] Add Task', props<{ task: Task }>());
export const updateTask = createAction('[Task] Update Task', props<{ task: Task }>());
export const deleteTask = createAction('[Task] Delete Task', props<{ id: number }>());
```

여기에서 우리는 createAction 함수를 사용하여 우리의 작업을 정의했습니다. createAction의 첫 번째 인수는 작업 유형이며, Task 기능에 속한다는 것을 나타내기 위해 대괄호로 묶여 있습니다. 두 번째 인수인 props는 작업의 페이로드를 정의하는 데 사용됩니다. 우리의 경우 addTask 및 updateTask 작업은 task 객체를 예상하며, deleteTask 작업은 페이로드로 id를 기대합니다.

<div class="content-ad"></div>

# 5. 리듀서 구현하기

이제 우리가 정의한 액션들을 처리하고 상태를 업데이트하는 리듀서를 구현해야 합니다.

task.reducer.ts 파일을 열고 이전에 정의한 액션들을 import 해주세요:

```js
// src/app/task.reducer.ts

import { Task } from "./task.model";
import { addTask, updateTask, deleteTask } from "./task.actions";

export interface AppState {
  tasks: Task[];
}

export const initialState: AppState = {
  tasks: [],
};

export function taskReducer(state = initialState, action): AppState {
  switch (action.type) {
    case addTask.type:
      return { ...state, tasks: [...state.tasks, action.task] };
    case updateTask.type:
      return {
        ...state,
        tasks: state.tasks.map((task) => (task.id === action.task.id ? action.task : task)),
      };
    case deleteTask.type:
      return { ...state, tasks: state.tasks.filter((task) => task.id !== action.id) };
    default:
      return state;
  }
}
```

<div class="content-ad"></div>

이 업데이트된 코드에서는 actions을 가져와 taskReducer 함수에 각 액션 유형을 처리하는 case를 추가했습니다. addTask의 경우 새로운 작업을 기존 작업 배열에 추가하여 새 상태를 생성합니다. updateTask의 경우 작업을 매핑하고 ID가 일치하는 것을 업데이트합니다. deleteTask의 경우 지정된 ID의 작업을 필터링합니다.

# 6. 셀렉터 생성

NGRX에서 셀렉터는 상태 스토어에서 특정 부분에 액세스하는 데 사용됩니다. 이를 통해 상태를 직접 액세스하지 않고 상태를 기반으로 파생된 데이터를 계산할 수 있습니다.

src/app 디렉토리에 task.selectors.ts라는 새 파일을 만들고 셀렉터를 정의하세요.

<div class="content-ad"></div>

```js
// src/app/task.selectors.ts

import { createSelector } from "@ngrx/store";
import { AppState } from "./task.reducer";

export const selectTasks = (state: AppState) => state.tasks;

export const selectTaskById = (id: number) =>
  createSelector(selectTasks, (tasks) => tasks.find((task) => task.id === id));
```

이 코드 스니펫에서는 @ngrx/store에서 제공하는 createSelector 함수를 사용하여 selector를 정의했습니다. selectTasks selector는 상태에서 tasks 배열을 가져오고, selectTaskById는 id 인수를 사용하여 해당 id와 일치하는 task를 반환합니다.

# 7. 액션 디스패치

스토어를 설정하고 액션을 정의하며 리듀서와 셀렉터를 구현했으므로, 이제 컴포넌트에서 액션을 디스패치하는 방법을 살펴봅시다.

<div class="content-ad"></div>

src/app 디렉토리의 app.component.ts 파일을 열어봐주세요. 이 파일에서는 작업을 추가, 업데이트 및 삭제하기 위해 액션을 디스패치할 것입니다.

```js
// src/app/app.component.ts

import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { AppState } from './task.reducer';
import { addTask, updateTask, deleteTask } from './task.actions';
import { Task } from './task.model';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  tasks: Task[] = [];

  constructor(private store: Store<AppState>) {
    this.store.select('tasks').subscribe(tasks => this.tasks = tasks);
  }

  addNewTask() {
    const newTask: Task = {
      id: this.tasks.length + 1,
      title: 'New Task',
      description: 'This is a new task.'
    };
    this.store.dispatch(addTask({ task: newTask }));
  }

  updateTask(task: Task) {
    const updatedTask: Task = { ...task, title: 'Updated Task', description: 'This task has been updated.' };
    this.store.dispatch(updateTask({ task: updatedTask }));
  }

  deleteTask(id: number) {
    this.store.dispatch(deleteTask({ id }));
  }
}
```

이 코드 스니펫에서는 @ngrx/store에서 Store와 액션, 그리고 Task 인터페이스를 포함하여 필요한 모듈 및 종속성을 가져왔습니다. 또한 작업을 추가, 업데이트 및 삭제하는 방법을 정의했으며, store.dispatch() 메서드를 사용하여 해당 액션을 디스패치했습니다.

# 8. 컴포넌트에서 데이터 표시

<div class="content-ad"></div>

지금 우리는 액션을 전송할 수 있기 때문에 작업을 표시하고 사용자가 작업을 추가, 업데이트 및 삭제할 수 있는 사용자 인터페이스를 만들어 봅시다.

src/app 디렉토리의 app.component.html 파일을 열고 다음과 같이 업데이트하세요.

```js
<!-- src/app/app.component.html -->

<div *ngFor="let task of tasks" class="task">
  <h3>{ task.title }</h3>
  <p>{ task.description }</p>
  <button (click)="updateTask(task)">업데이트</button>
  <button (click)="deleteTask(task.id)">삭제</button>
</div>
<button (click)="addNewTask()">새 작업 추가</button>
```

이 HTML 템플릿에서는 Angular의 \*ngFor 디렉티브를 사용하여 작업을 순회하고 제목과 설명을 표시합니다. 또한 각 작업을 업데이트하고 삭제할 수 있는 버튼과 새 작업을 추가하는 버튼을 추가했습니다.

<div class="content-ad"></div>

# 9. 작업을 업데이트하는 방법

작업의 "업데이트" 버튼을 클릭하면 updateTask() 메서드가 호출되어 업데이트된 작업 객체와 함께 updateTask 액션을 디스패치합니다.

# 10. 데이터를 삭제하는 방법

작업의 "삭제" 버튼을 클릭하면 deleteTask() 메서드가 호출되어 해당 작업의 id를 함께 deleteTask 액션을 디스패치합니다.

<div class="content-ad"></div>

# 11. 결론

본 글에서는 NGRX를 사용하여 Angular 애플리케이션에서 상태를 관리하고 간단한 CRUD 작업을 처리하는 방법을 살펴보았습니다. 우리는 스토어를 설정하고, 액션을 정의하고, 리듀서를 구현하고, 선택기를 생성하여 특정 상태 조각에 액세스하는 방법을 알아보았습니다. 또한 컴포넌트에서 액션을 디스패치하여 상태를 수정하는 방법도 알아보았습니다.

NGRX는 응용 프로그램 상태를 관리하는 견고하고 확장 가능한 솔루션을 제공하며, 크고 복잡한 애플리케이션에 특히 유용합니다. NGRX를 활용함으로써 Angular 애플리케이션을 유지 관리 가능하고 예측 가능하며 효율적으로 만들 수 있습니다.

이러한 지식을 통해 이제 NGRX를 나만의 Angular 프로젝트에 통합하고 더 강력하고 잘 구조화된 애플리케이션을 구축할 수 있습니다.
