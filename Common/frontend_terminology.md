# Frontend Terminology

아래는 **2025년 시점**에서, 프론트엔드 개발자가 흔히 쓰는 **핵심 개념**(버즈워드)들을 **최신 트렌드** 중심으로 추린 목록이다.

## A

1. **API (Application Programming Interface)**

   - 애플리케이션(서버)과 클라이언트가 **데이터**를 주고받는 방식을 정의한 규약.
   - REST, GraphQL 등이 대표적인 예시.

2. **Accessibility (접근성)**
   - 장애나 환경적 제약이 있는 사용자도 **불편 없이** 웹사이트를 이용할 수 있도록 고려하는 방법(ARIA, 시맨틱 마크업, 컬러 대비 등).

## B

3. **Babel (바벨)**

   - 최신 JavaScript 코드를 구형 브라우저에서도 동작하도록 **트랜스파일**해주는 도구.

4. **Bootstrap (부트스트랩)**

   - UI 컴포넌트와 그리드 시스템을 제공하는 **CSS 프레임워크**.
   - 아직도 꾸준히 사용되고 있음.

5. **Browser (브라우저)**
   - 웹사이트 접속 프로그램(Chrome, Firefox, Safari, Edge 등).
   - 최신 버전별로 지원하는 웹 표준/기능이 달라, **크로스 브라우징** 이슈가 종종 발생.

## C

6. **Caching (캐싱)**

   - 웹 리소스(HTML, CSS, JS, 이미지 등)를 **브라우저나 CDN** 등에 임시 저장하여, **재방문 시** 빠르게 불러오는 기법.

7. **CI/CD (Continuous Integration / Continuous Delivery/Deployment)**

   - 코드 변경 내용을 **자동 빌드, 테스트, 배포**하는 파이프라인.
   - GitHub Actions, GitLab CI, Jenkins 등을 주로 사용.

8. **CSS & CSS Preprocessors**

   - **CSS (Cascading Style Sheets)**: 웹 문서 스타일링 기본 언어.
   - **CSS Preprocessor**: SASS, LESS 등. 변수, 중첩, 믹스인 등 편리한 기능 제공.

9. **CSS Frameworks**

   - 미리 정의된 스타일/컴포넌트를 제공해, 빠르게 UI를 구성하도록 돕는 도구.
   - **Tailwind CSS**, Bootstrap, Bulma, Material UI 등이 대표적.

10. **CORS (Cross-Origin Resource Sharing)**

- 웹 페이지가 **다른 도메인**의 서버로 요청을 보낼 때 허용/차단을 제어하는 메커니즘.

## D

11. **DOM (Document Object Model)**

- HTML을 트리 형태의 객체로 표현한 구조.
- 자바스크립트로 **조작** 가능.

12. **DX (Developer Experience)**

- 개발자가 도구·프레임워크·언어 등을 사용하면서 겪는 **편의성과 만족도**.
- 프레임워크와 빌드 시스템이 발전함에 따라 점점 중요한 화두가 됨.

## E

13. **Edge Functions (엣지 함수)**

- CDN 엣지 노드에서 서버 사이드 로직을 **서버리스** 형태로 실행해, 지연 시간을 최소화하는 방식.
- Vercel, Netlify 등이 제공.

14. **ES6+ (ECMAScript 2015 이후 문법)**

- 자바스크립트의 새로운 문법(클래스, 화살표 함수, 모듈 import/export 등).
- Babel·Webpack 등으로 **구형 브라우저 호환** 가능.

## F

15. **FCP (First Contentful Paint)**

- 웹 페이지가 처음으로 **유의미한 콘텐츠(텍스트/이미지)**를 그리는 데까지 걸린 시간.
- 성능 지표(Core Web Vitals) 중 하나.

16. **Framework (프레임워크)**

- 복잡한 웹 개발을 쉽게 하기 위한 **기능·규칙·도구 세트**.
- 예: React, Vue, Angular(프론트엔드), Next.js, Nuxt(SSR/SSG 프레임워크) 등.

