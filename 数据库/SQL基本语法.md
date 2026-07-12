# MySQL打开
1，MySQL 8.0 Command Line Client
2,cmd中，mysql \[-h 127.0.0.1\] \[-P 3306\] -u root -p

# 数据类型
****数值类型、字符串类型、日期时间类型***
## 数据类型
**无符号写 tinyint unsigned**
- TINYINT(1 byte)
- SMALLINT(2 bytes)
- MEDIUMINT(3 bytes)
- INT 或 INTEGER(4 bytes)
- BIGINT(8 bytes)
- FLOAT(4 bytes)
- DOUBLE(8 bytes)
- DECIMAL(依赖于M(精度)和D(标度)的值)
	无符号数: tinyint unsigned
	浮点数(以100.0 为最高):double([^1]4,[^2]1)


## 字符串类型
- CHAR(0-255 bytes)  定长字符串
- VARCHAR(0-65535 bytes) 变长字符串
- TINYBLOB(0-255 bytes) 不超过255个字符的二进制数据
- TINYTEXT(0-255 bytes)  短文本字符串
- BLOB(0-65535 bytes) 二进制形式的长文本数据
- MEDIUMBLOB(0-16777215 bytes)二进制形式的中等长度文本数据
- MEDIUMTEXT(0-16777215 bytes)中等长度文本数据
- LANGBLOB(0-4294967295 bytes)二进制形式的极大文本数据
- LANGTEXT(0-4294967295 bytes)极大文本数据

## 日期时间类型

| 类型        | 大小  | 范围                                        | 格式                  | 描述           |
| --------- | --- | ----------------------------------------- | ------------------- | ------------ |
| DATE      | 3   | 1000-01-01 至 9999-12-31                   | YYYY-MM-DD          | 日期值          |
| TIME      | 3   | -838:59:59 至 838:59:59                    | HH:MM:SS            | 时间值或持续时间     |
| YEAR      | 1   | 1901 至 2155                               | YYYY                | 年份值          |
| DATETIME  | 6   | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM_DD HH:MM:SS | 混合日期和时间值     |
| TIMESTAMP | 4   | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |



# SQL分类
- DDL(Data Definition Language):数据定义语言，用来定义数据库对象（数据库，表，字段）
- DML(Data Manipulation Language):数据操作语言，用来对数据库表中的数据进行增删改
- DQL(Data Query Language):数据查询语言，用来查询数据库中表的记录
- DCL(Data Control Language):数据控制语言，用来创建数据库用户、控制数据库的访问权限
## DDL(Data Definition Language)
### 数据库操作
#### 查询
```
SHOW DATABASES;   查询所有数据库
SELEST DATABASES();  查询当前数据库
```
#### 创建
```
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则]
#用utf-8 则填入utf8mb4
```
#### 删除
```
DROP DATABASE [IF EXISTS] 数据库名;
```
#### 使用
```
USE 数据库名;
```

### 表操作
#### 查询
```
SHOW TABLES;         查询当前数据库所有表
DESC 表名;              查询表结构
SHOW CREATE TABLE 表名;          查询指定表的建表语句
```
#### 创建
```
CREATE TABLE 表名(
	字段1  字段1类型[ COMMENT 字段1注释],
	字段2  字段2类型[ COMMENT 字段2注释],
	字段3  字段3类型[ COMMENT 字段3注释],
	.....
	字段n  字段n类型[ COMMENT 字段n注释]
)

例
create table tb_user(
	id int comment '编号',
	name varchar(50) comment '姓名',
	age int comment '年龄',
	gender varchar(1) comment '性别'
) comment '用户表';
```

#### 修改
##### 添加字段
```
ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
```
##### 修改字段
```
#修改数据类型
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
#修改字段名和字段类型
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
```
##### 删除字段
```
ALTER TABLE 表名 DROP 字段名;
```
##### 修改表名
```
ALTER TABLE 表名 RENAME TO 新表名;
```
##### 删除表
```
#删除表
DROP TABLE [IF EXISTS] 表名;
#删除指定表，并重新创建该表
TRUNCATE TABLE 表名;
```



[^1]: 精度:整体长度,如100.0有四位

[^2]: 标度:小数位长度
