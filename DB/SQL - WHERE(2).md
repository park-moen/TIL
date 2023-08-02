# [SQL] 내가 사용하기 위해 공부하는 WHERE(2)

`WHERE`절에서 논리 연산자, 비교 연산자만을 사용해서 다양한 정보를 가져올 수 있습니다. 하지만 두 연산자만(비교, 논리)을 사용하면 제한적이면서 불필요한 `SQL Query`를 사용할 수 있는 문제가 있습니다. 이런 문제를 해결하기 위해 `SQL`에서 지원하는 다양한 연산자를 소개하겠습니다.

<br>

## WHERE절에서 BETWEEN 연산자 사용하기

#### BETWEEN 연산자 소개

- `BETWEEN` 연산자는 값의 범위를 지정할 수 있는 연산자입니다. 기본적인 문법은 논리 연산자와 함께 사용해서 범위를 지정할 수 있습니다.

```sql
value BETWEEN low AND high;
```

- 위 예제의 `BETWEEN A AND B` 연산자가 동작하는 방법은 `value`를 기준으로 `value >= low AND value <= high`를 `SQL`이 내부적으로 확인 후 조건을 만족하면 `1(true)`을 반환하고 조건을 만족하지 못하면 `0(false)`를 반환합니다.

```sql
# 실제  BETWEEN A AND B 연산자가 범위를 지정할때 진행하는 내부 동작입니다.
value >= low AND value <= high;
```

- 부정 논리 연산자와 `BETWEEN` 연산자를 조합하면 조건의 반대 결과를 반환합니다.

```sql
value NOT BETWEEN low AND high;
```

```sql
value < low OR value > high
```

- `low, high`의 값에 `NULL TYPE`의 값을 지정하면 `BETWEEN` 연산자는 `NULL` 값을 반환합니다. 이 점을 앚지 말고 `BETWEEN` 연산자를 사용해야 합니다.

#### WHERE절에서 사용하기

```sql
SELECT
  productCode,
  productName,
  buyPrice
FROM
  products
WHERE
  buyPrice BETWEEN 90 AND 100;
```

| productCode | productName                          | buyPrice |
| ----------- | ------------------------------------ | -------- |
| S10_1949    | 1952 Alpine Renault 1300             | 98.58    |
| S10_4698    | 2003 Harley-Davidson Eagle Drag Bike | 91.02    |
| S12_1099    | 1968 Ford Mustang                    | 95.34    |
| S12_1108    | 2001 Ferrari Enzo                    | 95.59    |
| S18_1984    | 1995 Honda Civic                     | 93.89    |
| S18_4027    | 1970 Triumph Spitfire                | 91.92    |
| S24_3856    | 1956 Porsche 356A Coupe              | 98.30    |

- 위 예제에서는 `products.buyPrice Column`의 모든 값을 `products.buyPrice[n] >= 90`이면서 `products.buyPrice[n] <= 100`의 조건을 만족하는 결과만을 반환합니다.
- 이런 `sql`의 내부적인 동작으로 `BETWEEN` 연산자 대신에 `AND` 연산자만을 사용해서 범위를 지정할 수 있습니다.

```sql
# 위의 BETWEEN 연산자 예제와 동일한 결과를 반환
SELECT
  productCode,
  productName,
  buyPrice
FROM
  products
WHERE
  buyPrice >= 90 AND buyPrice <= 100;
```

#### NOT BETWEEN 연산자

- `NOT` 논리 연산자와 `BETWEEN` 연산자를 조합해서 사용할 수 있습니다.
- `NOT` 연산자와 `BETWEEN` 연산자를 조합하면 지정한 범위에 포함되지 않는 결과를 반환합니다.

```sql
SELECT
  productCode,
  productName,
  buyPrice
FROM
  products
WHERE
  buyPrice NOT BETWEEN 90 AND 100;
```

| productCode | productName                           | buyPrice |
| ----------- | ------------------------------------- | -------- |
| S10_1678    | 1969 Harley Davidson Ultimate Chopper | 48.81    |
| S10_2016    | 1996 Moto Guzzi 1100i                 | 68.99    |
| S10_4757    | 1972 Alfa Romeo GTA                   | 85.68    |
| S10_4962    | 1962 LanciaA Delta 16V                | 103.42   |
| S12_1666    | 1958 Setra Bus                        | 77.90    |
| S12_2823    | 2002 Suzuki XREO                      | 66.27    |

