# 정규화 (Normalization)

## I. 이상(Anomaly) 현상

: 정규화를 거치지 않은 DB에서 발생할 수 있는 현상

> ### 원인

- 데이터들의 불필요하게 중복돼 Relation 조작에 예상치 못한 문제 발생
- Attribute들의 `종속관계`를 하나의 Relation으로 표현하기 때문에 발생

> ### 종류

- Insertion Anomaly
- Delete Anomaly
- Update Anomaly

<br>

## II. 정규화 (Normalization)

: 이상현상이 있는 릴레이션을 분해하여 이상현상을 없애는 과정

- 테이블 간 중복 데이터를 허용하지 않음
- 무결성 유지
- 저장 공간 확보

> ### i) 제 1정규화

- 테이블의 컬럼이 `원자값`(Atomic Value)을 갖도록 테이블 분해

**_AS-IS_**

| name | social                   |
| ---- | ------------------------ |
| tom  | twitter, instagram       |
| jane | facebook                 |
| khan | twitter, line, instagram |

**_TO-BE_**

| name | social    |
| ---- | --------- |
| tom  | twitter   |
| tom  | instagram |
| jane | facebook  |
| khan | twitter   |
| khan | line      |
| khan | instagram |

<br>

> ### 제 2정규화

- 제 1정규화를 진행한 테이블에 대해 `완전 함수 종속`을 만족하도록 테이블 분해
- 완전 함수 종속: PK의 부분집합이 결정자가 돼선 안된다.

**_AS-IS_** (company와 car는 복합키)

| company | car      | max_speed | country |
| ------- | -------- | --------- | ------- |
| hyundai | sonata   | 264       | korea   |
| hyundai | avante   | 250       | korea   |
| kia     | carnival | 190       | korea   |
| porsche | macan    | 272       | germany |

**_TO-BE_**

| car      | company | max_speed |
| -------- | ------- | --------- |
| sonata   | hyundai | 264       |
| avante   | hyundai | 250       |
| carnival | kia     | 190       |
| macan    | porsche | 272       |

| company | country |
| ------- | ------- |
| hyundai | korea   |
| kia     | korea   |
| porsche | germany |

<br>

> ### 제 3정규화

- 제 2정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블 분해
- 이행적 종속: A→B, B→C가 성립할 때 A→C가 성립되는 것

**_AS-IS_** (company와 car는 복합키)

| car      | fuel             | price |
| -------- | ---------------- | ----- |
| sonata   | gasoline         | 1824  |
| avante   | gasoline         | 1824  |
| carnival | gasoline         | 1824  |
| macan    | premium gasoline | 2071  |

**_TO-BE_**

| car      | fuel             |
| -------- | ---------------- |
| sonata   | gasoline         |
| avante   | gasoline         |
| carnival | gasoline         |
| macan    | premium gasoline |

| fuel             | price |
| ---------------- | ----- |
| gasoline         | 1824  |
| premium gasoline | 2071  |
