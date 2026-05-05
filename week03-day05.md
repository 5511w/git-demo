select 查询数据

```sql
SELECT column1, column2, ...
FROM table_name
[WHERE condition]
[ORDER BY column_name [ASC | DESC]]
[LIMIT number];
```

* column1, column2, ... 是你想要选择的列的名称，如果使用 * 表示选择所有列。
* table_name 是你要从中查询数据的表的名称。
* WHERE condition 是一个可选的子句，用于指定过滤条件，只返回符合条件的行。
* ORDER BY column_name [ASC | DESC] 是一个可选的子句，用于指定结果集的排序顺序，默认是升序（ASC）。
* LIMIT number 是一个可选的子句，用于限制返回的行数。

实例:

```sql
-- 选择所有列的所有行
SELECT * FROM users;

-- 选择特定列的所有行
SELECT username, email FROM users;

-- 添加 WHERE 子句，选择满足条件的行
SELECT * FROM users WHERE is_active = TRUE;

-- 添加 ORDER BY 子句，按照某列的升序排序
SELECT * FROM users ORDER BY birthdate;

-- 添加 ORDER BY 子句，按照某列的降序排序
SELECT * FROM users ORDER BY birthdate DESC;

-- 添加 LIMIT 子句，限制返回的行数
SELECT * FROM users LIMIT 10;

```

|操作符|描述|实例|
|----|----|----|
|=|等号，检测两个值是否相等，如果相等返回true|(A = B) 返回false。|
|<>, !=|不等于，检测两个值是否相等，如果不相等返回true|(A != B) 返回 true。|
|>|大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true|(A > B) 返回false。|
|<|小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true|(A < B) 返回 true。|
|>=|大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true|(A >= B) 返回false。|
|<=|小于等于号，检测左边的值是否小于或等于右边的值, 如果左边的值小于或等于右边的值返回true	|(A <= B) 返回 true。|

```sql
-- 使用 AND 运算符和通配符
SELECT * FROM users WHERE username LIKE 'j%' AND is_active = TRUE;

-- 使用 OR 运算符
SELECT * FROM users WHERE is_active = TRUE OR birthdate < '1990-01-01';

-- 使用 IN 子句
SELECT * FROM users WHERE birthdate IN ('1990-01-01', '1992-03-15', '1993-05-03');

--IN 条件:
SELECT * FROM countries WHERE country_code IN ('US', 'CA', 'MX');

--NOT 条件:
SELECT * FROM products WHERE NOT category = 'Clothing';

--BETWEEN 条件:
SELECT * FROM orders WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';

--IS NULL 条件
SELECT * FROM employees WHERE department IS NULL;

--IS NOT NULL 条件:
SELECT * FROM customers WHERE email IS NOT NULL;
```
使用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写的

```
mysql> SELECT * from runoob_tbl WHERE BINARY runoob_author='runoob.com';
Empty set (0.01 sec)
 
mysql> SELECT * from runoob_tbl WHERE BINARY runoob_author='RUNOOB.COM';
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 3         | JAVA 教程   | RUNOOB.COM    | 2016-05-06      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
+-----------+---------------+---------------+-----------------+
2 rows in set (0.01 sec)
```

LIKE 子句

在 MySQL 中用于在 WHERE 子句中进行模糊匹配的关键字。它通常与通配符一起使用，用于搜索符合某种模式的字符串。

```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name LIKE pattern;
```

* column1, column2, ... 是你要选择的列的名称，如果使用 * 表示选择所有列。
* table_name 是你要从中查询数据的表的名称。
* column_name 是你要应用 LIKE 子句的列的名称。
* pattern 是用于匹配的模式，可以包含通配符。

1. 百分号通配符 %：

% 通配符表示零个或多个字符。例如，'a%' 匹配以字母 'a' 开头的任何字符串。

```sql
SELECT * FROM customers WHERE last_name LIKE 'S%';
```

2. 下划线通配符 _：

_ 通配符表示一个字符。例如，'_r%' 匹配第二个字母为 'r' 的任何字符串。

```sql
ELECT * FROM products WHERE product_name LIKE '_a%';
```

3. 不区分大小写的匹配：

```sql
SELECT * FROM employees WHERE last_name LIKE 'smi%' COLLATE utf8mb4_general_ci;
```

实例
以下是我们将 runoob_tbl 表中获取 runoob_author 字段中以 COM 为结尾的的所有记录：

```
mysql> use RUNOOB;
Database changed
mysql> SELECT * from runoob_tbl  WHERE runoob_author LIKE '%COM';
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 3         | 学习 Java   | RUNOOB.COM    | 2015-05-01      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
+-----------+---------------+---------------+-----------------+
2 rows in set (0.01 sec)
```

ORDER BY(排序) 语句

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC | DESC], column2 [ASC | DESC], ...;
```

* ORDER BY column1 [ASC | DESC], column2 [ASC | DESC], ... 是用于指定排序顺序的子句。ASC 表示升序（默认），DESC 表示降序。

1. 多列排序：

```sql
SELECT * FROM employees
ORDER BY department_id ASC, hire_date DESC;
```

先按部门 ID 升序 ASC 排序，然后在相同部门中按雇佣日期降序 DESC 排序。

2. 使用数字表示列的位置：

```sql
SELECT first_name, last_name, salary
FROM employees
ORDER BY 3 DESC, 1 ASC;
```
按第三列（salary）降序 DESC 排序，然后按第一列（first_name）升序 ASC 排序。

3.  使用表达式排序：

```sql
SELECT product_name, price * discount_rate AS discounted_price
FROM products
ORDER BY discounted_price DESC;
```

选择产品表 products 中的产品名称和根据折扣率计算的折扣后价格，并按折扣后价格降序 DESC 排序。

4. 从 MySQL 8.0.16 版本开始，可以使用 NULLS FIRST 或 NULLS LAST 处理 NULL 值：

```sql
SELECT product_name, price
FROM products
ORDER BY price DESC NULLS LAST;
```

选择产品表 products 中的产品名称和价格，并按价格降序 DESC 排序，将 NULL 值排在最后。NULLS FIRST 将空值排在前面。