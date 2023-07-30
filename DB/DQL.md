# [SQL] 내가 사용하기 위해 공부하는 SELECT

> [MySQLTUTORIAL](https://www.mysqltutorial.org/mysql-sample-database.aspx)로 간단한 예제 데이터를 받아서 사용했습니다.

SELECT는 선택한 테이블에서 데이터를 추출하기 위한 SQL의 데이터 조작 언어(DML) 중 하나입니다.

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

### 특정 Column 데이터 추출

```sql
SELECT column1, column2, Column... FROM table_name;
```

```sql
SELECT FirstName ,LastName FROM employees;
```

```sql
SELECT firstName ,lastName FROM employees;
```

SQL은 따로 대소문자를 구분하지 않아서 위의 두 예제는 동일한 아래 결과를 반환합니다.

| firstName | LastName  |
| --------- | --------- |
| Diane     | Murphy    |
| Mary      | Patterson |
| Jeff      | Firrelli  |

### ALL / DISTINCT 지정 데이터 추출

### WHERE절을 사용해서 데이터 추출
