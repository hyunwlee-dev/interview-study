# (면접) 쿠키란 무엇입니까?

> 쿠키는 웹사이트를 방문할 때 생성되는 정보를 담은 작은 파일로, 클라이언트의 로컬에 저장됩니다. 쿠키는 사용자가 웹사이트를 방문할 때마다 서버로 전송되어 사용자를 식별하는데 사용됩니다.

- 예를 들어 아래와 같이 username이라는 쿠키를 만들 수 있습니다.

```javascript
document.cookie = "username=John";
```

  

![cookie image by sudheer](https://github.com/sudheerj/javascript-interview-questions/raw/master/images/cookie.png)

​    

## (꼬리 질문) 쿠키의 사용 목적은 무엇인가요?

- 사용자를 식별하기 위해 사용됩니다.
- 사용자의 선호 설정을 저장하기 위해 사용됩니다.
- 장바구니에 상품을 담거나, 로그인 정보를 저장하기 위해 사용됩니다.

​    

## (꼬리 질문) 쿠키의 단점은 무엇인가요?

- 쿠키는 사용자의 로컬에 저장되기 때문에 보안에 취약합니다.
- 쿠키는 사용자의 로컬에 저장되기 때문에 사용자의 로컬에 저장된 쿠키를 삭제하지 않는 한 계속해서 서버로 전송됩니다.
- 쿠키는 사용자의 로컬에 저장되기 때문에 저장할 수 있는 데이터의 크기가 제한적입니다.

  

## (꼬리 질문) 쿠키의 구조는 어떻게 되나요?

- 쿠키는 이름, 값, 만료 날짜, 경로, 도메인, 보안 여부 등의 정보를 담고 있습니다.
- 쿠키는 `name=value; expires=날짜; path=경로; domain=도메인; secure`와 같은 형식으로 구성되어 있습니다.
- 쿠키는 `document.cookie`를 통해 생성하거나 읽을 수 있습니다.
- 쿠키는 `Set-Cookie` 헤더를 통해 생성하거나 읽을 수 있습니다.

  

## (꼬리 질문) 쿠키의 보안을 위한 방법은 무엇인가요?

- 쿠키에 중요한 정보를 담지 않는 것이 좋습니다.
- 쿠키에 보안을 위한 옵션을 설정하는 것이 좋습니다. (secure, httpOnly, SameSite)
- 쿠키의 만료 날짜를 설정하여 보안을 강화하는 것이 좋습니다.
- 쿠키를 사용할 때는 HTTPS를 사용하는 것이 좋습니다.



---

  

## 쿠키

쿠키를 사용할 때 이 두 개 헤더를 쓰게 된다.  

- `Set-Cookie`: 서버에서 클라이언트로 쿠키 전달(응답)
- `Cookie`: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달

  

## HTTP 특징

>http는 메시지 전송 다 되고 나면 연결 끊어버린다.  
>
>- HTTP는 무상태(Stateless) 프로토콜이다.
>- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
>- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
>- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

​    

---

  

## 쿠키 설명

서버에서 쿠키를 세팅할 때  

- 예) set-cookie: `sessionId=abcde1234;` `expires=Sat, 26-Dec-2020 00:00:00 GMT;` `path=/;` `domain=.google.com;` `Secure`
- 사용처
  - `사용자 로그인 세션 관리`
    - 사실 `user=홍길동`은 보안상 취약하여 sessionId를 보내고 데이터베이스에서 user값을 매핑시킴
  - `광고 정보 트래킹`
    - 이 웹브라우저 사용하는 사람이 이런 걸 자주보는 구나 => 광고 매칭할 트래킹 용도

- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
    - 최소한의 정보만 사용(세션 id, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고

- 주의!
  - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)

  

---



## 쿠키 - 생명 주기

###### Expires, max-age

- Set-Cookie: <strong>expires</strong>=Sat, 26-Dec-2020 04:39:21 GMT
  - 만료일이 되면 쿠키 삭제
- Set-Cookie: <strong>max-age</strong>=3600 (3600초)
  - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

  

---

  

## 쿠키 - 도메인

###### Domain

- 예) domain=example.org
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
  - domain=example.org를 지정해서 쿠키 생성
    - example.org는 물론이고
    - dev.example.org도 쿠키 접근
- 생략: 현재 문서 기준 도메인만 적용
  - example.org에서 쿠키를 생성하고 domain 지정을 생략
    - example.org 에서만 쿠키 접근
    - dev.example.org는 쿠키 미접근

  

---

  

## 쿠키 - 경로

###### path

- 예) `path=/home`
- 이 경로를 포함한 `하위 경로 페이지만 쿠키 접근`
- 일반적으로 path=/ 루트로 지정
  - 예)
  - path=/home  지정
  - /home -> 가능
  - /home/level1 -> 가능
  - /home/level1/level2 -> 가능
  - /hello -> 불가능

  

---

  

## 쿠키 - 보안

###### Secure, HttpOnly, SameSite

- `Secure`
  - 쿠키는 http, https를 구분하지 않고 전송
  - Secure를 적용하면 https인 경우에만 전송
- `HttpOnly`
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP전송에만 사용
- `SameSite`
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

