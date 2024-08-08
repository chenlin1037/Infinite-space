## 求累加和的几种方式

 总结的5种方式,分别给出代码示例:

1. 使用SUM窗口函数:

```sql
SELECT score, SUM(score) OVER(ORDER BY id) AS total  
FROM table
```

2. 使用子查询:

```sql
SELECT t1.score, 
  (SELECT SUM(t2.score) FROM table t2 WHERE t2.id <= t1.id) AS total
FROM table t1  
```

3. 使用临时表:

```sql
CREATE TEMPORARY TABLE tmp
  SELECT *, @sum:=@sum+score AS total
  FROM table CROSS JOIN (SELECT @sum:=0) vars
```

4. 使用预处理语句:

```sql
-- 定义一个命名查询语句
PREPARE get_all_records FROM 'SELECT * FROM user_table';

-- 循环执行语句返回结果  
LOOP
  FETCH NEXT FROM get_all_records INTO @user_id, @name, @age;
  
  -- 在这里对查询结果进行循环处理,比如累加 age  
  SET @total_age = @total_age + @age;
  
  IF done THEN 
     LEAVE loop;
  END IF;
END LOOP;

-- 打印总人口年龄    
SELECT @total_age;

-- 释放查询语句句柄
DEALLOCATE PREPARE get_all_records;
```

5. 使用表的自连接:

```sql
SELECT t1.score, SUM(t2.score) AS total
FROM table t1, table t2
WHERE t1.id >= t2.id
GROUP BY t1.id
```

## 求中位数的方法

```sql
select id,job,score,rk
from 
    (select id,job,score,
     row_number() over(partition by job order by score desc) as rk,
     count(score) over(partition by job) cnt
    from grade
    )a
where rk=round(cnt/2) or rk=round((1+cnt)/2)
order by id
```

## DELETE和TRUNCATE

在SQL中，常见的删除方式有两种：DELETE和TRUNCATE。

1. DELETE：
   DELETE语句用于从数据库表中删除满足指定条件的记录。它可以根据条件删除一部分或全部记录，并且删除操作是可回滚的，即可以通过回滚事务或恢复备份来还原删除的数据。DELETE语句会触发表上的触发器，并且可以与SELECT语句结合使用，以选择要删除的记录。
   例如，删除订单表中已取消的订单：
   ````sql
   DELETE FROM orders
   WHERE status = 'Cancelled';
   ```

2. TRUNCATE：
   TRUNCATE语句用于快速删除数据库表中的所有记录。与DELETE不同，TRUNCATE操作是非事务性的，即无法回滚，且无法仅删除部分记录。TRUNCATE操作是通过释放表的存储空间来实现的，因此执行速度比DELETE更快。TRUNCATE语句不会触发触发器，并且无法与WHERE子句一起使用。
   例如，清空订单表中的所有记录：
   ````sql
   TRUNCATE TABLE orders;
   ```

需要注意的是，DELETE和TRUNCATE都是用于删除数据，但它们的操作方式和效果略有不同。选择使用哪种方式取决于具体的需求和场景。如果需要删除部分记录，或者需要进行事务管理和触发器操作，可以使用DELETE。如果需要快速删除整个表的所有记录，并且不需要进行事务管理和触发器操作，可以使用TRUNCATE。

DROP语句用于删除数据库中的表。与DELETE和TRUNCATE不同，DROP操作会完全删除表和表的所有数据，包括表的结构和关联的索引、约束、触发器等。执行DROP操作后，表将不再存在于数据库中，无法恢复。

下面是DROP语句的示例：

```sql
DROP TABLE table_name;
```

其中，table_name是要删除的表的名称。执行此语句后，系统将删除该表及其所有相关内容。

需要注意的是，执行DROP操作是一个潜在的危险操作，因为它会导致数据的永久性丢失。因此，在执行DROP操作之前，请务必进行谨慎的确认，并确保你真正想要删除该表及其所有数据。



## HAVING

`HAVING` 子句通常用于SQL查询中，以过滤`GROUP BY`子句生成的聚合结果。它的作用类似于 `WHERE` 子句，但 `HAVING` 是在分组和聚合操作之后进行过滤，而 `WHERE` 是在分组和聚合操作之前进行过滤。

### 使用场景：

`HAVING` 子句通常用于需要对聚合函数（如 `COUNT`、`SUM`、`AVG`、`MAX`、`MIN` 等）的结果进行过滤的场景。

### 示例：

假设你有一个 `Sales` 表，该表包含以下列：

- `sales_id`: 销售记录的ID
- `product_id`: 产品的ID
- `sales_amount`: 销售金额

你想要查询销售总额超过5000的产品。此时，你需要先通过 `GROUP BY` 分组，计算每个产品的销售总额，然后使用 `HAVING` 进行过滤。

```mysql
SELECT 
    product_id, 
    SUM(sales_amount) AS total_sales
FROM 
    Sales
GROUP BY 
    product_id
HAVING 
    SUM(sales_amount) > 5000;
```

### 查询逻辑解释：

1. **分组（`GROUP BY`）**: 根据 `product_id` 分组，计算每个产品的销售总额。
2. **聚合函数（`SUM(sales_amount)`）**: 计算每个产品的总销售额。
3. **过滤（`HAVING`）**: 只保留那些总销售额超过5000的产品。

### 使用区别：

- **`WHERE`**: 用于在数据分组之前过滤记录。例如，选择销售金额大于1000的记录。
- **`HAVING`**: 用于在数据分组之后，过滤聚合结果。例如，选择销售总额大于5000的产品。

```mysql
SELECT 
    product_id, 
    SUM(sales_amount) AS total_sales
FROM 
    Sales
WHERE
    sales_amount > 1000
GROUP BY 
    product_id;
```

