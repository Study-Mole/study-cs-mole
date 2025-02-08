# Lighthouse

## 1. Lighthouse란?

**Lighthouse**는 Google에서 개발한 **오픈소스 자동화 분석 도구**이다. 웹사이트의 **성능(Performance)**, **접근성(Accessibility)**, **베스트 프랙티스(Best Practices)**, **SEO**, **PWA(Progressive Web App)** 총 5가지 영역을 자동으로 테스트하여, **지표(Metrics)**와 **개선 권장사항**을 리포트 형태로 제공한다.

- **주요 목적**: 웹 페이지 품질(성능, 접근성, SEO 등)을 높이고, 사용자 경험을 개선
- **장점**:
  - Chrome DevTools 내장 기능 제공 (쉽게 접근 가능)
  - CLI, Node.js API 등 다양한 방식으로 자동화/확장 가능
  - CI/CD 파이프라인에서 성능 회귀(regression) 테스트로 활용 가능

## 2. Lighthouse 주요 특징

1. **다양한 지표 측정**

   - FCP(First Contentful Paint), LCP(Largest Contentful Paint), CLS(Cumulative Layout Shift), TTI(Time To Interactive), TBT(Total Blocking Time) 등 **사용자 체감**과 직결된 지표를 수집하고 시각화한다.

2. **각 항목별 점수화**

   - Performance, Accessibility, Best Practices, SEO, PWA 항목을 각각 **0~100점**으로 평가한다.
   - 이 점수는 웹사이트의 상태를 빠르게 파악하고, **개선 우선순위**를 잡는 데 도움을 준다.

3. **개선 가이드 제공**

   - 예: `이미지 크기 줄이기`, `폰트 로드 방식 개선`, `불필요한 JS/CSS 제거` 등 항목별로 **구체적인 해결책**을 제시한다.

4. **자동화 및 반복 가능**
   - 동일한 환경(네트워크, CPU 성능 등)에서 반복 테스트를 수행할 수 있어, 개선 효과를 **비교**하고 **추적**하기가 용이하다.
   - CI/CD에 통합하여, 배포마다 성능 변화를 감지할 수 있다.

## 3. 주요 측정 지표(Performance 기준)

아래 지표들은 **Performance** 탭 점수 산정에 큰 비중을 차지한다.

1. **FCP (First Contentful Paint)**

   - 화면에 **첫 번째 컨텐츠(텍스트·이미지 등)**가 그려지는 시점
   - 빠른 FCP는 사용자가 로딩 시작을 체감할 수 있게 하므로 중요하다.

2. **LCP (Largest Contentful Paint)**

   - 화면에서 **가장 큰 요소**(주로 큰 이미지·텍스트 블록)가 렌더링 완료되는 시점
   - 사용자는 이 시점에 사이트의 주요 콘텐츠를 거의 볼 수 있다.

3. **CLS (Cumulative Layout Shift)**

   - 페이지 로딩 중 예상치 못하게 **레이아웃이 이동**한 정도를 수치화한 값
   - 버튼이 갑자기 밀리거나, 텍스트가 튀는 등의 사용성 저하를 측정한다.

4. **TTI (Time To Interactive)**

   - 페이지가 **완전히 인터랙션 가능한 상태**가 되는 시점
   - 사용자가 클릭·입력 등을 했을 때 **바로 반응**할 수 있는지 여부를 확인한다.

5. **TBT (Total Blocking Time)**
   - 메인 스레드가 길게 블로킹되어 사용자가 **즉각적인 반응**을 받지 못하는 시간을 모두 합산한 지표
   - 자바스크립트가 과도하게 실행되어 조작이 지연되지 않도록 관리한다.

## 4. Lighthouse 실행 방법

### 4.1 Chrome DevTools를 통한 실행

1. **Chrome** 브라우저에서 원하는 웹 페이지를 연다.
2. **개발자 도구(DevTools)**를 열기
   - Windows: `F12` 또는 `Ctrl + Shift + I`
   - macOS: `Cmd + Opt + I`
3. 상단 탭 중 **Lighthouse** (또는 **Audits**)를 선택한다.
4. **카테고리**(Performance, Accessibility 등)와 **디바이스 유형**(Mobile/Desktop)을 지정한다.
5. **Analyze page load**(또는 Generate report) 버튼 클릭.
6. 잠시 후 분석 결과(점수, 지표, 개선 가이드)가 표시된다.

> **팁**: 성능 측정 시 네트워크/CPU 속도를 실제 모바일 환경에 가깝게 **제한(Throttling)** 하는 것도 가능하다. 오버레이된 설정(gear 아이콘)에서 설정할수 있다.

### 4.2 CLI(Command Line Interface)에서 실행

#### 사전 준비

- Node.js가 설치되어 있어야 한다.

#### 실행 절차

1. Lighthouse 전역 설치
   ```bash
   npm install -g lighthouse
   ```
2. CLI에서 Lighthouse 실행

   ```bash
   lighthouse https://example.com --view
   ```

   - `--view` 옵션을 주면 분석 완료 후 자동으로 리포트를 브라우저에서 열어준다.
   - 결과 HTML(또는 JSON) 리포트를 로컬에 저장하여 참고할 수도 있다.

3. CI/CD 파이프라인에서 활용 예시
   ```bash
   lighthouse https://example.com \
     --chrome-flags="--headless" \
     --output html \
     --output-path=./reports/lighthouse_report.html
   ```
   - 배포 후 자동으로 Lighthouse를 실행해 **성능 점수**가 일정 기준 이하일 시 빌드 실패·알림을 설정할 수도 있다.

