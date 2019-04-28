# SQL学习
* MySQL的配置
* SQL分类
* 数据库管理
* python3与MySQL
* 创建计算字段
* 函数
* 分组
* 子查询
* 联结
* 组合查询
## Mysql的配置
一般可直接启动服务，不需要设置参数，若要修改默认值，则必须配置参数文件。
文件名类似于my.ini
## SQL分类
* DDL(Data Definition Language)语句：数据定义语言，常用关键字包括creat, drop, alter等
* DML(Data Manipulation Language)语句：数据操纵语言，用于添加，删除，更新和查询数据库记录，并检查数据完整性，包括insert, delete, update, select等
* DCL(Data Control Language)语句：数据控制语句，用于控制不同数据段直接的许可和访问，包括grant, revoke等
## 数据库管理
```
show databases;
# 创建名为test的数据库
create database test;
# 删除名为test的数据库
drop database test;
# 选择test为要操作的数据库
use test;
# 查看数据test下的表
show tables;
```
将样例表的脚本复制到命令行，创建课程所需要的表，插入记录
```
# 查看表的定义
desc products;
#查看创建表的sql语句
show create table products;
#查看表中的记录
select * from products;
```
## python3与MySQL
库: pymysql

命令行安装pymysql
```
$ pip install PyMySQL
```
## 创建计算字段
计算字段：利用存储在数据库表中的数据产生的新的字段
#### 拼接字段
例：创建由两列组成的标题，输出vend_name (vend_country)
```
SELECT Concat(vend_name, ' (', vend_country, ')')
FROM Vendors
ORDER BY vend_name;
```
使用别名，使客户端能够应用
```
SELECT Concat(vend_name, ' (', vend_country, ')') AS vend_title
FROM Vendors
ORDER BY vend_name;
```
#### 执行算术计算
例：汇总物品价格（单价乘以订购数）
```
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price
FROM OrderItems
Where order_num = 20008;
```
## 函数
不可移植性
#### 文本处理函数
* LEFT()
* LENGTH()
* LOWER()
* LTRIM() 去掉字符串左边的空格
* RIGHT()
* RTRIM()
* SOUNDEX() 将任何文本字符串转换为描述其语音表示的字母数字模式的算法
* UPPER()
```
SELECT vend_name, UPPER(vend_name) AS vend_name_upcase
FROM Vendors
ORDER BY vend_name;
```
#### 日期和时间处理函数
可移植性最差，参考文档
```
SELECT order_num
FROM Orders
WHERE YEAR(order_date) = 2012;
```
#### 数值处理函数
* ABS()
* COS()
* EXP()
* PI()
* SIN()
* SQRT()
* TAN()
#### 聚集函数
* AVG()
* COUNT()
* MAX()
* MIN()
* SUM()
```
SELECT AVG(prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```
只用于单个列，忽略列值为NULL的行
#### 聚集不同值
ALL或不指定参数，对所有行执行计算
DISTTINCT参数，只包含不同的值
```
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';
```
#### 组合聚集函数
SELECT语句可包含多个聚集函数
```
SELECT COUNT(*) AS num_items, MIN(prod_price) AS price_min, MAX(prod_price) AS price_max, AVG(prod_price) AS price_avg
FROM Products;
```
## 分组
GROUP BY子句和HAVING子句
#### 创建分组
例：各供应商有多少产品
```
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id;
```
#### 过滤分组
例：至少有两个订单的所有顾客
```
SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >=2;
```
WHERE过滤行，HAVING过滤分组
#### SELECT子句顺序
SELECT> FROM> WHERE> GROUP BY> HAVING> ORDER BY
#### 练习
检索包含3个或以上物品的订单号和订购物品的数目，按订购物品的数目和订单号排序输出
提示：OrderItems表
## 子查询
嵌套在其他查询的查询
#### 利用子查询进行过滤
例：列出订购物品RGAN01的所有顾客
```
SELECT cust_id 
FROM Orders
WHERE order_num IN (SELECT order_num FROM OrderItems WHERE prod_id = 'RGAN01');
```
想要进一步获得顾客信息
```
SELECT cust_name, cust_contact 
FROM Customers
WHERE cust_id IN (SELECT cust_id FROM Orders WHERE order_num IN (SELECT order_num FROM OrderItems WHERE prod_id = 'RGAN01'));
```
过多嵌套效率低，并非最佳方法，联结更佳
#### 作为计算字段使用子查询
例：显示Customers表中每个顾客的订单总数
```
SELECT cust_name, cust_state, (SELECT COUNT(*) FROM Orders WHERE Orders.cust_id = Customers.cust_id ) AS orders
FROM Customers
ORDER BY cust_name;
```
完全限定列名！
## 联结
SELECT能执行的最重要的操作,可以将存在多个表的信息通过联结用一条SELECT语句检索,比子查询快
#### 创建联结
```
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
```
两个表用WHERE子句联结，进行配对，不要忘记
#### 内联结
基于表的相等测试，又称等值联结
```
SELECT vend_name, prod_name, prod_price 
FROM Vendors INNER JOIN Products
ON Vendors.vend_id = Products.vend_id;
```
联结多个表，子查询的例子用联结完成，速度更快
```
SELECT cust_name, cust_contact
FROM Customers, Orders, OrderItems
WHERE Customers.cust_id = Orders.cust_id AND Orders.order_num = OrderItems.order_num AND OrderItems.prod_id = 'RGAN01';
```
#### 使用表别名
```
SELECT cust_name, cust_contact
FROM Customers AS C, Orders AS O, OrderItems AS OI
WHERE C.cust_id = O.cust_id AND O.order_num = OI.order_num AND OI.prod_id = 'RGAN01';
```
练习：显示订单20007的prod_name, vend_name, prod_price, quantity
#### 自联结
从相同的表中检索数据

