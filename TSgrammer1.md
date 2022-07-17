### 1. 타입표기

- 어떤 변수 or 값에 타입 표기
- 콜론(:)붙임 ⇒ `value : type` 형태
- 12가지 타입
    - **문자열**
        
        `let sentence: string = ‘Hello’;`
        
         - 백틱으로 표현한 형태도 가능
        
        `let sentence: string = `Hello, my name is ${ fullName }`;`
        
    - **숫자**
        
        `let num: number = 10;
         let binary: number = 0b1010;`
        
    - **불리언**
        
        `let check: boolean = true;`
        
    - **배열**
        
        `let list: number[] = [1,2,3,4,5];`
        
         - 제네릭 표현
        
         `let list: Array<number> = [1,2,3,4,5];`
        
    - **튜플**
        
        `let arr: [string, boolean] = ["suin", true];`
        
    - **열거**
        
         - 상수들의 집합
        
         `enum Color {Red, Yello, Green, Blue}//상수들의 집합`
        
         `let c: Color = Color.Green;`
        
         - 인덱스를 통한 접근
        
         `let c: Color = Color[2]; //인덱스를 통한 접근`
        
    - **any**
        
         - 모든 타입에 대해 허용
        
         - 최대한 사용 자제
        
        `let str: any = 'hi';
         let num: any = 10;`
        
    - **never**
        
         - 함수의 끝에 절대 도달 X
         - 항상 오류를 출력 or 리턴값을 절대로 내보내지 않음(like 무한 루프)
        
        ```tsx
        function error(message: string): never {
            throw new Error(message); 
        }
        
        function fail() {
            return error("Something failed");
        }//반환 타입이 never로 추론됨
         
        function infiniteLoop(): never {
            while (true) {
            }
        ```
        
    - **void**
        
         - 어떤 타입도 존재 X
        
        ```tsx
        function foo(): void {
          console.log('hi'); //반환값이 없음
        } 
        ```
        
    - **null / undefined**
        
        `let undefinedValue: undefined = undefined; //이름 그대로 사용
         let nullValue: null = null`
        
    - **객체**
        
         - 원시타입이 아닌 타입
        
         - 중괄호(`{ }`)를 이용해 표현 가능
        
         - 구별자로 콤마(`,`),세미콜론(`;`)사용 가능
        
        `const user: { name: string; height: number; }={ name: '박수인', height: 165 };`
        
         - 선택 속성(=해당 속성이 존재하지 않을 수 있음) : ? 로 표현
        
        ```tsx
        const user: { name: string; height: ?; }={ 
        	name: '박수인'
        };
        ```
        
         - 읽기 전용 속성 : `readonly` 키워드 사용
        
        ```tsx
        const user: { 
        	readonly name: string; 
        	height: number; }={ name: '박수인', height: 165;};
        
        user.name = '우영우'//에러 발생
        ```
        

---

### 2. 타입 별칭(type alias)

- `type NewType = Type;`
    
    예시)
    
    ```tsx
    type UUID = string;
    
    type User = {
      name: string;
      height: number;
    };
    ```
    
- 새로운 타입 생성 X
    
    ```tsx
    type UUID = string;
    
    function getUser(uuid: UUID) {
      //...
    }
    
    getUser('박수인');
    ```
    

---

### 3. 함수

- 매개변수의 타입 + 리턴값의 타입 정보 ⇒ 함수의 타입 결정
    
    ```tsx
    function greeting(name: string): void {
      console.log(`Hello, ${name}!`);
    }
    
    function subtract(a: number, b: number): number {
      return (a-b);
    }
    ```
    

- **타입표기**
    
    `(...매개변수 나열) => 반환 타입`
    
     - 매개변수가 없는 경우
    
    `() => 반환 타입`
    
    예시)
    
    ```tsx
    function sum(a: number, b: number): number {
          return (a + b);
    }
    
    const anotherSum: (a: number, b: number) => number = sum;
    const arrowSum: (a: number, b: number) => number = (a, b) => (a + b);
    const onePlusOne: () => number = () => 2;
    ```
    
     - 타입별칭 사용 가능
    
    ```tsx
    type sum = (a: number, b: number) => number;
    const definitelySum: sum = (a, b) => (a + b)
    ```
    

