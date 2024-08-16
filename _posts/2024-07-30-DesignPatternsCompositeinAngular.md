---
title: "Angular에서 Composite 디자인 패턴 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-30-DesignPatternsCompositeinAngular_0.png"
date: 2024-07-30 16:59
ogImage: 
  url: /assets/img/2024-07-30-DesignPatternsCompositeinAngular_0.png
tag: Tech
originalTitle: "Design Patterns  Composite in Angular"
link: "https://medium.com/design-patterns-composite-in-angular/wzorce-projektowe-kompozytowe-w-angular-445c289e4e9f"
isUpdated: true
updatedAt: 1723816474640
---



![이미지](/assets/img/2024-07-30-DesignPatternsCompositeinAngular_0.png)

구조 패턴은 클래스와 객체를 더 큰 구조로 결합하는 방법을 설명합니다.

구조 패턴은 다음과 같은 것들로 이루어져 있습니다:
1. 어댑터
2. 컴포지트
3. 프록시
4. 플라이급
5. 파사드
6. 브릿지
7. 데코레이터

컴포지트 디자인 패턴은 전체-부분 관계의 계층 구조를 만들거나 트리 데이터 구조를 만들 때 사용됩니다.

<div class="content-ad"></div>

복합 패턴의 구조는 여러 요소로 구성되어 있습니다:

- **클라이언트(Client)**: 구성 요소를 통해 모든 요소와 상호 작용합니다. 결과적으로 클라이언트는 간단한 요소와 복잡한 트리 요소 모두에 동일한 방식으로 작동할 수 있습니다.

- **컴포넌트(Component)**: 컴포넌트 인터페이스는 간단한 요소와 복잡한 트리 요소에 공통된 작업을 설명합니다.

- **리프(Leaf)**: 하위 요소가 없는 트리의 기본 요소입니다. 일반적으로 리프 컴포넌트는 대부분의 기능을 수행합니다. 왜냐하면 하위 요소가 없기 때문이죠.

<div class="content-ad"></div>

컨테이너 (또는 컴포지트) - 자식 요소인 잎이나 다른 컨테이너를 가지고 있는 요소입니다. 컨테이너는 내용물의 구체적인 클래스를 모르고 있습니다. 모든 자식 요소와는 구성 요소 인터페이스를 통해서만 통신합니다.

컴포지트와 구성을 혼동하지 마세요. 컴포지트는 객체의 계층 구조에서 작동하는 패턴입니다. 구성은 한 객체가 다른 객체를 포함하는 것입니다.

전체 데모를 보고 싶다면, 내 데모 프로젝트를 확인해보세요.

컴포지트 디자인 패턴은 여러 방법으로 설명할 수 있습니다. 저는 회사 구조를 표시하면서 매니저를 추가하고 추가된 직원을 매니저에 할당하는 기능의 예시를 제시할 것입니다.

<div class="content-ad"></div>

처음에 EmployeeHierarchyStructure 인터페이스에 일부 메서드가 추가되었습니다. ?는 선택 사항이라는 것을 나타냅니다. EmployeeHierarchyStructure 인터페이스의 구현은 다음과 같이 제시됩니다.

```js
export interface EmployeeHierarchyStructure {
    add?(employee: EmployeeHierarchyStructure): void;
    remove?(employee: EmployeeHierarchyStructure): void;
    getChild?(index: number): EmployeeHierarchyStructure | null;
    getName(): string;
    printDetails(indent?: string): void;
    getEmployees?(): EmployeeHierarchyStructure[];
}
```

Employee 클래스는 구현을 필요로하지 않습니다. 복합 디자인 패턴의 구조에서 Employee 클래스는 단말 요소를 대표합니다. Employee 클래스의 구현은 다음과 같습니다.

```js
export class Employee implements EmployeeHierarchyStructure {
    private name: string; 

    constructor(name: string ) {
        this.name = name; 
    }

    add(employee: EmployeeHierarchyStructure): void {
        throw new Error("Cannot add to a employee.");
    }

    remove(employee: EmployeeHierarchyStructure): void {
        throw new Error("Cannot remove from a employee.");
    }

    getChild(index: number): EmployeeHierarchyStructure | null {
        return null;
    }

    getName(): string {
        return this.name;
    }

    printDetails(indent: string = ''): void {
        console.info(`${indent}Employee: ${this.getName()}`);
    }

    getEmployees(): EmployeeHierarchyStructure[] {
        return [];
    }
}
```

<div class="content-ad"></div>

Manager 클래스는 Composite (Container) 엘리먼트를 반영합니다. 아래는 Manager 클래스의 구현 내용입니다:

