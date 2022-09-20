# DDL--操作数据库



1.查询

```
show databases;
```
2.创建

创建数据库
```
create database 数据库名称;
```

创建数据库(判断，如果不存在则创建)
```
create database if not exists 数据库名称;
```

3.删除
删除数据库
```
drop database 数据库名称;
```
删除数据库(判断，如果存在则删除)
```
drop database if exists 数据库名称;
```

4.使用数据库

查看当前使用的数据库
```
select database();
```

使用数据库
```
use 数据库名称
```



# DDL  --操作表

查询表当前数据库下所有的表名称
```
show tablas;
```

查询表结构
```
desc 表名称;
```



创建表
```
create table 表名(
	字段名1 数据类型1,
	字段名2 数据类型2,
	...
	字段名n 数据类型n
);
```



删除表

```
drop table 表名;
```

删除表时判断表是否存在 

```
drop table if exists 表名;
```



修改表

1.修改表名

```
alter talbe 表名 rename to 新的表名;
```

2.添加一列

```
alter table 表名 add 列名 数据类型;
```



3.修改数据类型

```
alter table 表名 modify 列名 新的数据类型;
```



4.修改列名和数据类型

```
alter table 表名 change 列名 新列名 新数据类型;
```



5.删除列

```
alter table 表名 drop 列名;
```

