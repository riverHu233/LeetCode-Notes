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
**当一个查询语句同时出现了WHERE, GROUP BY, HAVING, ORDER BY的时候， 相应的执行顺序和
编写顺序：**  
**执行顺序：**   
1、执行`WHERE xx`对全表数据做筛选，返回第一个结果集  
2、针对第一个结果集使用`GROUP BY`分组，返回第二个结果集  
3、针对第二个结果集中的每一组数据执行`SELECT xx`  
4、针对第三个结果集执行`HAVING xx`筛选，返回第四个结果集  
5、针对第四个结果集按照升序(ASC)或者降序(DESC)进行排序  
即 WHERE >> GROUP BY >> HAVING >> ORDER BY

**编写顺序：**  
```
SELECT  
FROM  
WHERE (先过滤表/视图/结果集)  
GROUP BY  
HAVING (WHERE过滤的是行，GROUP对结果进行分组，HAVING对分组进行过滤)  
ORDER BY
```

---
