# Typescript

### 1. Javascript

- interpreter language(소스코드 한 번에 한 줄씩 바로 번역 & 실행)
    
    → 런타임 속도 빠름 but 타입 안정성 보장 X
    
- 소규모 프로젝트 작업 시 선호됨
- 독립적으로 사용가능

---

### 2. Typescript

```tsx
class Person{
	private name: string;

	constructor(name: string){
		this.name=name;
	}

	sayHello(){
		return "Hello"+this.name;
	}
}

const person = new Person("Park");
console.log(person.sayHello());
```

- 정적  타입 시스템을 도입한 자바스크립트
- type checker을 통해 실행 전, 예상 동작 여부를 확인 가능
- js의 상위 집합(superset) → js의 모든 기능 존재
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/acb88d12-f0e4-4c65-a15f-9a5ab0c7352f/Untitled.png)
    
- compiled language(1. build : 소스코드→ 다른 목적 코드로 번역 2. 컴파일 시 실행가능한 binary program 생성됨)
    
    → 컴파일 시간이 조금 걸리지만, 안정성 보장됨
    
     - 동작 방식 : ts→compile(transfile)→js→실행
    
                                ↘️ helper코드가 위아래로 추가됨 → 절대적인 코드 양 : ts>js
    
- 클래스, 인터페이스 특징 지원 → 상속, 캡슐화, 생성자 지원가능(객체지향 프로그래밍 환경 제공)
- ES6+ 문법들을 ES5(or ES3)로 변경해줌 → 크로스브라우징 문제 해결
- 변수에 타입 선언 → 변수에 엄격한 타이핑 적용되어 타입 안정성 확보됨
- 복잡한 프로젝트 처리 작업시 선호됨
- js에 의존적(js로 컴파일된 후 실행)
- 타입 선언 생략하면 동적으로 타입 결정(타입 추론)
    
    `foo=’Hello’; // string 추론`
    
    `foo = true; // boolean 추론`
    
- 타입 선언 생략 + 값 할당 x → any 타입
    
    `let foo // any와 동치`
    

---

### 3. 정적 타입분석의 장점

1) 빠른 버그 발견 : 프로그램이 실제로 진행되기 전 상당수의 오류 캐치 가능

2) 툴링 : 컴파일러, 코드에디터가 코드르 실행하지 않고도 프로그램에 대한 정보 획득 가능 → 에디터의 자동완성 기능

3) 주석 역할 : 타입 정의와 다르게 동작하는 프로그램은 실행 자체가 불가능

---

참조 : [https://choseongho93.tistory.com/319](https://choseongho93.tistory.com/319) 

[https://imagineu.tistory.com/6](https://imagineu.tistory.com/6) 

[https://ahnheejong.gitbook.io/ts-for-jsdev](https://ahnheejong.gitbook.io/ts-for-jsdev/01-introducing-typescript/static-type-analysis)
