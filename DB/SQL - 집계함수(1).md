# [SQL] 내가 사용하기 위해 공부하는 집계함수

<br>

## 집계함수 소개하기

- 집계함수는 하나 이상의 값에 대한 계산을 수행하고 단일 값을 반환합니다.
- 집계함수는 `AVG(), COUNT(), SUM(), MAX(), MIN()`가 대표적인 집계함수입니다.
- 다음은 집계 함수의 기본문법입니다.

```sql
aggregate_function(DISTINCT | ALL expression)
```

1. 집계함수(`AVG(), COUNT(), SUM(), MAX(), MIN()`)르 지정합니다.
2. 고유한 값을 기반으로 계산하려면 `DISTINCT`절을 사용하고 중복을 포함해서 계산하려면 `ALL`절을 사용합니다. 기본값은 `ALL`절입니다.
3. `Column`이 될 수 있는 표현식을 집계함수에 지정합니다. 표현식은 `Column`과 연산자를 포함할 수 있있습니다.

| 집계함수 | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| AVG( )   | NULL이 아닌 값의 평균을 계산합니다.                          |
| COUNT( ) | NULL 값이 있는 행을 포함하여 그룹의 행(row) 수를 반환합니다. |
| SUM( )   | NULL이 아닌 모든 값의 함계를 계산합니다.                     |
| MAX( )   | NULL이 아닌 값 중에서 최대값을 반환합니다.                   |
| MIN( )   | NULL이 아닌 값 중에서 최소값을 반환합니다.                   |

<br>

## AVG 함수 사용하기

- 기본 문법은 집계함수와 동일하게 동작합니다. 총합(`SUM`)에서 개수(`COUNT`)를 나눈 결과와 동일합니다.

```sql
SELECT
  AVG(expression),
  SUM(expression) / COUNT(expression)
FROM
  table_name;
```

```sql
SELECT
  AVG(buyPrice) 'AverageP_Price'
FROM
  products;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/917c4e1b-5ca2-4685-8c42-8c4c3dea2330" />

- `WHERE`절과 같이 사용하여 원하는 타겟팅의 평균을 구할 수 있습니다.

```sql
SELECT
    AVG(buyPrice) 'Average Classic Cars Price'
FROM
    products
WHERE
    productLine = 'Classic Cars';
```

<img src="https://github.com/park-moen/TIL/assets/57402711/b2cf71e6-dddb-41ec-b5d9-9b1ec928e7b8" />

- `NULL` 값은 연산에서 제외됩니다.
- 총합(`SUM`)에서 개수(`COUNT`)를 나눈 결과는 평균값으로 `NULL` 값으로 평가되는 행(`row`)은 개수(`COUNT`)에서 제외됩니다.
- 아래 예시의 평균값은 `1 + 2 + 3 + 0(NULL) / 4`로 연산되지 않고 `1 + 2 + 3 / 3`으로 연산되어서 `2`라는 평균값을 반환합니다.

```sql
CREATE TABLE IF NOT EXISTS t (
  id INT AUTO_INCREMENT PRIMARY KEY,
  val INT
);

INSERT INTO t(val) VALUES(1),(2),(nulL),(3);
```

```sql
SELECT
  AVG(val)
FROM
  t;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/bdec212f-6998-4699-969e-90b98bcd640b" />

- `NULL` 값을 0으로 계산해서 평균을 구하고 싶다면 `control flow functions`을 사용할 수 있습니다.

```sql
SELECT
  AVG(
    CASE
    WHEN val IS NULL THEN 0
    ELSE val
    END
  ) AS AVG
FROM
  t;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/8e41f5f3-b583-4432-bc73-9b10ae85f47f" />

- `DISTINCT`절을 사용해서 고유한 값을 기반으로 평균을 구할 수 있습니다.

```sql
CREATE TABLE t (
  id INT AUTO_INCREMENT PRIMARY KEY,
  val INT
);

INSERT INTO t(val) VALUE(1),(1),(1),(2),(2),(2),(3);
```

```sql
SELECT * FROM t;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/735ba830-6db4-4e45-96a6-0c738e65262f" />

