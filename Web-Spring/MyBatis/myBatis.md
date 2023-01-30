## MyBatis

: Java Object와 SQL 사이의 자동 매핑 기능을 지원하는 ORM 프레임워크

- 쉬운 접근성과 코드의 간결함
    - JDBC의 모든 기능을 대부분 제공
    - 복잡한 JDBC 코드 ➝ 깔끔한 소스코드
    - 수동적인 파라미터 설정
    - 쿼리 결과에 대한 맵핑 구문 제거
- SQL문과 프로그래밍 코드의 분리
    - SQL에 변경이 있어도 Java 코드를 수정하지 않아도 됨
- 다양한 프로그래밍 언어로 구현 가능
    - Java, C#, .NET, Ruby 등

<br>

## MyBatis 구성

> ### MyBatis Configuration File

- MyBatis3의 `작업 설정`을 설명하는 XML 파일
    - DB 연결 대상
    - 매핑 파일의 경로
    - MyBatis3의 작업 설정 

> ### SqlSessionFactoryBuilder

- MyBatis3 구성 파일을 `읽고 생성`하는 SqlSessionFactory 구성 요소
- Spring과 통합돼 사용할 떄 애플리케이션 클래스에서 직접 처리하지 않음

> ### SqlSessionFactory

- SqlSession을 `생성`하는 구성 요소
- Spring과 통합돼 사용할 떄 애플리케이션 클래스에서 직접 처리하지 않음

> ### SqlSession

- `SQL 실행` 및 `트랜잭션 제어`를 위한 API를 제공하는 구성 요소
- Spring과 통합돼 사용할 떄 애플리케이션 클래스에서 직접 처리하지 않음

> ### Mapper interface

- typesafe에서 매핑 파일에 정의된 SQL을 호출하는 인터페이스

> ### Mapper File

- SQL 및 O/R 매핑 설정을 설명하는 XML 파일

<br>

## MyBatis Database Access

<div align=center width=700>
    <img src='../../resources/spring/MyBatis/myBatisDatabaseAccess.png'>
</div>

1. `Application`이 `SqlSessionFactoryBuilder` 호출
2. `SqlSessionFactoryBuilder`가 SqlSessionFactory를 생성하기 위해 `MyBatis Config File`을 읽음
3. SqlSessionFactoryBuilder가 `SqlSessionFactory`를 생성
4. Client가 `Request`를 보냄
5. `Application`이 `SqlSessionFactory`에서 SqlSession을 가져옴
6. SqlSessionFactory가 `SqlSession` 생성 및 `Application`에게 반환
7. Application이 SqlSession에서 `Mapper Interface`의 구현 개체를 가져옵니다.
8. Application이 Mapper Interface 메서드 호출
9. Mapper Interface의 구현 개체가 SqlSession `메서드 호출` 및 `SQL 실행 요청`
10. SqlSession이 Mapping File에서 실행할 SQL을 가져와 SQL 실행