## G

17. **Git / GitHub**

- **Git**: 분산 버전관리 시스템.
- **GitHub**: Git 저장소를 호스팅·협업할 수 있는 웹 플랫폼(이슈·PR·Actions 등).

18. **GraphQL**

- 페이스북(메타)발 **쿼리 언어**로, 필요한 데이터만 요청해 받아오는 방식.
- REST를 대체하거나 병행해서 사용.

## H

19. **HTML (HyperText Markup Language)**

- 웹 페이지의 **구조(골격)**를 만들기 위한 마크업 언어.

20. **HTTP/HTTPS**

- 웹에서 데이터 전송을 위한 프로토콜(HTTPS는 암호화+보안 추가).

## I

21. **i18n (Internationalization)**

- 다국어/다문화 지원을 위한 설계.
- 문자열 리소스를 분리하고, 자동 번역/언어 감지 등을 고려.

22. **iFrame**

- 웹페이지 내부에 **다른 페이지(외부 콘텐츠)**를 삽입할 때 사용.
- 유튜브 동영상 임베드 등에 활용.

## J

23. **JavaScript**

- 웹 프론트엔드(클라이언트 사이드)에서 가장 중요한 언어.
- Node.js로 서버 사이드, 모바일, 데스크톱 앱 등 확장 가능.

## L

24. **Largest Contentful Paint (LCP)**

- 페이지 로드 중, 화면에서 **가장 큰 콘텐츠**가 렌더링되는 시간.

25. **Lighthouse**

- 구글이 제공하는 웹 성능/접근성/SEO 측정 자동화 도구.
- 페이지 품질에 대한 점수와 개선안 제공.

26. **Library (라이브러리)**

- 특정 기능을 모아둔 **재사용 가능한 코드 집합**.
- 프레임워크보다 규모가 작고, “**호출해서 쓰는**” 개념에 가깝다.

## M

27. **Micro Frontend (마이크로 프론트엔드)**

- 대규모 프론트엔드 앱을 **여러 작은 앱**으로 나누어 개발/배포하는 아키텍처.
- 팀별 독립적 배포가 가능해, 복잡도를 줄이는 장점.

28. **Minification (경량화)**

- HTML/CSS/JS에서 불필요한 공백, 주석 등을 제거해 **파일 크기**를 줄이는 과정.

29. **Mobile-first (모바일 퍼스트)**

- 웹/앱을 **작은 화면(모바일)**에서 먼저 설계·개발하고, 큰 화면으로 확장하는 접근 방식.

30. **MVP (Minimum Viable Product)**

- 출시 가능한 **가장 간단한 핵심 기능**만 담은 제품 버전.
- 빠른 시장 반응 테스트를 위함.

## N

31. **Navigation (내비게이션)**

- 웹사이트 내에서 다른 페이지로 **이동**할 수 있는 메뉴·링크 구조.

32. **Next.js**

- React 기반의 **서버 사이드 렌더링(SSR)/정적 사이트 생성(SSG)** 프레임워크.
- 라우팅, 코드 분할, 이미지 최적화 등 편의 제공.

33. **NPM (Node Package Manager)**

- Node.js의 **기본 패키지 매니저**. 자바스크립트 라이브러리·프레임워크 설치에 주로 사용.

## P

34. **PNPM**

- NPM, Yarn과 유사한 **패키지 매니저**지만, 중복 의존성 최소화로 디스크 공간 활용에 강점.

35. **PWA (Progressive Web App)**

- 웹앱을 **네이티브 앱처럼** 사용할 수 있게 하는 기술(오프라인 지원, 홈 화면 아이콘, 푸시 알림 등).

36. **Promises (프로미스)**

- 자바스크립트 **비동기 처리**를 표현하는 객체.
- `async/await` 구문으로도 사용 가능.

## R

37. **React**

