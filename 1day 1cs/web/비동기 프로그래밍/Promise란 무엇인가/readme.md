### Promise란 무엇인가 (1차)?



참조 래퍼런스

(https://ljtaek2.tistory.com/158)



#### Promiss란?

- 자바스크립트에서 <b>비동기 동작을 다루는 하나의 패턴</b>이며,  <b>어떤 일의 진행 상태를 나타내는 객체</b>로 진행"상태"과 "값"이라는 속성을 가지고 있음



####  Promise 탄생 전 비동기 처리방식

1. jQuery와 ajax를 활용한 방식
2. setTimeout()과 콜백을 통한 비동기 처리 방식
   - 콜백함수안에 콜백함수를 넣는 무한 콜백의 방식
     - 코드의 가독성 및 수정이 매우 어려움
     - 콜백 지옥의 발생
     - 이러한 단점을 해결하기 위해 Promise와 async , await가 등장



#### 콜백 함수와 Promise의 비동기 방식 차이

```javascript
// callback 함수 사용

function delay(sec,callback){
    setTimeout(()=>{
        callback(new Date().toString());
    },sec*1000)
}

delay(1, (result)=>{
    console.log(1,result)
})


// promise 활용

function delayP(sec){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve(new Date().toString())
        },sec*1000)
    })
}

delayP(1).then((result) => {
    console.log(1,result)
})
```

- delay함수의 경우 옵션과 콜백함수를 넘김
- delayP함수의 경우 옵션만 넘김

#### promise함수를 보다 자세히 뜯어보면

```javascript
function delayP(sec){
    return new Promise((resolve,reject)=>{
        if{
            resolve('result')
        }else{
            reject('failure reason')
        }
    })
}




function delayP(sec){
	return new Promise((resolve, reject) => {
    	setTimeout(() => {
    		resolve(new Date().toString());
    	}, sec * 1000);
    });
}



//////// 이런식으로 구현
delayP(1).then((result) => {
	console.log(1, result);
    
    delayP(1).then((result) => {
		console.log(2, result);
	})
})

```





- Promise를 사용하려면 new promise로 인스턴트를 생성하고 resolve, reject값을 반환한다.
- Promise를 사용하려면 Promise 인스턴스를 반드시 리턴해야함
- then() 메소드를 통해 처리함









##### <b>자바 스크립트는 싱글 스레드의 동기적인 언어이고, 브라우저에서 지원하는 별도의 API 덕분에 비동기 처리가 가능하다</b>



