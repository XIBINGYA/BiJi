# DML(操作表)

添加数据

1.给指定的铁添加数据

```
insert into 表名(列名1，列名2,...) values(值1,值2,...);
```

2.给全部列添加数据

```
insert into 表名 values(值1，值2,...);
```

3.批量添加数据

```mysql
insert into 表名(列名1，列名2,...) values(值1，值2,...),(值1，值2,...),(值1，值2...)...;
```



```mysql
insert into 表名 values(值1,值2,...),(值1,值2,...),(值1,值2,...)...;
```



修改数据

```mysql
update 表名 set 列名1=值1，列名2=值2，... [where 条件];
```

​	注意：修改语句中如果不加条件，则将所有数据都修改!



删除数据

1.删除数据

```mysql
delete from 表名 [where条件];
```





如：

​	

```
-- 创建用户1 tcast,只能够在当前主机Localh0st访问，密码123456:
create user 'itcast'@'localhost' identified by '123456';

-- 创建用户he1na,可以在任意主机访问该数据岸，密码123456:
create user 'heima'@'%' identified by '123456';

-- 修改用户he1ma的访问密码为1234;
alter user 'heima'@'%' IDENTIFIED with mysql_native_password BY '666666';

-- 删除itcast0 Localhost用户
drop user 'heima'@'%';
```
