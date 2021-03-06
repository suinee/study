# ECMAScript

<aside>
π‘ ES5κΉμ§λ λ³μ μ μΈ μλ¨μ΄ var ν€μλ λΏ

</aside>

### 1. ν¨μ μμ€ μ€μ½ν

- λͺ¨λ  λ³μ μ μΈμ΄ ν¨μ μμ€μμ μ΄λ£¨μ΄μ§
- μ½λλΈλ‘(`{β¦}`)μ΄ μλ‘μ΄ μ€μ½ν μμ± X

```tsx
function foo(){
	var abc = 1;
	if(true){
		var abc = 2;//μμ abcμ λμΌν λ³μ
	}
console.log(abc)
}

foo();//μ€νκ²°κ³Ό:2
```

```tsx
function foo(){
	var abc = 1;
	function bar(){
		var abc = 2;//μμ abcμ λμΌ X
	}
console.log(abc)
}

foo();//μ€νκ²°κ³Ό:1
```

---

### 2. νΈμ΄μ€ν

- λ³μ μ μΈ & μ΄κΈ°νκ° λμμ μ΄λ£¨μ΄μ‘μ λ, js interpreterκ° λ³μμ μ μΈμ ν¨μμ λ§¨ μλ‘ μ΄λμν€λ λμ

```tsx
/*μ€μ  μ½λ*/
function foo() {
  console.log(bar);//undefined
  var bar = 123;//λ²μμ μ μΈ&μ΄κΈ°ν
}

/*js engineμ΄ ν΄μν μ½λ*/
function foo() {
	var bar;//λ²μμ μ μΈ
  console.log(bar);//undefined
  bar = 123;////λ²μμ μ΄κΈ°ν
}
```

 - ν¨μκ° μ μμ μΌλ‘ μ€νλ¨ + μ½μμλ `undefined`κ° μ°ν

---

<aside>
π‘ ES6λ let, const λ³μ μ μΈ ν€μλ λμ

</aside>

### 3. λΈλ‘ μμ€ μ€μ½ν

- let, const ν€μλλ₯Ό μ¬μ©ν΄ μλ‘μ΄ ν¨μκ° λ§λ€μ΄μ§ λ + λκ΄νΈ(`{β¦}`)λ‘ κ°μΌ λΈλ‘λ§λ€ μμ±λλ λΈλ‘ μμ€ λ³μ μ μ κ°λ₯

1) let 

- μ¬ν λΉ κ°λ₯ν λΈλ‘ λ λ²¨ λ³μ μ μΈ
- ν΄λΉ λΈλ‘ μμμλ§ μλ―Έλ₯Ό κ°μ§
- letμ μ΄μ©ν μ μΈμ ν λΈλ‘ λ΄μμ ν λ²λ§ κ°λ₯ (varλ μ¬λ¬λ² μ μΈ κ°λ₯)

2) const

- μ¬ν λΉ λΆκ°λ₯ν λΈλ‘ λ λ²¨ λ³μ μ μΈ
    
    ```tsx
    function foo(){
    	const a = 1;
    	a = 2;
    }
    
    foo();//TypeError: Assignment to constant variable.
    ```
    
     - μ μΈ ν μ¬ν λΉ λΆκ°λ₯ β ν­μ μ μΈμ κ°μ μ΄κΈ°νλ₯Ό μλ°ν΄μΌ ν¨ (μ μΈλ§ ν΄λμ λ€ κ·Έ κ°μ μΆνμ μ΄κΈ°ννλ κ²μ΄ λΆκ°λ₯)
    
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
    
- μ¬ν λΉμ΄ λΆκ°λ₯ν  λΏ, λΆλ³κ° μλ !
    
    ```tsx
    const a = 1;
    a= 4; //TypeError: Assignment to constant variable.
    
    const obj = {};
    obj.a=2; //OK
    
    const arr = [];
    arr.push(3); //OK
    ```
    
     - Objectλ Array νμμ λ³μλ κ·Έ κ°μ²΄μ λ΄λΆ μν μ‘°μ κ°λ₯
    

<aside>
π‘ constλ₯Ό κΈ°λ³Έμ μΌλ‘ μ°κ³ ,

μ¬ν λΉμ΄ λ°λμ νμν λ³μλ§ letμ μ΄μ©ν΄μ μ μΈνκΈ° !

</aside>

---

### 4. λΉκ΅¬μ‘°ν ν λΉ (ES6λΆν° μ§μ)

1) λ°°μ΄μ λΉκ΅¬μ‘°ν ν λΉ

