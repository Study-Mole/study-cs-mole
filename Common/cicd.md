# CI/CD

### 프론트엔드 배포가 필요한 이유

프론트엔드 코드가 배포되는 프론트엔드 서버는 웹 페이지를 렌더링 하는 데 필요한 HTML, CSS, JavaScript 파일을 브라우저로 전송하는 역할을 한다.<br/>
백엔드 서버는 API를 생성하고 프론트엔드 서버에게 전송하는 역할을 하기 때문에, 하나의 웹 사이트를 배포하기 위해서는 프론트엔드, 백엔드 두 방향에서 모두 배포가 필요하다.

## CI/CD

애플리케이션의 개발 단계는 아래와 같다.<br/>
`개발` -> `빌드` -> `테스트` -> `릴리즈` -> `배포`

**CI/CD**는 `지속적 통합(Continuous Integration)` / `지속적 제공 or 배포(Delivery or Deployment)`의 줄임말로, CI는 빌드 및 테스트의 자동화, CD는 배포 자동화 과정을 뜻한다.

### CI

GitHub 레포지토리의 특정 브랜치에 push, PR 등을 요청하였을 때 자동화된 빌드, 테스트를 실행하여 코드를 레포지토리에 지속적으로 통합.

== **`빌드` -> `테스트` -> `릴리즈`의 과정을 자동화** 하는 것을 말한다.

### CD

CI를 통해 빌드와 테스트에 성공한 코드를 자동으로 실제 환경에 배포.

== **`릴리즈`된 버전의 코드를 `배포`**하는 과정을 말한다.

- 지속적 제공 (Continuius Delivery)
  - CI를 마치고 릴리즈가 가능한 상태라면 자동으로 배포
- 지속적 배포 (Continuous Deployment)
  - CI를 마치고 릴리즈가 가능한 상태라면 사람의 검증을 통해 수동으로 배포

## 배포를 위한 툴 결정하기

프론트엔드 배포를 위해서는 **CI/CD 툴**과 **배포 플랫폼**을 결정해야한다.

## GitHub Actions (CI/CD 툴 결정하기)

CI/CD 툴에는 Jenkins, GitHub Actions, TravisCI 등이 있다.<br/>

이번 스터디에서는 무료로 레포지토리 생성이 가능하고, 간편하게 설정할 수 있어 타 서비스 대비 진입 장벽이 낮은 GitHub Actions를 사용해보자.
<br/>

GitHub Actions은 push나 pull_request를 트리거로 빌드, 테스트 등 반복적으로 실행할 작업들을 자동으로 수행한다.
<br/>

우리는 `main` -> `develop` -> `feat`의 flow를 사용한다.

- `main`: 언제든 출시될 수 있는 릴리즈 버전을 가지고 있는 브랜치
- `develop`: 기능 및 수정 사항이 반영되고 있는, feat 브랜치의 내용이 모이는 브랜치
- `feat`: 각 기능들이 구현되는 브랜치

<br/>

_우리의 목표는 GitHub Actions를 통해서 develop으로의 push가 이루어졌을 때 빌드와 테스트를 실행하고 성공했다면 main으로 자동 push 하여 릴리즈 버전을 통합하는 것이다._

### workflow 작성하기

workflow는 .github/workflows 폴더에 yml 확장자 파일을 생성하여 작성한다.

```yaml
# test-cicd/.github/workflows/ci.yml

name: CI

on: # develop 브랜치에 push가 발생하면 자동 실행된다.
  push:
    branches: develop

jobs:
  build: # 이 workflow는 build라는 작업(job)을 포함하며 아래의 세부 단계(steps)로 진행된다.
    runs-on: ubuntu-latest # 이 작업은 ubuntu 최신 버전의 가상 환경에서 실행된다.

    steps:
      # 저장소의 코드를 가상 환경으로 가져온다.
      - name: Code Checkout
        uses: actions/checkout@v4

        # 가상 환경에 node를 설치한다.
      - name: Use Node.js 22.x
        uses: actions/setup-node@v4
        with:
          node-version: 22.x
          cache: "npm"

          # 프로젝트의 의존성을 설치한다.
      - name: Install Dependencies
        run: npm ci

        # 빌드
      - name: Build
        run: npm run build --if-present

        # 테스트
      - name: test
        run: npm test

      # push 작업을 위한 사용자 계정 정보를 설정한다.
      - name: Config Github Setting
        run: |
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"

      # 빌드가 성공했을 경우 main으로 자동 push
      - name: Push to main
        # 이전 단계들이 모두 성공했을 경우에만 실행된다.
        if: success()
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # push 권한을 부여한다.
          branch: main
          force: false
```

