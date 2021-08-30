# Koa 后台搭建





# MongoDB 手册

## 下载与安装

[mongodb.msi](https://www.mongodb.com/download-center/community)

OS: windows 64

Package: MSI



安装详见 [菜鸟教程](https://www.runoob.com/mongodb/mongodb-window-install.html)



## 配置

在 D 盘符下创建文件夹 data,并在文件夹 data 分别创建文件夹 db, log.



在 MongoDB 安装目录 bin 下执行以下命令:

```bash
mongod --dbpath 'd:\data\db'
```



在系统环境变量下 path 新建记录 (安装在D盘为例)

```bash
D:\Program Files\MongoDB\Server\4.2\bin
```



## 启动 MongoDB 服务

命令:

```bash
mongod
```

如果存在报错,请自行检查 (dbpath, port ...)





## MongoDB 操作符

```sql
>  ==>  $gt
<  ==>  $lt
>=  ==>  $gte
<=  ==>  $lte

/* 查询集合 custome_table 中文档字段 count > 100 的记录 */
db.custome_table.find({ count: { $gt: 100 } })
```



## MongoDB $type 操作符

```sql
/* $type 有以下类型和数值代表 */
Double  ==>  1
String  ==>  2
Object  ==>  3
Array  ==>  4
Binary data  ==>  5
Object id  ==>  7
Boolean  ==>  8
Date  ==>  9
Null  ==>  10
Regular Expression  ==>  11
JavaScript  ==>  13
Symbol  ==>  14
JavaScript (with scope)  ==>  15
32-bit interger  ==>  16
Timestamp  ==>  17
64-bit interger  ==>  18
Min key  ==>  255
Max key  ==>  127

/* 查询集合 custome_table 中文档字段 count 类型为字符串(2) 的记录 */
db.custome_table.find({ count: { $type: 'string' } })
db.custome_table.find({ count: { $type: 2 } })
```





## 常用命令

### 创建数据库

```sql
use custome_data
```



### 删除数据库

```sql
use custome_data

/* db 指代当前数据库 custome_data */ 
db.dropDatabase()
```



### 查看所有集合 (数据表)

```sql
show collections
```





### 创建集合 (数据表)

```sql
use custome_data

/* mongodb 只有向表中插入数据才会被创建 */
db.custom_table.insert({ t_name: 'custome_table' })
```



### 删除集合 (数据表)

```
use custome_data

db.custom_table.drop()
```



### 新建文档(数据)

```js
use custome_data

db.custome_table.create(document)
```





### 插入文档 (数据)

```sql
use custome_data

/* document 为 JSON 格式数据; 插入时会检测 _id 是否存在,存在则报错 _id 重复; 插入失败不保存数据 */
db.custome_table.insert(document)
/* 插入一条文档 */
db.custome_table.insertOne(document)
/* 插入一条或多条文档 */
db.custome_table.insertMary(document)
```



### 更新集合 (数据)

```sql
use custome_data

db.custome_table.update({
	$set: {
		/* 更新的条件 */
	}
}, {
	/* 更新的字段复制操作 */
}, { multi: false, /* 是否更新多条 */ })
```



### 删除集合 (数据)

```sql
use custome_data

/* remove 已过时 */
db.custome_table.remove({
	/* 删除的条件 */
}, { justOne: false, /* 是否删除匹配的全部记录 */ })

/* 删除一个文档 */
db.custome_table.deleteOne({ /* 删除的条件 */ })

/* 删除全部文档 */
db.custome_table.deleteMary({})

/* 删除指定条件的所有文档 */
db.custome_table.deleteMary({ name: 'xiaoyimi' })
```



### 查询文档 (数据)

```sql
use custome_data

/* 查询一个指定条件的文档 */
db.custome_table.findOne({ /* 查询条件 */ })

/* 查询所有文档 */
db.custome_table.find({})

/* 查询指定条件的所有文档 */
db.custome_table.find({ /* 查询条件 */ })
```



### limit 条数与 skip 条数

```sql
/* limit 指定数量记录, skip 跳过指定数量数据 */

use custome_data

/* 查询所有文档的前 5 条记录 */
db.custome_table.find({}).limit(5)

/* 查询所有文档,除去前 4 条文档 */
db.custome_table.find({}).skip(4)
```



### 集合排序 (sort)

```sql
use custome_data

/* 查询所有文档,并按指定字段 key 进行排序(1: 升序, -1: 降序) */
db.custome_table.find({}).sort({ key: 1 | -1 })

/* limit, skip, sort 的执行顺序: sort(), skip(), limit() */ 
```



### 创建索引

```sql
use custome_data

/* 创建索引字段,并指定升|降序 */
db.custome_table.createIndex({ key: 1 | -1 })
```



### 聚合方法 (aggregate)

- 聚合:  相当于对数据进行分组统计.
- 管道: 将输出结果作为下一个命令的参数 .



```sql
use custome_data

/* 将所有文档文组进行分组 (filed),并统计输出 新字段 (new_filed) */
db.custome.aggregate([{
	$group: {
		field: '$key1',
		new_field2: '$key2'
	}
}])

/* 聚合方法 其它方法 */
$sum  ==>  计算总和
$avg  ==>  计算平均值
$min  ==>  计算最小值
$max  ==>  计算最大值
$push  ==>  在结果文档中插入一个值到数组
$first  ==>  根据资源文的排序件获取第一个文档数据
$last  ==> 根据资源文件的排序获取最后一个文档数据

/* 对字段 key1 进行分组, 算计分组里字段 count 的总数 */
db.custome.aggregate([{
	$group: {
		field: '$key1',
		new_field2: { $sum: '$count' }
	}
}])

/* 管道: 将输出结果作为下一个命令的参数 */
/* 管道方法 其它方法 */

$project  ==>  修改文档结构(1 显示字段, 0 隐藏字段)
$match  ==>  过滤数据
$limit  ==>  限制返回的文档数
$skip  ==>  跳过指定文档数,返回余下文档数
$group  ==>  将集合分组,可用于统计
$sort  ==>  将文档排序

/* 对字段 count > 60 进行过滤, 并返回指定字段 (name, count) 的所有文档 */
db.custome.aggregate([{
	$match: { count: { $gt: 60 } },
	$projects: {
		_id: 0,
		name: 1,
		count: 1
	}
}])

/* 理解管道
	$match 进行过滤后返回文档，将这些文档作为下一个管道命令的参数进行处理;
	$projects 接收来自 $match 处理后的结果,继续管道命令操作
*/

/* 跳过前 5 个文档,返回余下所有文档 */
db.custome.aggregate([{ $skip: 5 }])
```





### MongoDB 复制



### MongoDB 分片



### MongoDB 备份及恢复

#### MongoDB 备份 (mongodump)

```bash
# -h 数据库服务器地址(可以指定端口号)
# -d 数据库实例名称
# -o 备份的数据存放地址

mongodump -h dbhost -d dbname -o dbdirectory

# --dbpath 数据库服务器地址
# --out 备份的数据存放地址

mongodump --dbpath dbpath -out backupdirectory

# 将指定某数据库的某个集合备份
# --collection 指定备份的集合
# --db 指定备份的数据库
mongodump --collection COLLECTION --db DB_NAME
```



#### MongoDB 恢复 (mongorestore)

```bash
# -h 数据库服务器地址
# -d 需要恢复的数据库实例
# --drop 先删除当前数据,再恢复备份的数据 (慎用).
# <path> 备份的数据位置,不能与 --dir 同时使用.
# --dir 备份数据的目录,不能与 --dir 同时使用.

mongorestore -h <hostname><:port> -d dbname <path>
```





### MongoDB 监控

- mongostat 是 mongodb 自带的状态监测工具; 可进行查看数据库操作变量或其它问题.
- mongotop 是 mongodb 内置工具;可进行跟踪 MongoDB 实例 的读取或写入情况.



#### mongostat 命令

```bash
# 启动 mongod 服务, 开启 mongostat 状态监测.

# bash 1
mongod

# bash 2
mongostat
```



#### mongotop 命令

```bash
# 启动 mongod 服务, 开启 mongotop 描述 MongoDB 实例的读写状态.

# bash 1
mongod

# bash 2 (3 情况)
# demo1
mongotop

# demo2, 间隔 10 s
mongotop 10

# demo3 查看 lock (锁) 的使用情况
mongotop --locks
```





## MongoDB 进阶

### MongoDB 关系

- 1 : 1 (一对一)

- 1 : N (一对多)

- N : 1 (多对一)

- N : N (多对多)



#### 嵌入式关系





#### 引用式关系







# Redis 手册

## 下载与安装

[Redis 地址.MSI](https://github.com/MSOpenTech/redis/releases)



## 启动服务

命令:

```bash
redis-server.exe redis.windows.conf
```



若报错: Creating Server TCP listening socket 127.0.0.1:6379: bind: No error

请通过以下命令解决:

```bash
redis-cli.exe
	shutdown
	exit

redis-server.exe redis.windows.conf
```



## 客户端命令

### 进入客户端面板

```bash
redis-cli
```



### 命令操作

```bash
# 设置(存储)键值对
SET key value

# 获取键值对
GET key

# 删除键值对
DEL key

# 检查键值对是否存在
EXISTS key

# 系列化键值对
DUMP key
```

