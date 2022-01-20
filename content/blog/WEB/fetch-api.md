---
title: 'Fetch API'
date: 2022-01-20 16:40:00
category: 'WEB'
draft: false
---

> **Javascript 에서 Fetch API 란 ?**

- [Promise](#promise)
- [Fetch API](#fetch)

### Promise <a id="promise"></a>

---

fetch 메소드는 Promise 를 리턴한다.

Promise 는 무엇인가 ?

Promise 는 비동기 오퍼레이션 _Asynchronous Operation_ 에서 사용된다.

그렇다면 비동기 오퍼레이션이란 무엇인가 ?

자바스크립트는 싱글 스레드 환경에서 동작한다.

그 말은 HTTP 요청을 백엔드에 보냈는데, 백엔드가 이를 처리하는 데 1분이 걸리면

브라우저는 1분간 아무것도 못하는 상태가 된다는 것이다.

이를 극복하려고 자바스크립트 엔진은 자바스크립트 스레드 밖에서

이런 오퍼레이션 (Web API) 을 실행해준다.

```jsx
var oReq = new XMLHttpRequest()
oReq.open('GET', 'http://localhost:8080/todo')
oReq.onload = function() {
  // callback 함수
  console.log(oReq.response)
}
oReq.send()
```

위 코드는 fetch 가 아닌 XMLHttpRequest 오브젝트를 이용해 GET 요청을 보내는 방법이다.

여기서 문제는 무엇일까 ?

콜백 함수 내에서 다른 HTTP 요청을 하고 그 두 번째 요청을 위한 콜백을 또 정의하는 과정에서

코드가 굉장히 복잡해진다.

이런 것은 콜백 지옥 **_Callback Hell_** 이라고 부른다.

Promise 는 콜백 지옥을 피할 수 있는 방법 중 하나다.

Promise 는 말 그대로 이 함수를 실행한 후

Promise 오브젝트에 명시된 사항들을 실행시켜 주겠다는 약속이다.

Promise 에는 세 가지 상태가 있다.

- Pending
- Resolve
- Reject

```jsx
// Promise 를 사용한 XMLHttpRequest
function example() {
  return new Promise((resolve, reject) => {
    var oReq = new XMLHttpRequest()
    oReq.open('GET', 'http://localhost:8080/todo')
    oReq.onload = function() {
      resolve(oReq.response) // Resolve 상태
    }
    oReq.onerror = function() {
      reject(oReq.response) // Reject 상태
    }
    oReq.send() // Pending 상태
  })
}

example()
  .then(r => console.log('Resolved ' + r))
  .catch(e => console.log('Rejected ' + e))
```

Pending 은 말 그대로 오퍼레이션이 끝나길 기다리는 상태다.

오퍼레이션이 성공적으로 끝나면 resolve() 함수를 통해

이 오퍼레이션이 성공적으로 끝났음을 알리고 원하는 값을 전달할 수 있다.

이때 resolve 는 then 의 매개변수로 넘어오는 함수를 실행한다.

오퍼레이션 중 에러가 나는 경우 reject() 함수를 콜한다.

그 결과로 catch 매개변수로 넘어오는 함수가 실행된다.

then 이나 catch 로 넘기는 함수들은 당장 실행되는 것이 아니라

매개변수로 할 일을 넘겨주기만 한다.

실제 이 함수들이 실행되는 것은 resolve 와 reject 가 실행되는 시점이다.

### Fetch API <a id="fetch"></a>

---

fetch 는 자바스크립트가 제공하는 메소드로,

API 서버로 http 요청을 송수신 할 수 있도록 도와준다.

fetch 는 url 을 매개변수로 받거나 url 과 options 를 매개변수로 받을 수 있다.

fetch() 함수는 Promise 오브젝트를 리턴한다.

따라서 then 과 catch 에 콜백 함수를 전달해 응답을 처리할 수 있다.

먼저 url 만 이용해 GET 요청을 보내는 방법을 살펴보자.

```jsx
fetch('localhost:8080/todo') // GET
  .then(response => {
    // response 수신 시 하고 싶은 작업
  })
  .catch(e => {
    // 에러 시 하고 싶은 작업
  })
```

fetch 는 첫 번째 매개변수로 uri 를 받는다.

디폴트로는 GET 메소드를 사용하는 것과 같다.

then 에는 응답을 받은 후 실행할 함수 response ⇒ {} 를 매개변수로,

catch 에는 예외 발생 시 실행할 함수 e ⇒ {} 를 넘긴다.

메소드를 명시하고 싶은 경우나 헤더와 바디를 함께 보내야 할 경우에는

아래와 같이 두 번째 매개변수에 요청에 대한 정보가 담긴 오브젝트를 넘겨준다.

```jsx
options = {
  method: 'POST',
  headers: [['Content-Type', 'application/json']],
  body: JSON.stringify(data),
}

fetch('localhost:8080/todo', options)
  .then(response => {
    // response 수신 시 하고 싶은 작업
  })
  .catch(e => {
    // 에러 시 하고 싶은 작업
  })
```
