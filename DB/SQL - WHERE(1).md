# [SQL] 내가 사용하기 위해 공부하는 WHERE(1)

<br>

## WHERE절 소개

- `SELECT`문의 기본 문법을 사용해서 정보를 가져오게 된다면 선택된 `Column`에 대한 모든 정보를 전부 가져옵니다.
- 하지만 원하는 정보만을 가져오고 싶은 경우에는 `SELECT`문의 기본 문법으로는 불가능합니다. 이런 문제를 해결하기 위해서 `WHERE`절을 사용할 수 있습니다.
- `WHERE`절을 통해 단일 또는 복수의 검색 조건을 지정하여 `Tuples(rows, recode)`을 봔환할 수 있습니다.

```sql
SELECT column FROM table_name WHERE search_condition;
```

- `search_condition` 조건에는 집합 함수(`aggregate function`)를 제외한 `SQL`에서 지원하는 모든 함수, 연산자를 사용할 수 있습니다.
- `search_condition` 조건이 `true`로 평가되는 `tuple(row, record)`만을 조회합니다.
- `SELECT`문 이외에도 `UPDATE`, `DELETE`문에서 `WHERE`절을 사용할 수 있습니다.
- 아래의 사진처럼 `WHERE`절이 평가되어 실행되는 순서는 `SELECT`, `ORDER BY`절 전과 `FROM`절 뒤에 실행됩니다.
- 이런 이유로 `Alias`를 `WHERE`절에서 사용할 수 없다는 점을 주의해야 합니다.