```js
export class Manager implements EmployeeHierarchyStructure {
    private name: string; 
    private employees: Set<EmployeeHierarchyStructure>;

    constructor(name: string) {
        this.name = name; 
        this.employees = new Set<EmployeeHierarchyStructure>();
    }

    add(employee: EmployeeHierarchyStructure): void {
        this.employees.add(employee);
    }

    remove(employee: EmployeeHierarchyStructure): void {
        this.employees.delete(employee);
    }

    getChild(index: number): EmployeeHierarchyStructure | null {
        return Array.from(this.employees)[index] || null;
    }

    getName(): string {
        return this.name;
    }

    printDetails(indent: string = ''): void {
        console.info(`${indent} Manager: ${this.getName()}`);
        for (const employee of this.employees) {
            employee.printDetails(indent + '  ');
        }
    }

    getEmployees(): EmployeeHierarchyStructure[] {
        return Array.from(this.employees);
    }
}
```

Client 엘리먼트는 CompositeComponent로, 최상위 레벨에서 회사 소유주를 소유자로 정의합니다. addEmployeeForManager 메서드에서 일부 집계가 발생합니다. 매니저 객체를 제거하면 직원 객체를 생성하는 동안에는 오류가 발생하지 않고, 매니저에게 직원을 할당하는 과정에서만 오류가 발생합니다. 직원을 매니저에게 할당하는 것은 완전한 집계 (구성)를 나타냅니다. CompositeComponent 클래스의 구성요소는 다음과 같이 구현되었습니다:

```js
import { Component } from '@angular/core';
import { Employee, Manager } from '../model/model'

export interface ConfigurationOfAddingAnEmployee {
  managerName: string;
  employeeNeme: string;
  selectedManagerName: string;
}

@Component({
  selector: 'app-composite',
  templateUrl: './composite.component.html',
  styleUrls: ['./composite.component.scss']
})
export class CompositeComponent {

  boss: Manager = new Manager('Owner');
  selectedManager: Manager | null = null;

  company: ConfigurationOfAddingAnEmployee = {
    managerName: '',
    employeeNeme: '',
    selectedManagerName: ''
  };

  addManager(name: string) {
    const manager = new Manager(name);
    this.boss.add(manager);
    this.company.managerName = '';
  }

  addEmployeeForManager(managerName: string, employeeNeme: string) {
    const manager = this.findManager(managerName);
    if (manager) {
      const developer = new Employee(employeeNeme);
      manager.add(developer);
      this.company.employeeNeme = '';
    }
  }

  findManager(name: string): Manager | null {
    const managers = this.getAllManagers(this.boss);
    return managers.find(manager => manager.getName() === name) || null;
  }

  getAllManagers(manager: Manager): Manager[] {
    let managers: Manager[] = [manager];
    for (const employee of manager.getEmployees()) {
      if (employee instanceof Manager) {
        managers = managers.concat(this.getAllManagers(employee));
      }
    }
    return managers;
  }

  setSelectedManager(managerName: string) {
    this.selectedManager = this.findManager(managerName);
  }

}
```

<div class="content-ad"></div>

`CompositeComponent` 템플릿은 다음과 같습니다:

```js
<div class="container">
  <div class="row">
    <label for="selectedManager">매니저 선택: </label>
    <select id="selectedManager" [(ngModel)]="company.selectedManagerName" (ngModelChange)="setSelectedManager($event)">
      <option *ngFor="let manager of getAllManagers(boss)" [value]="manager.getName()">
        { manager.getName() }
      </option>
    </select>
  </div>

  <div class="row">
    <label for="managerName">매니저 이름: </label>
    <input type="text" id="managerName" name="managerName" [(ngModel)]="company.managerName">
    <button (click)="addManager(company.managerName)">매니저 추가</button>
  </div>

  <div class="row" *ngIf="selectedManager">
    <label for="employeeName">직원 이름: </label>
    <input type="text" id="employeeName" name="employeeName" [(ngModel)]="company.employeeName">
    <button (click)="addEmployeeForManager(company.selectedManagerName, company.employeeName)">
      매니저에게 직원 추가
    </button>
  </div>
</div>

<div>
  <h3>회사 구조</h3>
  <ul>
    <ng-container *ngIf="boss">
      <li *ngFor="let manager of boss.getEmployees()">
        <app-employee [manager]="manager"/>
      </li>
    </ng-container>
  </ul>
</div>
```

개요

Composite 패턴은 같은 인터페이스를 공유하는 단순한 leaf와 복잡한 container 두 가지 기본 요소 유형을 정의합니다. container는 leaf와 다른 container의 모음일 수 있습니다. 이를 통해 tree와 유사한 중첩 및 재귀적 객체 구조를 구성할 수 있습니다.

<div class="content-ad"></div>

자료:

- Java. Wzorce projektowe. 번역: Piotr Badarycz. 원제: Java design patterns. A Tutorial. 저자: James William Cooper
- [Composite Design Pattern - Refactoring Guru](https://refactoring.guru/pl/design-patterns/composite)
- [Composite Design Pattern - Java.edu.pl](https://java.edu.pl/inne/designPatterns/8.composite.php)

Angular에서 디자인 패턴과 관련된 다른 내 기사:

구조 패턴:
- Angular에서 프록시 디자인 패턴
- Angular에서 어댑터 디자인 패턴
생성 패턴:
- Angular에서 빌더 디자인 패턴

<div class="content-ad"></div>

영어 실력이 좋지 않아요. 영어 실력 향상을 위해 글을 쓰고 있어요.