例：找到Jim Jones同一公司的所有顾客
```
SELECT C1.cust_id, C1.cust_name, C1.cust_contact
FROM Customers AS C1, Customers AS C2
WHERE C1.cust_name = C2.cust_name AND C2.cust_contact = 'Jim Jones';
```
#### 自然联结
内联结相同的列可能出现多次，自然联结派出多次出现，每列只返回一次，但不怎么会用到
```
SELECT C.*, O.*
FROM Customers AS C, Orders AS O
WHERE C.cust_id = O.cust_id;
```
#### 外联结
联结包含了哪些在相关表中没有关联行的行

例：检索所有顾客及订单
```
#内联结
SELECT C.cust_id, O.order_num
FROM Customers AS C INNER JOIN Orders AS O
ON C.cust_id = O.cust_id;
#外联结
SELECT C.cust_id, O.order_num
FROM Customers AS C LEFT OUTER JOIN Orders AS O
ON C.cust_id = O.cust_id;
```
LEFT OUTER JOIN使用左边的表的所有行
#### 使用带聚集函数的联结
例：要检索所有顾客及每个顾客所下的订单数
```
SELECT C.cust_id, COUNT(O.order_num) AS num_ord
FROM Customers AS C LEFT OUTER JOIN Orders AS O
ON C.cust_id = O.cust_id
GROUP BY C.cust_id;
```
## 组合查询
利用UNION操作符将多条SELECT语句组成一个结果集

例：需要Illinois,Indiana和Michigan的所有顾客报表，以及不管哪个州的所有Fun4All
```
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IN', 'IL', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```
UNION自动去除了重复的行，UNION ALL可返回所有
```
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IN', 'IL', 'MI')
UNION ALL
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All';
```
当想对结果集排序，将ORDER BY放在最后面
```
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_state IN ('IN', 'IL', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM Customers
WHERE cust_name = 'Fun4All'
ORDER BY cust_name, cust_contact;
```