- 페이스북(메타)에서 만든 UI 라이브러리.
- 컴포넌트, 가상 DOM 개념을 통해 동적 화면을 효율적으로 관리.

38. **Redux**

- React 생태계에서 주로 사용되는 **상태 관리** 라이브러리.
- 단일 스토어와 액션/리듀서 개념으로 전역 상태를 효율적으로 핸들링.

39. **Responsive Design (반응형 디자인)**

- 화면 크기에 따라 **레이아웃**이 유동적으로 변하도록 설계.

## S

40. **SaaS (Software as a Service)**

- 소프트웨어를 **클라우드**로 제공, 사용자는 설치 없이 웹 브라우저 등으로 이용.

41. **SEO (Search Engine Optimization)**

- 검색 엔진에서 **상위 노출**을 위해 웹사이트 구조와 콘텐츠를 최적화하는 과정.

42. **Semantic HTML**

- 의미에 맞는 태그(`<header>`, `<section>`, `<article>` 등)를 사용해, **문서 구조**를 명확히 표현.

43. **Serverless**

- 서버를 직접 관리하지 않고, **클라우드 플랫폼**에서 함수 단위로 코드 실행.
- AWS Lambda, Netlify Functions, Vercel Edge Functions 등.

44. **Server-Side Rendering (SSR)**

- 첫 페이지 로드를 **서버에서 미리** HTML을 그려서 제공.
- 초기 로딩 속도, SEO 등에 이점.

45. **SPA (Single Page Application)**

- 페이지 전체를 다시 로드하지 않고, **동적인 라우팅**으로 화면 전환하는 웹 앱 구조.
- 예: React, Vue 등이 SPA 방식.

46. **Static Site Generation (SSG)**

- 빌드 시점에 **HTML 파일을 미리 생성**해 배포.
- Next.js, Gatsby, Hugo 등에서 지원.

## T

47. **Tailwind CSS**

- Utility-first 스타일링 프레임워크.
- 미리 만들어진 CSS 클래스를 조합해 빠르게 UI 구현.

48. **Testing (테스팅)**

- **단위 테스트**, **통합 테스트**, **E2E 테스트** 등으로 버그를 사전에 방지.
- Jest, Mocha, Cypress, Playwright 등이 유명.

49. **TypeScript**

- 자바스크립트에 **정적 타입**을 추가한 상위 언어.
- IDE 자동완성, 오류 방지 등에 큰 장점.

## U

50. **UI (User Interface)**

- 사용자와 시스템이 직접 **상호 작용**하는 영역(화면, 버튼, 입력 필드 등).

51. **UX (User Experience)**

- 사용자가 제품/서비스를 이용하며 느끼는 **총체적 경험**.
- UI는 UX의 일부.

## V

52. **Vite (비트)**

- 에반 유(Evan You, Vue.js 창시자)가 만든 차세대 **프론트엔드 빌드 도구**.
- 개발 서버 속도가 매우 빠르고 설정이 간단해, 현재 주목받고 있음.

53. **Vue**

- 가볍고 직관적인 **프론트엔드 프레임워크**.
- 컴포넌트 기반, 양방향 바인딩, 가상 DOM 등을 제공.

## W

54. **Web Components**

- 표준화된 방식으로 **재사용 가능한 커스텀 요소**(Custom Elements, Shadow DOM, HTML Templates) 등을 만들 수 있는 기술.

55. **Webpack**

- JS, CSS, 이미지 등 모든 자원을 **모듈**로 만들어 묶어주는 빌드 툴.
- 오래된 대표주자이지만, 여전히 대규모 프로젝트에서 많이 사용.

56. **Widgets (위젯)**

- 간단한 기능을 독립된 형태로 제공하는 **UI 컴포넌트**.
- 예: 날씨 위젯, 검색 위젯, 배너 등.

## Y

57. **Yarn**

- 페이스북(메타)에서 만든 **패키지 매니저**.
- NPM과 비슷하지만, 의존성 관리 구조나 속도에서 차이가 있음.