### 4.3 Node.js API 이용

#### 사전 준비

```bash
npm install lighthouse chrome-launcher
```

#### 간단 예시 코드

```js
// lighthouse-run.js
const lighthouse = require("lighthouse");
const chromeLauncher = require("chrome-launcher");

(async () => {
  const chrome = await chromeLauncher.launch({ chromeFlags: ["--headless"] });
  const options = { logLevel: "info", output: "json", port: chrome.port };
  const runnerResult = await lighthouse("https://example.com", options);

  // Lighthouse 결과
  console.log(
    "Performance Score:",
    runnerResult.lhr.categories.performance.score * 100
  );

  await chrome.kill();
})();
```

- 이 방식으로 **커스텀 레포트**, **알림**(슬랙 등) 연동, **대시보드** 구축 등이 가능하다.

### 4.4 PageSpeed Insights (웹 서비스)

- **[PageSpeed Insights](https://pagespeed.web.dev/)** 웹사이트에 접속
- 분석하려는 URL 입력 후 “분석” 버튼 클릭
- Lighthouse 기반의 모바일·데스크톱 성능 점수와 개선 사항을 즉시 확인 가능

## 5. 리포트 해석 및 개선 가이드

Lighthouse 리포트는 각 카테고리에 대해 **0~100점**을 매긴다.

- **90점 이상**: 우수(Green)
- **50~89점**: 평균(Orange)
- **49점 이하**: 낮음(Red)

각 지표별 세부 항목을 클릭하면 **개선 권장사항**을 확인할 수 있다.

### 예시 개선 항목

- **이미지 최적화**: 이미지 파일 사이즈 줄이기, 현대적 포맷(WebP, AVIF 등) 사용, Lazy Loading 적용
- **JS/CSS 리소스 최적화**: 불필요한 코드 제거, Code Splitting, 압축/번들링
- **브라우저 캐시 사용**: `Cache-Control`, `ETag` 등으로 재방문 시 로딩속도 향상
- **텍스트 압축 활성화**: gzip, brotli 등으로 텍스트 기반 리소스를 효율적으로 전송
- **폰트 로딩 최적화**: `font-display: swap` 등 적용으로 텍스트 표시 지연 방지
- **레이아웃 안정화**: 이미지 크기 고정, 광고/동적 요소의 위치 할당으로 CLS 줄이기

## 6. 실무 활용 예시

### 6.1 CI/CD 파이프라인 연동

1. **GitHub Actions**, **Jenkins**, **GitLab CI** 등의 파이프라인에서 배포 후 Lighthouse 실행
2. **성능 점수**가 일정 기준 미달 시 빌드 실패, 슬랙 알림 등을 구현
3. 매 배포마다 성능 점수 변화를 **지속 모니터링**하여, 성능 회귀를 조기에 발견

```yaml
name: Lighthouse CI

on: [push]

jobs:
  run-lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 16
      - name: Install dependencies
        run: npm install
      - name: Run Lighthouse
        run: |
          npx lhci autorun
```

### 6.2 성능 대시보드 구축

- **Lighthouse 결과(JSON)**를 주기적으로 수집하고,
- **Grafana**, **Kibana**, 혹은 사내 대시보드에 **시계열 그래프**로 시각화
- 특정 날짜나 배포 버전부터 점수가 급락했다면, 해당 시점의 커밋·이슈 등을 추적하여 신속하게 개선

## 7. FAQ

1. **테스트마다 점수가 달라집니다. 왜 그런가요?**

   - 브라우저·CPU·네트워크 상태 등 **환경적 요인**에 의해 측정값이 달라질 수 있다.
   - 가능하다면 **헤드리스 크롬**과 같은 일정한 환경에서 **반복 측정**하여 평균값/중앙값을 참고하는 것이 좋다.

2. **Lighthouse 점수가 높으면 사용자 체감도 좋은 건가요?**

   - 일반적으로 **높은 점수**는 웹 성능이나 접근성이 개선되었음을 의미한다.
   - 다만 **실제 사용자 환경(RUM)**, **비즈니스 요구** 등도 고려해야 하므로, 점수만을 맹신하기보다는 **종합적인 개선**을 목표로 삼아야 한다.

3. **구버전 브라우저나 저성능 기기 환경은 어떻게 측정하나요?**

   - Lighthouse는 주로 최신 크롬 환경을 기준으로 한다.
   - 구형 브라우저나 IoT 디바이스 등 특수 환경은 별도로 **실 기기를 활용한 성능 테스트**나 **RUM**을 병행하는 방법이 있다.

4. **PageSpeed Insights와 Lighthouse 점수가 다를 수 있나요?**
   - 둘 다 Lighthouse 엔진을 기반으로 하지만, PageSpeed Insights는 **실제 사용자 데이터**(Chrome User Experience Report)도 일부 반영할 수 있다.
   - 측정 시점, 네트워크, 테스트 환경에 따라 차이가 발생할 수 있다.

## 8. 참고 자료

- [Lighthouse 공식 깃허브 (오픈소스)](https://github.com/GoogleChrome/lighthouse)
- [Google Developers Lighthouse 문서](https://developers.google.com/web/tools/lighthouse)
- [PageSpeed Insights](https://pagespeed.web.dev/)
- [Chrome DevTools 공식 문서](https://developer.chrome.com/docs/devtools/)
