+++
title = 'Data_frame_api'
date = 2024-07-20T16:45:40+09:00
featuredImage = "/images/feature_images/spark.webp"
tags = ["Spark", "Architecture"]
categories = ["Data Engineering"]
draft = true
+++

# DataFrame Transformations

## Selecting Columns

## Renaming Columns

## Change Columns data type

## Adding Columns to a DataFrame

## Removing Columns from a DataFrame

## Basics Arithmetic with DataFrame

## Apache Spark Architecture: DataFrame Immutability

## How to filter a DataFrame

## Apache Spark Architecture: Narrow Transformations

## Dropping Rows

## How to Drop rows and columns

## Handling NULL Values I - Null Functions

```python
Dfn = customerDf.selectExpr(
    "salutation",
    "firstname",
    "lastname",
    "email_address",
    "year(birthdate) birthyear"
)
```

| salutation | firstname | lastname | email_address        | birthyear |
| ---------- | --------- | -------- | -------------------- | --------- |
| null       | James     | null     | james@efsefa.org     | null      |
| null       | Adrian    | null     | null                 | null      |
| null       | Ruth      | null     | null                 | null      |
| null       | Christine | Jimenez  | Christine@adfacg.net | null      |

### Where 사용

```python
Dfn.where($"salutation".isNotNull) # Good
```

```python
Dfn.where($"salutation" === null) # Error
```

## Handling NULL Values II - DataFrameNaFunctions

### na 사용

```python
# 모든 Column 값이 NULL인 것 삭제.
Dfn.na.drop(how="all")

# 어떤 Columnd 이던 NULL 인 것 삭제.
Dfn.na.drop("any")

# firstname && lastname이 NULL인 것 삭제.
Dfn.na.drop("all", Seq("firstname", "lastname"))

# firstname || lastname이 NULL 인 것 삭제.
Dfn.na.drop("any", Seq("firstname", "lastname"))
```

### na & fill 사용

```python
# 모든 NULL 값에 abcdefg 채워준다.
# 주의할 점) Type이 일치해야 채워줄 수 있다.
# abcedfg(String) != birthyear(Datetime)이기에 그대로 NULL로 남아있다.
Dfn.na.fill("abcdefg")

# 특정 Column이 NULL 값일 때 특정 값을 채워줄 수 있다.
Dfn.na.fill(Map(
    "salutation" -> "Unknown",
    "firstname" -> "John",
    "lastname" -> "Doe",
    "bithyear" -> 9999
))
```

## Sort and Order Rows - Sort & OrderBy

```python
# NULL 값이 없는 데이터들을
# firstname을 오름차순 정렬
# month를 내림차순 정렬

# sort와 orderBy는 서로 연관이 없기 때문에
# 최종적으로는 firstname 정렬은 적용되지 않고 month만 적용된다.
customerDf
.na.drop("any")
.sort("firstname") == .sort(expr("firstname"))
.orderBy(expr("month(birthdate)").desc)
.select("firstname", "lastname", "birthdate")

# firtname 오름차순 & lastname 내림차순으로 정렬하려면 다음과 같다.
customerDf
.na.drop("any")
.sort($"firstname", $"lastname".desc)
.select("firstname", "lastname", "birthdate")
```

## Create Group of Rows: GroupBy

```python
customerPurchases = webSalesDf.selectExpr(
    "ws_bill_customer_sk customer_id",
    "ws_item_sk item_id"
)
```

| customer_id | item_id |
| ----------- | ------- |
| 83074       | 4591    |
| 83074       | 3566    |
| 83074       | 7286    |
| 83074       | 2755    |
| 83074       | 2516    |

### groupBy 사용

```python
# customer_id로 묶고
# 각 customer가 구매한 모든 물품 개수를 count
customerPurchases.groupBy(
    "customer_id"
    )
    .agg(count("item_id"))
    .alias("item_count")
```

## DataFrame Statics

### aggregation 사용

```python
webSalesDf.agg(
    max("ws_sales_price")
    min("ws_sales_price")
    avg("ws_sales_price")
    count("ws_sales_price")
)
```

## Joining DataFrames - Inner Join

Address DF

| address_id | country       | state | city      | zip   | street_name    | street_number | location_type |
| ---------- | ------------- | ----- | --------- | ----- | -------------- | ------------- | ------------- |
| 1          | United States | AZ    | FairField | 86192 | Jackson        | 18            | condo         |
| 2          | United States | NM    | Fairview  | 85709 | Washington 6th | 362           | condo         |

Customer DF

| address_id | birth_country | birthdate  | customer_id | demographics                                                                             |
| ---------- | ------------- | ---------- | ----------- | ---------------------------------------------------------------------------------------- |
| 2133       | RWANDA        | 1935-09-13 | 45721       | "buy_potential": "10000", "credint_rating": "High Rist", "education_status": "Secondary" |

```python
# Apache Spark의 default JOIN = Inner
customerWithAddress = customerDf.join(
    addressDf,
    customerDf.col("address_id") === addressDf.col("address_id"),
    "inner" # 생략 가능
)
.select(
    "customer_id",
    "address_id",
    "demographics.education_status",
    "location_type",
    "country", "city",
    "street_name"
)
```

## Joining DataFrames - Right Outer Join

