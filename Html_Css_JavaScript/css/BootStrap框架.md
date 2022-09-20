* # BootStrap使用
  ​	1.引入:BootStrap提供Css代码
  ​		<link rel="stylesheet" href="./bootstrap-3.3.7/css/bootstrap.css">
  ​	2.调用类:使用BootStrap提供的样式
  ​		container:响应式布局版心类

  

* BootStrap栅栏系统
  	栅格化时指整个网页的宽度分成若干等分

  ​	BootStrap3默认将网页分成12等份

  ​	

  |          | 超小屏幕 |  小屏幕  | 中等屏幕 |  大屏幕  |
  | :------: | :------: | :------: | :------: | :------: |
  | 响应断点 |  <768px  | >=768px  | >=992px  | >=1200px |
  |   别名   |    xs    |    sm    |    md    |    lg    |
  | 容器宽度 |   100%   |  750px   |  970px   |  1170px  |
  |  类前缀  | col-xs-* | col-sm-* | col-md-* | col-lg-* |
  |   列数   |    12    |    12    |    12    |    12    |
  |  列间隙  |   30px   |   30px   |   30px   |   30px   |

  * .container是 Bootstrap 中专门提供的类名，所有应用该类名的盒子，默认已被指定宽度且居中。
  * .container-fluid也是 Bootstrap 中专门提供的类名，所有应用该类名的盒子，宽度均为 100%。
  *  分别使用.row类名和 .col类名定义栅格布局的行和列	

  注：	
  	container类自带间距15px;
  	row类自带间距-15px;
  	row类的作用是抵消container类的15px的内边距.row有-15px的外边距