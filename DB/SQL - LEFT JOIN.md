# [SQL] 내가 사용하기 위해 공부하는 LEFT JOIN

<br>

## LEFT JOIN절 소개

- `LEFT JOIN`절은 `INNER JOIN`절과 유사한 문법을 사용합니다.
- `LEFT JOIN`절을 사용하게 된다면 왼쪽 테이블과 오른쪽 테이블 개념이 도입됩니다.
- 테이블 방향 개념(`LEFT, RIGHT`)이 도입되면서 `Query`문의 순서에 따라 다른 결과를 반환합니다.

```sql
SELECT
  column
FROM
  left_table_name
LEFT JOIN right_table_name ON
  join_condition;
```

```sql
SELECT
  column
FROM
  right_table_name
RIGHT JOIN left_table_name ON
  join_condition;
```

- `LEFT JOIN`절은 `FROM`절의 테이블을 왼쪽 테이블이고 `LEFT JOIN`절의 테이블을 오른쪽 테이블로 지정합니다.
- `LEFT JOIN`절에서 `JOIN` 조건(`ON, USING`)이 `FALSE`로 평가되는 경우 왼쪽 테이블의 `tuple`의 결과는 반환하지만 오른쪽 테이블의 `tuple`은 `NUll`값을 반환합니다.
- 즉, `LEFT JOIN`절은 `JOIN` 조건에 관계없이 왼쪽 테이블의 선택된 모든 `tuple`을 반환합니다.
- `LEFT JOIN`절에서 `NUll`값을 포함하지 않은 결과를 반환하고 싶다면 `WHERE column_name IS NULL` 조건을 사용할 수 있습니다.
- `LEFT JOIN`절의 벤 다이어그램은 아래 사진과 동일합니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/45e8fdbf-698a-4a2e-ac10-34785ee2a1a0" width="30%" />

<br>

## SELECT문에서 LEFT JOIN절 사용하기

### 2개의 테이블에서 `LEFT JOIN`절을 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/fef7150a-2099-46c4-acb6-a7acfceb6008" />

```sql
# ON절 사용
SELECT
  c.customerNumber,
  customerName,
  orderNumber,
  status
FROM
  customers c
LEFT JOIN orders o ON
  o.customerNumber = c.customerNumber;

# USING절 사용
SELECT
  customerNumber,
  customerName,
  orderNumber,
  status
FROM
  customers c
LEFT JOIN orders o USING(customerNumber);
```

<img src="https://github.com/park-moen/TIL/assets/57402711/637fff08-3b41-4227-86db-d4fa64cbee42" />

- 위 예제는 `customers` 테이블은 왼쪽 테이블이고 `orders` 테이블은 오른쪽 테이블로 지정됩니다.
- `ON o.customerNumber = c.customerNumber` 조건이 `FALSE`로 평가되면 `orders` 테이블의 `tuple(orderNumber, status)`운 `NULL` 값을 반환합니다.
- `JOIN` 조건인 `ON`절을 `USING`절로 바꿔서 사용할 수 있습니다.

### 일치하지 않는 결과 반환하기(EXCLUSIVE JOIN)

- `LEFT/RIGHT JOIN`절에서 다른 테이블에서 일치하지 않는 행을 찾으려는 경우에 매우 유용합니다.
- 아래 그림처럼 결과를 반환하기 위해서는 `IS NULL` 연산자를 사용해서 원하는 결과를 반환할 수 있습니다.

<img src="https://github.com/park-moen/TIL/assets/57402711/f53f6847-6a70-406d-bf0c-ed7633cf314e" width="30%" />
<br>
<img src="https://github.com/park-moen/TIL/assets/57402711/500d328a-fefc-447d-aa6e-9466d25ba5dc" width="30%" />

```sql
SELECT
  customerNumber,
  customerName,
  orderNumber,
  status
FROM
  customers c
LEFT JOIN orders o USING(customerNumber)
WHERE
  orderNumber IS NULL;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/e73caed5-2035-4c4b-91a2-8af7678d342b">

### 3개의 테이블에서 `LEFT JOIN`절을 사용하기

<img src="https://github.com/park-moen/TIL/assets/57402711/25a1bae4-f264-4ab8-819b-cd0188238e17" />

```sql
SELECT
	firstName,
    lastName,
    customerName,
	checkNumber,
    amount
FROM
	employees
LEFT JOIN customers ON
	employeeNumber = salesRepEmployeeNumber
LEFT JOIN payments ON
	payments.customerNumber = customers.customerNumber;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/6a07d30c-0252-426b-a8e2-678a19772fb8" />

**3개의 테이블에서 LEFT JOIN이 동작하는 순서는 아래와 같습니다.**

1. `employees` 테이블은 왼쪽 테이블이고 `customers` 테이블이 오른쪽 테이블입니다.
2. 만약 `employeeNumber = salesRepEmployeeNumber` `JOIN` 조건이 `FALSE`로 평가되는 경우 `customerName`은 `NULL` 값을 반환합니다.
3. 첫 `LEFT JOIN`절이 끝나면 `customers` 테이블과 `payments` 테이블의 `LEFT JOIN`절이 시작됩니다.
4. `customers` 테이블이 왼쪽 테이블이고 `payments` 테이블이 오른쪽 테이블입니다.
5. 만약 `payments.customerNumber = customers.customerNumber` `JOIN` 조건이 `FALSE`로 평가되는 경우 `checkNumber, amount`은 `NULL` 값을 반환합니다.

<br>

## ON절과 WHERE절 조건 순서 차이

- `LEFT JOIN`절을 사용하는 과정에서 다양한 조건을 통해 원하는 결과를 반환할 수 있습니다.
- 이 과정에서 `ON`절에서 논리 연산자를 통해 필터링을 진행할 수 있습니다. 또한, `WHERE`절을 사용해서 필터링을 진행할 수 있습니다.

```sql
SELECT
  o.orderNumber,
  customerNumber,
  productCode
FROM
  orders o
LEFT JOIN orderDetails
  USING (orderNumber)
WHERE
  orderNumber = 10123;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/f3ccc0f8-f2f2-4cc2-a0ca-ee4cfeaa722b" />

```sql
SELECT
  o.orderNumber,
  customerNumber,
  productCode
FROM
  orders o
LEFT JOIN orderDetails d
  ON o.orderNumber = d.orderNumber AND
     o.orderNumber = 10123;
```

<img src="https://github.com/park-moen/TIL/assets/57402711/83089368-fcca-4b60-be3a-528b0a1aba7b" />

- 위 두 `SQL Query`문을 통해 동일한 결과를 반환한다고 예측했지만 반환하는 결과는 다르게 나와서 당황할 수 있습니다.
- 이런 결과가 나오는 이유는 `ON`절과 `WHERE`절의 조건이 실행되는 순서가 달라서입니다.
- 위의 첫번째 예제처럼 `ON`절에서 비교 연산자 `=`과 논리 연산자 `AND`를 함께 사용하게 된다면 `LEFT JOIN`을 실행과 함께 연산자가 평가되어서 `AND`연산자 조건에 일치하지 않는 경우 `NULL` 값을 반환합니다.
- `LEFT JOIN`절에서 필터링하기 위해 `WHERE`절을 사용하는 경우에는 `LEFT JOIN`이 실행된 후에 `WHERE`절 조건과 일치하는 결과만을 반환합니다.

<br>

## Reference

- [MySQLTUTORIAL - MySQL LEFT JOIN](https://www.mysqltutorial.org/mysql-left-join.aspx)
- [[SQL] JOIN ON 절과 WHERE 절 차이](https://okimaru.tistory.com/251)
