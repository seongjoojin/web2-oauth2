# web2-oauth2

생활코딩 oauth2 강의 정리

## 수업 소개

- oAuth를 통하여서 구글, 페이스북, 트위터 등의 서비스에서 accessToken 얻어낼 수 있게 되고 그것을 통하여서 만든 서비스는 구글, 페이스북, 트위터에서 필요한 사용자의 정보를 가져올 수 있음 (이메일 등)

## 역할

Role

- Resource Server (Google, Facebook, ...) => 제어하고자 하는 자원을 가지고 있는 서버, 데이터만 가지고 있음
- Resource Owner(User) => 자원의 소유자
- Client (자신(회사)의 서버) => 리소스 서버에 접속해서 정보를 가져가는 클라이언트
- Authorization Server : 인증과 관련된 것을 처리해주는 서버

- 강의에서는 Resource Server + Authorization Server를 Resource Server 명칭함

## 등록

- Client가 Resource Server를 이용하기 위해서는 Resource Server에 사전에 승인을 받아야함.
- 위의 과정을 register(등록)이라고 함

- Client ID, Client Secret, Authorized redirect URls를 공통으로 가짐

- Client ID : 어플리케이션을 식별하는 식별자 => 외부에 노출되도 괜찮음
- Client Secret : 식별자에 대한 비밀번호 => 절대로 외부에 노출되면 안 됨
- Authorized redirect URls : 리소스 서버가 권한을 부여하는 과정에서 클라이언트에게 Authorization Code라는 값을 전달해주는 주소

## Resource Owner의 승인

- 등록 과정을 거치면 Resource Server는 Client ID, Client Secret, Authorized redirect URls 정보를 가지게 되고 Client는 Client ID, Client Secret를 가지게 됨

- client에서 Resource Owner가 요청하면 소셜 로그인이나 해당 Resource Server의 일부 기능을 사용하는 것에 대한 승인을 받는 화면을 내려줌 (사용자가 동의를 해야지 그 다음 과정을 진행할 수 있음)

- 소셜 로그인 등은 결국 `https://resource.server?client_id=1&scope=B,C&redirect_uri=https://client/callback`등의 uri로 전달하여 Resource Owner가 Resource Server에 로그인 후 client id를 체크한 후 같으면 scope의 해당하는 권한을 클라이언트에게 부여할 것인지 확인하는 메세지를 보내게 됨
- Resource Owner가 허용하면 Resource Server에서 허용한 user id와 허용한 권한을 기억하게 됨

## Resource Server의 승인

- Resource Owner가 허용 후 Resource Server는 바로 Access Token을 발행하는 것이 아니고 Authorization Code로 한 번 더 Resource Owner에게 전송함 (location : https://client/callback?code=3 등의 형태로 전송)
- Resource Owner은 받은 url로 이동하게 됨 (이 과정으로 client는 Authorized Code를 알게됨)
- Client는 Resource Server에 직접 접근함 (https://resource.server/token?grant_type=authorization_code&code=3&redirect_uri=https://client/callback&client_id=1&client_secret=2 등의 형태로 접근)
- Resource Server에서 authorization code, redirec uri, client id, client secret가 완전히 일치 하는지 체크 후 Access Token을 발급함

## 액세스 토큰 발급

- client가 준 정보가 완전히 일치하면 먼저 Resource Server와 client authorization code를 지움
- Resource Server에서는 accessToken를 발급하게 됨
- Resource Server에서는 발급한 accessToken을 client에게 전달해줌
- Client는 accessToken를 저장함

## API 호출

- accessToken를 통하여 Resource Server를 조작함
- client에서 Resource Server를 조작하기 위해서는 api를 통하여서 가능합니다.
- API(Application Programming Interface)
- 예 : https://developers.google.com/calendar/v3/reference
- https://developers.google.com/identity/protocols/oauth2/web-server#callinganapi
- 보통 HTTP header의 Authorization에 억세스 토큰을 보내어서 사용함

## Refresh Token

- Access Token은 수명이 존재함
- Access Token이 수명을 다 할 경우 다시 발급할 수 있도록 도와주는 것이 Refresh Token
- https://tools.ietf.org/html/rfc6749#section-1.5

```
  +--------+                                           +---------------+
  |        |--(A)------- Authorization Grant --------->|               |
  |        |                                           |               |
  |        |<-(B)----------- Access Token -------------|               |
  |        |               & Refresh Token             |               |
  |        |                                           |               |
  |        |                            +----------+   |               |
  |        |--(C)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(D)- Protected Resource --| Resource |   | Authorization |
  | Client |                            |  Server  |   |     Server    |
  |        |--(E)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(F)- Invalid Token Error -|          |   |               |
  |        |                            +----------+   |               |
  |        |                                           |               |
  |        |--(G)----------- Refresh Token ----------->|               |
  |        |                                           |               |
  |        |<-(H)----------- Access Token -------------|               |
  +--------+           & Optional Refresh Token        +---------------+
```

- https://developers.google.com/identity/protocols/oauth2/web-server#offline

## 수업을 마치며

- oauth는 인증 과정에 참여하고 있는 3자(Resource Server, Client, Resource Owner)가 어떻게 하면 서로를 신뢰할 수 있을까라는 고민에서 출발한 기술
- ouath를 이해하고 해당 기능을 도와주는 라이브러리를 사용하는 것이 더 좋음

공부할만한 주제

- federated identity (login with facebook, login with google 등이 이러한 기술을 이용한 것)
- oauth의 궁극적인 목표는 API 제어 (Restful, JSON, XML => 해당 지식을 습득해보는 것도 좋음)