## 배포 플랫폼 결정하기

**방법 1. Netlify, Vercel, Cloudflare (서버리스 플랫폼)**

자체 서버를 구성하지 않고 플랫폼에서 제공해주는 클라우드를 사용하여 간단하게 배포 가능하며 CI/CD 기능을 제공한다.

- Netlify → 한 달 빌드의 최대 시간을 제한
- Vercel → 하루 빌드 최대 갯수를 제한
- Cloudflare → 무료, 관련자료 많지 않음

**방법 2. S3 + Cloudfront**

플랫폼을 사용할 때보다 자유도가 높다. SSR 활용이 어렵다.

- S3 (Amazion Simple Storage Service)
  - 객체 스토리지 서비스로 정적 웹사이트 호스팅을 지원
  - 데이터를 버킷이라고 불리는 리소스에 객체로 저장
- Amazon Cloudfront
  - 직접 만들 수 있는 CDN 서비스
  - 세계 이곳저곳에 중계 서버를 놓아 웹 자원을 대신 전달하는 네트워크

**방법 3. Amplify**

AWS에서 제공하는 서버리스 배포 서비스로 인증, 스토리지, 배포 등을 보다 쉽게 설정할 수 있다. 내부적으로 S3 + Cloudfront를 사용해 배포한다.

## Vercel

GitHub과 연동이 가능하고 간편한 설정으로 자동 배포 기능을 이용할 수 있는 Vercel을 사용하기로 한다.

_Vercel에 GitHub을 연동하면 자동으로 main(default)에 연결되어 main 브랜치에 변경사항이 있을 경우 자동으로 재배포한다._

배포 과정은 아래와 같다!

### 1. Vercel에 GitHub을 연동하고 레포지토리를 선택한다.

아래의 search bar를 클릭하여 배포할 레포지토리를 선택할 수 있다.
![vercel(1)](</Images/vercel(1).png>)

### 2. 배포할 프로젝트를 설정한다.

![vervel(2)](</Images/vercel(2).png>)

### 3. 배포 완료!

배포된 주소를 통해 프론트엔드 프로젝트를 확인할 수 있다.
![vercel(3)](</Images/vercel(3).png>)

Vercel를 통해서 너무나도 쉽게 배포가 되었다..<br/>
이외에도 Vercel에서는 자체적으로 preview, production 이라는 서로 다른 환경의 배포 모드를 제공한다. 이를 활용하여 간편하게 개발 환경과 운영 환경을 분리하여 프로젝트를 관리할 수 도 있다.

이번 스터디를 통해서 프론트엔드 배포를 찍먹해봤다.. 세부적인 설정들은 아직 까보지도(?) 않았지만, 프론트 개발자라면 프론트엔드 배포의 흐름 정도는 꼭 알고있으면 좋겠다는 생각이 든다.

[참고]

- [GitHub Actions를 이용한 프론트엔드 CI/CD 파이프라인 구축](https://velog.io/@gugitgugit/GitHub-Actions%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-CICD-%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8-%EA%B5%AC%EC%B6%95)
- [Vercel로 React 프로젝트를 배포해 보자 - (1) Vercel 배포 기본](https://velog.io/@lovelys0731/Vercel%EB%A1%9C-React-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-%EB%B0%B0%ED%8F%AC%ED%95%B4-%EB%B3%B4%EC%9E%90-1-Vercel-%EB%B0%B0%ED%8F%AC-%EA%B8%B0%EB%B3%B8)
