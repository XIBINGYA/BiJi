# SpringFramework系统架构

![image-20220917154425848](..\.imgs\image-20220917154425848.png)

* IoC(Inversion of Control)控制反转
  * 使用对象时，由主动ew产生对象转换为由外部提供对象，此过程中对象创建控制权由程序转移到```外部```，此思想称为控制反转
* Spring技术对Ioc思想进行了实现
    * Spring提供了一个容器，称为IoC容器，用来充当IoC思想中的``` 外部 ```
    * Ioc容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为Bean
* DI(Dependency Injection)依赖注入
    * 在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

![image-20220917155206274](..\.imgs\image-20220917155206274.png)

* 目标：充分解耦
  * 使用IoC容器管理bean(Ioc)
  * 在Ioc容器内将有依赖关系的bean进行关系绑定(DI)
* 最终效果
  * 使用对象时不仅可以直接从Ioc容器中获取，并且获取到的bean已经绑定了所有的依赖关系



## IoC入门案例思路分析

* 1.管理什么？(Service.与Dao)
* 2.如何将被管理的对象告知IoC容器？（配置）
* 3.被管理的对象交给IoC容器，如何获取到IoC容器？（接口）
* 4.Ioc容器得到后，如何从容器中获取bean?（接口方法）
* 5.使用Spring导入哪些坐标？(pom.xml)

![image-20220917160658442](..\.imgs\image-20220917160658442.png)

![image-20220917160708207](..\.imgs\image-20220917160708207.png)

![image-20220917160715510](..\.imgs\image-20220917160715510.png)

![image-20220917160733265](..\.imgs\image-20220917160733265.png)

## DI入门案例思路分析

* 1.基于IoC管理bean
* 2.Service中使用new形式创建的Dao对象是否保留？（否）
* 3.Service中需要的Dao对象如何进入到Service中？（提供方法）
* 4.Service与Dao间的关系如何描述？（配置）

![image-20220917161822752](..\.imgs\image-20220917161822752.png)

![image-20220917161834415](..\.imgs\image-20220917161834415.png)

![image-20220917161942551](..\.imgs\image-20220917161942551.png)

## Bean基本配置

![image-20220917162304986](..\.imgs\image-20220917162304986.png)

### Bean别名

![image-20220917164632337](..\.imgs\image-20220917164632337.png)

### bean作用范围

![image-20220917164956370](..\.imgs\image-20220917164956370.png)

* 为什么bean默认为单例？
  * 适合交给容器进行管理的bean
    * 表现层对象	
    * 业务层对象
    * 数据层对象
    * 工具对象
  * 不适合交给容器进行管理的bean
    * 封装实体的域对象

# setter注入--简单类型

![image-20220917192956042](..\.imgs\image-20220917192956042.png)

## 构造器注入



![image-20220917194056053](..\.imgs\image-20220917194056053.png)



![image-20220917194104823](..\.imgs\image-20220917194104823.png)

* 依赖注入方式选择
  * 1.强制依赖使用构造器进行，使用setteri注入有概率不进行注入导致nu11对象出现
  * 2.可选依赖使用setteri注入进行，灵活性强
  * 3.Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
  * 4.如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用sette注入完成可选依赖的注）
  * 5.实际开发过程中还要根据实际情况分析，如果受控对象没有提供sette方法就必须使用构造器注入
  * 6. 自己开发的模块推荐使用setteri注入

## 自动装配

![image-20220917195900893](..\.imgs\image-20220917195900893.png)

## 加载properties文件

![image-20220918112437823](..\.imgs\image-20220918112437823-16634714965881.png)

![image-20220918112937226](..\.imgs\image-20220918112937226.png)

# 创建容器

![image-20220918113819781](.\SpringFramework系统架构.assets\image-20220918113819781.png)

# 获取bean

![image-20220918113837343](.\SpringFramework系统架构.assets\image-20220918113837343.png)

# 注解开发定义Bean



![image-20220918120113240](C:\Users\89867\Desktop\笔记（各种笔记）\Java\Spring\SpringFramework系统架构.assets\image-20220918120113240.png)

# 纯注解开发

![image-20220918142906037](.\SpringFramework系统架构.assets\image-20220918142906037.png)

![image-20220918142925636](.\SpringFramework系统架构.assets\image-20220918142925636.png)

![image-20220918143044370](C:\Users\89867\Desktop\笔记（各种笔记）\Java\Spring\SpringFramework系统架构.assets\image-20220918143044370.png)

## bean作用范围

![image-20220918144156596](.\SpringFramework系统架构.assets\image-20220918144156596.png)

## bean的生命周期

![image-20220918144220681](.\SpringFramework系统架构.assets\image-20220918144220681.png)

## 依赖注入

![image-20220918145018480](.\SpringFramework系统架构.assets\image-20220918145018480.png)

![image-20220918145034357](.\SpringFramework系统架构.assets\image-20220918145034357.png)

![image-20220918145134638](.\SpringFramework系统架构.assets\image-20220918145134638.png)

![image-20220918145502340](.\SpringFramework系统架构.assets\image-20220918145502340.png)

## 管理第三方Bean



![image-20220918153323037](.\SpringFramework系统架构.assets\image-20220918153323037.png)

## 第三方依赖注入

![image-20220918153644442](.\SpringFramework系统架构.assets\image-20220918153644442.png)

![image-20220918153700980](.\SpringFramework系统架构.assets\image-20220918153700980.png)

## Xml配置对比注解配置

![image-20220918153833159](.\SpringFramework系统架构.assets\image-20220918153833159.png)