<br>

## WHERE절에서 IN 연산자 사용하기

#### IN 연산자 소개

- `IN` 연산자는 `list`와 일치하는 값을 확인하기 위한 연산자입니다.
- 기본적인 문법은 `IN` 연산자 왼쪽에 `Column` 또는 표현식이 올 수 있고, 오른쪽에는 확인하고 싶은 `list`가 올 수 있습니다. `list`는 괄호`()`로 표현할 수 있습니다.

```sql
value IN (value1, value2, value3,...)
```

```sql
value = value1 OR value = value2 OR value = value3 OR ...
```

- `IN` 연산자는 여러 `OR` 연산자의 조합과 기능적으로 동일하게 동작합니다.
- list에 포함된 값이 존재하면 1(true)를 반환하고 list에 포함된 값이 존재하지 않으면 0(false)를 반환합니다.

#### IN 연산자에서 NULL을 반환하는 두가지 CASE

- `IN` 연산자 왼쪽 값에 `NULL TYPE`의 값이 오는 경우
- `list`에 `NULL TYPE` 값이 포함된 경우

```sql
SELECT NULL IN (1,2,3);
```

| NULL IN (1,2,3) |
| --------------- |
| NULL            |

```sql
SELECT 0 IN (1 , 2, 3, NULL);
```

| 0 IN (1 , 2, 3, NULL) |
| --------------------- |
| NULL                  |

#### WHERE절에서 사용하기

```sql
SELECT
  officeCode,
  city,
  phone,
  country
FROM
  offices
WHERE
  country IN ('USA' , 'France');
```

| officeCode | city          | phone           | country |
| ---------- | ------------- | --------------- | ------- |
| 1          | San Francisco | +1 650 219 4782 | USA     |
| 2          | Boston        | +1 215 837 0825 | USA     |
| 3          | NYC           | +1 212 555 3000 | USA     |
| 4          | Paris         | +33 14 723 4404 | France  |

- 위 예제에서는 `office.country Column`의 모든 값을 `office.country[n] = 'USA'`이거나 `office.country[n] = 'France'`의 조건을 만족하는 결과만을 반환합니다.
- 위 예제의 내부 동작은 `office.country[n] = 'USA' OR office.country[n] = 'France'`으로 동작합니다 이런 이유로 여러 `OR` 조건을 사용할 경우에 `IN` 연산자를 사용해서 `Query`문을 줄일 수 있습니다.

#### NOT IN 연산자

- `NOT` 논리 연산자와 `IN` 연산자를 조합해서 사용할 수 있습니다.
- `NOT` 연산자와 `IN` 연산자를 조합하면 `list`에 포함되지 않는 결과를 반환합니다.

```sql
SELECT
  officeCode,
  city,
  phone,
  country
FROM
  offices
WHERE
  country NOT IN ('USA' , 'France');
```

| officeCode | city   | phone            | country   |
| ---------- | ------ | ---------------- | --------- |
| 5          | Tokyo  | +81 33 224 5000  | Japan     |
| 6          | Sydney | +61 2 9264 2451  | Australia |
| 7          | London | +44 20 7877 2041 | UK        |

<br>

## WHERE절에서 LIKE 연산자로 문자열 패칭하기

#### LIKE 연산자 소개

- LIKE 연산자는 문자열에 지정된 패턴이 포함되어 있는지 여부를 확인하는 연산자입니다.
- `MYSQL`에서는 두 가지 와이드카드(`'%', '\_'`)를 제공합니다.

#### `%` 와이드 카드 사용

- `%`: 특정 문자가 포함되어 있는지 확인하는 와이드카드입니다. `%` 위치에 따라서 원하는 결과를 반환할 수 있습니다.

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  firstName LIKE 'a%';
```

| employeeNumber | lastName | firstName |
| -------------- | -------- | --------- |
| 1143           | Bow      | Anthony   |
| 1611           | Fixter   | Andy      |

- 위 예제는 `employees.firstName column`에서 `a` 문자로 시작하는 모드 결과를 반환합니다.

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  firstName LIKE '%y';
```

| employeeNumber | lastName  | firstName |
| -------------- | --------- | --------- |
| 1056           | Patterson | Mary      |
| 1143           | Bow       | Anthony   |
| 1501           | Bott      | Larry     |
| 1504           | Jones     | Barry     |
| 1611           | Fixter    | Andy      |

