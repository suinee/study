### 1. 프로미스

- 실행은 바로 하지만, 결과는 나중에 받는 객체
- 생성자 인자로 `reslove`, `reject`, 하나의 함수를 받음
- 함수 본문에 비동기로 작동할 수 있는 특정 로직 실행 → 결과에 따라 reslove / reject 호출
    
    ```tsx
    //프로미스 객체 생성
    const promise = new Promise((resolve, reject) => {
    	if(condition){
    		resolve('성공');
    	}else{
    		reject('실패');
    	}	
    });
    
    /*
    new Promise 바로 실행 
    but 결과값은 then을 붙였을때 받음
    */
    ```
    
- **then, catch 메소드**
    
    ```tsx
    promise
    	.then((message)=>{ //성공(reslove)한 경우 실행
    		console.log(message); //message ='성공'
    	})
    	.catch((error)=>{ //실패(reject)한 경우 실행
    		console.log(error); //error='실패'
    	})
    	.finally(()=>{ //끝나고 무조건 실행
    		console.log('must');
    	});
    ```
    
    1. 프로미스 객체 내부에 reslove, reject를 매개변수로 갖는 콜백함수 넣음
        
         - reslove에 넣은 인수 → then의 매개변수에서 받음
        
         - reject에 넣은 인수 → catch의 매개변수에서 넣음
        
    2. promise 변수에 then, catch 매서드 붙일 수 있음
        
         - then, catch 순으로 실행되는 것 X , 서로 각자 다른 상황에서 실행되는 것
        
         - then 메서드들끼리 → 순차적으로 실행 
        
         - 프로미스 내부에서 reslove 호출 → then 실행
        
         - 프로미스 내부에서 reject 호출 → catch 실행
        
         - 성공/실패 여부 무관 → finally 실행
        
- 프로미스 체인
    
    ```tsx
    //프로미스 객체 생성
    const promise = new Promise((resolve, reject) =>{
    	if(condition){
    		resolve('성공');
    	}else{
    		reject('실패');
    	}
    });
    
    //...//
    
    promise
    	//1. '성공'을 받아 다음 then에 넘겨줌
    	.then((message)=>{ 
    		return new Promise((resolve, reject)=>{
    			resolve(message); //message = '성공'
    		});
    	})
    	//유의 : then에서 new Promise를 return해야 다음 then에서 받을 수 있음
    
    	//2. message2로 '성공'을 받아 그대로 다음 then에 넘겨줌
    	.then((message2)=>{ 
    		console.log(message2);
    		return new Primise((resolve,reject)=>{
    			resolve(message2);
    		});
    	})
    	//유의 : then에서 new Promise를 return해야 다음 then에서 받을 수 있음
    
    	//3. message3로 '성공'을 받아 콘솔에 출력
    	.then((message3)=>{
    		console.log(message3);
    	})
    
    	//실패(reject)한 경우 실행
    	.catch((error)=>{
    		console.log(error); //error='실패'
    	})
    	//끝나고 무조건 실행
    	.finally(()=>{
    		console.log('must');
    	});
    ```
    

---

### 2. Async/Await
- Promise가 사용되어 콜백 지옥 해결했지만, then-catch가 반복되어 코드가 장황함
- Promise를 사용한 코드를 더 깔끔하게 줄여줌
    
    ```tsx
    /*기존 : 프로미스로 변환한 코드*/
    
    function findAndSaveUser(Users){
    	User.findOne({})
    		.then(user)=>{
    			user.name='zero';
    			return user.save;
    		})
    		.then((user)=>{
    			return User.findOne({gender:'m'});
    		})
    		.then((user)=>{
    			//...
    		})
    		.catch(err=>{
    			console.error(err);
    		});
    }
    
    /*개선 : async/await문법 사용한 코드*/
    
    async function findAndSaveUser(Users){
    	let user = await Users.findOne({}); // 1.resolve될 때까지 기다림
    	user.name='zero'; // 2.user변수 초기화
    	user = await user.save();
    	user = await Users.findOne({gender:'m'});
    
    	//...
    }
    ```
    
    - 함수 선언부 : `async function`으로 교체
    
     - 프로미스 : 앞에 `await` 붙임
    
     → 함수는 해당 프로미스가 resolve될 때까지 기다림 → 다음 로직으로 넘어감
    
     - 에러를 처리하는 부분 없음 
    
    ```jsx
    /*에러 처리 코드 추가*/
    
    async function findAndSaveUser(Users){
    	try{	
    		let user = await Users.findOne({}); // 1.resolve될 때까지 기다림
    		user.name='zero'; // 2.user변수 초기화
    		user = await user.save();
    		user = await Users.findOne({gender:'m'});
    		
    		//...
    	}catch(error){
    		console.error(error);
    	}
    }
    ```
    
     - catch문이 에러 처리 
    
     - try-catch문으로 로직 감쌈
 - 최상위 레벨 코드(top-level code)에 `await`을 사용할 수 없음 !
    
    ⇒ 익명 async 함수로 코드를 감싸면 최상위 레벨 코드에도 `await` 사용가능 
    
    ```tsx
    //최상위 레벨에서 문법 에러
    let response = await fetch('/article/promise-chaining/user.json');
    let user = await response.json();
    
    /*익명 asnyc함수로 코드 감쌈*/
    (async () => {
      let response = await fetch('/article/promise-chaining/user.json');
      let user = await response.json();
      ...
    })();
    ```
    

---

참조 : [https://ko.javascript.info/async-await](https://ko.javascript.info/async-await)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  
   
