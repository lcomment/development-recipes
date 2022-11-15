> ### 해당 문법은 `MySQL` 기준입니다❗️

<br>

## 1. 집계함수

> ### COUNT

- 해당 열들의 `개수`
- `NULL` 값을 제외하고 모두 셈

```sql
SELECT COUNT(id)
```

> ### SUM

- 해당 열들을 모두 `더함`

```sql
SELECT SUM(weight)
```

> ### AVG

- 해당 열들의 `평균`

```sql
SELECT AVG(weight)
```

> ### MAX, MIN

- 해당 열들의 `최대값`, `최소값`
- 다른 집계함수와 달리 `문자열`이나 `날짜`에도 사용 가능

```sql
SELECT MAX(weight), MIN(weight)
```

> ### + DISTINCT

- 해당 열에서 같은 값을 가진 중복된 행 제거
- 집계 함수와 같이 사용 가능

```sql
SELECT DISTINCT COUNT(name)
```

<br>

## 2. DATE_FORMAT(날짜, 형식)

→ 날짜를 지정한 형식으로 출력

| 기호 |            역할             |
| :--: | :-------------------------: |
| `%Y` |         4자리 년도          |
| `%y` |         2자리 년도          |
| `%M` |        긴 월 (영문)         |
| `%b` |       짧은 월 (영문)        |
| `%W` |    긴 요일 이름 (영문))     |
| `%a` |    짧은 요일 이름 (영문)    |
| `%i` |             분              |
| `%T` |          hh:mm:ss           |
| `%m` |      숫자 월 (두 자리)      |
| `%c` | 숫자 월 (한 자리는 한 자리) |
| `%d` |       일자 (두 자리)        |
| `%e` |  일자 (한 자리는 한 자리)   |
| `%I` |        시간 (12시간)        |
| `%H` |        시간 (24시간)        |
| `%r` |       hh:mm:ss AM, PM       |
| `%s` |             초              |

```sql
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d) AS DATE

# DATE | 2022-11-16
```

<br>

## 3. GROUP BY와 HAVING

- `GROUP BY`: 특정 컬럼을 그룹화 하는 명령어
- `HAVING`: 특정 컬럼을 그룹화한 결과에 조건을 거는 명령어

> ### 사용법

```sql
# 1. 컬럼 그룹화
SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼;

# 2. 조건(WHERE) 처리 후 컬럼 그룹화
SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼;

# 3. 컬럼 그룹화 후에 조건 처리
SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼 HAVING 조건식;

# 4. 조건 처리 후에 컬럼 그룹화 후에 조건 처리
SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼 HAVING 조건식;
```

<br>

## 그 외 문법

> ### IFNULL(컬럼명, 대체값)

: Column 값이 NULL일 때, 대체 값으로 출력할 수 있도록 하는 함수

```sql
SELECT IFNULL(SCORE, "0")
```

<br>

> ### LIKE '문자열'

: 뒤에 오는 문자열에 포함되는지 판단

```sql
# 1. 접두사가 word인 NAME 찾깁
WHERE NAME LIKE 'word%'

# 2. 접미사가 word인 NAME 찾깁
WHERE NAME LIKE '%word'

# 3. word를 포함하고 있는 NAME 찾깁
WHERE NAME LIKE '%word%'
```
