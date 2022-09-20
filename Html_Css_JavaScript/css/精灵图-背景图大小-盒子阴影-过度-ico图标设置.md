* # 精灵图:

  ​	项目中将多张小图片，合成一张大图片，这张大图片称之为精灵图
  ​	优点:减少服务器发送次数，减轻服务器的压力，提高页面加载速度

	使用步骤:
	
		* 1.创建一个盒子，设置盒子的尺寸和小兔尺寸相同
		* 2.将精灵图设置为盒子的背景图片
	 * 3.修改背景图位置
	   	* 通过PxCook测试小图片在左上角坐标，分别取负值设置给盒子的background-position:x y;
	
		一般存放精灵图的标签都用行内标签 span,b,i;



* # 背景图大小
  	* 作用：设置背景图片的大小(盒子取诀为盒子大小)
  	* background-size:宽度 高度;
   * 取值
     	* 数字px
     	* 百分比
     	* contain		包含,将背景图片等比例缩放，直到不会超出盒子的最大
     	* cover		覆盖，将背景图片等比例缩放，直到刚好填满整个盒子没有空白

* # 盒子阴影:

  * 作用：给盒子添加阴影效果，吸引用户注意，体现页面的制作细节
  * 属性名:box-shadow
  * 取值
    * h-shadow		必须,水平偏移量,允许负值，像素单位
    * v-shadow		必须,垂直偏移量,允许负值  像素单位
    * blur			可选	,模糊度				像素单位
    * spread          可选,阴影扩大				像素单位
    * color			可选,阴影颜色
    * inset			可选,将阴影改为内部阴影



* # 过度：

  * 作用：让元素的样式慢慢变化，配合hover使用。

  * 属性名：transition-谁过度谁用

    * 如

    ```
    .box{
    	width:100px;
    	height:100px;
    	background-color:pink;
    	transition:all 1s;
    }
    .box:hover{
    	width:200px;
    	background-color:skyblue;
    }
    取值:过度属性名,过度时长.
    ```



* # html骨架

  * DOCTYPE文档声明

    * <!DOCTYPE html>(html) 文档类型声明，告诉浏览器该网页的Html版本

      * <meta charset="Utf-8"> 字符编码	

      * </!doctype><html lang="en"> 识别网页使用的语言
        * 作用：搜索归类+浏览器翻译
        * 常见语言zh-CN简体中文/en英文









* # SEO 搜索引起优化

  * 作用：让网站在搜索引擎上的排名靠前
  * 标签语义化:
    * title:网页标题标签
    * description网页描述
    * Keywords	网页关键字描述







* # ico图标设置

  * link:favicon（浏览器标题栏图标）