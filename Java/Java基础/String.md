# String字符串

​	

* ### String是什么,可以做什么？
  
  * 字符串类型，可以定义字符串变量指向字符串对象



 * ### String是不可变字符串的原因
      
      * String变量每次的修改其实都是产生并指向了新的字符串对象
      * 原来的字符串对象都是没有改变的，所以称为不可变字符



## 创建字符串	

​		双引号创建字符串对象，在字符串常量池中存储同一个 

​		通过new构造器创建字符串对象，在堆内存中分开存储

​	



# API

​	String类型比较不适合用==号

​	比较字符串API	

| 方法名                                                 | 说明                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| public boolean equals (Object anObject)                | 将此字符串雨指定对象进行比较。只关心字符串内容是否一致!      |
| public boolean equalsIgnoreCase (String anotherString) | 将此字符串雨指定对象进行比较，忽略大小写比较字符串。只关心字符串内容是否一致! |



	## 常用API

​	

| 方法名                                                       | 说明                                                     |
| ------------------------------------------------------------ | :------------------------------------------------------- |
| public int length（）                                        | 返回此字符串的长度                                       |
| public char charAt(int index)                                | 获取某个索引位置处的字符                                 |
| public char[] toCharArray（）:                               | 将当前字符串转换成字符数组返回                           |
| public String substring(int beginIndex,int endIndex)         | 根据开始和结束索引进行截取，得到新的字符串（包前不包后） |
| public String substring(int beginIndex)                      | 从传入的索引处截取，截取到末尾，得到新的字符串           |
| public String replace(CharSequence target,CharSequence replacement) | 使用新值，将字符串中的旧值替换，得到新的字符串           |
| public String[] split(String regex)                          | 根据传入的规则切割字符串，得到字符串数组返回             |



# 案例

 	### 	验证码		

```java
 String str = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
 String yzm = "";
  // 验证码 生成一个5位的验证码
 Random rd = new Random();
 for (int i = 0; i < 5; i++) {
 	int num = rd.nextInt(str.length());
    yzm += str.charAt(num);
 }

```



		### 手机号码屏蔽

​		

```

//屏蔽手机号码(中间四位)
 Scanner sc = new Scanner(System.in);
 System.out.println("请输入你的手机号码");
 String phoneNumber = sc.next();
 // 得到手机号码的前三位和后四位
 String firstNumber = phoneNumber.substring(0,3);
 String lastNumber = phoneNumber.substring(7);
 
 String newN = firstNumber +"****" + lastNumber;
 System.out.println("屏蔽后:"+newN);
```