- **기본 매개변수**
    
    ```tsx
    function greetings(name: string = 'stranger'): void {
      console.log(`Hello, ${name}`);
    }
    greetings('suin'); // Hello, suin!
    greetings(); // Hello, stranger! (기본 매개변수)
    ```
    

- **함수 오버로딩**
    
     - 함수는 하나 이상의 타입 시그니처를 가질 수 있다.
    
     - 함수는 단 하나의 구현을 가질 수 있다
    
    ⇒ 여러 형태의 함수 타입 정의 가능 but 실제 구현은 한 번만 가능 ! 
    
    예시) 문자열, 숫자, 불리언의 배열을 받아 2배로 만드는 각각의 함수
    
    ```tsx
    function doubleString(str: string): string {
        return `${str}${str}`;
    }
    function doubleNumber(num: number): number {
        return (num * 2);
    }
    function doubleBooleanArray(arr: boolean[]): boolean[] {
        return arr.concat(arr);
    }
    
    /*오버로딩을 사용하여 double이라는 함수로 합치기*/
    //1. 각 경우의 타입 시그니처 정의 - 함수 정의와 동일 but 본문이 생략된 형태
    function double(str: string): string;
    function double(num: number): number;
    function double(arr: boolean[]): boolean[];
    
    //2. 함수의 본문 구현
    function double(arg) {
        if (typeof arg === 'string') {
            return `${arg}${arg}`;
        } else if (typeof arg === 'number') {
            return arg * 2;
        } else if (Array.isArray(arg)) {
            return arg.concat(arg);
        }
    }
    
    //3. 함수 호출
    const num = double(3); // 6(number)
    const str = double('ab'); // abab(string)
    const arr = double([true, false]); // [true, false, true, false](boolean[])
    ```
    

- **this 타입**
    
     - 함수의 타입 시그니처에서 첫번째 매개변수 자리에  `this`  추가
    
    `function funcName( this: type, input1: inputType, input2: inputType ): returnType { ... }`
    
     - 타입 시스템을 위해서만 존재하는 일종의 가짜 타입 ( this는 매개변수가 아님 !)
    
    → 두번째 매개변수부터 매개변수로 인식
    
     - this의 타입을 정의해주지 않으면 → this는 기본적으로 `any` 타입으로 지정됨
    
    ```tsx
    interface HTMLElement {
      tagName: string;
      // ...
    }
    
    interface Handler {
      (this: HTMLElement, event: Event, callback: () => void): void;
    }
    
    let cb: any;
    
    // 실제 함수 매개변수에는 this가 나타나지 않음
    const onClick: Handler = function(event, cb) {
      // this는 HTMLElement 타입
      console.log(this.tagName);
      cb();
    }
    ```
    
     - this의 타입을 `void`로 명시하면 → 함수 내부에서 `this`에 접근하는 일 자체를 막음
    
    ```tsx
    interface NoThis {
      (this: void): void;
    }
    const noThis: NoThis = function() {
      console.log(this.a); // Property 'a' does not exist on type 'void'.
    }
    ```
    

---

참조 : [https://yamoo9.gitbook.io/typescript/types/never](https://yamoo9.gitbook.io/typescript/types/never)

[https://ahnheejong.gitbook.io/ts-for-jsdev/03-basic-grammar](https://ahnheejong.gitbook.io/ts-for-jsdev/03-basic-grammar/object) 

[https://intrepidgeeks.com/tutorial/type-script-function-type-4-this-type-definition-of-the-function-in-the-first-place-of-the-parameter-typescript](https://intrepidgeeks.com/tutorial/type-script-function-type-4-this-type-definition-of-the-function-in-the-first-place-of-the-parameter-typescript)
