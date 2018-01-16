## 联结JOIN
#### 内联结(EQUIJOIN等值联结)
当两个表中都存在匹配时，才返回行。
```SQL
SELECT table1.column1, table2.column2...
FROM table1
INNER JOIN table2
ON table1.common_field = table2.common_field;
```
```SQL
SELECT  ID, NAME, AMOUNT, DATE
     FROM CUSTOMERS
     INNER JOIN ORDERS
     ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
#### 左联接
左链接返回左表中的所有记录，即使右表中没有任何满足匹配条件的记录。这就意味着，左连接会返回左表中的所有记录，加上右表中匹配到的记录，或者是 NULL （如果连接谓词无法匹配到任何记录的话）。
```SQL
SELECT table1.column1, table2.column2...
FROM table1
LEFT JOIN table2
ON table1.common_field = table2.common_field;
```
```SQL
SELECT  ID, NAME, AMOUNT, DATE
     FROM CUSTOMERS
     RIGHT JOIN ORDERS
     ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
#### 右联结
右链接返回右表中的所有记录，即是左表中没有任何满足匹配条件的记录。这就意味着，右连接会返回右表中的所有记录，加上左表中匹配到的记录，或者是 NULL （如果连接谓词无法匹配到任何记录的话）。
```SQL
SELECT table1.column1, table2.column2...
FROM table1
RIGHT JOIN table2
ON table1.common_field = table2.common_field;
```
#### 全联结
全连接将左连接和右连接的结果组合在一起。
```SQL
SELECT table1.column1, table2.column2...
FROM table1
FULL JOIN table2
ON table1.common_field = table2.common_field;
```
#### 笛卡尔联结
它相当于连接谓词总是为真或者缺少连接谓词的内连接。
```SQL
SELECT table1.column1, table2.column2...
FROM  table1, table2 [, table3 ]
```
## UNION、INTERSECT、EXCEPT
## 别名
#### 表别名
```SQL
SELECT C.ID, C.NAME, C.AGE, O.AMOUNT 
        FROM CUSTOMERS AS C, ORDERS AS O
        WHERE  C.ID = O.CUSTOMER_ID;
```
#### 列别名
```SQL
SELECT  ID AS CUSTOMER_ID, NAME AS CUSTOMER_NAME
     FROM CUSTOMERS
     WHERE SALARY IS NOT NULL;
```
## 索引
可以被数据库搜索引擎用来加速数据的检索。简单说来，索引就是指向表中数据的指针。
```SQL
CREATE INDEX index_name
ON table_name (column_name);

CREATE UNIQUE INDEX index_name
on table_name (column_name);

CREATE INDEX index_name
on table_name (column1, column2);

DROP INDEX index_name;
```
创建单列索引还是聚簇索引，要看每次查询中，哪些列在作为过滤条件的 WHERE 子句中最常出现。  
如果只需要一列，那么就应当创建单列索引。如果作为过滤条件的 WHERE 子句用到了两个或者更多的列，那么聚簇索引就是最好的选择。
## 视图
视图无非就是存储在数据库中并具有名字的 SQL 语句，视图可以包含表中的所有列，或者仅包含选定的列。视图可以创建自一个或者多个表，这取决于创建该视图的 SQL 语句的写法。  
视图是一种虚拟的表，允许用户执行以下操作：
- 以用户或者某些类型的用户感觉自然或者直观的方式来组织数据；
- 限制对数据的访问，从而使得用户仅能够看到或者修改（某些情况下）他们需要的数据；
- 从多个表中汇总数据，以产生报表。
```
CREATE VIEW CUSTOMERS_VIEW AS
SELECT name, age
FROM  CUSTOMERS;

SELECT * FROM CUSTOMERS_VIEW;


CREATE VIEW CUSTOMERS_VIEW AS
SELECT name, age
FROM  CUSTOMERS
WHERE age IS NOT NULL
WITH CHECK OPTION;
```
WITH CHECK OPTION 是 CREATE VIEW 语句的一个可选项。WITH CHECK OPTION 用于保证所有的 UPDATE 和 INSERT 语句都满足视图定义中的条件。
## SQL GROUP BY 子句
SQL GROUP BY 子句与 SELECT 语句结合在一起使用，可以将相同数据分成一组。
```
SELECT column1, column2
    FROM table_name
    WHERE [ conditions ]
    GROUP BY column1, column2
    ORDER BY column1, column2
```
```
SELECT NAME, SUM(SALARY) FROM CUSTOMERS
         GROUP BY NAME;
```
## SQL HAVING 子句
WHERE 子句对被选择的列施加条件，而 HAVING 子句则对 GROUP BY 子句所产生的组施加条件。
```
SELECT ID, NAME, AGE, ADDRESS, SALARY
FROM CUSTOMERS
GROUP BY age
HAVING COUNT(age) >= 2;
```
## 事物
事务具有以下四个标准属性：
- 原子性
- 一致性
- 隔离性
- 持久性
事物控制：
有四个命令用于控制事务：
- COMMIT：提交更改；
- ROLLBACK：回滚更改；
- SAVEPOINT：在事务内部创建一系列可以 ROLLBACK 的还原点；
- SET TRANSACTION：命名事务；

