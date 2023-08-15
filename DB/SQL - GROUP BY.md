# [SQL] 내가 사용하기 위해 공부하는 GROUP BY

<br>

## GROUP BY절 소개

- `GROUP BY`절은 행(`row`) 집합을 열(`Column`) 또는 표현식(`expression`)을 기준으로 그룹화할 수 있는 문법입니다.
- 즉, 집계함수를 사용해야 하는 상황에서 열 또는 표현식으로 기준을 정해서 그룹화된 각 그룹에 대한 통계를 구할 수 있습니다.
- `GROUP BY`절의 평가 순서는 아래 사진처럼 동작합니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/99a25a19-a17b-4059-bf9e-8c8802b4f8b0">

- `GROUP BY`절을 집계함수와 같이 사용하지 않으면 `DISTINCT`절과 동일하게 중복이 발생하지 않는 행(`row, tuple, record`)을 반환합니다.

<br>

## GROUP BY절 사용하기

### 집계함수와 같이 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/11e2ff9a-8423-4535-b1e3-de7d36fa1101" />

```sql
SELECT
  status,
  COUNT(*)
FROM
  orders
GROUP BY
  status;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/755388aa-cb40-438f-8dff-b64220707eb8" />

**위 예제에서 SQL Query의 동작 방식**

1. `orders.status`을 기준으로 그룹화를 진행합니다.
2. `COUNT(*)` 집계함수가 각 `status` 행(`Shipped, Resolved, Cancelled, On Hold, Disputed, In Process`)에 대한 행 수를 반환합니다.

```sql
SELECT
  YEAR(orderDate) AS year,
  COUNT(orderNumber)
FROM
  orders
GROUP BY
  year;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/26b2d7d4-ca25-40ee-b282-d616bea95c9f" />

- `SQL` 평가 순서는 `GROUP BY`절 -> `SELECT`절로 평가되기 때문에 `Alias`를 `GROUP BY`절에서 사용할 수 없습니다.
- 특별하게 `MySQL`은 `SELECT`절에서 지정한 `Alias`를 `GROUP BY, HAVING`절에서 사용할 수 있습니다.
- 하지만, 지원하지 않는 데이터베이스도 있다는 것을 주의해야 합니다.

<br>

## 그룹화하지 않는 Column을 선택하면 발생하는 문제

- `GROUP BY`절에 그룹화하지 않는 `Column`을 선택하면 어떤 문제가 발생할까요?

```sql
CREATE TABLE template (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(20),
  quantity INT
);

INSERT INTO template(name, quantity)
VALUE('A', 1),('A', 2),('B', 10),('B', 3),('C', 3),(NULL, NULL);

SELECT * FROM template;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/6be79a1b-906d-4d6b-b42e-e065314539d9" />

```sql
SELECT
  id, name, quantity
FROM
  template
GROUP BY
  name;
```

- 위 예제는 `GROUP BY`절로 `name`을 그룹화하면 그룹당 하나의 행을 반환한다고 예상합니다.
- 하지만 `name Column` 값이 `A`인 그룹에서 `quantity Column`의 값은 1과 2로 2개가 존재합니다.
- `GROUP BY`절로 `name`을 그룹화한다면 하나의 행을 반환해야 하지만 `quantity Column`을 같이 선택해서 두개의 행이 반환되는 충돌이 발생해서 실행이 되지 않습니다.
- `GROUP BY`절을 언제 사용해야 하는지 명확하게 알고 있다면 충분히 해결할 수 있는 문제로 `GROUP BY`절은 그룹화를 진행해서 집계함수를 통해 통계를 내기 위한 용도라는 점을 항상 생각하고 사용해야 합니다.

<br>

## HAVING절 사용하기

- `GROUP BY`절에서 조건을 주기 위해서 `WHERE`절을 사용하면 어떤 문제가 발생할까요?
- `WHERE`절 -> `GROUP BY`절 -> `SELECT`절 -> `ORDER BY`절 순서로 실행됩니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/8125689f-4dd5-4c31-ba10-861a1b56c7b8" />

- `WHERE`절이 `GROUP BY`절 보다 먼저 평가되는 문제로 인해 `GROUP BY`절에서는 `WHERE`절을 사용할 수 없습니다.
- 이런 문제를 해결하기 위해서 `HAVING`절을 사용할 수 있습니다.
- `HAVING`절은 `GROUP BY`절 뒤에 평가되어서 각 그룹에 대한 조건을 지정할 수 있습니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/e9a8381e-5c52-42d4-be13-fbf58ebd3f59" />

```sql
SELECT
  orderNumber,
  SUM(quantityOrdered) AS item_count,
  SUM(quantityOrdered * priceEach) AS total
FROM
  orderDetails
GROUP BY
  orderNumber;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/74fdf807-ad37-4e72-9fc5-191612c558ee" />

- 이제 `HAVING`절을 사용해서 다양한 조건을 줄 수 있습니다. 그렇다면 `total` 결과가 1,000 이상인 경우는 어떻게 해야 할까요?
- 아래 예제처럼 `GROUP BY`절 뒤에 `HAVING`절을 사용해서 조건을 주면 됩니다.

```sql
SELECT
  orderNumber,
  SUM(quantityOrdered) AS item_count,
  SUM(quantityOrdered * priceEach) AS total
FROM
  orderDetails
GROUP BY
  orderNumber
HAVING
  total > 1000;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/4b01709b-9618-4a02-8f33-7eb998f42853" />

- `MySQL`은 `SELECT`절에서 지정한 `Alias`를 `GROUP BY, HAVING`절에서 사용할 수 있는 점을 이용해서 `HAVING total > 1000 SQL Query`문이 문제없이 동작합니다.

<br>

## Reference

- [MySQLTUTORIAL - MySQL GROUP BY](https://www.mysqltutorial.org/mysql-group-by.aspx)
- [young's devlog - 그룹화 GROUP BY](https://lxxjn0-dev.netlify.app/first-step-sql-lec-20)
