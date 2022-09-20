# 基础查询

1.查询多个字段

```
select 字段列表 from 表名;
select * from 表名; -- 查询所有数据(不建议使用)
```

2.去除重复记录

```
select distinct 字段列表 from 表名;
```

3.其别名

```
as: as也可以省略
```



# 条件查询(where)

1.条件查询语法

```
select 字段列表 from 表名 where 条件列表;
```

![image-20220604163304297](.\DQL.assets\image-20220604163304297.png)

# 排序查询(order by)

排序查询语法

```
select 字段列表 from 表名 order by 排序字段名1 [排序方式]
```

排序方式：

* ASC:升序排序(默认值)
* DESC:降序排序，

注意:如果有多个排序条件，当前边的条件值一样时，才会根据第二条件进行排序



# 分组查询(group by)

聚合函数

  * 概念

    	 * 将一列数据作为一个整体，进行纵向计算。 

* 集合函数分类:

  * ![image-20220605112711117](.\DQL.assets\image-20220605112711117.png)

* 集合函数语法

  * ```
    select 集合函数名(列名) from 表名;
    ```

    注意：null 值不参与所有聚合函数运算

分组查询

​	语法
```
select 字段列表 from 表名 [where 分组钱条件限定] group by 分组字段名 [HAVING 分组后条件过滤];
```

注意:分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

where和having区别：

 * 执行时机不一样：where是分组之前进行限定，不满足where条件，则不参与分组，而having是分组之后对结果进行过滤，

 * 可判断的条件不一样：where不能对聚合函数进行判断，having可以。

   执行顺序：where>聚合函数>having



# 分页查询

语法:

```
select 字段列表 from 表名 limit 起始索引,查询条目数;
```

* 起始索引:从0开始
* 计算公式：起始索引=(当前页码-1)*每页显示的条数

* tips:
   	* 分页查询limit是MySQL数据库的方言
   
   * Oracle分页查询使用rownumber
  * SQL Server分页查询使用top