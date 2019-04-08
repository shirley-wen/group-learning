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
基本算术操作符：+，-，*，/
## 函数
