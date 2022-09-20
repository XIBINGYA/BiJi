# Mybatis

* MyBatis是一款优秀的特久层框架，用于简化JDBC开发
* MyBatis本是Apache的一个开源项目iBatis,2010年这个项目由apache software
  foundation迁移到了google code,并且改名为MyBatis.2013年11月迁移到Github
* 官网：https:l/mybatis..orq/mybatis-3/zh/index.html



## 持久层

* 负责将数据保存到数据库的那一层代码
* JavaEE三层架构:表现层，业务层，持久层



## 框架

* 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展



## Mybatis 简化

![image-20220607143718707](.\Mybatis.assets\image-20220607143718707.png)

![image-20220607144157264](.\Mybatis.assets\image-20220607144157264.png)

java:代码

```java
        // 1.加载mybatis的核心配置文件，获取SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        // 2.获取SqlSession对象,用来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 3.执行sql
        List<User> users = sqlSession.selectList("test.selectAll");
        
        // 处理结果
        System.out.println(users);
        // 释放资源
        sqlSession.close();
```

***Mapper.xml 代码:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.BrandMapper">

        <resultMap id="brandResultMap" type="Brand">
            <result column="brand_name" property="brandName"/>
            <result column="company_name" property="companyName"/>
        </resultMap>

    <!--查询全部-->
    <select id="selectAll" resultMap="brandResultMap">
            select
                id, brand_name, company_name, ordered, description, status
            from tb_brand;
        </select>
<!--    通过id查询-->
        <select id="selectById" resultMap="brandResultMap">
            select *
            from tb_brand
            where id
                      <![CDATA[
                         =
                      ]]>
                  #{id};

        </select>
    <select id="selectByCondition"  resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
              <if test="status != null">
                  status = #{status}
              </if>
              <if test="companyName != null and companyName != ''">
                    and company_name like #{companyName}
              </if>
              <if test="brandName != null and brandName != '' ">
                  and brand_name like #{brandName}
              </if>
        </where>
    </select>





<!--    <select id="selectByConditionSingle" resultMap="brandResultMap">-->
<!--        select *-->
<!--        from tb_brand-->
<!--        <where>-->
<!--        <choose> &lt;!&ndash; 类似switch &ndash;&gt;-->
<!--            <when test="status != null">  &lt;!&ndash; 类似case &ndash;&gt;-->
<!--                 status = #{status}-->
<!--            </when>-->
<!--            <when test="companyName != null and companyName != ''">&lt;!&ndash; 类似case &ndash;&gt;-->
<!--                companyName like #{companyName}-->
<!--            </when>-->
<!--            <when test="brandName != null and brandName != ''">&lt;!&ndash; 类似case &ndash;&gt;-->
<!--                 brandName like #{brandName}-->
<!--            </when>-->

<!--&lt;!&ndash;            <otherwise>    &lt;!&ndash; 类似于default &ndash;&gt;&ndash;&gt;-->
<!--&lt;!&ndash;                1=1&ndash;&gt;-->
<!--&lt;!&ndash;            </otherwise>&ndash;&gt;-->
<!--        </choose>-->
<!--        </where>-->
<!--    </select>-->

    <select id="selectByConditionSingle"  resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <choose>
                <when test="status != null">
                    status = #{status}
                </when>
                <when test="companyName != null and companyName != ''">
                    company_name like #{companyName}

                </when>
                <when test="brandName != null and brandName != ''">
                    brand_name like #{brandName}
                </when>
            </choose>

        </where>
    </select>


<!--    添加-->
    <insert id="add">
        insert into tb_brand (brand_name, company_name, ordered, description, status)
        values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
    </insert>


<!--    删除一个-->
    <delete id="deleteById">
        delete from tb_brand where id = #{id}

    </delete>

<!--    批量删除-->
    <delete id="deleteByIds">
        delete from tb_brand
        where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>

</mapper>
```

mybatis-config.xml 代码

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 数据库连接信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybatis?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!-- 加载sql映射文件 -->
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```



## Mapper 代理开发

* 目的
  * 解决原生方式中的硬编码
  * 简化后期执行SQL

![image-20220607152829906](.\Mybatis.assets\image-20220607152829906.png)

步骤:

![image-20220607154606422](C:\Users\89867\Desktop\笔记（各种笔记）\Java\MyBatis\Mybatis.assets\image-20220607154606422.png)





参数占位符:

* 参数占位符：

  * 1.#{}:会将其替换为？，为了防止S0L注入

  * 2.${}:拼sqL,会存在SQL注入问题
  * 3.使用时机：
	  * 参数传递的时候：#{}
    * 表名或者列名不固定的情况下：${}
  
* ```xml
  <select id="selectById" resultMap="brandResultMap">
        select * 
       	from 
      		tb_brand
      	where id 
              <![CDATA[
                  =
              ]]>
                 #{id};
  
  </select>
  
  ```

## 条件查询

多条件查询

![image-20220608082121554](.\Mybatis.assets\image-20220608082121554.png)

![image-20220608084422401](.\Mybatis.assets\image-20220608084422401.png)

```java
    @Test
    public void selectByCondition() throws Exception{
        int status = 1;
        String brandName = "华为";
        String companyName = "华为";

        brandName = "%" + brandName + "%";
        companyName = "%" + companyName + "%";

        // 1.加载mybatis的核心配置文件，获取SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        SqlSession sqlSession = sqlSessionFactory.openSession();

        BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);


        List<Brand> list = brandMapper.selectByCondition(status, companyName, brandName);

//        Brand brand = new Brand();
//        brand.setStatus(1);
//        brand.setCompanyName("华为技术有限公司");
//        brand.setBrandName("华为");
//        List<Brand> list = brandMapper.selectByCondition(brand);


        System.out.println(list);

        sqlSession.close();
}
```

## 动态条件查询

![image-20220608084648763](.\Mybatis.assets\image-20220608084648763.png)

动态条件查询

* if:条件判断
  * test:逻辑表达式

* 问题：
  * 恒等式
  * <where>替换where关键字

## 单条件-动态条件查询

![image-20220608085855809](.\Mybatis.assets\image-20220608085855809.png)



## 添加

![image-20220608092306902](.\Mybatis.assets\image-20220608092306902.png)

主键返回

![image-20220608093604909](.\Mybatis.assets\image-20220608093604909.png)





## 修改

![image-20220608093911860](.\Mybatis.assets\image-20220608093911860.png)



动态修改

![image-20220608094116099](.\Mybatis.assets\image-20220608094116099.png)

## 删除

![image-20220608095147957](.\Mybatis.assets\image-20220608095147957.png)

批量删除

![image-20220608095642048](.\Mybatis.assets\image-20220608095642048.png)



## 参数传递

![image-20220608100647269](.\Mybatis.assets\image-20220608100647269.png)



![image-20220608101627918](.\Mybatis.assets\image-20220608101627918.png)





## 注解开发（完成一些简单的操作)

![image-20220608101753798](.\Mybatis.assets\image-20220608101753798.png)