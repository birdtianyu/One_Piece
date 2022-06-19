## SQL语句

(Structured Query Language:结构化查询语言) 是用于管理关系数据库管理系统（R-DBMS）

**注释方法**：1. `#这里是单行注释内容`   2. `-- 这里是单行注释内容` 3. `/*这里是多行注释内容*/`

**其它：SQL对大小写不敏感**



### 1. 基础语句

---

#### 1. SELECT

```sql
# 可以在这里定义表达式, 别名 .etc
SELECT column_name1 AS New_name1, column_name2, 3*(column_name2+100) "New_name2"
FROM table_name1;
```



```sql
# 连接两列内容变成一列
SELECT column_name1 || column_name2 AS "New_name"
FROM table_name1;
```



**DISTINCT关键字**：在表中，一个列可能会包含多个重复值，在结果表中仅希望列出不同（distinct）的值时使用

```sql
SELECT DISTINCT column_name1, column_name2 
FROM table_name1;
```



**TOP或者LIMIT关键字**：用于限制返回的记录的个数（不同数据库管理系统中不同）

```sql
# SQL Sever / MS Access
SELECT TOP number|percent column_name(s)
FROM table_name1;
```

```sql
# MySQL
SELECT column_name(s)
FROM table_name1
LIMIT number;
```



#### 2. WHERE 子句: AND & OR & NOT

```sql
SELECT column_name1, column_name2
FROM table_name1
WHERE column_name1 operator value1;
```



```sql
SELECT column_name1, column_name2
FROM table_name1
WHERE column_name1 = "value1" AND (column_name2 = 100 OR column_name1 = "value3");
```



#### 3. ORDER BY

对结果集进行升序排序

```sql
-- 默认升序(ASC)
ORDER BY column_name1, column_name2;

-- 先升序再降序
ORDER BY column_name1 ASC, column_name2 DESC;
# 等同于
ORDER BY column_name1, column_name2 DESC;
```



#### 4. INSERT INTO

插入新数据到数据库。

第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：

```sql
INSERT INTO table_name1
VALUES (value1, value2, value3, ...);
```

第二种形式需要指定列名及被插入的值：

```sql
INSERT INTO table_name1 (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```



#### 5. UPDATE 

用于更新表中数据。

```sql
UPDATE table_name1
SET column1=value1, column2=value2, ...
WHERE some_column=some_value;
```



#### 6. DELETE 

用于删除表中的行。

```sql
DELETE FROM table_name1
WHERE some_column=some_value;
```

<font color="red">删除所有数据</font>

```sql
DELETE FROM table_name1;
或
DELETE * FROM table_name1;
```



`TRUNCATE TABLE ` 用于删除表的数据，但不删除表结构。

> **高效：**用 TRUNCATE 代替 DELETE，因为 DELETE 在没有 Commit 时还会存放一些恢复信息用于 Rollback，如果确认要删除直接使用 TRUNCATE。



#### 7. GROUP BY

用于结合聚合函数，根据一个或多个列对结果集进行分组。

```sql
SELECT column_name1, group_function(column_name2)
FROM table_name1
WHERE column_name operator value1
GROUP BY column_name1;
```

**注意执行顺序：先进行分组，然后执行SELECT子句。**（对各个分组的结果执行`group_function`，最终返回一个表）



#### 8. HAVING

用于分组时，筛选组

```sql
...
GROUP BY column_name1
HAVING condition1;
```





### 2. 表连接

---

>1. 自连接 （要重命名这个表，使其拥有两个名称，便于操作）
>2. 等值连接
>3. 外连接：a. 左连接    b. 右连接    c. 全连接



#### 等值连接

```sql
SELECT table1.dept, table2.emp
FROM table1, table2
WHERE table1.deptno = table2.deptno;
```

#### 左/右连接

```sql
SELECT table1.dept, table2.emp
FROM table1, table2
WHERE table1.deptno = table2.deptno(+);
```

#### 全连接

```sql
SELECT table1.dept, table2.emp
FROM table1 FULL JOIN table2
ON table1.deptno = table2.deptno;
```



- **INNER JOIN** （等值连接）：如果表中有任意一对匹配，则返回这行
- **LEFT JOIN** （左连接）：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN** （右连接）：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN** （全连接）：只要其中一个表中存在匹配，则返回行

![img](D:\技法\笔记图片\sql-join.png)



```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count;
```



### 3. 子查询

---

**<font color="red">在查询时基于未知时使用子查询</font>**

**子查询结构：**

```sql
# 主查询
SELECT column_name [, column_name ]
FROM   table1 [, table2 ]
WHERE  column_name operator
      # 子查询
      (
          SELECT column_name [, column_name ]
          FROM table1 [, table2 ]
          [WHERE]
      )
```



#### 3.1 单行子查询

子查询返回单行结果

| 单行操作符     | 描述     |
| -------------- | -------- |
| =              | 等于     |
| >              | 大于     |
| >= （比>高效） | 大于等于 |
| <              | 小于     |
| <=             | 小于等于 |
| <>             | 不等于   |



#### 3.2 多行子查询

子查询返回多行结果

**单行操作符** + **多行操作符**( )

| 多行操作符 | 描述                   |
| ---------- | ---------------------- |
| IN( )      | 等于列表中任意一个值   |
| ANY( )     | 和列表中任意一个值比较 |
| ALL( )     | 和列表中所有值比较     |



### 4. 表结构操作

创建表，调整列名，删除表等等



### 5. 数据操作

对每一行数据增，删，改，查



### 6. 事务处理



#### Commit 提交数据库（更新远端数据库） 

#### Rollback 回滚

#### Savepoint 保存上一步操作时的结点，方便以后Rallback到该结点







### 备注：

#### a. NULL值

表示没有被定义的值，而不是像0或者“ ”这样已经定义的值。





#### b. 常用操作符

| 操作符                             | 描述                         |
| ---------------------------------- | ---------------------------- |
| =                                  | 等于                         |
| >                                  | 大于                         |
| >=                                 | 大于等于                     |
| <                                  | 小于                         |
| <=                                 | 小于等于                     |
| <>                                 | 不等于                       |
| BETWEEN ... AND ...                | 在 ... 和 ... 之间（闭区间） |
| [NOT] IN ("value1", "value2", ...) | 匹配规定集合中的任意一个值   |
| LIKE *pattern*                     | 模糊匹配，后接正则表达式     |
| IS NULL                            | 是否是一个NULL值             |



#### c. 常用单行函数

| 函数名                           | 说明         |
| -------------------------------- | ------------ |
| SYSDATE                          | 返回当前日期 |
|                                  |              |
|                                  |              |
| TO_CHAR(column_name, 输出格式)   | 转换成字符   |
| TO_NUMBER(column_name, 输出格式) | 转换成数值   |
| TO_DATE()                        | 转换成日期   |
|                                  |              |
| AVG()                            | 返回平均值   |
| MAX()                            | 返回最大值   |
| MIN()                            | 返回最小值   |
| SUM()                            | 返回总和     |
| COUNT()                          | 返回行数     |
|                                  |              |
|                                  |              |