Right table을 기준으로 Left table에서 일치하는 값들을 매핑 해준다. 만약 Left table에서 일치하는 값이 없다면 NULL을 넣는다.

```python
webSalesDf.join(
    customerDf,
    "customer_id" === "ws_bill_customer_sk",
    "right"
)
.select(
    "customer_id",
    "ws_bill_customer_sk",
)
.where(
    "ws_bill_customer_sk is null"
)
```

## Joining DataFrames - Left Outer Join

```python
webSalesDf.join(
    customerDf,
    $"customer_id" === $"ws_bill_customer_sk",
    "left"
)
```

## Appending Rows to a DataFrame - Union

```python
df1 = customerDf.select(
    "firstname",
    "lastname",
    "customer_id"
)
.withColumn(
    "from",
    lit("df1")
)

df2 = customerDf.select(
    "lastname",
    "firstname",
    "customer_id"
)
.withColumn(
    "from",
    lit("df2")
)
```

```python
# df2 의 모든 데이터를 df1에 합쳐준다.
# 같은 데이터 임에도 column 위치를 기반으로 합치기 때문에 다른 것으로 인식한다.
df1.union(df2)
```

| firstname | lastname | customer_id | from |
| --------- | -------- | ----------- | ---- |
| Tiffany   | Skinner  | 45721       | df1  |
| Skinner   | Tiffany  | 45721       | df2  |

```python
df1.unionByName(df2)
```

| firstname | lastname | customer_id | from |
| --------- | -------- | ----------- | ---- |
| Tiffany   | Skinner  | 45721       | df1  |
| Tiffany   | Skinner  | 45721       | df2  |

```python
# distinct() 와 결합해서 중복을 없애준다.
df1.unionByName(df2).distinct()
```

| firstname | lastname | customer_id |
| --------- | -------- | ----------- |
| Tiffany   | Skinner  | 45721       |
| Tiffany   | Skinner  | 45721       |

## Cashing a DataFrame

## DataFrameWriter I

```python
customerWithAddress = customerDf
.na.drop("any")
.join(
    addressDf,
    customerDf.address_id == addressDf.address_id,
)
.select(
    'customer_id',
    'demographics',
    concat_ws(" ", "firstname", "lastname").as("Name")
    addressDf("*")
)
```

```python
salesWithItem = webSalesDf
.na.drop("any")
.join(
    itemDf,
    webSalesDf.ws_item_sk == itemDf.i_item_sk,
)
.selectExpr(
    "ws_bill_customer_sk customer_id",
    "ws_ship_addr_sk ship_address_id",
)
```

```python
# default로 partition은 200이다.
# 불필요하게 많은 경우, 8개로 낮춰준다.
# 파일 형식은 json으로 해준다.
# SaveMode.Overwrite로 똑같은 파일을 덮어써준다.
customerWithAddress
.repartition(8)
.write
.format("json")
.mode(SaveMode.Overwrite)
.option("path", "tmp/output/customerWithAddress")
.save
```

## DataFrameWriter II - PartitionBy

Partitions the output by the given columns on the file system.

```python
customerWithAddress
.repartition(8)
.write
.partitionBy("item_category")
.format("json")
.mode(SaveMode.Overwrite)
.option("path", "tmp/output/customerWithAddress")
.save
```

## User Defined Functions

### 함수 정의

```python
@udf
def stringConcat(sep, first, second):
    return first + sep + second
```

### 함수 등록

```python
customerDf.select(
    "firstname",
    "lastname"
    stringConcat(
        "-",
        "firstname",
        "lastname"
    )
)
```

# Apache Spark Architecture: Execution

## Partitioning a DataFrame

````python
df.repartition(10)\ # df with 10 partitions
      .rdd \
      .getNumPartitions()

df.coalesce(1).rdd.getNumPartitions() #df with 1 partition```
````

## Repartition VS Coalesce

RDD를 생성한 뒤 filter() 연산을 비롯한 다양한 transformation 연산을 수행하다보면 최초에 설정된 partition 개수가 적합하지 않은 경우가 발생할 수 있다.

이 경우 `coalesce()`나 `repartition()`연산을 사용해 현재의 RDD의 파티션 개수를 조정할 수 있다.

두 메서드는 모두 파티션의 크기를 나타내는 정수를 인자로 받아서 파티션의 수를 조정한다는 점에서 공통점이 있지만 **repartition()이 파티션 수를 늘리거나 줄이는 것을 모두 할 수 있는 반면 coalesce()는 줄이는 것만 가능**하다.

그럼 모든 것이 가능한 repartition() 메서드가 있음에도 coalesce() 메서드를 따로 두는 이유는 바로 처리 방식에 따른 성능 차이 때문이다. 즉 **repartition()은 셔플을 기반으로 동작을 수행**하는 데 반해 **coalesce()는 강제로 셔플을 수행하라는 옵션을 지정하지 않는 한 셔플을 사용하지 않기 때문이다.** 따라서 데이터 필터링 등의 작업으로 데이터 수가 줄어들어 파티션 수를 줄이고자 할 때는 상대적으로 성능이 좋은 coalesce()를 사용하고, 파티션 수를 늘려야 하는 경우에만 repartition() 메서드를 사용하는 것이 좋다.


