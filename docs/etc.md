# Etc

## useRouter 커스텀 훅 만들기

- 유틸리티화
- `useNavigate()` 이용
- 1. currentPath: 현재 pathname. `window.location.pathname` 반환
- 2. routeTo(string): 전달 받은 값인 path 로 이동
- 3. back(): 이전 페이지로 이동, 이때 `-1` 값 지정
- 4. forward(): 이후 페이지로 이동, 이때 `1` 값 지정
- 5. isActiveRouter(string): 전달받은 값이 현재 pathname 과 같은지 조건 판단하여 `true` 또는 `false` 반환

## 역할에 선을 긋지 않을 것

- 역할 나누기, R&R(Role and Responsibility)
- 프론트에서/백에서 해야하는 일이니까 나는 모르는 일이에요...  
  ==> 이렇게 선을 긋게 되면 오류가 났을 때 난처해진다.

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

## HTTP

- Hyper Text Transfer Protocol
- start-line, HTTP headers, empty line, body 구조

### HTTPS

- SSL 인증
- 서버 측에서 인증서 기반으로 데이터 암호화
- 인증서가 올바를 경우에만 복호화 가능

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

## typescript

### 여러 타입 지정 후 순회 문제

- `type typeAB = typeA | typeB` 와 같이 OR 로 묶인 타입일 경우
- `typeB` 에만 있는 property 의 값을 가져오려고 할 때:

1. 값이 `undefined` 인지 아닌지, 즉 값이 truthy 한지 판단해서 값을 가져오는 코드 작성
2. 그러나, 해당 property 및 가져온 값을 optional 로 지정하더라도 에러 발생
3. typescript 는... : `typeA` 에는 해당 property 가 없으므로 호출 자체에 문제가 있다고 판단
4. 따라서 해당 property 가 있는지 조건 판단 후 값을 가져와야 함

### 컴포넌트에 React.FC 적용 지양

- FC: Function Component 의 약자
- `children` prop 이 optional 하게 적용되어 type 이 명확하지 않게 된다.
- props 에 Generic 을 사용할 경우, 컴포넌트에 Generic 값을 전달할 수 없다.
- ~~`defaultProps` 속성이 적용되지 않는다~~

## 라이브러리나 프레임워크 선정 기준

- document 가 잘 만들어져 있는가?
- 업데이트가 잘 이루어지는가?

## 신입에게 필요한 정보

- 신입으로 취업 준비할 때는 이 회사가 뭘 하는지 알아야 한다.  
  신규 프로젝트보다 유지보수가 좋다. 만약 신규 프로젝트라면 사수가 필요하다.
- 나는 무엇을 알고 있는가:  
  어떤 대상에게서 일어나는 현상은 대상을 정의하는 것이 아니다.
  ~ 하는 "것": X ~ 하는 "무언가": O  
  오류를 설명할 때도...=> 이거를 클릭하면 작동이 안돼요. X
- 문제가 생겼을 때 본인이 수습하려 하지 말지 무조건 주변에 알려야 한다.
- 상태관리 라이브러리: recoil 은 사용자가 줄어드는 추세.  
  멘토님은 zustand 추천  
  취업 목적이라면 redux 공부
- 알고리즘, 인프라 공부 필요
- 필요할 때, next.js 부터 공부

### 신입개발자로서...(멘토님의 개인적 견해)

- 나는 정말 알고 있을까? 얼마나 소통이 되고, 학습 역량이 어떤가?  
  내가 뭘 알고, 뭘 모르는지 메타인지 ==> 많이 질문하라
- 기본적인 타입스크립트
- 기술적 역량, 일정 컨트롤은 크게 요구하지 않는다.  
  기능 구현 나열보다는... 무슨 문제를 어떻게 해결했는가? 어떤 점이 개선되었는가?
  그러나, 앞으로 자신의 역량에 맞게 개발 일정을 객관화할 수 있어야 한다.
- 라이브러리, 프레임워크가 회사와 매칭되는지 여부는 중요치 않다.

## 그 외...

- [https://velog.io/@teo/MVI-Architecture](https://velog.io/@teo/MVI-Architecture)
- 자주 접하게 되는 자료구조: 트리, 큐, 스택
- 함수나 변수, 컴포넌트: 역할을 한 문장으로 정의할 수 없을 때 분리한다.
- GA: Google 애널리틱스의 약자
- 주니어가 꼭 읽으면 좋은 책: 피플웨어
- feature 를 완성할 수 있는가?:  
  이게 어떻게 동작하는지 기술명세, 기획명세를 보고 알 수 있는가?
- 프로젝트 구조가 기능상 영향을 주는 경우:  
  webpack, rollpu.js 사용, next.js 사용
