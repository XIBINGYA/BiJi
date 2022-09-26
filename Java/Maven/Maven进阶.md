# 依赖传递



* 依赖具有传递性
  * 直接依赖:在当前项目中通过依赖配置建立的依赖关系
  * 间接依赖:被资源的资源如果依赖其他资源，当前项目间接依赖其他资源

![image-20220924103700785](.\Maven进阶.assets\image-20220924103700785.png)

## 依赖传递冲突问题

* 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
* 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
* 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

![image-20220924103959933](.\Maven进阶.assets\image-20220924103959933.png)

* 可选依赖(不给)
  * 是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性
  * <optional> true</optional>

* 排除依赖(不要)
  * 是隐藏当前对应的依赖关系
  * <exclusions>
    * <exclusion>
      * <groupId>org.mybatis</groupId>
      * <artifactId>mybatis</artifactId>
    * </exlusion>
  * </exclsions>

# 继承与集合

## 聚合

* 聚合：将多个模块组织成一个整体，同时进行项目构建的过程称为聚合
* 聚合工程：通常是一个不具有业务功能的“空”工程(有且仅有一个pom文件)
* 作用：使用聚合工程可以将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建
  * 当工程中某个模块发生更新（变更）时，必须保障工程中与已更新模块关联的模块同步更新，此时可以使用聚合工程来解决批量模块同步构建的问题

![image-20220924105143805](.\Maven进阶.assets\image-20220924105143805.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>maven_JuHe01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <!--聚合模块 -->
    <modules>
        <module>../SpringMVC03_ssm</module>
        <module>../maven_mapper</module>
        <module>../maven_pojo</module>
    </modules>

</project>
```

## 继承

* 在子模块中继承父模块

* 概念：继承描述的是两个工程间的关系，与jva中的继承相似，子工程可以继承父工程中的配置信息，常见于依赖关系的继承

* 作用：

  * 简化配置

  * 减少版本冲突

## 聚合与继承的区别
* 作用
  * 聚合用于快速构建项目
  * 继承用于快速配置

* 相同点：
  * 聚合与继承的pom.xml文件打包方式均为pom,可以将两种关系制作到同一个pom文件中
  * 聚合与继承均属于设计型模块，并无实际的模块内容
* 不同点：
  * 聚合是在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己





## 版本管理

* 工程版本：
  * SNAPSHOT（快照版本）
    * 项目开发过程中临时输出的版本，称为快照版本
    * 快照版本会随着开发的进展不断更新
  * RELEASE（发布版本）
    * 项目开发到进入阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构件文件是稳定的，即便进行功能的后续开发，也不会改变当前发布版本内容，这种版本称为发布版本
* 发布版本
  * alpha版
  * beta版
  * 纯数字版

## 多环境开发与应用

### 多环境开发

* maven提供配置多种环境的设定，帮助开发者使用过程中快速切换环境

```xml

    <profiles>
        <!-- 开发 -->
        <profile>
            <id>env_dep</id>
            <properties>
            </properties>
            <!-- 默认启动环境 -->
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <!-- 发布  -->
        <profile>
            <id>env_pro</id>
            <properties>
            </properties>
            <!-- 默认启动环境 -->
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>

        <!-- 测试 -->
        <profile>
            <id>env_test</id>
            <properties>
            </properties>
            <!-- 默认启动环境 -->
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
    </profiles>

```

### 跳过测试

* 应用场景

  * 功能更新中并且没有开发完毕

  * 快速打包

  * ...

```xml
    <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.22.1</version>
        <configuration>
            <skipTests>true</skipTests><!--设置跳过测试-->
            <includes><!--包含指定的测试用例-->
                <include>**/User*Test.java</include>
            </includes>
            <excludes><!--排除指定的测试用例-->
                <exclude>**/User*TestCase.java</exclude>
            </excludes>
        </configuration>
    </plugin>
```

# 私服

* 私服介绍
  * 私服是一台独立的服务器，用于解决团队内部的资源共享与资源同步问题
* Nexus
  * Sonatype公司的一款maven私服产品
  * 下载地址：https://help.sonatype.com/repomanager3/download

## Nexus安装与启动

* 启动服务器（命令行启动）
  ```
  nexus.exe /run nexus
  ```

* 访问服务器(默认端口：8081)

  ```
  http://localhost:8081
  ```

* 修改基础配置信息

  * 安装路径下etc目录中nexus-default.properties文件保存有nexus.基础配置信息，例如默认访问端口
* 修改服务器运行配置信息

    * 安装路径下bin目录中nexus,vmoptions文件保存有nexus服务器启动对应的配置信息，例如默认占用内存空间

## 私服仓库分类

| 仓库类别 | 英文名称 | 功能                    | 关联操作 |
| -------- | -------- | ----------------------- | -------- |
| 宿主仓库 | hosted   | 保存自主研发+第三方资源 | 上传     |
| 代理仓库 | proxy    | 代理连接中央仓库        | 下载     |
| 仓库组   | group    | 为仓库编组简化下载操作  | 下载     |

```xml
    <!--置当前工程保存在私服中的具体位置 -->
    <distributionManagement>
        <repository>
            <id>itheima-release</id>
            <url>http://localhost:8081/repository/itheima-release/</url>
        </repository>
        <snapshotRepository>
            <id>itheima-snapshot</id>
            <url>http://localhost:8081/repository/itheima-snapshot/</url>
        </snapshotRepository>
    </distributionManagement>
```

