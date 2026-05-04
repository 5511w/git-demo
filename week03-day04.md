1. 插入数据 INSERT

```sql
-- 逐条插入
INSERT INTO student (id,name,age,birthday) 
VALUES (1,'张三',20,'2000-01-10');

INSERT INTO student (id,name,age,birthday) 
VALUES (2,'李四',19,'2001-02-15');
```

2. 修改数据 UPDATE(一定要加 WHERE)

```sql
-- 修改单人年龄
UPDATE student SET age=21 WHERE id=1;
```

3. 删除数据 DELETE


不加 WHERE 会删掉所有数据
```sql
-- 删除指定一条
DELETE FROM student WHERE id=2;
```
4. 安全提醒（记笔记必写）

永远不要随便写：UPDATE 表名 SET ... 不带条件

永远不要随便写：DELETE FROM 表名 不带条件

工作中删改先 用 SELECT 查一遍 再操作

