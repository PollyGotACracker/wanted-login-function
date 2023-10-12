# Session

- 사용자의 로그인 이후 로그아웃, 또는 로그인 만료까지의 기간
- 세션 방식 로그인: 사용자 로그인이 유효한 시간 동안 서버에 세션 아이디를 기록해두고 인증에 사용하는 방식
- ==> 로그인에서의 세션은 커넥션(연결) 자체이다.

## 동작 방식

1. 진입한 유저에게 세션이 유효한가? (session check):

   - 유효하다면 세션 사용
   - 그렇지 않다면 로그인...

2. 유저가 로그인하면...

   - 서버가 username, session id(sid) 등을 저장
   - 그리고 브라우저에서 재요청 시 sid 를 보내도록 세팅

3. 유저가 요청하면...

   - 클라이언트에서 요청을 보냄
   - 이때, 같은 sid 에서 요청되었음을 서버가 어떻게 아는가? => cookie

## Cookie

- [mdn: HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies)
- [wikipedia: HTTP 쿠키](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4)
- 개발자 도구에서...
  - Application => Storage => Cookies 에서 확인 가능
  - Response Headers 에서 `Set-Cookie` 속성 확인 가능
- 상태가 없는 HTTP 프로토콜에서 쿠키를 사용하여 상태 정보를 기억
- 쿠키는 이제 클라이언트에서 관리하지 않는다.  
  => 보안문제, 저장공간 제한, string 만 저장 가능, 매 요청마다 전달됨...
- 서버에서 설정해두면, 클라이언트가 같은 도메인으로 요청을 보낼 때 쿠키는 자동으로 보내진다.

### credential

- 서버로 HTTP 요청을 보낼 때, 요청에 대한 인증 정보(예: 쿠키 및 HTTP 인증)를 포함시킬지 여부를 제어하는 옵션
- `same-origin`: 같은 origin 간 요청에만 인증 정보 포함
- `include`: 모든 요청에 대해 인증 정보 포함  
  CORS(Cross-Origin Resource Sharing) 정책을 준수하는 경우에 사용
- `omit` 또는 생략: 모든 요청에 인증 정보를 포함하지 않음

### 쿠키 관련 정책 지정:

- SameSite
  - `none`: 같은 origin, 다른 origin 모두 전송 허용(사용 불가능한 값)
  - `lax`: 안전한 몇 개의 operation 만 허용  
    : 동일 origin 요청에서 전송 허용
    : 단, 사용자가 링크 등을 통해 외부 사이트에서 해당 사이트로 이동할 때도(cross-site 요청) 전송 허용
  - `strict`: 모든 상황에서 같은 origin 으로만 전송 허용
- HttpOnly
  - `true`: 클라이언트에서 스크립트 접근 불가
- Secure
  - `true`: HTTPS 연결에서만 사용

```js
import * as cookieParser from "cookie-parser";
import * as session from "express-session";

// ...
app.use(cookieParser());

app.enableCors({
  origin: /^(http:\/\/localhost:[0-9]{4})|(http:\/\/127.0.0.1:[0-9]{4})$/,
  methods: ["GET", "POST", "OPTIONS"],
  credentials: true,
});

// res.cookies() 로 설정 가능
app.use(
  session({
    secret: "SECRET",
    resave: false,
    saveUninitializaed: true,
    cookie: {
      maxAge: 24 * 6 * 60 * 1000,
      sameSite: "lax",
      httpOnly: true,
    },
  })
);
```

## CORS

- Cross-Origin Resource Sharing
- mkcert: 프론트엔드 localhost 환경에서 HTTPS를 사용할 수 있는 라이브러리

## JWT vs Session

### JWT 장단점

- 서버/백엔드 비용 감소
- 프론트엔드 복잡도 높아짐
- 보안에서 세션보다 다소 위험

### 세션 장단점

- 서버/백엔드 비용 대폭 증가
- 프론트엔드 인증 쉬워짐
- 보안에서 약간 향상

### 고려사항

- 동시 접속자 수
- 서비스 규모
- 앱/웹 동시 운용 여부
- 팀 내 인력 구성
- 일정
