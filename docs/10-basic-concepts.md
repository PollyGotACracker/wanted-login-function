# Basic Concepts

## 로그인의 정의

- 사용자의 시스템 접근(액세스)을 제한하고, 활동을 기록 및 추적하기 위하여 시스템에 식별자 정보를 입력하는 컴퓨터 보안 절차

## 로그인 구현을 위한 개념

- BE:
  - 사용자 식별
  - 접근 및 동작 제어
- FE:
  - 권한이 없는 자원에 접근하지 않는 구조 만들기:
    - 개인/기업의 페이지 및 데이터 분리
  - 권한이 없는 자원의 존재를 모르도록 하기:
    - 권한이 없는 페이지에 접근할 경우 404 페이지 표시
  - 로그인 및 로그아웃 만들기
  - 인증 정보 관리하기

### FE 에서 필요한 최소한의 구현 요구사항

- 로그인 페이지
- 로그인 인증관련 데이터 관리
- 로그인 상태에 따른 화면/기능 제어
- 로그아웃

## Mock API

### Mock API 의 장점

- 원하는 형태의 API 설계 가능
- 서버 배포 없이 개발 가능
- 초기 인프라스트럭쳐 구성 비용 절감
  Client - Web Server - Application Servers - Database
  ㄴ Http requests and responses

### Mock API 라이브러리

- msw(Mock Service Worker)
- axios-mock-adapter

## 토큰 저장 위치

- cookies
- local storage
- session storage

## 로그인 여부에 따른 페이지 구조 만들기

1. 서비스를 구성하는 페이지들 구분

   - /main, /login => 로그인 불필요
   - /mypage, /repo, /page => 로그인 필요

2. 반복되는 로그인 여부 확인 코드 및 로직 모듈화

   - 로그인이 필요한 페이지마다 사용됨

3. 페이지를 감싸는 레이아웃 컴포넌트 생성

   - `generalLayout` 컴포넌트가 각 페이지를 children 으로 가져가도록 함
   - 필요에 따라 props 작성(`currentPath`, `withLogin` 등)
   - 페이지 redirect 구현:
     - 로그인 여부를 확인하는 state 변수와, 로그인이 필요한 페이지 여부를 확인하는 props 를 조건으로 함
     - 결과가 `false` 일 경우 별도의 JSX return 값을 주어 해당 페이지를 보여주지 않도록 함
     - `useEffect` 는 사용하지 않음
   - 그 외 페이지의 공통 부분 구현

4. router 객체 생성
   - router 파일에서 각 페이지의 정보를 담은 `routerInfo` 배열 생성:
     - `[{path, element, withAuth, label, icon}, ...]`
     - 만든 배열은 사이드바 생성에 사용 가능
   - 배열을 사용해 `Array.map()` 으로 router 객체 생성:
     - `withAuth` 가 `true` 라면 `element` 값을 `GeneralLayout` 컴포넌트로 감싸서 return 하도록 조건 지정
