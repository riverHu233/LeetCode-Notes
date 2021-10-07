#### SQL语句知识点

---
##### SQL通配符
`%` : `代表零个或多个字符`  
`_` : `仅替代一个字符`  
`[charlist]` : `字符列中的任意单一字符a-ZA-Z`  
`[^charlist]` : `不在字符列中的任何单一字符` (等价于`[!charlist]`)   
**SQL通配符必须与`LIKE`运算符一起搭配使用**

---
##### Join(连接) -- Inner Join、 Left Outer Join、 Right Outer Join、 Full Join

+ JOIN: 如果表中有至少一个匹配，则返回行
+ LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行
+ RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行
+ FULL JOIN: 只要其中一个表中存在匹配，就返回行

关于Join的解读参见[Inner Join vs. Outer Join](https://www.diffen.com/difference/Inner_Join_vs_Outer_Join)

---
**当一个查询语句同时出现了WHERE, GROUP BY, HAVING, ORDER BY的时候， 相应的执行顺序和编写顺序：**  
**执行顺序：**   
1、执行`WHERE xx`对全表数据(先`FROM`表)做筛选，返回第一个结果集  
2、针对第一个结果集使用`GROUP BY`分组，返回第二个结果集  
3、针对第二个结果集中的每一组数据执行`SELECT xx`  
4、针对第三个结果集执行`HAVING xx`筛选，返回第四个结果集  
5、针对第四个结果集按照升序(ASC)或者降序(DESC)进行排序  
FROM >> WHERE >> GROUP BY >> HAVING >> SELECT >> ORDER BY

**编写顺序：**  
SELECT >> FROM >> WHERE >> GROUP BY >> HAVING >> ORDER BY
```
SELECT  
FROM  
WHERE (先过滤表/视图/结果集)  
GROUP BY  
HAVING (WHERE过滤的是行，GROUP对结果进行分组，HAVING对分组进行过滤)  
ORDER BY
```

---
##### WHERE 子句 -- 对表中数据进行筛选
语法：
```
SELECT 列名称  
FROM 表名称  
WHERE 列 运算符 值
```
可在WHERE子句中使用的运算符：  
`=` : `等于`  
`<>` : `不等于`  
`>` : `大于` ,  &emsp;  `>=` : `大于等于`  
`<` : `小于` ,  &emsp;  `<=` : `小于等于`  
`BETWEEN` : `在某个范围内(包含两边的边界值)`  
`LIKE` : `搜索某种模式`  


---
##### HAVING 子句
在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。

SQL HAVING 语法
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value
```

---
##### 聚合函数(Aggregate function)  
聚合函数的操作面向一系列的值，并返回一个单一的值。具体函数和描述如下：    
`AVG(column)` : `返回某列的平均值`  
`COUNT(column)` : `返回某列的行数(不包括NULL值)`  
`COUNT(DISTINCT column)` : `返回相异结果的数目`
`COUNT(*)` : `返回被选中的行数`  
`MAX(column)` : `返回某列的最高值`  
`MIN(column)` : `返回某列的最低值`  
`SUM(column)` : `返回某列的总和`  

**注**: `COUNT(*)`是对选中的结果集做筛选，因此允许结果集中存在为NULL; 而`COUNT(column)`则是
对某一列做统计，返回的是不包括NULL值的行数。

---
##### 向表格插入新的行和将列插入新表
**INSERT INTO 语句用于向表格中插入新的行。**
```
INSERT INTO table_name VALUES (值1, 值2,....)
指定所要插入数据的列：
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```
**SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。常用于创建表的备份复件或者用于对记录进行存档。**
```
把所有的列插入新表 
SELECT *
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
只把希望的列插入新表 
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

---
#### ALTER 关键字
CHANGE 用来修改字段名字以及类型  
MODIFY 用来修改字段类型  
ALTER COLUMN ... SET  用来修改字段数据  
DROP 用来删除相应的字段  
RENAME TO 用来修改表名  
```
ALTER TABLE <表名> [修改选项]
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名> }
```

---
#### 数据库索引 index
索引的优点：
+ 大大加快数据的检索速度(最主要的原因)
+ 加速表和表之间的连接；
+ 在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间；
+ 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性；

索引失效的情况：

---
#### 存储过程(procedure)
存储过程(stored procedure)是一组为了完成特定功能的SQL语句集合，经编译后存储在服务器端的数据库中，利用存储过程可以加速SQL语句的执行。
它可以提高SQL的速度，存储过程是编译过的，如果某一个操作包含大量的SQL代码或分别被执行多次，那么使用存储过程比直接使用单条SQL语句执行速度快的多。

---
1、默认情况下，表的列可以存放NULL值。**无法使用比较运算符来测试NULL值，如 `=`，`<`或者`<>`，
必须使用`IS NULL`和`IS NOT NULL`操作符**。  
2、`LIMIT`子句用于限制查询结果返回的数量，常用于分页查询。(LIMIT n  OFFSET m 等同于 LIMIT 0, n； 
其中，)  
3、