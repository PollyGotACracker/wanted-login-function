# JWT

- Json Web Token
- 로그인에 토큰 사용 이유: HTTP 요청은 Stateless 하므로 사용자의 신원을 알 수 없기 때문

## Hashing

- 단방향 암호화 기법(복호화 불가능)
- Plain text string => Hashing Algorithm => Hashed Text

## 토큰 기반 로그인의 흐름

### 전체 흐름

1. 클라이언트: 로그인 요청
2. 서비스: 토큰 발급 및 전달
3. 클라이언트: 토큰 보관
4. 클라이언트: 토큰을 사용해 요청

### 구조

1. 진입한 유저에게 토큰이 있는가?:
   - 토큰이 없을 경우 로그인 진행 후 토큰 발급
   - 토큰이 있을 경우 토큰 유효성 검증...
2. 저장된 토큰이 유효한가? (validity check):
   - 유효하다면 토큰 사용
   - 유효하지 않다면 로그인 진행 후 토큰 발급

### 필요한 기능

- 로그인 후 로그인 상태 유지
- 자동 로그인

## JWT 의 구조

- HEADER.PAYLOAD.SIGNATURE 로 구성됨

### Header

- alg: 암호화 규칙
- typ: 토큰 타입

```json
{ "alg": "HS256", "typ": "JWT" }
```

### Payload

- 데이터  
  (claim)
- [rfc-editor.org: JWT Claims](https://www.rfc-editor.org/rfc/rfc7519#section-4)

```json
{ "sub": "1234567890", "name": "John Doe", "iat": 1516239022 }
```

### Signature

- 암호화를 위한 데이터  
  (보안 필요)

1. Header 와 Payload 를 Base64Url 로 인코딩 후 점으로 연결
2. Secret Key 는 Signature 생성 및 토큰 무결성 검증에 사용됨
3. 문자열에 HMAC SHA256, RSA 등 Signature 알고리즘을 사용

```js
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  MY_256_BIT_SECRET_KEY
);
```

## 토큰의 완결성

- 토큰에 중요한 정보를 저장할 수 있는 이유
- 토큰 자체가 스스로의 유효성을 검증하는 완결성을 가진다.
- 즉, 토큰이 한번 발급되고 서명되면, 이 토큰의 내용이나 권한은 무결성이 보장되어야 한다. 그리고 토큰이 자체적으로 스스로의 유효성을 확인할 수 있어야 한다.

### 인증 및 권한 부여

- 토큰은 사용자가 누구인지 확인하고 해당 사용자에게 어떤 작업 또는 자원에 접근 권한이 있는지 결정하는 데 사용된다.  
  시스템은 토큰 정보를 기반으로 사용자를 인증하고 적절한 권한을 부여할 수 있다.

### 정보 전달

- 토큰은 사용자 관련 정보나 세션 정보를 다른 시스템 또는 서비스로 전달하는 데 사용된다.  
  이를 통해 사용자의 상태를 유지하고 필요한 정보를 공유할 수 있다.

### 개인화

- 토큰은 사용자의 세션 및 상호작용 기록을 추적하고, 이를 기반으로 개인화된 경험을 제공하는 데 사용될 수 있다.

## Stateless Token

- 모든 필요한 정보를 토큰에 포함
- 서버에서 세션 상태를 관리하지 않고, 토큰 내용만 확인  
  (유저 정보가 올바른지, Signature 가 올바른지 등)

## JWT 보관 방식과 보안

- 서버:
  - 시크릿 키 노출
  - 데이터 복호화로 인한 정보 유출
- 클라이언트:
  - 토큰 탈취

### XSS(Cross Site Scripting)

- 웹 애플리케이션의 취약성을 이용해 악의적인 스크립트를 삽입하고 실행하여 개인정보 탈취
- 로컬 스토리지 또는 보안 속성 없이 쿠키에 저장할 경우

### CSRF(Cross-site Request Forgery)

- 로그인 상태의 사용자에게 스팸 메일, 링크 등을 보내 사용자 권한으로 비정상적인 요청을 서버로 전송
- 보안 속성 없이 쿠키에 저장할 경우

## 시스템 설계: Refresh token / Access token

- 요청 => Access token 만료 => Refresh token 으로 Access token 재발급 요청 => 새 Access token 발급
- 방법 1: Refresh Token 은 로컬 스토리지에 저장, Access Token 은 메모리에 저장
- 방법 2: Refresh Token 은 HttpOnly Cookie 에 저장, Access Token 은 메모리에 저장

### Access Token

- 런타임 메모리(변수)에 저장
- 짧은 시간 유효하여 탈취(재접근)가 어려우므로 XSS 방어 가능
- +다른 방법으로 탈취하더라도 추가 인증 필요하도록 구현

### Refresh Token

- 서버에서 HttpOnly Cookie 로 발급
- 그 외 `SameSite`, `Secure` 속성 등을 추가 사용
- `HttpOnly` 속성을 사용하면 JS로 쿠키에 접근할 수 없게 되어 CSRF 방어 가능
- +Access token 재발급 요청 시, 이미 발급된 Access token 이 유효하다면 모든 토큰을 무효화되도록 구현
- +Refresh Token Rotation 전략: Refresh token 을 일회용으로 생성하도록 구현
