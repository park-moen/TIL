# [SQL] 내가 사용하기 위해 공부하는 SELECT

> 모든 SQL TIL은 [MySQLTUTORIAL](https://www.mysqltutorial.org/mysql-sample-database.aspx) 사이트에서 예제 데이터를 받아서 사용했습니다.

<br>

### SELECT문 소개

- `SELECT`문는 선택한 테이블에서 데이터를 추출하기 위한 `SQL`의 데이터 조작 언어(DML) 중 하나입니다.
- `SELECT keyword(예약어)` 뒤에 하나 이상의 `Column(attribute, field)`을 지정할 수 있습니다.
- 복수의 `Column`을 지정하는 경우 `,`로 구분해야 합니다.
- `Column`을 선택한 후에는 `FROM keyword`를 사용해서 `Table`을 선택할 수 있습니다.

```sql
SELECT column FROM table_name;
```

<br>

### 테이블에서 모든 데이터 추출

```sql
SELECT * FROM table_name;
```

```sql
SELECT * FROM employees;
```

| employeeNumber | lastName  | firstName | extension | email                          | officeCode | reportsTo | jobTitle     |
| -------------- | --------- | --------- | --------- | ------------------------------ | ---------- | --------- | ------------ |
| 1002           | Murphy    | Diane     | x5800     | dmurphy@classicmodelcars.com   | 1          | NULL      | President    |
| 1056           | Patterson | Mary      | x4611     | mpatterso@classicmodelcars.com | 1          | 1002      | VP Sales     |
| 1076           | Firrelli  | Jeff      | x9273     | jfirrelli@classicmodelcars.com | 1          | 1002      | VP Marketing |

<br>

### 특정 Column 데이터 추출

```sql
SELECT column1, column2, column... FROM table_name;
```

```sql
SELECT FirstName ,LastName FROM employees;
```

```sql
SELECT firstName ,lastName FROM employees;
```

SQL은 대소문자를 구분하지 않아서 위의 두 예제는 동일한 아래 결과를 반환합니다.

| firstName | LastName  |
| --------- | --------- |
| Diane     | Murphy    |
| Mary      | Patterson |
| Jeff      | Firrelli  |

<br>

### ALL / DISTINCT절을 사용해서 데이터 추출

- `ALL`: 테이블에 동일한 데이터가 있는 경우에도 모든 데이터를 반환합니다. 기본으로 `ALL`이 선택되어서 생략이 가능합니다.
- `DISTINCT(UNIQUE)`: 테이블에 동일한 데이터가 있는 경우 중복을 제거한 1개만을 반환합니다.
- `NULL`값이 포함된 `Column`에 `DISTINCT`를 사용하면 중복을 제거한 하나의 `NULL`값을 반환합니다.
- `DISTINCT`의 실행 순서는 `FROM`, `WHERE`이 끝난 후에 `DISTINCT`가 실행되면서 중복을 제거 후에 `ORDER BY`로 정렬합니다.

![DISTINCT Order](https://github.com/park-moen/TIL/assets/57402711/8f1245ea-db22-4d02-a9cf-0726df9fdc94)

```sql
SELECT lastName FROM employees;
```

| lastName |
| -------- |
| Bondur   |
| Bondur   |
| Bott     |
| Bow      |
| Castillo |
| Firrelli |
| Firrelli |

```sql
SELECT DISTINCT lastName FROM employees;
```

| lastName |
| -------- |
| Bondur   |
| Bott     |
| Bow      |
| Castillo |
| Firrelli |

<br>

### 여러 개의 Column에서 DISTINCT절 사용하기

- 여러 개의 `Column` 추출하는 과정에서 `DISTINCT`를 사용하게 된다면서 선택된 모든 `Column`을 고려해서 중복을 제거합니다.
- 중복이 발생하지 않는 `Column`에서 사용하면 불필요한 리소스가 발생해서 성능적으로 문제가 발생할 수 있습니다.
- `DISTINCT`는 원하는 일부만 선택해서 중복을 제거하지 않고 모든 `Column`을 고려해서 중복을 제거하기 때문에 속도가 느려지는 원인이 될 수 있습니다.

```sql
SELECT
  extension, jobTitle
FROM
  employees
ORDER BY
  extension;
```

| extension | jobTitle  |
| --------- | --------- |
| x101      | Sales Rep |
| x101      | Sales Rep |
| x102      | Sales Rep |
| x102      | Sales Rep |
| x102      | Sales Rep |
| x103      | Sales Rep |

```sql
SELECT DISTINCT
  extension, jobTitle
FROM
  employees
ORDER BY
  extension;
```

| extension | jobTitle  |
| --------- | --------- |
| x101      | Sales Rep |
| x102      | Sales Rep |
| x103      | Sales Rep |

<br>

### LIMIT(TOP)절로 데이터 개수 지정

- `LIMIT`절은 반환할 `Tuples(rows)`의 개수를 제한하기 위해 `SELECT`문에서 사용됩니다.
- `MYSQL`에서는 `TOP` 문법을 지원하지 않고 `LIMIT` 문법을 사용해서 개수를 제한합니다. `MSSQL`과 같은 `RDBMS`에서는 TOP 문법을 사용할 수 있습니다.
- 하나 또는 두개의 `arguments`를 지정할 수 있습니다. `arguments`는 0 이상의 숫자 타입의 값을 지정해야 합니다.

```sql
# MYSQL 문법
SELECT
  column...
FROM
  table_name
LIMIT offset, row_count

# MSSQL
SELECT
  TOP (expression) Column...
FROM
  table_name
```

- `offset`은 반환할 첫 번째 `tuple`을 지정합니다. `offset`은 1이 아닌 0이 기준입니다.
- `SELECT`문에서 `LIMIT`을 사용하면 추가된 데이터 순서에 따라서 예측할 수 없는 결과를 반합합니다. 이런 문제를 해결하기 위해서는 `ORDER BY`로 정렬을 해서 예측할 수 있는 결과를 반환해야 합니다.

```sql
SELECT
  column...
FROM
  table_name
ORDER BY
  sort_expression
LIMIT offset, row_count;
```

```sql
SELECT
  customerNumber, customerName, creditLimit
FROM
  customers
ORDER BY creditLimit
LIMIT 5;
```

| customerNumber | customerName               | creditLimit |
| -------------- | -------------------------- | ----------- |
| 223            | Natürlich Autos            | 0.00        |
| 168            | American Souvenirs Inc     | 0.00        |
| 169            | Porto Imports Co.          | 0.00        |
| 206            | Asian Shopping Network, Co | 0.00        |
| 125            | Havel & Zbyszek Co         | 0.00        |

- 위의 예시에 발생한 문제를 해결하려면 `ORDER BY`절에 `Column`을 추가해야 합니다.

```sql
SELECT
  customerNumber,
  customerName,
  creditLimit
FROM
  customers
ORDER BY
  creditLimit,
  customerNumber
LIMIT 5;
```

| customerNumber | customerName               | creditLimit |
| -------------- | -------------------------- | ----------- |
| 125            | Havel & Zbyszek Co         | 0.00        |
| 168            | American Souvenirs Inc     | 0.00        |
| 169            | Porto Imports Co.          | 0.00        |
| 206            | Asian Shopping Network, Co | 0.00        |
| 223            | Natürlich Autos            | 0.00        |

<br>

### Reference

- [MySQLTUTORIAL- MySQL SELECT](https://www.mysqltutorial.org/mysql-select-statement-query-data.aspx)
- [MySQLTUTORIAL- MySQL DISTINCT](https://www.mysqltutorial.org/mysql-distinct.aspx)
- [MySQLTUTORIAL- MySQL LIMIT](https://www.mysqltutorial.org/mysql-limit.aspx)
- [wikipedia - Select(SQL)](<https://ko.wikipedia.org/wiki/Select_(SQL)>)
