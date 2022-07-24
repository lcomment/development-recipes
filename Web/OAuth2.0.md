## OAuth 2.0 이란?

- Open Authorization 2.0의 줄임말
- `인증`을 위한 개방형 프로토콜
- `Third-Party` 프로그램에게 리소스 소유자를 대신해 리소스 서버에게 제공하는 `자원에 대한 접근 권한을 위임`하는 방식 제공
- Google, Facebook, Kakao 등

</br>

## OAuth 2.0 주요 용어

- `Authentication`
  - 인증 접근 자격이 있는지 검증 단계
- `Authorization`
  - 인가, 자원에 접근할 권한을 부여하는 것
  - 인가가 완료되면 리소스 접근 권한이 담긴 Access Token이 클라이언트에게 부여
- `Access Token`
  - 리소스 서버에게서 리소스 소유자의 보호된 자원을 획득할 때 사용되는 만료 기간이 있는 Token
- `Refresh Token`
  - Access Token 만료시 이를 갱신하기 위한 용도로 사용하는 Token
  - 일반적으로 Access Token보다 만료 기간이 긺

</br>

## OAuth 2.0의 4가지 역할

- 리소스 소유자 (Resource Owner)
  - 보호된 자원에 접근할 수 있는 자격을 부여해 주는 주체
  - OAuth2 프로토콜 흐름에서 클라이언트를 인증(Authorize)하는 역할을 수행
  - 인증이 완료되면 권한 획득 자격(Authorization Grant)을 클라이언트에게 부여
  - 개념적으로는 리소스 소유자가 자격을 부여하는 것이지만 일반적으로 권한 서버가 리소스 소유자와 클라이언트 사이에서 중개 역할을 수행하게 됨
- 클라이언트 (Client)
  - 보호된 자원을 사용하려고 접근 요청을 하는 애플리케이션입니다.
- 리소스 서버(Resource Server)
  - 사용자의 보호된 자원을 호스팅하는 서버
- 권한 서버 (Authorization Server)
  - 인증/인가를 수행하는 서버
  - 클라이언트의 접근 자격을 확인하고 Access Token을 발급하여 권한을 부여하는 역할을 수행

## OAuth 2.0 프로토콜의 종류

### ① 권한 부여 승인 코드 방식 (Authorization Code Grant)

<p align=center><img src='../resources/web/oauth1.png' width=512></p>

- 권한 부여 승인을 위해 자체 생성한 Authorization Code를 전달하는 방식
- 많이 쓰이고, 기본이 되는 방식
- 간편 로그인 기능에서 사용되는 방식
- 클라이언트가 사용자를 대신하여 특정 자원에 접근을 요청할 때 사용되는 방식
- 보통 타사의 클라이언트에게 보호된 자원을 제공하기 위한 인증에 사용
- Refresh Token의 사용이 가능

---

### **참고자료**

- Web
  - [@mds_datasecurity](https://blog.naver.com/mds_datasecurity/222182943542)
  - [@meetup](https://meetup.toast.com/posts/105)
