# 소셜로그인

사용자들은 회원가입을 통해 다양한 웹서비스를 이용한다.
<br>서비스들이 많아지면서 매번 회원가입을 하기 번거로웠던 경험이 있을 것이다.

소셜로그인은 이런 문제를 해결할 방법 중 하나다.
<br>대부분의 소셜로그인 OAuth 2.0 인증방식을 사용한다.

## 용어 정리

- Resource owner: 자원 소유자(로그인을 시도하는 사용자)
- Client: 서비스 제공자(사용자가 이용하는 서비스를 제공)
- Authorization server: 인증 서버(인증 처리 및 토큰 발행)
- Resource server: 자원 서버(실제 사용자 정보를 저장)

이 때 인증 서버와 자원 서버는 소셜로그인을 지원하는 기업에 존재한다.

## 과정

1. 클라이언트가 서비스 출시 후 인증 기업에 사용을 신청
2. 원하는 사용자 정보와 로그인 성공 시 접속할 주소를 설정
3. 인증 기관에서 해당 정보를 검토한 뒤 적합한 경우 승인
4. 이후 사용자가 소셜로그인을 요청하면 클라인트는 인증 서버로 안내
5. 사용자가 안내받은 인증 서버에서 로그인 시도
6. 인증 서버는 사용자가 처음 로그인하면 클라이언트가 요구하는 정보를 알려주고 동의를 받음
7. 사용자가 정보 제공에 동의하고 로그인에 성공하면 등록된 클라이언트에게 토큰을 발행
8. 클라이언트는 전달받은 토큰을 사용하여 자원 서버에서 사용자의 정보 요청
9. 이 때 토큰 정보 중 access token에 해당하는 정보 필요
10. 자원 서버는 전달받은 토큰이 유효한지 검사한 뒤 요청한 정보를 클라이언트에게 전달
11. OAuth 인증절차가 끝나고 클라이언트는 사용자 정보를 획득

사용자는 최초 한 번의 동의와 로그인만 진행하면 된다.
<br>2차 로그인 및 자동 로그인 기능으로 더 편리한 서비스 이용 경험을 제공할  수 있다.

## 토큰

서버의 응답 중 중괄호 안에 있는 정보가 토큰에 해당한다.
<br>access_token, token_type, expires_in, refresh_token 등이 포함되며,
<br>그 외에 필요에 따라 여러 종류의 값을 추가할 수 있다.

- access_token: 자원 서버에서 사용자 정보를 불러오기 위한 랜덤한 문자열.
- expires_in: 생성된 시점부터 지정된 시간(초)만큼 사용 가능하다.
- refresh_token: 시간이 만료되었다면 이 토큰을 사용해 access_token을 재발급할 수 있다.
- token_type: 토큰의 유형. OAuth 방식의 경우 일반적으로 Bearer 유형으로 표기한다.

### 카카오 액세스 토큰

카카오 API를 통해 발급된 액세스 토큰 예시이다.

```json
{
  "token_type": "Bearer",
  "access_token": "1f3ffb6bfa702493b6aaebedc107e...",
  "expires_in": 21599,
  "refresh_token": "a3f5f7f108c3da45c4de...",
  "refresh_token_expires_in": 5183999,
  "scope": "profile"
}
```

### 구글 액세스 토큰

구글 OAuth 2.0 API로 발급된 액세스 토큰 예시이다.

```json
{
  "access_token": "ya29.a0AfH6SMA...",
  "expires_in": 3600,
  "scope": "https://www.googleapis.com/auth/userinfo.profile",
  "token_type": "Bearer",
  "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6..."
}
```

### 네이버 액세스 토큰

```json
{
  "access_token": "AAAA2vK1yE56rs3NY3...",
  "refresh_token": "AAAA2vK1yE56rs3NY3...",
  "token_type": "bearer",
  "expires_in": "3600"
}
```

### 실제 사용 예

액세스 토큰을 헤더에 포함하여 요청
```http
GET /api/v1/userinfo HTTP/1.1
Host: api.example.com
Authorization: Bearer ya29.a0AfH6SMA...
```

리프레시 토큰으로 새로운 액세스 토큰 요청

```http
POST /oauth/token HTTP/1.1
Host: api.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=a3f5f7f108c3...
```

## 소셜로그인의 장단점

### 장점 - 사용자

1. 추가 회원 가입을 하지 않아도 된다.
2. 비밀번호를 공개하지 않는다.
3. 원하는 정보만 공개할 수 있다.
4. 언제라도 로그인을 차단할 수 있다.
5. 통합 로그인 관리가 가능하다.

### 단점 - 사용자

1. SNS 계정이 털린다면 연동된 모든 서비스에서 피해가 발생할 수 있다.
2. 연동한 SNS 종류를 알고 있어야 한다. 사용하는 SNS 종류가 많다면 중복 가입의 우려가 있다.

### 장점 - 클라이언트

1. 사용자 유입이 증가한다.
2. SNS에서 제공하는 유입 분석 등의 추가 기능을 사용할 수 있다.

### 단점 - 클라이언트

1. SNS 종류가 다양하여 구현의 어려움이 있다.
2. SNS마다 제공받을 수 있는 정보가 달라 정보 통일이 어렵고 추가 정보를 요구해야 할 수도 있다.
3. 다양한 사유로 기능 사용을 거절당할 수 있다.

### 장점 - 인증 기업(SNS)

1. 계정의 활용 가치를 높여 시장 점유율을 증가시킬 수 있다.
2. 로그인 정보 분석을 통한 가치 창출이 가능하다.

### 단점 - 인증 기업(SNS)

1. 정보 유출 시 큰 피해가 발생한다.
2. 대량 트래픽 처리가 가능해야 하는데, 잦은 서버 다운 등 구현 프로세스의 결함이 발견된다면 브랜드에 대한 신뢰도가 떨어질 수 있다.

[참고]

- [SNS 로그인의 이해 (feat. OAuth 2.0)](https://youtu.be/xqBxOUFl3g8?si=ih-JhW8NJEBx7u47)