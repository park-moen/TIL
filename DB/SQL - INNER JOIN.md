# [SQL] 내가 사용하기 위해 공부하는 INNER JOIN

<br>

## JOIN절 소개

`JOIN`절을 사용하기 전에는 하나의 테이블에 대한 정보를 가져올 수 있었습니다. 하지만 관계형 데이터 베이스에서는 다양한 테이블 조합의 결과를 반환하는 조건을 필수적으로 필요합니다. `JOIN`절은 여러 테이블을 조건(`ON, USING`)을 사용해서 결합하기 위해 사용할 수 있는 `SQL` 문법입니다.

<br>

## INNER JOIN절 소개

- `INNER JOIN`은 `JOIN` 조건(`ON, USING`)이 일치하는 `tuple(row, record)`를 반환합니다.
- `INNER JOIN`절은 벤 다이어그램으로 표현하면 교집합을 의미합니다.
  <img src="https://github.com/park-moen/TIL/assets/57402711/a0d466db-60cc-489c-a9d7-a1135a09c237" width="60%">
- `MySQL`에서는 `JOIN`절과 `INNER JOIN`절은 동일하게 동작합니다.
- `SELECT`문에서 `JOIN`절은 `FROM`절 뒤에 위치합니다.

```sql
SELECT
  column
FROM
  table_name
INNER JOIN each_column ON join_condition;
```

- `JOIN` 조건이 `TRUE`로 평가되는 결과가 없을 경우 `INNER JOIN`은 빈 결과를 반환합니다.

<br>

## SELECT문에서 INNER JOIN 사용하기

```sql
SELECT
  m.member_id,
  m.name AS member,
  c.committee_id,
  c.name AS committee
FROM
  members m
INNER JOIN committees c
  ON c.name = m.name;
```

| member_id | member | committee_id | committee |
| --------- | ------ | ------------ | --------- |
| 1         | John   | 1            | John      |
| 3         | Mary   | 2            | Mary      |
| 5         | Amelia | 3            | Amelia    |

- 위 예제는 `JOIN` 조건인 `ON c.name = m.name`이 `TRUE`로 평가되는 `tuple(row, record)`만을 반환합니다.
- 일반적으로 `foreign key`를 기준으로 `JOIN` 조건을 사용합니다.
- `JOIN` 조건을 `ON`절 대신 `USING`절을 사용할 수 있습니다.

```sql
SELECT
  m.member_id,
  m.name AS member,
  c.committee_id,
  c.name AS committee
FROM
  members m
INNER JOIN committees c
  USING(name);
```

- `INNER JOIN`절을 `WHERE`을 사용해서 구현할 수 있습니다.

```sql
SELECT
  m.member_id,
  m.name,
  c.committee_id,
  c.name
FROM
  members m, committees c
WHERE
  m.name = c.name;
```

<br>

## 다중 테이블에서 INNER JOIN 사용하기

### 3개의 테이블에서 `INNER JOIN`절을 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/752583c5-7bb6-41c8-a2b9-57c618c3b804" >

```sql
SELECT
  orderNumber,
  productCode,
  orderLineNumber,
  quantityOrdered,
  priceEach,
  orderDate,
  productName
FROM
  orderDetails
INNER JOIN
  orders USING(orderNumber)
INNER JOIN
  products USING(productCode)
```

<img src="https://github.com/park-moen/TIL/assets/57402711/cc68ce0b-119e-42b8-ae70-c40ae5d57b95" />

- 위 예제는 `products` 테이블과 `orders` 테이블은 `foreign key` 또는 동일한 `Column`이 존재하지 않아서 `INNER JOIN`을 사용해도 빈 결과를 반환합니다.
- 이런 문제를 해결하기 위해서 `orderDetails` 테이블에 존재하는 `orderCode`와 `productCode Column`을 사용해서 `products` 테이블과 `orders` 테이블을 연결했습니다.

### 4개의 테이블에서 `INNER JOIN`절을 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/79438044-ec61-42bc-a094-cb0bdf5f8ddb">

```sql
SELECT
  orderNumber,
  orderDate,
  customerName,
  orderLineNumber,
  productName,
  quantityOrdered,
  priceEach
FROM
  orderDetails
INNER JOIN products
  USING(productCode)
INNER JOIN orders
  USING(orderNumber)
INNER JOIN customers
  USING(customerNumber);
```

- 위 예제는 `orderDetails` 테이블의 `productCode, orderNumber Column`을 기준으로 `orders, products` 테이블을 `JOIN`합니다.
- 그리고 `orders` 테이블의 `customerNumber Column`으로 `cumsters` 테이블을 `JOIN`합니다

<br>

## INNER JOIN에서 연산자 사용하기

- 지금까지는 동일 연산자(`=`)만을 사용해서 `INNER JOIN`을 진행했습니다.
- `JOIN`절에서는 동일 연산자 외에도 다양한 연산자를 조합할 수 있습니다.
- 또한, `WHERE`절을 같이 사용하게 된다면 더욱 풍부한 `Query`문을 작성할 수 있습니다.
- `JOIN`절로 테이블을 조합하게 된다면 테이블끼리 연산자를 사용해서 원하는 결과를 반환할 수 있습니다.
- 아래 예제를 보면 `products.MSRP > orderDetails.priceEach`은 서로 다른 테이블의 `Column` 이지만 비교 연사자를 사용해서 `JOIN` 조건에서 사용해도 문제 없이 결과를 반환합니다.

```sql
SELECT
  orderNumber,
  productName,
  MSRP,
  priceEach
FROM
  products p
INNER JOIN orderDetails o
  ON p.productCode = o.productCode
    AND p.MSRP > o.priceEach
WHERE
  p.productCode = 'S10_1678';
```

<img src="https://github.com/park-moen/TIL/assets/57402711/f1feda3b-fc36-4d85-a559-f7dc834bb2e5">

<br>

## Reference

- [MySQLTUTORIAL - MySQL INNER JOIN](https://www.mysqltutorial.org/mysql-inner-join.aspx)
- [DATA ON AIR - 조인(JOIN)](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&kboard_search_option%5Btree_category_1%5D%5Bkey%5D=tree_category_1&kboard_search_option%5Btree_category_1%5D%5Bvalue%5D=SQL+%EA%B8%B0%EB%B3%B8+%EB%B0%8F+%ED%99%9C%EC%9A%A9&uid=345)