- 위 예제는 `employees.firstName column`에서 `y` 문자로 끝나는 모든 결과를 반환합니다.

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  lastName LIKE '%on%';
```

| employeeNumber | lastName  | firstName |
| -------------- | --------- | --------- |
| 1056           | Patterson | Mary      |
| 1088           | Patterson | William   |
| 1102           | Bondur    | Gerard    |
| 1166           | Thompson  | Leslie    |
| 1216           | Patterson | Steve     |
| 1337           | Bondur    | Loui      |
| 1504           | Jones     | Barry     |

- 위 예제는 `employees.lastName column`에서 `on` 문자를 포함하고 있는 모든 결과를 반환합니다.
- 다양한 `LIKE` 연산자 예제를 확인하고 싶은면 [sqlzoo](https://sqlzoo.net/wiki/SELECT_names)를 확인해보세요.

#### `_` 와이드 카드 사용

- `_`: 단일 문자를 확인하기 위한 와이드카드입니다. `_` 개수에 따라 원하는 문자 개수를 정해서 결과를 반환합니다.

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  firstName LIKE 'T_m';
```

| employeeNumber | lastName | firstName |
| -------------- | -------- | --------- |
| 1619           | King     | Tom       |

- 위 예제는 `employees.firstName column`에서 `T`로 시작하고 `m`으로 끝나는 3글자의 문자열을 모두 반환합니다.

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  firstName LIKE 'P___r';
```

- 위 예제는 `employees.firstName column`에서 `P`로 시작하고 `r`으로 끝나는 5글자의 문자열을 모두 반환합니다.
- `_` 개수와 와이드카드 개수는 매핑되어 있습니다. 위 예제는 `_`를 3개 사용하고 있어서 `p + 3개의 _ + r` 조합이라서 5글자가 올 수 있습니다.

#### NOT LIKE 연산자

```sql
SELECT
  employeeNumber,
  lastName,
  firstName
FROM
  employees
WHERE
  lastName NOT LIKE 'B%';
```

| employeeNumber | lastName  | firstName |
| -------------- | --------- | --------- |
| 1002           | Murphy    | Diane     |
| 1056           | Patterson | Mary      |
| 1076           | Firrelli  | Jeff      |
| 1088           | Patterson | William   |
| 1165           | Jennings  | Leslie    |
| 1166           | Thompson  | Leslie    |
| 1188           | Firrelli  | Julie     |

- 위 예제는 `employees.lastName column에`서 `B` 문자로 시작하지 않는 모드 결과를 반환합니다.

## WHERE절에서 IS NULL 연산자 사용하기

- `NULL TYPE`의 값은 비교 연산으로 확인할 수 없습니다. 이런 문제를 해결하기 위해서 `IS NULL`, `IS NOT NULL` 연산자를 사용할 수 있습니다.
- `IS NULL` 연산자는 `NULL TYPE`의 값이 있는 결과를 반환합니다.
- `IS NOT NULL` 연산자는 `NULL TYPE`의 값이 아닌 결과를 반환합니다.

```sql
SELECT
  customerName, state
FROM
  customers
WHERE
  state IS NULL;
```

| customerName       | state |
| ------------------ | ----- |
| Atelier graphique  | NULL  |
| La Rochelle Gifts  | NULL  |
| Baane Mini Imports | NULL  |
| Havel & Zbyszek Co | NULL  |

```sql
SELECT
  customerName, state
FROM
  customers
WHERE
  state IS NOT NULL;
```

| customerName               | state    |
| -------------------------- | -------- |
| Signal Gift Stores         | NV       |
| Australian Collectors, Co. | Victoria |
| Mini Wheels Co.            | CA       |
| Muscle Machine Inc         | NY       |

<br>

## Reference

- [MySQLTUTORIAL - MySQL WHERE](https://www.mysqltutorial.org/mysql-where/)
- [MySQL Docs - Comparison Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html)
- [DATA ON AIR - WHERE절](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?pageid=2&mod=document&kboard_search_option%5Btree_category_1%5D%5Bkey%5D=tree_category_1&kboard_search_option%5Btree_category_1%5D%5Bvalue%5D=SQL+%EA%B8%B0%EB%B3%B8+%EB%B0%8F+%ED%99%9C%EC%9A%A9&uid=341)
