# SQL学习
* MySQL的配置
* SQL分类
* 数据库管理
* python3与MySQL
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

