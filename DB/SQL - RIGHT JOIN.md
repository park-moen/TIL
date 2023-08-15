# [SQL] 내가 사용하기 위해 공부하는 RIGHT JOIN

<br>

## RIGHT JOIN절 소개

- `RIGHT JOIN`절은 `LEFT JOIN`절과 유사한 문법을 따르지만, 테이블의 위치가 반대입니다.

```sql
SELECT
  column
FROM left_table_name
RIGHT JOIN right_table_name ON
  join_condition;
```

- 테이블 위치가 반대라는 부분을 보다 명확하게 이야기해보면, `FROM`절 뒤에 오는 테이블이 왼쪽 테이블로 지정되고 `RIGHT JOIN`절 뒤에서 오는 테이블이 오른쪽 테이블로 지정된다는 의미입니다.
- 다른 `JOIN`절처럼 `JOIN` 조건을 동일한 `Column` 이름이라면 `ON`절 대신에 `USING`절로 대체할 수 있습니다.
- **RIGHT JOIN 동작 방식**
  1. 오른쪽 테이블: `RIGHT JOIN`절 뒤에 오는 테이블입니다. 오른쪽 테이블에서 선택한 `Column`(`SELECT`절에서 선택된 `Column`)의 결과는 `JOIN` 조건과 관계없이 모두 반환합니다.
  2. 왼쪽 테이블: `FROM`절 뒤에 오는 테이블입니다. 왼쪽 테이블에서 선택된 `Column`(`SELECT`절에서 선택된 `Column`) `JOIN` 조건이 `TRUE`로 평가되는 결과는 `DB`에 있는 `Data`를 반환하고 `FALSE` 평가되면 `NULL` 값을 반환합니다.
- `RIGHT JOIN`절의 벤 다이어그램은 아래 사진과 동일합니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/ecf4f16a-06cd-4386-8a4a-f780e353c412" width="30%" />

<br>

## SELECT문에서 RIGHT JOIN 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/ffda2c97-f9a5-4361-bb38-62b5dbeeefda" />

```sql
SELECT
  employeeNumber,
  customerNumber
FROM
  customers
RIGHT JOIN employees
  ON salesRepEmployeeNumber = employeeNumber
ORDER BY
  employeeNumber DESC;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/bc1d50a3-aa91-4878-9770-9939ce28a52d" />

- `customers`테이블의 `salesRepEmployeeNumber Column`은 employees 테이블의 `employeeNumber Column`에 연결됩니다.
- `employees` 테이블이 오른쪽 테이블로 `JOIN` 조건과 관계없이 선택된 `Column(employeeNumber Column)`의 `tuple` 결과를 반환합니다.
- `customers` 테이블은 왼쪽 테이블로 `JOIN` 조건이 `FALSE`로 평가된다면 선택된 `Column(customerNumber Column)`은 `NULL` 값을 반환합니다.

<br>

## Reference

- [MySQLTUTORIAL - MySQL RIGHT JOIN](https://www.mysqltutorial.org/mysql-right-join/)
