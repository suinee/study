# 타입스크립트 기초 문법(2)

### 1. 제네릭

- 존재할 수 있는 모든 타입에 대해 오버로딩을 구현하는 것은 불가능(가능한 타입의 수가 무한하므로)
    
    → 인자와 반환 타입을 `any`로 정의한다면 동작함 but  타입 정보를 모두 잃게 됨
    
- 여러 타입에 대해 동작하는 함수 정의
- 해당 함수를 정의할 때는 알 수 없고, 사용할 때에만 알 수 있는 타입 정보 사용

- **타입 변수**
    
     - 요소를 사용하는 시점에서만 알 수 있는 타입을 담아두기 위해서 필요함 !
    
     - PascalCase 명명법을 사용
    
    |  | 함수 | 제네릭 |
    | --- | --- | --- |
    | 정의 시점 | 매개변수 a: number | 타입 변수 <T> |
    | 정의 예시 | function (a: number) { ... } | type MyArray<T> = T[] |
    | 사용 시 | 실제 값 사용 (3, 41.2 등) | 실제 타입 사용 (number, string 등) |
    | 사용 예시 | a(3); a(41.2); | type MyNumberArray = MyArray<number>;  type MyStringArray = MyArray<string>;  |
    
    <aside>
    💡 타입 변수- 타입의 관계는 매개변수-인자 값의 관계와 비슷함
    
    </aside>
    

- **제네릭 함수**
    1. **타입변수를 이용하여 정의**
        
        `function 함수명<타입 변수>(인자 타입): 반환 타입 { … }`
        
        예시) 
        
        ```tsx
        // 임의의 타입 T에 대해, getFirstElem은 T 타입 값의 배열 arr를 인자로 받아 
        // T 타입 값을 반환하는 함수
        
        function getFirstElem<T>(arr: T[]): T { 
        	/* ... */
        }
        ```
        
    2. **타입 변수가 있던 자리에 타입 인자를 넣어 호출**
        
        ```tsx
        const languages: string[] = ['TypeScript', 'JavaScript'];
        const language = getFirstElem<string>(languages); //language의 타입은 문자열
        ```
        

- **제네릭 타입 별칭**
    
    ```tsx
    type MyArray<T> = T[];
    
    //별칭이름:타입변수 정의
    const languages: MyArray<string> = ['TypeScript', 'JavaScript'];
    ```
    

---

### 2. 유니온 타입

- 어떤 타입이 가질 수 있는 경우의 수를 나열할 때 사용 → 여러 경우 중 하나인 타입 표현
- 가능한 모든 타입 ⇒ 파이프(`|`)로 표현
    
    ex) `A | B`= `A`또는 `B`타입일 수 있는 타입
    
    ```tsx
    function square(value: number, returnString: boolean = false): string | number {
      const squared = value * value;
    
    	//returnString 값이 true이면 
      if (returnString) {
        return squared.toString(); //string 타입 반환
      }
    
    	//returnString 값이 false이면
      return squared; //number 타입 반환
    }
    
    const stringOrNumber: string | number = square(randomNumber, randomBoolean);
    
    /*별칭 사용*/
    type SquaredType = string | number;
    function square(value: number, returnOnString: boolean = false): SquaredType {
      /* ... */
    }
    ```
    
- 여러 줄에 적을 시
    
    ```tsx
    /* 방법 1 */
    type SquaredType 
    	= string 
    	| number;
    
    /* 방법 2 */
    type SquaredType =
    	| string 
    	| number;
    ```
    

---

### 3. 인터섹션 타입

- 이미 존재하는 여러 타입을 모두 만족하는 타입 표현
- 여러 타입 ⇒ 앰퍼샌드(`&`)로 이어서 표현
    
    ex) `A & B` =  `A`타입에도, `B`타입에도 할당 가능
    
    ```tsx
    type Korean = { language: string };
    type Student = { major: string };
    
    type KoreanStudent = Korean & Student;
    
    const suinPark: KoreanStudent = { 
      language: 'Korean',
      major: 'cyber security',
    };
    ```
    
- 어떤 값도 만족하지 않는 인터섹션 타입 가능
    
    `type Infeasible = string & number // 문자열이면서 동시에 숫자인 값 존재 X` 
    
- 여러 줄에 적을 시
    
    ```tsx
    /* 방법 1 */
    type KoreanStudent
    	= Korean 
    	& Student;
    
    /* 방법 2 */
    type KoreanStudent =
    	& Korean 
    	& Student;
    ```
    

---

### 4. 열거형