![WHERE Clause order](https://github.com/park-moen/TIL/assets/57402711/e2a60a78-ef05-4f58-a428-ad26c68ed260)

<br>

## WHERE절에서 비교 연산자 사용하기

<table>
  <tr>
    <th>연산자</th>
    <th>내용</th>
  </tr>
  <tr>
    <td>=</td>
    <td>같다.</td>
  </tr>
  <tr>
    <td>!=, <></td>
    <td>
      <span>같지 않다.</span>
      <span style="font-weight: bold"><> 연산자는 ISO 표준으로 모든 운영체제에서 사용 가능</span>
    </td>
  </tr>
  <tr>
    <td>></td>
    <td>보다 크다.</td>
  </tr>
  <tr>
    <td>>=</td>
    <td>보다 크거나 같다.</td>
  </tr>
  <tr>
    <td><</td>
    <td>보다 작다.</td>
  </tr>
  <tr>
    <td><=</td>
    <td>보다 작거나 같다.</td>
  </tr>
</table>

#### WHERE절에 `=` 연산자 사용

```sql
SELECT
  firstname,
  lastname,
  jobtitle
FROM
  employees
WHERE
  jobtitle = 'Sales Rep';
```

| firstname | lastname  | jobtitle  |
| --------- | --------- | --------- |
| Leslie    | Jennings  | Sales Rep |
| Leslie    | Thompson  | Sales Rep |
| Julie     | Firrelli  | Sales Rep |
| Steve     | Patterson | Sales Rep |

- `=` 연산자를 사용해서 조건과 일치하는 값을 가져올 수 있습니다.
- `=`을 평가하는 과정에서 모든 `Tuple(row, record)`을 검사하고 `WHERE = search_condition` 조건에 부합하는 값을 반환합니다.

#### WHERE절에 `<>` 연산자 사용

```sql
SELECT
  customerName,
  state,
FROM
  customers
WHERE
  state <> 'Sales Rep';
```

- `<>` 연산자를 사용해서 `SELECT`문을 조회하면 `NULL`값도 `WHERE <>` 조건과 같이 분류되어서 `tuple(row, record)`에 조회되지 않습니다.
- `NULL` 값만을 조회하고 싶은 경우에는 `IS NULL` 연산자를 사용해서 조회할 수 있습니다.`IS NULL` 연사자에 관련된 글은 [내가 사용하기 위해 공부하는 WHERE(2)]()에서 확인할 수 있습니다.

| customerName                 | state    |
| ---------------------------- | -------- |
| Australian Collectors, Co.   | Victoria |
| Mini Gifts Distributors Ltd. | CA       |
| Mini Wheels Co.              | CA       |
| Land of Toys Inc.            | NY       |
| Muscle Machine Inc           | NY       |

#### WHERE절에 `>` 연산자 사용

- `WHERE`절에서 비교 연산자를 사용하게 된다면 왼쪽 피연산자(Column)에서 오른쪽 피연산자 조건을 만족하는 결과를 봔환합니다.
- 아래 예제에서 `offocecode Column` 값이 5를 초과하는 조건을 만족하는 값을 반환합니다.

```sql
SELECT
  firstname,
  lastname,
  officeCode
FROM
  employees
WHERE
  officecode > 5;
```

| firstname | lastname  | officeCode |
| --------- | --------- | ---------- |
| William   | Patterson | 6          |
| Larry     | Bott      | 7          |
| Barry     | Jones     | 7          |
| Andy      | Fixter    | 6          |
| Peter     | Marsh     | 6          |
| Tom       | King      | 6          |

#### WHERE절에 `<=` 연산자 사용

- 아래 예제에서 `offocecode Column` 값이 2 또는 2보다 작은 조건을 만족하는 값을 반환합니다.

```sql
SELECT
  firstname,
  lastname,
  officeCode
FROM
  employees
WHERE
  officecode <= 2;
```

| firstname | lastname  | officeCode |
| --------- | --------- | ---------- |
| Diane     | Murphy    | 1          |
| Mary      | Patterson | 1          |
| Jeff      | Firrelli  | 1          |
| Anthony   | Bow       | 1          |
| Leslie    | Jennings  | 1          |
| Leslie    | Thompson  | 1          |
| Julie     | Firrelli  | 2          |
| Steve     | Patterson | 2          |

<br>

## WHERE절에서 논리 연산자 사용하기

<style>
td{
    text-align: center
}
</style>
<table>
  <tr>
    <th>연산자</th>
    <th>내용</th>
  </tr>
  <tr>
    <td>AND, &&</td>
    <td>
      AND 연산자를 기준으로 왼쪽 조건과 오른쪽 조건이 모두 true인 경우. 즉, 모든 조건을 만족하는 결과를 반환합니다.
    </td>
  </tr>
  <tr>
    <td>OR, ||</td>
    <td>
      OR 연산자를 기준으로 왼쪽 조건이 true이거나 오른쪽 조건이 true인 경우, 즉, 둘 중 하나의 조건만 만족하는 결과를 반환합니다.
    </td>
  </tr>
  <tr>
    <td>NOT, !</td>
    <td>조건에 반대되는 결과를 반환합니다. 즉, 조건이 FALSE 경우 결과를 반환합니다.</td>
  </tr>
</table>

- 논리 연산자를 사용해서 다양한 조건을 조합해서 사용할 수 있습니다. 논리 연산자는 개별적으로 사용할 수 있지만 `BETWEEN`연사자, `IN`연산자와 같은 다른 연산자랑 결합해서 사용할 수 있습니다.

#### WHERE절에 AND 연산자 사용

- 아래 예제에서 `jobTitle`이 `Sales Rep`으면서 `officeCode`가 1인 결과를 반환합니다. 즉 `jobTitle`의 조건 `Sales Rep`의 연산 결과가 `true`면서 `officeCode`의 조건 1의 연산 결과도 `true`인 `tuple(row, record)`을 반환합니다.

```sql
SELECT
  firstName,
  lastName,
  jobTitle,
  officeCode
FROM
  employees
WHERE
  jobtitle = 'Sales Rep' AND
  officeCode = 1;
```

| firstName | lastName | jobTitle  | officeCode |
| --------- | -------- | --------- | ---------- |
| Leslie    | Jennings | Sales Rep | 1          |
| Leslie    | Thompson | Sales Rep | 1          |

#### WHERE절에 OR 연산자 사용

- 아래 예제에서 `jobTitle`이 `Sales Rep`이거나 `officeCode`가 1인 결과를 반환합니다.
- `ORDER BY` 연산자를 사용하지 않으면 기존 테이블에 추가된 순서로 결과를 반환합니다.

```sql
SELECT
  firstName,
  lastName,
  jobTitle,
  officeCode
FROM
  employees
WHERE
  jobTitle = 'Sales Rep' OR
  officeCode = 1
```

| lastName  | firstName | jobTitle  | officeCode |
| --------- | --------- | --------- | ---------- |
| Murphy    | Diane     | President | 1          |
| Patterson | Mary      | VP Sales  | 1          |
| Firrelli  | Julie     | Sales Rep | 2          |
| Patterson | Steve     | Sales Rep | 2          |
| Tseng     | Foon Yue  | Sales Rep | 3          |
| Vanauf    | George    | Sales Rep | 3          |
| Bondur    | Loui      | Sales Rep | 4          |
| Bott      | Larry     | Sales Rep | 7          |
| Fixter    | Andy      | Sales Rep | 6          |
| Marsh     | Peter     | Sales Rep | 6          |
| Nishi     | Mami      | Sales Rep | 5          |
| Kato      | Yoshimi   | Sales Rep | 5          |
| Gerard    | Martin    | Sales Rep | 4          |

> NOTE: `AND`, `OR` 연산자 사용시 주의 사항으로는 동일한 `Column`에서 다른 조건을 사용하고 싶은 경우 각 조건마다 `Column`을 작성해줘야 합니다.

```sql
# 올바르지 못한 Query문
SELECT
  firstName, lastName, jobTitle, officeCode
FROM
  employees
WHERE
  officeCode = 1 OR 2;
```

| firstName | lastName  | jobTitle             | officeCode |
| --------- | --------- | -------------------- | ---------- |
| Diane     | Murphy    | President            | 1          |
| Mary      | Patterson | VP Sales             | 1          |
| Jeff      | Firrelli  | VP Marketing         | 1          |
| William   | Patterson | Sales Manager (APAC) | 6          |
| Gerard    | Bondur    | Sale Manager (EMEA)  | 4          |
| Anthony   | Bow       | Sales Manager (NA)   | 1          |
| Leslie    | Jennings  | Sales Rep            | 1          |
| Leslie    | Thompson  | Sales Rep            | 1          |
| Julie     | Firrelli  | Sales Rep            | 2          |
| Loui      | Bondur    | Sales Rep            | 4          |
| Gerard    | Hernandez | Sales Rep            | 4          |
| Pamela    | Castillo  | Sales Rep            | 4          |
| Larry     | Bott      | Sales Rep            | 7          |
| Barry     | Jones     | Sales Rep            | 7          |
| Andy      | Fixter    | Sales Rep            | 6          |

- 만약 `OR` 연산자에서는 `officeCode`가 1 또는 2인 경우의 결과를 반환하고 싶었지만 조건을 무시하고 모든 결과를 반환하는 문제가 발생합니다.

```sql
# 올바른 Query문
SELECT
  firstName, lastName, jobTitle, officeCode
FROM
  employees
WHERE
  officeCode = 1 OR
  officeCode = 2;
```

| firstName | lastName  | jobTitle           | officeCode |
| --------- | --------- | ------------------ | ---------- |
| Diane     | Murphy    | President          | 1          |
| Mary      | Patterson | VP Sales           | 1          |
| Jeff      | Firrelli  | VP Marketing       | 1          |
| Anthony   | Bow       | Sales Manager (NA) | 1          |
| Leslie    | Jennings  | Sales Rep          | 1          |
| Leslie    | Thompson  | Sales Rep          | 1          |
| Julie     | Firrelli  | Sales Rep          | 2          |
| Steve     | Patterson | Sales Rep          | 2          |

- `OR` 연산자에서는 `officeCode`가 1 또는 2인 경우의 결과를 반환하고 싶다면 동일한 `Column`이라도 조건마다 `Column`을 따로 작성해줘야 합니다.
- `AND` 연산자에서 조건마다 `Column`을 작성하지 않을 경우에는 첫 조건만을 만족하는 결과를 반환합니다.

```sql
# 올바르지 못한 Query
SELECT
  firstName, lastName, jobTitle, officeCode
FROM
  employees
WHERE
  officeCode = 1 AND
  officeCode = 2;
```

| firstName | lastName  | jobTitle           | officeCode |
| --------- | --------- | ------------------ | ---------- |
| Diane     | Murphy    | President          | 1          |
| Mary      | Patterson | VP Sales           | 1          |
| Jeff      | Firrelli  | VP Marketing       | 1          |
| Anthony   | Bow       | Sales Manager (NA) | 1          |
| Leslie    | Jennings  | Sales Rep          | 1          |
| Leslie    | Thompson  | Sales Rep          | 1          |

#### WHERE절에 NOT 연산자 사용

- 아래 예제에서 `jobTitle`이 `Sales Rep`이 아닌 결과룰 반환합니다. 즉, `JobTitle = Sales Rep`이 `false`입니다.
- `NOT` 연산자는 `WHERE`절과 선택한 `Column`의 사이에 위치해야 합니다.

```sql
SELECT
  lastName, firstName, jobTitle, officeCode
FROM
  employees
WHERE
  NOT jobTItle = "Sales Rep";
```

| lastName  | firstName | jobTitle             | officeCode |
| --------- | --------- | -------------------- | ---------- |
| Murphy    | Diane     | President            | 1          |
| Patterson | Mary      | VP Sales             | 1          |
| Firrelli  | Jeff      | VP Marketing         | 1          |
| Patterson | William   | Sales Manager (APAC) | 6          |
| Bondur    | Gerard    | Sale Manager (EMEA)  | 4          |
| Bow       | Anthony   | Sales Manager (NA)   | 1          |

#### 논리 연산자 조합

- `WHERE`절에 다양한 조건을 주기 위해 `AND, OR, NOT`을 조합할 수 있습니다.
- 논리 연산자 우선순위를 정확히 지정하지 않으면 예측할 수 없는 결과를 반환합니다.
- 이런 문제를 해결하기 위해서 우선순위가 가장 높은 `()`를 사용해서 예측할 수 있는 결과를 반환받는 방법을 선택할 수 있습니다.

| 우선순위 | 연산자                                                              |
| -------- | ------------------------------------------------------------------- |
| 1        | 괄호( )                                                             |
| 2        | NOT 연산자                                                          |
| 3        | 비교 연산자, BETWEEN 연산자, IN 연산자, Like 연산자, IS NULL 연산자 |
| 4        | AND 연산자                                                          |
| 5        | OR 연산자                                                           |

```sql
SELECT
  firstName, lastName, jobTitle, officeCode
FROM
  employees
WHERE
  jobTitle = "Sales Rep" AND
  officeCode = 1 OR
  officeCode = 4;
```

| firstname | lastname  | jobtitle            | officeCode |
| --------- | --------- | ------------------- | ---------- |
| Gerard    | Bondur    | Sale Manager (EMEA) | 4          |
| Leslie    | Jennings  | Sales Rep           | 1          |
| Leslie    | Thompson  | Sales Rep           | 1          |
| Loui      | Bondur    | Sales Rep           | 4          |
| Gerard    | Hernandez | Sales Rep           | 4          |
| Pamela    | Castillo  | Sales Rep           | 4          |
| Martin    | Gerard    | Sales Rep           | 4          |

- **원하는 결과**: `jobTitle`이 `Sales Rep`이면서 `officeCode`가 1이거나 `jobTitle`이 `Sales Rep`이면서 `officeCode`가 2인 `tuple(row, record)`
- **실제 반환하는 결과**: `jobTitle`이 `Sales Rep`이면서 `officeCode`가 1이거나 `officeCode`가 2인 `tuple(row, record)`
- **원인**: `AND` 연산자 우선순위가 `OR` 연산자 우선순위보다 높아서 실제 `Query`는 `WHERE (jobTitle = "Sales Rep" AND officeCode = 1) OR officeCode = 4;`처럼 동작해서 원하는 결과를 받아오지 못했습니다.

```sql
SELECT
  firstName, lastName, jobTitle, officeCode
FROM
  employees
WHERE
  jobTitle = "Sales Rep" AND
  (officeCode = 1 OR officeCode = 4);
```

| firstName | lastName  | jobTitle  | officeCode |
| --------- | --------- | --------- | ---------- |
| Leslie    | Jennings  | Sales Rep | 1          |
| Leslie    | Thompson  | Sales Rep | 1          |
| Loui      | Bondur    | Sales Rep | 4          |
| Gerard    | Hernandez | Sales Rep | 4          |
| Pamela    | Castillo  | Sales Rep | 4          |
| Martin    | Gerard    | Sales Rep | 4          |

- `OR` 연산자의 우선순위의 `AND` 연산자 우선순위보다 높이기 위한 해결책으로는 우선순위가 가장 높은 괄호`()`를 사용할 수 있습니다.

<br>

## Reference

- [MySQLTUTORIAL - MySQL WHERE](https://www.mysqltutorial.org/mysql-where/)
- [DATA ON AIR - WHERE절](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&kboard_search_option%5Btree_category_1%5D%5Bkey%5D=tree_category_1&kboard_search_option%5Btree_category_1%5D%5Bvalue%5D=SQL+%EA%B8%B0%EB%B3%B8+%EB%B0%8F+%ED%99%9C%EC%9A%A9&uid=341)
- [tutorialspoint - WHERE Clause](https://www.tutorialspoint.com/mysql/mysql-where-clause.htm)
- [young's develog - 조건 조합하기](https://lxxjn0-dev.netlify.app/first-step-sql-lec-06)
