## DDL, DML, DCL, TCL 구별하기

<br>

> ### 데이터 정의어 (DDL ; Data Definition Language)

: 테이블과 같은 데이터 구조를 정의하는 데에 사용되는 명령어들

- `CREATE` : 테이블 생성
- `ALTER` : 테이블 구조 수정
- `DROP` : 테이블 삭제
- `RENAME` : 테이블 이름 변경
- `TRUNCATE` : 테이블 초기화

<br>

> ### 데이터 조작어 (DML ; Data Manipulation Language)

: DB에 들어있는 데이터를 조회 및 검색하거나 변형을 가하는 종류의 명령어들

- `SELECT` : 데이터 조회
- `INSERT` : 데이터 추가
- `UPDATE` : 데이터 수정
- `DELETE` : 데이터 삭제

<br>

> ### 데이터 제어어 (DCL ; Data Control Language)

: DB에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어들

- `GRANT` : 권한을 정의할 때 사용
- `REVOKE` : 권한을 삭제할 때 사용

<br>

> ### 트랜잭션 제어어 (TCL ; Transaction Control Language)

: 논리적인 작업의 단위를 묶어 DML에 의해 조작된 결과를 트랜잭션 별로 제어하는 명령어들

- `COMMIT` : 모든 작업을 정상적으로 처리
- `ROLLBACK` : 모든 작업을 다시 돌려 놓음
- `SAVEPOINT` : COMMIT 전에 특정 시점까지만 반영하거나 ROLLBACK

<br>

---

### Reference

- [@brownbears](https://brownbears.tistory.com/180)
- [@alicesykim95](https://velog.io/@alicesykim95/DB-DDL-DML-DCL-TCL%EC%9D%B4%EB%9E%80)
