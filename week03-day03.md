1. create创建数据库

```

CREATE DATABASE 数据库名;

```

实例过程：

```

[root@host]# mysql -u root -p   
Enter password:******  # 登录后进入终端

mysql> create DATABASE RUNOOB;

```

建数据库的基本语法如下：

```
CREATE DATABASE [IF NOT EXISTS] database_name
#指定字符集和排序规则
  [CHARACTER SET charset_name]
  [COLLATE collation_name];
```

* 如果数据库已经存在，执行 CREATE DATABASE 将导致错误。
为了避免这种情况，你可以在 CREATE DATABASE 语句中添加 IF NOT EXISTS 子句。

2. drop删除数据库

```
DROP DATABASE <database_name>;        -- 直接删除数据库，不检查是否存在
或
DROP DATABASE [IF EXISTS] <database_name>;
```

* IF EXISTS 是一个可选的子句，表示如果数据库存在才执行删除操作，避免因为数据库不存在而引发错误。
该操作是不可逆的。为了避免误操作，通常建议在执行删除之前备份数据库。

3. use选择数据库

```
USE database_name;
```

4. 常用数据类型

    1. 数值类型

    |数据类型|定义示例|储存示例（实际值）|说明|
    |----|----|----|----|
    |INT|age INT|25|整数年龄|
    |TINYINT|status TINYINT|1(代表启用)|小范围状态码|
    |BIGINT|user_id BIGINT|123456789023|大ID或时间戳|
    |DECIMAL|price DECIMAL（10，2）|1999.99|金额，保留两位小数|
    |FLOAT|weight FLOAT|70.5|体重近似值|

    2. 字符串类型
    
    |数据类型|定义示例|储存示例|说明|
    |----|----|----|----|
    |VARCHAR|name VARCHAR（50）|'张三'|长度可变，不浪费空间|
    |CHAAR|sex CHAR(1)|'m'|固定长度，适合代码/性别|
    |TEXT|content TEXT|'这是很长很长的文字....'|大段文本，无长度限制（按数据库不同有上限）|

    3. 日期与时间类型

    |数据类型|定义示例|储存示例|说明|
    |----|----|----|----|
    |DATE|birthday DATE|'1995-08-20'|仅年月日|
    |TIME|start_time TIME|'09:30:00'|仅时分秒|
    |DATETIME|created_at DATETIME|'2025-05-03 14:25:10'|常见与MySQL，完整时间|

    4. 布尔类型

    |数据类型|定义示例|储存示例|说明|
    |----|----|----|----|
    |BOOLEAN|is_active BOOLEAN|TRUE或FALSE|实际存储可能是1/0，但逻辑为真/假|

    * MySQL中 BOOLEAN 是 TINYINT(1) 的同义词，值可为 1/0。

    5. 二进制类型

    |数据类型|定义示例|储存示例（概念）|说明|
    |----|----|----|----|
    |BLOB|file_data BLOB|0x89504E470D0A1A0A...（十六进制）|存储小图片、加密数据|
    |LONGBLOB|large_file LONGBLOB|最大4GB的二进制内容|视频、大文件|
    |BINARY|hash BINARY|十六进制表示的固定长度二进制串|如MD5结果|

    6. 其他特殊类型
   
    |数据类型|定义示例|储存示例|说明|
    |----|----|----|----|
    |JSON|attributes JSON|{"color": "red", "size": "M"}|存储半结构化数据|
    |ENUM|size ENUM('S','M','L')|'M'|只能从给定列表选一个值|
    |UUID|id CHAR(36)（也可用UUID类型）|'550e8400-e29b-41d4-a716-446655440000'|全局唯一标识符|


综合建表示例（SQL）

```
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    sex CHAR(1) CHECK (sex IN ('M','F')),
    age TINYINT,
    salary DECIMAL(12,2),
    birthday DATE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    is_vip BOOLEAN DEFAULT FALSE,
    avatar BLOB,
    extra JSON
);
```

插入一行数据示例：

```
INSERT INTO users VALUES (
    1,
    '李华',
    'M',
    28,
    12500.00,
    '1996-04-12',
    '2026-05-03 10:00:00',
    TRUE,
    NULL,
    '{"hobby": "basketball", "city": "Shanghai"}'
);
```

5. 创建数据表

```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);
```

* table_name是你要创建的表的名称。
* column1, , ...是表中的列名。
* datatype是每个列的数据类型。

```
-- 查看当前库所有表
SHOW TABLES;
-- 查看表结构
DESC student;
-- 删除表
DROP TABLE student;
```

created_at DATETIME DEFAULT CURRENT_TIMESTAMP

DEFAULT CURRENT_TIMESTAMP：当插入一条新记录且没有显式给 created_at 赋值时，数据库会自动将该字段设为当前的系统日期和时间。

is_vip BOOLEAN DEFAULT FALSE

DEFAULT FALSE：如果插入记录时没有指定 is_vip 的值，数据库会自动设为 FALSE（即非 VIP）。

主键 → 应写为 PRIMARY KEY

非null → 应写为 NOT NULL

布尔默认为真 → 应写为 BOOLEAN DEFAULT TRUE（MySQL 中 BOOLEAN 是 TINYINT(1) 的同义词）

AUTO_INCREMENT 定义列为自增的属性，一般用于主键，数值会自动加 1。

ENGINE 设置存储引擎，CHARSET 设置编码。

如果你希望在创建表时指定数据引擎，字符集和排序规则等，可以使用 CHARACTER SET 和 COLLATE 子句：

```
CREATE TABLE MYTABLE (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50)
) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
创建一个使用 utf8mb4 字符集和 utf8mb4_general_ci 排序规则的表。

6. 删除数据表

```
DROP TABLE table_name;     -- 直接删除表，不检查是否存在
DROP TABLE [IF EXISTS] table_name;  -- 会检查是否存在，如果存在则删除
```

* table_name 是要删除的表的名称。

* IF EXISTS 是一个可选的子句，表示如果表存在才执行删除操作，避免因为表不存在而引发错误。

如果你只是想删除表中的所有数据，但保留表的结构，可以使用 TRUNCATE TABLE 语句：

```
TRUNCATE TABLE table_name;  #这会清空表中的所有数据，但不会删除表本身。
```

###### 注意事项：

备份数据：在删除表之前，确保已经备份了数据，如果你需要的话。

外键约束：如果该表与其他表有外键约束，可能需要先删除外键约束，或者确保依赖关系被处理好。