# Etc

## CSR vs SSR, 그리고 SSG

- [dev.to: Visual Explanation and Comparison of CSR, SSR, SSG and ISR](https://dev.to/pahanperera/visual-explanation-and-comparison-of-csr-ssr-ssg-and-isr-34ea)
- Rendering: HTML structure 를 만드는 것
- 공부 순서: CSR => SSR => 인프라. 즉, 클라이언트 => 서버 => 인프라

### 선택 기준

- 유지보수 관점에서 생각해보기:
  - 추가로 서버가 필요한가?
  - SEO 가 필요한가?
  - 어떤 유저 인터렉션이 많은가?  
    (페이지 이동 vs 페이지 내 인터랙션)

### SSR 선택 시 고려해야 할 점

- 인프라 작업이 많이 필요
- 장애 지점 증가
- 서버 비용 증가

## HTTP Request

- Hyper Text Transfer Protocol
- start-line, HTTP headers, empty line, body 구조

### Bearer 토큰

- 토큰을 가지고 있는 사람이 누구이든 관계 없이, 토큰을 가지고 있는 사람이 할 수 있는 행동을 그대로 할 수 있는 속성을 가진 보안 토큰
- 암호화된 키 데이터의 소유를 증명하지 않아도 됨
- RFC6749 스펙 문서에 정의

### cURL

- 백엔드 개발자와 쉽게 소통하기 위한 도구
- curl 설치 후, 개발자 도구에서 cURL 생성 가능(Copy => Copy as cURL)
- 주요 옵션: [lesstif.com: curl 주요 사용법 요약](https://www.lesstif.com/software-architect/curl-91947158.html)

#### curl

- Command line tool and library for transferring data with URLs
- 명령줄이나 스트립트에서 데이터를 전송하는 데 사용하는 인터넷 전송 엔진

## typescript: 컴포넌트에 React.FC 적용 지양

- FC: Function Component 의 약자
- `children` prop 이 optional 하게 적용되어 type 이 명확하지 않게 된다.
- props 에 Generic 을 사용할 경우, 컴포넌트에 Generic 값을 전달할 수 없다.
- ~~`defaultProps` 속성이 적용되지 않는다~~

## 라이브러리나 프레임워크 선정 기준

- document 가 잘 만들어져 있는가?
- 업데이트가 잘 이루어지는가?

## 그 외...

- 유저의 개인정보는 보안 이슈가 있으므로 클라이언트에 저장하지 않는다.  
  판단이 어려울 때는 개인정보 보호법을 확인한다.
- GA: Google 애널리틱스의 약자
- 주니어가 꼭 읽으면 좋은 책: 피플웨어
- feature 를 완성할 수 있는가?: 이게 어떻게 동작하는지 기술명세, 기획명세를 보고 알 수 있는가?
