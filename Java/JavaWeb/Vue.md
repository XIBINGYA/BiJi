# Vue

* Vue是一套前端框架，免除原生avaScript中的DOM操作，简化书写
* 基于MVWM(Model--View-ViewModel)思想，实现数据的双向绑定，将编程的关注点放在数据上

* 官网：https:/cn.vuejs.org

![image-20220614131533946](..\.imgs\image-20220614131533946.png)



## Vue 快速入门

![image-20220614131655264](..\.imgs\image-20220614131655264.png)



## Vue 常用指令

![image-20220614140748455](..\.imgs\image-20220614140748455.png)

![image-20220614140715564](..\.imgs\image-20220614140715564.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script src="js/vue.js"></script>

<div id="app">
    <a :href="url">点击一下</a>
</div>

<script>

    new Vue({
        el:"#app",
        data(){
            return{
                username:"",
                url:"http://www.baidu.com"
            }
        }

        /* data:function(){
            return {
                username:""
            }

        }
         */
    });

</script>
</body>
</html>
```



![image-20220913092137612](..\.imgs\image-20220913092137612.png)

```html
<body>
<script src="js/vue.js"></script>

<div id="app">

    <input type="button" value="按钮" v-on:click="show()">
    <input type="button" value="按钮" @click="show()">
    
</div>

<script>

    new Vue({
        el:"#app",
        data(){
            return{
                username:""
            }
        },
        methods:{
            show(){
                alert("我被点击了");
            }
        }

        /* data:function(){
            return {
                username:""
            }

        }
         */
    })

</script>
</body>
```

![image-20220913092231832](..\.imgs\image-20220913092231832.png)

![image-20220913092805762](..\.imgs\image-20220913092805762.png)

## Vue 生命周期 

![image-20220913094131221](..\.imgs\image-20220913094131221.png)
