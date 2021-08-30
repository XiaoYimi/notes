# 数据库 MySQL 基础

## 基础语法

### 数据库

#### 创建数据库

```mysql
CREATE DATABASE DB_NAME [IF NOT EXISTS];
```



#### 查看数据库

```mysql
SHOW DATABASE [LIKE 'CONDITION'];
```



#### 修改数据库

```
ALTER DATABASE DB_NAME []
```



#### 删除数据库

```mysql
DROP DATABASE [IF EXISTS] DB_NAME;
```



#### 使用数据库

```mysql
USE DB_NAME;
```



#### 查看表结构

```mysql
DESCRIBE DB_NAME;
```





### 数据表

#### 添加字段

```mysql
ALTER TABLE TABLE_NAME ADD FIELD_NAME FIELD_TYPE;
```



#### 修改字段类型

```mysql
ALTER TABLE TABLE_NAME MODIFY FIELD_NAME FIELD_TYPE;
```



#### 修改字段名称

```mysql
ALTER TABLE TABLE_NAME CHANGE OLD_FILED_NAME NEW_FIELD_NAME FIELD_TYPE;
```



#### 修改表名

```mysql
ALTER TABLE OLD_TABLE_NAME RENAME NEW_TABLE_NAME;
```



#### 删除表

```
DROP TABLE [IF EXISTS] TABLE_NAME;
```





### 约束

#### 主键约束

##### 创建单个主键

```mysql
CREATE TABLE user (
	id int(11) PRIMARY KEY,
);

CREATE TABLE user (
    id int(11),
 	primary key(id)
}
```



##### 创建复合主键

```mysql
CREATE TABLE user (
	id int(11),
    classId int(11),
    PRIMARY KEY(id, classId)
);
```



##### 修改表主键

```mysql
ALTER TABLE TABLE_NAME ADD PRIMARY KEY(FIELD_NAME);
```





#### 外键约束

##### 创建外键约束

```mysql
CREATE TABLE user (
	id int(11) PRIMARY KEY,
    classId int(11),
    FOREIGN KEY(CURRENT_TABLE_NAME_FIELD_NAME) REFERENCES MASTER_TABLE_NAME(MASTER_TABLE_NAME_FIELD_NAME);
);
```



##### 修改表外键

```mysql
ALTER TABLE CURRENT_TABLE_NAME ADD FOREIGN KEY (CURRENT_TABLE_NAME_FIELD_NAME)
	REFERENCES MASTER_TABLE_NAME(MASTER_TABLE_NAME_FIELD_NAME);
```



##### 删除表外键

```mysql
ALTER TABLE TABLE_NAME DROP FOREIGN KEY (TABLE_NAME_FIELD_NAME);
```





#### 唯一约束

```mysql
CREATE TABLE user (
	id int(11) PRIMARY KEY,
	name varchar(30) unique, // 唯一用户名
}
```



#### 默认约束

```mysql
CREATE TABLE user (
	id int(11) PRIMARY KEY,
	name varchar(30) unique, // 唯一用户名
    gender tinyint(1) default 0, // 默认0 (0-保密, 1-男, 2-女)
}
```



#### 非空约束

```mysql
CREATE TABLE user (
	id int(11) PRIMARY KEY,
	name varchar(30) unique, // 唯一用户名
    remark varchar(50) not null, // 非空约束
}
```





### 数据库函数

#### 数值型函数

##### ABS(N)  返回 N 的绝对值

##### MOD(N, M) 返回 N 被 M 除的余数

##### SORT(N) 返回非负数 N 的二次方根

##### SIGN(N) 返回 N 的数值符号,正1负-1



##### GREATEST (N1, N2, N3...)  返回数值集合中最大值

##### LEAST (N1, N2, N3...)  返回数值集合中最小值



##### CEIL(N) 返回 大于或等于 N 的整数位, N 为浮点数值

##### FLOOR(N) 返回等于 N 的整数位, N 为浮点数值



##### TRUNCATE(N, M) 返回保留 Y 位小数点的 N 值, N 为浮点数值(相当于截取)

##### ROUND(N, M) 返回保留 Y 位小数点的 N 值, N 为浮点数值(会进行四舍五入)

##### 

#### 字符创函数

##### LENGTH(str) 返回字符串的字段长度(中文x3，英文x1)



##### UPPER(str) 返回已全部转换为大写的字符串

##### LOWER(str) 返回已全部转换为小写的字符串



##### TIRM(str) 返回已去除字符串左右两边空格的字符串

##### LEFT(str, n)  返回字符串 str 的最左边 n 个字符

##### RIGHT(str, n) 返回字符串 str 的最右边 n 个字符

##### SUNSREING(str, n, m) 返回从指定位置 n 截取 m 长度的字符串

##### CONCAT(str1, str2, str3...) 返回由多个字符串拼接的字符串

##### REPLACE(str, old, new) 返回将 字符 old 替换成 字符 new 的字符串

##### 

#### 日期函数

##### NOW(),SYSDATE() 返回系统时间(格式: YYYY-MM-DD HH:mm:ss)

##### CURDATE(), CURRENT_DATE() 返回当前日期(格式: YYYY-MM-DD)

##### CURTIME(), CURRENT_TIME() 返回当前时钟(格式: HH:mm:ss)

##### DAYOFWEEK(), WEEKDAY() 返回当天在一周中的索引值(0-6)

##### DAYOFMONTH(datetime) 返回当前 datetime是一个月的第几天

##### DAYOFYEAR(datetime) 返回当前 datetime是一年中的第几天

##### DATEDIFF(datetime1, datetime2) 返回日期间的相差天数(取整)





#### 聚合函数

##### MIN(field) 返回指定列 field 集合的最小值

##### MAX(field) 返回指定列 field 集合的最大值

##### SUM(field) 返回指定列 field 集合的总数

##### AVG(field) 返回指定列 field 集合的平均值

##### COUNT(field) 返回指定列总行数



#### 自定义函数

##### 创建无参函数

```mysql
CREATE FUNCTION FUNCTION_NAME() RETURNS FIELD_TYPE RETURN CONTROLLER_EXPRESSION;
```



##### 创建有参函数

```mysql
CREATE FUNCTION FUNCTION_NAME(ARG1 ARG1_FIELD_TYPE, ARG2 ARG2_FIELD_TYPE...) RETURNS FIELD_TYPE RETURN CONTROLLER_EXPRESSION;
```