```sql
SELECT
  AVG(DISTINCT val)
FROM
  t;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/b7f559f8-a465-4b03-81cb-ccb1bc775510" />

<br>
<br>

## COUNT 함수 사용하기

- `COUNT()` 함수는 테이블의 행(`row`) 수를 계산하는 집계함수입니다.
- `COUNT()`함수에는 `COUNT(*)`, `COUNT(expression)`, `COUNT(DISTINCT expression)`의 세 가지 형식이 있습니다.
- `COUNT(*)`함수는 `NULL`을 포함한 행 수를 반환합니다.

```sql
CREATE TABLE count_demos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  val INT
);

INSERT INTO count_demos(val)
VALUE(1),(1),(2),(2),(NULL),(3),(4),(NULL),(5);

SELECT * FROM count_demos;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/41728632-6e6c-4558-8154-3a16fd3418cf" />

```sql
SELECT COUNT(*) FROM count_demos;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/5e91e50b-93e6-4c59-8ab7-9e941892b4f3" />

- `WHERE`절을 사용해서 원하는 타겟팅의 행 수를 계산할 수 있습니다.

```sql
SELECT COUNT(*)
FROM count_demos
WHERE val = 2;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/a207ed27-cbd9-4624-a638-f838ca357cd1" />

- `COUNT(expression)`함수는 선택된 `expression`에서 `NULL` 값을 제외한 행 수를 반환합니다.

```sql
SELECT COUNT(val)
FROM count_demos;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/4608b089-07c6-49d2-9e2d-ea47a6b37b58" />

- `COUNT(DISTINCT expression)`함수는 NULL이 아닌 고유한 값에 대한 행 수를 반환합니다.

```sql
SELECT COUNT(DISTINCT val)
FROM count_demos;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/091bdd8c-bb68-47cf-a015-ef3290a150a8" />

<br>
<br>

## SUM 함수 사용하기

- `SUM` 함수는 집합에 있는 값의 합계를 계산합니다.
- `SUM` 함수는 문자열이나 날짜 타입에서는 사용할 수 없고 숫자형 타입에서만 사용할 수 있습니다.
- `NULL` 값은 연산에서 제외됩니다.

```sql
CREATE TABLE sum_demo (
  n INT
);

INSERT INTO sum_demo(n)
VALUES(1),(1),(2),(NULL),(3);

SELECT * FROM sum_demo;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/d034cb6d-fd42-4ee6-9587-ccfe3ca9e0e6" />

```sql
SELECT SUM(n) FROM sum_demo;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/c3452c18-90c1-4cf8-8891-20b91694eb20" />

- `DISTINCT`절을 사용해서 고유의 값의 합계를 구할 수 있습니다.

```sql
SELECT SUM(DISTINCT n) FROM sum_demo;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/62a230b4-d876-4360-afc6-f66ab6c0ae71" />

- `SUM` 함수의 인자에는 표현식이 올 수 있습니다. 그래서 연산자를 `SUM` 함수 내부에서 사용할 수 있습니다.
- `WHERE`절을 사용해서 원하는 타겟팅의 합계만을 구할 수 있습니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/70aa5281-e8c1-4820-be2d-ba263e0fe923" />

```sql
SELECT
  SUM(quantityOrdered * priceEach)  orderTotal
FROM
  orderDetails
WHERE
  orderNumber = 10100;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/fc6eee9c-3a35-4e48-9f05-ea76dac49898" />

<br>
<br>

## MIN, MAX 함수 사용하기

- `MIN,MAX` 함수는 집합에서 최소값과 최대값을 계산하는 집계함수입니다.
- `DISTINCT`절은 `MIN, MAX` 함수에 영향을 주지 않습니다. `AVG, SUM, COUNT` 함수에서만 `DISTINCT`절이 영향을 줍니다.

```sql
CREATE TABLE min_or_max (
  val INT
);

INSERT INTO min_or_max(val)
VALUE(1),(20),(30),(55),(102),(NULL),(102),(30);

SELECT * FROM min_or_max;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/92063e9f-6883-4d2a-acbf-c55463682f4a" />

```sql
SELECT MAX(val), MIN(val) FROM min_or_max;

SELECT MAX(DISTINCT val), MIN(DISTINCT val) FROM min_or_max;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/3d5efc69-b665-47e9-a81e-2572ca4acd4e" />

<br>
<br>

## Reference

- [MySQLTUTORIAL - MySQL Aggregate Functions](https://www.mysqltutorial.org/mysql-aggregate-functions.aspx)
- [young's devlog - 행 개수 구하기 COUNT](https://lxxjn0-dev.netlify.app/first-step-sql-lec-20)
