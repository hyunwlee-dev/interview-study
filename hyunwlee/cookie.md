# 쿠키

쿠키를 사용할 때 이 두 개 헤더를 쓰게 된다.  

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

  

## 쿠키 예시

### 쿠키 미사용

###### 처음 welcome 페이지 접근

  

![2024-01-24_21-52](/hyunwlee/images/cookie/2024-01-24_21-52.png)

  

###### 로그인  

  

######  ![2024-01-24_21-53](/hyunwlee/images/cookie/2024-01-24_21-53.png)



사용자가 payload {id, password}를 POST 메세지로 보내서 로그인을 하는 과정  

서버에서는 홍길동님이 로그인 했습니다 라고 응답.  

  

### 이 상태에서 다시 /welcome 페이지에 들어간다.

  

expected: "안녕하세요. 홍길동" 줄 것을 기대

result: "안녕하세요. 손님"

  

![2024-01-24_21-56](/hyunwlee/images/cookie/2024-01-24_21-56.png)

  

서버 입장에서는 아무리 눈을 씻고 봐도 이게 홍길동이 보낸 요청인지 아닌지 구분할 수 있는 방법이 없다.  

  

><i>http는 메시지 전송 다 되고나면 연결 끊어버린다.</i>  
>
>- HTTP는 무상태(Stateless) 프로토콜이다.
>- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
>- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
>- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

  

---



### 그럼 어떡해요?

  

### Solution-1)

모든 요청에 사용자 정보를 포함해서 보내면 될려나~.  

`GET /welecome?user=홍길동 HTTP/1.1`  

![2024-01-24_22-06](/hyunwlee/images/cookie/2024-01-24_22-06.png)

  

모든 요청과 링크에 사용자 정보를 넣는다?  

- 모든 요청에 사용자 정보가 포함되도록 개발 해야 함
- 브라우저를 완전히 종료하고 다시 열면?
  - 다시 다 넘겨야 한다.

  

<h3 style='color: red'>Solution-2)</h3>

#### 쿠키 사용

###### 로그인

![2024-01-24_22-11](/hyunwlee/images/cookie/2024-01-24_22-11.png)  

웹 브라우저가 포스트로 로그인을 한다.  

서버는 Set-Cookie로 user에 홍길동 내용을 쿠키헤더 만들어서 응답한다.  

  

웹 브라우저 내부에는 쿠키 저장소에 user는 홍길동이라 한 것을 저장해둔다.  

  

###### 로그인 이후 welcome 페이지 접근

![2024-01-24_22-11](/hyunwlee/images/cookie/2024-01-24_22-11.png)

웹 브라우저는 이제 서버에 요청을 보낼 때마다 cookie 저장소에서 cookie값을 무조건 꺼내서 http 헤더를 만들어서 서버에 보낸다.  

  

지저분하게 url에 넣을 필요가 없게 된 것이다.



#### 모든 요청에 쿠키 정보 자동 포함

![2024-01-24_22-18](/hyunwlee/images/cookie/2024-01-24_22-18.png)  

  

  

쿠키 메커니즘으로 자동 해결  

  

---

  

## 쿠키 설명



서버에서 쿠키를 세팅할 때  

- 예) set-cookie: <strong>sessionId=abcde1234;</strong> <strong>expires<strong>=Sat, 26-Dec-2020 00:00:00 GMT; <strong>path=/;</strong> <strong>domain</strong>=.google.com; <strong>Secure</strong>
- 사용처
  - 사용자 로그인 세션 관리
    - 사실 `user=홍길동`은 보안상 취약하여 sessionId를 보내고 데이터베이스에서 user값을 매핑시킴
  - 광고 정보 트래킹
    - 이 웹브라우저 사용하는 사람이 이런 걸 자주보는 구나 => 광고 매칭할 트래킹 용도

- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
    - 최소한의 정보만 사용(세션 id, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고

- 주의!
  - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호등등)

  

---



## 쿠키 - 생명주기

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

- 예) path=/home
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
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

- Secure
  - 쿠키는 http, https를 구분하지 않고 전송
  - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송





---

  

# Cookie vs LocalStorage vs SessionStorage

https://github.com/Esoolgnah/Frontend-Interview-Questions/blob/main/Notes/important-4/web-storage.md







  