```tsx
const arr = [1,2];
/*κΈ°μ‘΄*/
const a = arr[0];
const b = arr[1];

/*λΉκ΅¬μ‘°ν ν λΉ*/
const [a,b,c] = [1,2];
console.log(a); //1
console.log(b); //2
console.log(c); //undefined
```

2) κ°μ²΄μ λΉκ΅¬μ‘°ν ν λΉ

```tsx
const obj = { a: 1, b: 2 };
/*κΈ°μ‘΄*/
const a = obj.a;
const b = obj.b;

/*λΉκ΅¬μ‘°ν ν λΉ*/
const obj = { a: 1, b, c} = { c: 3 };
const { a,b } = obj;
console.log(a); //1
console.log(b); //undefined
console.log(c); //3
```

---

### 5. λλ¨Έμ§ μ°μ°μ(rest operator)

- λ°°μ΄μ μΌλΆ λΆλΆ λ°°μ΄ β λ€λ₯Έ λ³μμ ν λΉνκ³ μ ν  λ μ¬μ©

```tsx
const [a, ...restArr] = [1,2,3,4];
console.log(restArr); //[2,3,4]

/*ν¨μ μΈμμμλ μ¬μ©*/
function foo(p1,p2, ...rest){
	console.log(rest);
}
foo(1,2,3,4); //[3,4]
```

---

### 6. μ κ° μ°μ°μ(spread operator)

- νλμ λ°°μ΄ β μ¬λ¬ μμλ‘ νμ΄μ£Όλ μ­ν 

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

### 7. κΈ°λ³Έ λ§€κ°λ³μ

- ES6λΆν° λ§€κ°λ³μμ κΈ°λ³Έκ° μ€μ  κ°λ₯

```tsx
function default(a = 1, b = 2, c = 3) {
  console.log([a, b, c]);
}

default(1, 2); // [1, 2, 3]
default(1, undefined, 4); // [1, 2, 4]
default(2, 3, undefined); // [2, 3, 3]
```

---

### 8. νμ΄ν ν¨μ(arrow function)

- ν¨μ λ΄λΆμμ μ°Έμ‘°λ `this`ν€μλμ κ°μ ν¨μκ° μ μλλ μμ  X but μ€νλλ μμ μ κ²°μ λ¨
    
    β μ΄λ€ λ°©μμΌλ‘ νΈμΆλλμ λ°λΌ this κ° λ¬λΌμ§
    

```tsx
const a = {
  name: 'javascript',
  getName: function() {
		//λ΄λΆν¨μ
    const foo = function() {
      console.log(this.name);
    }
    foo();
  }
};

a.foo();	// undefined 
```

 -  fooλ΄λΆμ thisκ° μ§μ λμ΄ μμ§ μμ β μ μ­ κ°μ²΄λ₯Ό κ°λ¦¬ν€κ² λ¨

 - but μ μ­κ°μ²΄μ nameμ΄λΌλ μμ± μ‘΄μ¬ X

- **νμ΄ν ν¨μ** μ¬μ©νλ©΄ ν¨μκ° μ μλλ μμ μ κ²°μ  κ°λ₯
- νμ΄ν ν¨μ λ΄μ λͺ¨λ  this β ν΄λΉ ν¨μκ° μ μλλ μμ μμ ν¨μλ₯Ό λλ¬μΌ λ¬Έλ§₯μ thisκ°μΌλ‘ κ³ μ λ¨

```tsx
const a = {
  name: 'javascript',
  getName: function() {
		//λ΄λΆν¨μ
    const foo => {
      console.log(this.name);
    }
    foo();
  }
};

a.foo();	// undefined 
```

 - νμ΄ν ν¨μμλ thisλΌλ λ³μ μμ²΄κ° μ‘΄μ¬νμ§ μμ !

 - νμ΄ν ν¨μ λ΄μ λͺ¨λ  this μ°Έμ‘°λ ν΄λΉ ν¨μκ° μ μλλ μμ μμ ν¨μλ₯Ό λλ¬μΌ λ¬Έλ§₯(μμ μ€μ½ν)μ thisκ°μΌλ‘ κ³ μ λ¨

---

μ°Έμ‘° : [https://ahnheejong.gitbook.io/ts-for-jsdev/02-ecmascript](https://ahnheejong.gitbook.io/ts-for-jsdev/02-ecmascript/object-and-array/rest-and-spread-operator) 

[https://velog.io/@padoling/JavaScript-νμ΄ν-ν¨μμ-this-λ°μΈλ©](https://velog.io/@padoling/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%99%80-this-%EB%B0%94%EC%9D%B8%EB%94%A9)
