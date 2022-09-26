# SpringBoot

* SpringBoot是由Pivotall团队提供的全新框架，其设计目的是用来简化pring应用的初始搭建以及开发过程

* Springi程序缺点
  * 配置繁琐
  * 依赖设置繁琐
* SpringBoot程序优点
  * 自动配置
  * 起步依赖（简化依赖配置）
  * 辅助功能(内置服务器，…)

##  起步依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

* starter
  * pringBoot中常见项目名称，定义了当前项目使用的所有项目坐标，以达到减少依赖配置的目的
* parent
  * 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号(依赖管理，而非依赖)，以达到减少依赖冲突的目的
  * spring-boot-starter-parent(2.5.g)与spring-boot-starter-parent(2.4.6)共计57处坐标版本不同
* 实际开发
  * 使用任意坐标时，仅书写GAV中的G和A,V由SpringBoot提供
  * 如发生坐标错误，再指定version（要小心版本冲突）

## 基础配置

## Yaml

* YAML(YAML Ain't Markup Language),一种数据序列化格式
* 优点：
  * 容易阅读
  * 容易与脚本语言交互
  * 以数据为核心，重数据轻格式
* YAML文件扩展名
  * …ym1（主流）
  * yaml

* yaml语法规则

* 大小写敏感

* 属性层级关系使用多行描述，每行结尾使用冒号结束

* 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格(不允许使用Tb键)

* 属性值前面添加空格(属性名与属性值之间使用冒号+空格作为分隔)

* #表示注释

  

* 数组数据在数据书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔

```yaml
enterprise:
	nmae: itcast
	age: 16
	tel: 4006184000
	subject:
		- java
		- 前端
		- 大数据
```

## yaml数据读取

* 使用@Value读取单个数据，属性名应用方式:${一级属性名.二级属性名}

```java
    @Value("${lesson}")
    private String lesson;
```

* 封装全部数据到Environment对象

```java
    @Autowired
    private Environment environment;

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(environment.getProperty("lesson"));
        System.out.println(environment.getProperty("server.port"));
        System.out.println(environment.getProperty("enterprise.name"));
        System.out.println(environment.getProperty("enterprise.subject[1]"));
        System.out.println(environment.getProperty("enterprise.tel"));
        return "hello, spring boot!";
    }
```

* 自定义对象封装数据

```java
// 封装对象
@Component
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
    private String name;
    private Integer age;
    private String tel;
    private String[] subject;

    @Override
    public String toString() {
        return "Enterprise{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", tel='" + tel + '\'' +
                ", subject=" + Arrays.toString(subject) +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getTel() {
        return tel;
    }

    public void setTel(String tel) {
        this.tel = tel;
    }

    public String[] getSubject() {
        return subject;
    }

    public void setSubject(String[] subject) {
        this.subject = subject;
    }
}

// 注入对象
    @Autowired
    private Enterprise enterprise;

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(enterprise);
        return "hello, spring boot!";
    }
```

## 多环境开发

```yml
# 设置启用的环境
spring:
  profiles:
    active: test

---

# 开发
spring:
  profiles: dev

server:
  port: 80

---

# 生产环境
spring:
  profiles: pro

server:
  port: 81

---
# 测试
spring:
  profiles: test
server:
  port: 82
```