- 유한한 경우의 수를 갖는 값의 집합을 표현
- **숫자 열거형**
    
     - `number`타입 기반 
    
     - 멤버의 값 초기화 안하면 → 자동으로 멤버의 값은 0부터 순차적으로 증가함
    
    ```tsx
    enum Direction {
      East,// = 0
      West,// = 1
      South,// = 2
      North// = 3
    }
    
    /*객체의 멤버에 접근*/
    const south: Direction = Direction.South;
    console.log(south); // 2
    ```
    
     - 직접 초기화 시, 초기화 되지 않은 멤버가 존재하면 
    
    → 이전에 초기화된 멤버의 값으로부터 순차적으로 증가해서 결정
    
    ```tsx
    enum Direction {
      East = 4,
      West, // = 5
      South = 9,
      North // = 10
    }
    ```
    

- **문자열 열거형**
    
     - `string`타입 기반 
    
     - 자동 증가 개념 X  → 문자열 멤버 이후의 모든 멤버는 명시적으로 초기화되어야 함
    
    ```tsx
    enum Direction {
      East = 'EAST',
      West = 'WEST',
      South = 'SOUTH',
      North = 'NORTH'
    }
    ```
    
     - 값 → 키 의 역방향 매핑(reverse mapping) 존재 X
        <aside>
💡 **숫자 열거형** : 역방향 매핑 존재 O

```tsx
var Direction;
(function (Direction) {
    Direction[Direction["East"] = 0] = "East";
    Direction[Direction["West"] = 1] = "West";
    Direction[Direction["South"] = 2] = "South";
    Direction[Direction["North"] = 3] = "North";
})(Direction || (Direction = {}));
var east = Direction.East;
```

 - 키 → 값으로의 매핑 정의 : `Direction["EAST"] = 0`

 - 값 → 키로의 역방향 매핑 : `Direction[Direction["East"] = 0] = "East”`

**문자열 열거형** :  역방향 매핑 존재 X

```tsx
var Direction;
(function (Direction) {
    Direction["East"] = "EAST";
    Direction["West"] = "WEST";
    Direction["South"] = "SOUTH";
    Direction["North"] = "NORTH";
})(Direction || (Direction = {}));
```

 - 키 → 값으로의 매핑 : `Direction["East"] = "EAST"`

</aside>

- **상수 멤버 vs 계산된 멤버**
    - 상수 멤버(constant member) : 컴파일 타임에 알 수 있는 상수값으로 초기화된 멤버
        
         - `const enum` 키워드 이용 → 열거형의 구조가 컴파일 과정에서 완전히 사라짐 & 멤버의 값은 상수값으로 대체 → 오버헤드 ⬇️
        
        ```tsx
        enum Direction {
          East,
          West,
          South,
          North
        }
        console.log(Direction.South);
        ```
        
        → js파일로 컴파일
        
        ```tsx
        console.log(2);
        ```
        
    - 계산된 멤버(computed member) : 런타임에 결정되는 값으로 초기화된 멤버 → 실제로 코드를 실행해봐야 앎
        
        ```tsx
        function getAnswer() {
          return 42;
        }
        enum SpecialNumbers {
          Answer = getAnswer(), //런타임에 결정됨
          Mystery //에러 : 계산된 멤버 뒤에 오는 멤버는 반드시 초기화어야 함
        }
        ```
        
    
- **유니온 열거형**
    
     - 열거형의 모든 멤버가 아래 경우 중 하나에 해당하는 열거형
    
    (1) 암시적으로 초기화 된 값 (값이 표기되지 않음)
    
    (2) 문자열 리터럴
    
    (3) 숫자 리터럴
    
    <aside>
    💡 리터럴 타입 : 문자열이나 숫자에 단 하나의 값 지정
    
    `const a: 42 = 42;`
    
    </aside>
    
    ```tsx
    enum ShapeKind {
      Circle,
      Triangle = 3,
      Square
    }
    ```
    
     - 유니온 열거형의 멤버는 값인 동시에 타입 
    
    ```tsx
    type Circle = {
      kind: ShapeKind.Circle;
      radius: number;
    }
    type Triangle = {
      kind: ShapeKind.Triangle;
      maxAngle: number;
    }
    type Square = {
      kind: ShapeKind.Square;
      maxLength: number;
    }
    type Shape = Circle | Triangle | Square;
    ```
    

---

참조 : [https://ahnheejong.gitbook.io/ts-for-jsdev/03-basic-grammar](https://ahnheejong.gitbook.io/ts-for-jsdev/03-basic-grammar/generics)

[https://velog.io/@youn0097/TypeScript-4-리터럴-타입](https://velog.io/@youn0097/TypeScript-4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%ED%83%80%EC%9E%85)
