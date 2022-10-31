## JOIN

: 두 개의 테이블을 서로 묶어서 하나의 결과를 만들어 내는 것

- `INNER JOIN` : 두 테이블을 조인할 때, `두 테이블에 모두` 지정한 열의 데이터가 있어야 함
- `OUTER JOIN` : 두 테이블을 조인할 때, `1개의 테이블에만` 데이터가 있어도 결과가 나옴
- `CROSS JOIN` : 한쪽 테이블의 `모든 행`과 다른 쪽 테이블의 `모든 행`을 조인하는 기능
- `SELF JOIN` : 자신이 자신과 조인한다는 의미로, `1개의 테이블만 사용`

> ### 샘플 테이블

→ `USER_ORDER`

| NAME |  BUY_PRODUCT  |
| :--: | :-----------: |
| kim  |    macbook    |
| lee  | Yuna's Haptic |
| choi |   Galaxy S2   |
| park |    airpod     |

→ `APPLE_PRODUCT`

| PRODUCT_NAME |   PRICE   |
| :----------: | :-------: |
|   macbook    | 3,300,000 |
|     iPad     |  800,000  |
|    iPhone    | 1,400,000 |
|    airpod    |  320,000  |

<br>

## INNER JOIN

→ 두 테이블의 교집합만 출력되는 조인

```sql
SELECT *
FROM USER_ORDER UO
INNER JOIN APPLE_PRODUCT AP
    ON UO.BUY_PRODUCT = AP.PRODUCT_NAME
```

| NAME | BUY_PRODUCT |   PRICE   |
| :--: | :---------: | :-------: |
| kim  |   macbook   | 3,300,000 |
| park |   airpod    |  320,000  |

<br>

## OUTER JOIN

> ### `LEFT` OUTER JOIN

→ 왼쪽 테이블의 모든 값이 출력되는 조인

```sql
SELECT *
FROM USER_ORDER UO
LEFT OUTER JOIN APPLE_PRODUCT AP
    ON UO.BUY_PRODUCT = AP.PRODUCT_NAME
```

| NAME |  BUY_PRODUCT  |   PRICE   |
| :--: | :-----------: | :-------: |
| kim  |    macbook    | 3,300,000 |
| lee  | Yuna's Haptic |   NULL    |
| choi |   Galaxy S2   |   NULL    |
| park |    airpod     |  320,000  |

<br>

> ### `RIGHT` OUTER JOIN

→ 오른쪽 테이블의 모든 값이 출력되는 조인

```sql
SELECT *
FROM USER_ORDER UO
RIGHT OUTER JOIN APPLE_PRODUCT AP
    ON UO.BUY_PRODUCT = AP.PRODUCT_NAME;
```

| PRODUCT_NAME |   PRICE   | NAME |
| :----------: | :-------: | ---- |
|   macbook    | 3,300,000 | kim  |
|     iPad     |  800,000  | NULL |
|    iPhone    | 1,400,000 | NULL |
|    airpod    |  320,000  | park |

<br>

> ### `FULL` OUTER JOIN

```sql
SELECT *
FROM USER_ORDER UO
FULL OUTER JOIN APPLE_PRODUCT AP
    ON UO.BUY_PRODUCT = AP.PRODUCT_NAME;
```

→ 왼쪽 또는 오른쪽 테이블의 모든 값이 출력되는 조인

| NAME |  BUY_PRODUCT  |   PRICE   |
| :--: | :-----------: | :-------: |
| kim  |    macbook    | 3,300,000 |
| lee  | Yuna's Haptic |   NULL    |
| choi |   Galaxy S2   |   NULL    |
| NULL |     iPad      |  800,000  |
| NULL |    iPhone     | 1,400,000 |
| park |    airpod     |  320,000  |

<br>

## CROSS JOIN

- 한쪽 테이블의 모든 행과 다른 쪽 테이블의 모든 행을 `조인`시키는 기능
- `카티션 곱`이라고도 함

```sql
SELECT {열 목록}
FROM {첫 번째 테이블}
CROSS JOIN {두 번째 테이블};
```

<br>

## SELF JOIN

→ 자기 자신과 조인

```sql
SELECT {열 목록}
FROM {테이블}
INNER JOIN {테이블}
    ON {조건}
WHERE {조건}
```

---

### 참고자료

- [@hongong](https://hongong.hanbit.co.kr/sql-기본-문법-joininner-outer-cross-self-join/)
- [@doh-an](https://doh-an.tistory.com/30)
