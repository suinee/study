# ECMAScript

<aside>
💡 ES5까지는 변수 선언 수단이 var 키워드 뿐

</aside>

### 1. 함수 수준 스코프

- 모든 변수 선언이 함수 수준에서 이루어짐
- 코드블록(`{…}`)이 새로운 스코프 생성 X

```tsx
function foo(){
	var abc = 1;
	if(true){
		var abc = 2;//위의 abc와 동일한 변수
	}
console.log(abc)
}

foo();//실행결과:2
```

```tsx
function foo(){
	var abc = 1;
	function bar(){
		var abc = 2;//위의 abc와 동일 X
	}
console.log(abc)
}

foo();//실행결과:1
```

---

### 2. 호이스팅

- 변수 선언 & 초기화가 동시에 이루어졌을 때, js interpreter가 변수의 선언을 함수의 맨 위로 이동시키는 동작

```tsx
/*실제 코드*/
function foo() {
  console.log(bar);//undefined
  var bar = 123;//번수의 선언&초기화
}

/*js engine이 해석한 코드*/
function foo() {
	var bar;//번수의 선언
  console.log(bar);//undefined
  bar = 123;////번수의 초기화
}
```

 - 함수가 정상적으로 실행됨 + 콘솔에는 `undefined`가 찍힘

---

<aside>
💡 ES6는 let, const 변수 선언 키워드 도입

</aside>

### 3. 블록 수준 스코프

- let, const 키워드를 사용해 새로운 함수가 만들어질 때 + 대괄호(`{…}`)로 감싼 블록마다 생성되는 블록 수준 변수 정의 가능

1) let 

- 재할당 가능한 블록 레벨 변수 선언
- 해당 블록 안에서만 의미를 가짐
- let을 이용한 선언은 한 블록 내에서 한 번만 가능 (var는 여러번 선언 가능)

2) const

- 재할당 불가능한 블록 레벨 변수 선언
    
    ```tsx
    function foo(){
    	const a = 1;
    	a = 2;
    }
    
    foo();//TypeError: Assignment to constant variable.
    ```
    
     - 선언 후 재할당 불가능 → 항상 선언은 값의 초기화를 수반해야 함 (선언만 해놓은 뒤 그 값을 추후에 초기화하는 것이 불가능)
    
    ```tsx
    function foo(){
    	const a = 1;
    	console.log(a);
    }
    
    function notfoo(){
    	const a;
    	a=2;
    	console.log(a);
    }
    
    foo(); //1
    notfoo(); //SyntaxError:Missing initialize in const declaration
    ```
    
- 재할당이 불가능할 뿐, 불변값 아님 !
    
    ```tsx
    const a = 1;
    a= 4; //TypeError: Assignment to constant variable.
    
    const obj = {};
    obj.a=2; //OK
    
    const arr = [];
    arr.push(3); //OK
    ```
    
     - Object나 Array 타입의 변수는 그 객체의 내부 상태 조작 가능
    

<aside>
💡 const를 기본적으로 쓰고,

재할당이 반드시 필요한 변수만 let을 이용해서 선언하기 !

</aside>

---

### 4. 비구조화 할당 (ES6부터 지원)

1) 배열의 비구조화 할당

```tsx
const arr = [1,2];
/*기존*/
const a = arr[0];
const b = arr[1];

/*비구조화 할당*/
const [a,b,c] = [1,2];
console.log(a); //1
console.log(b); //2
console.log(c); //undefined
```

2) 객체의 비구조화 할당

```tsx
const obj = { a: 1, b: 2 };
/*기존*/
const a = obj.a;
const b = obj.b;

/*비구조화 할당*/
const obj = { a: 1, b, c} = { c: 3 };
const { a,b } = obj;
console.log(a); //1
console.log(b); //undefined
console.log(c); //3
```

---

### 5. 나머지 연산자(rest operator)

- 배열의 일부 부분 배열 → 다른 변수에 할당하고자 할 때 사용

```tsx
const [a, ...restArr] = [1,2,3,4];
console.log(restArr); //[2,3,4]

/*함수 인자에서도 사용*/
function foo(p1,p2, ...rest){
	console.log(rest);
}
foo(1,2,3,4); //[3,4]
```

---

### 6. 전개 연산자(spread operator)

- 하나의 배열 → 여러 원소로 풀어주는 역할

```tsx
function foo(a,b,c){
	console.log(a);
	console.log(b);
	console.log(c);
}

const arr[1,2,3];
foo(...[1,2,3]);
```

---

### 7. 기본 매개변수

- ES6부터 매개변수의 기본값 설정 가능

```tsx
function default(a = 1, b = 2, c = 3) {
  console.log([a, b, c]);
}

default(1, 2); // [1, 2, 3]
default(1, undefined, 4); // [1, 2, 4]
default(2, 3, undefined); // [2, 3, 3]
```

---

### 8. 화살표 함수(arrow function)

- 함수 내부에서 참조된 `this`키워드의 값은 함수가 정의되는 시점 X but 실행되는 시점에 결정됨
    
    → 어떤 방식으로 호출되냐에 따라 this 값 달라짐
    

```tsx
const a = {
  name: 'javascript',
  getName: function() {
		//내부함수
    const foo = function() {
      console.log(this.name);
    }
    foo();
  }
};

a.foo();	// undefined 
```

 -  foo내부에 this가 지정되어 있지 않음 → 전역 객체를 가리키게 됨

 - but 전역객체에 name이라는 속성 존재 X

- **화살표 함수** 사용하면 함수가 정의되는 시점에 결정 가능
- 화살표 함수 내의 모든 this ⇒ 해당 함수가 정의되는 시점에서 함수를 둘러싼 문맥의 this값으로 고정됨

```tsx
const a = {
  name: 'javascript',
  getName: function() {
		//내부함수
    const foo => {
      console.log(this.name);
    }
    foo();
  }
};

a.foo();	// undefined 
```

 - 화살표 함수에는 this라는 변수 자체가 존재하지 않음 !

 - 화살표 함수 내의 모든 this 참조는 해당 함수가 정의되는 시점에서 함수를 둘러싼 문맥(상위 스코프)의 this값으로 고정됨

---

참조 : [https://ahnheejong.gitbook.io/ts-for-jsdev/02-ecmascript](https://ahnheejong.gitbook.io/ts-for-jsdev/02-ecmascript/object-and-array/rest-and-spread-operator) 

[https://velog.io/@padoling/JavaScript-화살표-함수와-this-바인딩](https://velog.io/@padoling/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%99%80-this-%EB%B0%94%EC%9D%B8%EB%94%A9)
