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

## typescript: 컴포넌트에 React.FC 적용 지양

- FC: Function Component 의 약자
- `children` prop 이 optional 하게 적용되어 type 이 명확하지 않게 된다.
- props 에 Generic 을 사용할 경우, 컴포넌트에 Generic 값을 전달할 수 없다.
- ~~`defaultProps` 속성이 적용되지 않는다~~

## 그 외...

- 유저의 개인정보는 보안 이슈가 있으므로 클라이언트에 저장하지 않는다.  
  판단이 어려울 때는 개인정보 보호법을 확인한다.
- GA: Google 애널리틱스의 약자