在上述查询中，`WHERE` 先过滤掉所有 `sales_amount` 小于或等于 1000 的记录，然后才对结果进行分组和聚合。

## DISTINCT的使用

在SQL中，`DISTINCT` 关键字用于从查询结果中去除重复的记录。它确保返回的结果集只包含唯一值，而不会有重复的行。

### `DISTINCT` 的常见使用场景：

1. **选择唯一值**： 当你希望从某个列中提取所有唯一值时，可以使用 `DISTINCT`。例如，如果你有一张包含客户所在城市的表，你希望查看所有不同的城市，可以使用 `DISTINCT`。

   ```
   sql复制代码SELECT DISTINCT city 
   FROM Customers;
   ```

   这个查询会返回 `Customers` 表中所有唯一的城市名称。

2. **多个列的唯一组合**： 当你希望获取多个列组合后唯一的结果时，可以将 `DISTINCT` 应用于多个列。例如，如果你想查看某个表中所有唯一的（城市，国家）组合，可以这样做：

   ```
   sql复制代码SELECT DISTINCT city, country
   FROM Customers;
   ```

   这个查询会返回 `Customers` 表中所有唯一的（城市，国家）组合。

3. **与其他 SQL 关键字结合使用**： `DISTINCT` 可以与其他 SQL 关键字结合使用，比如 `COUNT`，以获取唯一值的数量。

   ```
   sql复制代码SELECT COUNT(DISTINCT city)
   FROM Customers;
   ```

   这个查询会返回 `Customers` 表中唯一城市的数量。

[leetcode题目](https://leetcode.cn/problems/second-degree-follower/description/)

## COALESCE

`COALESCE` 是一个 SQL 函数，用于处理可能包含 `NULL` 的数据。它的主要作用是返回其参数中第一个非 `NULL` 的值。如果所有的参数都是 `NULL`，那么 `COALESCE` 也会返回 `NULL`。

### 语法

```
sql
复制代码
COALESCE(value1, value2, ..., valueN)
```

- `value1, value2, ..., valueN`：这些是 `COALESCE` 函数会检查的值。它会从左到右依次检查这些值，并返回第一个非 `NULL` 的值。如果所有值都是 `NULL`，则返回 `NULL`。

### 使用场景

1. **处理 `NULL` 值**：
   - 在 SQL 查询中，如果你有一个字段可能会包含 `NULL`，你可以使用 `COALESCE` 来提供一个默认值。例如，在统计总数时，如果某个书籍没有任何订单记录，`SUM(quantity)` 可能会返回 `NULL`。使用 `COALESCE` 可以将这种 `NULL` 转换为 `0`，从而避免计算中的问题。
2. **选择默认值**：
   - 当你希望某个字段的值为 `NULL` 时使用默认值，`COALESCE` 可以帮你实现。例如，你希望将 `NULL` 替换为 `0` 或其他特定值。

### 示例

假设你有一个表 `Sales`，记录了不同书籍的销售额。如果某些书籍的销售额是 `NULL`（可能因为没有销售记录），你希望将这些 `NULL` 替换为 `0`。

```
sql复制代码SELECT book_id,
       COALESCE(sales_amount, 0) AS sales_amount
FROM Sales;
```

在这个例子中，如果 `sales_amount` 为 `NULL`，`COALESCE` 将返回 `0`。

### 窗口函数的替换语句

示例如下：

>表: `Logins`

```
+----------------+----------+
| 列名           | 类型      |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
(user_id, time_stamp) 是这个表的主键(具有唯一值的列的组合)。
每一行包含的信息是user_id 这个用户的登录时间。
```

 

编写解决方案以获取在 `2020` 年登录过的所有用户的本年度 **最后一次** 登录时间。结果集 **不** 包含 `2020` 年没有登录过的用户。

返回的结果集可以按 **任意顺序** 排列。

返回结果格式如下例。

 

**示例 1:**

```
输入：
Logins 表:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 6       | 2021-04-21 14:06:06 |
| 6       | 2019-03-07 00:18:15 |
| 8       | 2020-02-01 05:10:53 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
| 2       | 2019-08-25 07:59:08 |
| 14      | 2019-07-14 09:00:00 |
| 14      | 2021-01-06 11:59:59 |
+---------+---------------------+
输出：
+---------+---------------------+
| user_id | last_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
+---------+---------------------+
解释：
6号用户登录了3次，但是在2020年仅有一次，所以结果集应包含此次登录。
8号用户在2020年登录了2次，一次在2月，一次在12月，所以，结果集应该包含12月的这次登录。
2号用户登录了2次，但是在2020年仅有一次，所以结果集应包含此次登录。
14号用户在2020年没有登录，所以结果集不应包含。
```

第一种写法：

```mysql
WITH temp_table AS (
    SELECT * 
    FROM Logins 
    WHERE time_stamp >= '2020-01-01 00:00:00' 
    AND time_stamp < '2021-01-01 00:00:00'
)
select user_id ,Max(time_stamp) as last_stamp
from temp_table
group by user_id
```

第二种写法

```mysql
WITH temp_table AS (
    SELECT * 
    FROM Logins 
    WHERE time_stamp >= '2020-01-01 00:00:00' 
    AND time_stamp < '2021-01-01 00:00:00'
),
Ranked AS (
    SELECT 
        t.*, 
        ROW_NUMBER() OVER (PARTITION BY t.user_id ORDER BY t.time_stamp DESC) AS rn
    FROM 
        temp_table t
)
SELECT 
    user_id,
    time_stamp AS last_stamp
FROM 
    Ranked
WHERE 
    rn = 1;
```

虽然第二种 更复杂
