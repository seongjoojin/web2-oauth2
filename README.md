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
