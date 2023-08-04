# [SQL] 내가 사용하기 위해 공부하는 ORDER BY

<br>

## ORDER BY절 소개

- `SELECT`문을 사용하여 원하는 `Table`에서 정보를 가져오게 된다면 예상하지 못한(처음 정보를 추가한 순서) 순서로 정보를 가져옵니다. 하지만 실제 정보를 `Client`에서 사용하고 관리하기 위해서는 예상 가능한 순서의 정보를 가져와야 합니다.
- **정렬된 정보(예상 가능한)** 를 가져오기 위해서는 `ORDER BY`절을 추가할 수 있습니다.

```sql
SELECT
  column
FROM
  table_name
ORDER BY
  column1 [ASC|DESC],
  column2 [ASC|DESC],
  columns...
```

<br>

## ORDER BY절 정렬 기준

- `ORDER BY`절에 하나 이상의 `Column`을 선택할 수 있습니다. `ORDER BY`절에서 정렬 기준은 `ASC`(오름차순), `DESC`(내림차순)으로 선택할 수 있습니다.
- 정렬 기준의 기본값은 `ASC`이며, 정렬 기준`(ASC, DESC)`를 생략하게 된다면 `ASC로` 정렬을 합니다.

```sql
# 오름차순으로 정렬을 진행합니다.
SELECT
  column
FROM
  table_name
ORDER BY column ASC;

# 위의 SELECT문과 동일하게 동작합니다.
SELECT
  column
FROM
  table_name
ORDER BY column;
```

```sql
# 내림차순으로 정렬합니다.
SELECT
  column
FROM
  table_name
ORDER BY column DESC;
```

<br>

## ORDER BY절 다중 정렬

- 다중 정렬을 `ORDER BY`절에서 사용하려면 콤마`(,)`로 구분된 `Column` 목록을 지정해야 합니다. 다중 정렬이 실행되는 순서는 `Column`이 지정된 순서를 따릅니다.
- `SELECT`문에서 `ORDER BY`절이 평가되는 순서는 `FROM`절 및 `SELECT`절 다음에 평가됩니다.
  <image src="https://github.com/park-moen/TIL/assets/57402711/376fa33e-6044-4a83-9cae-d574a22c3a79" width="60%" />

```sql
SELECT
  contactFirstName,
  contactLastName
FROM
  customers
ORDER BY
  contactLastName DESC,
  contactFirstName ASC;
```

| contactFirstName | contactLastName |
| ---------------- | --------------- |
| Dorothy          | Young           |
| Jeff             | Young           |
| Julie            | Young           |
| Mary             | Young           |
| Juri             | Yoshido         |
| Brydey           | Walker          |
| Wendy            | Victorino       |
| Braun            | Urs             |
| Jerry            | Tseng           |

- 위 예제에서 다중 정렬이 진행되는 순서는 `contactLastName Column`이 `DESC`기준으로 먼저 정렬이 되고 `contactFirstName Column`이 `ASC`기준으로 정렬되어 결과를 반환합니다.

<br>

## ORDER BY절에서 표현식 사용하기

- `ORDER BY`절에서 다양한 정보를 표현식을 사용해서 정렬할 수 있습니다.
- 표현식을 사용해서 정렬을 하는 과정은 먼저 표현식을 평가한 후에 평가된 결과를 기준으로 정렬을 진행합니다.

```sql
SELECT
  orderNumber,
  productCode,
  orderLineNumber,
  quantityOrdered * priceEach
FROM
  orderDetails
ORDER BY
  quantityOrdered * priceEach DESC;
```

| orderNumber | productCode | orderLineNumber | quantityOrdered \* priceEach |
| ----------- | ----------- | --------------- | ---------------------------- |
| 10403       | S10_4698    | 9               | 11503.14                     |
| 10405       | S12_4675    | 5               | 11170.52                     |
| 10407       | S18_1749    | 2               | 10723.60                     |
| 10404       | S12_1099    | 3               | 10460.16                     |
| 10312       | S10_1949    | 3               | 10286.40                     |

- `ORDER BY`절에서 표현식을 사용하는 과정에서 가독성을 높이기 위해서 `Alias`를 사용할 수 있습니다.
- `ORDER BY`절에서 `Alias`를 사용할 수 있는 이유는 `SELECT`절이 `ORDER BY`절보다 우선적으로 평가되어서 `ORDER BY`절에서 `Alias`를 사용할 수 있습니다.

```sql
SELECT
  orderNumber,
  productCode,
  orderLineNumber,
  quantityOrdered * priceEach AS subtotal
FROM
  orderDetails
ORDER BY subtotal DESC;
```

| orderNumber | productCode | orderLineNumber | subtotal |
| ----------- | ----------- | --------------- | -------- |
| 10403       | S10_4698    | 9               | 11503.14 |
| 10405       | S12_4675    | 5               | 11170.52 |
| 10407       | S18_1749    | 2               | 10723.60 |
| 10404       | S12_1099    | 3               | 10460.16 |
| 10312       | S10_1949    | 3               | 10286.40 |

<br>

## ORDER BY절에서 특정 값 우선 정렬하기

#### FIELD() 함수 소개

- `FIELD()` 함수를 사용해서 `ORDER BY`절에서 특정 값을 기준으로 정렬을 할 수 있습니다.

```sql
SELECT FIELD(value, value1, value2, ...)
```

- `FIELD()` 함수는 `value1, value2, ...` 목록에서 `value`의 위치를 반환합니다. `value` 이 목록에 존재하지 않으면 `FIELD()` 함수는 0을 반환합니다.

```sql
SELECT FIELD('B', 'A','B','C');
```

| FIELD('B', 'A','B','C') |
| ----------------------- |
| 2                       |

#### WHERE절에서 사용하기

```sql
SELECT
  orderNumber, status
FROM
  orders
ORDER BY FIELD(status,
    'In Process',
    'On Hold',
    'Cancelled',
    'Resolved',
    'Disputed',
    'Shipped');
```

| orderNumber | status     |
| ----------- | ---------- |
| 10422       | In Process |
| 10425       | In Process |
| 10401       | On Hold    |
| 10253       | Cancelled  |
| 10260       | Cancelled  |
| 10248       | Cancelled  |
| 10327       | Resolved   |
| 10406       | Disputed   |
| 10104       | Shipped    |

<!-- - 여기부터 작성하기

<br>

## ORDER BY절을 사용하면서 주의 사항

#### ORDER BY절과 NULL

#### 문자열 타입을 정렬하는 기준 -->

<br>

## Reference

- [MySQLTUTORIAL - MySQL ORDER BY](https://www.mysqltutorial.org/mysql-order-by/)
- [young's devlog - 9. 정렬 - ORDER BY](https://lxxjn0-dev.netlify.app/first-step-sql-lec-09)
