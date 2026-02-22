# QuickBuild

#### Vue3文档

https://cn.vuejs.org/guide/quick-start.html

#### 传统开发方式 借助 script 标签直接通过 CDN 来使用 Vue

下载 https://unpkg.com/vue@3/dist/vue.global.js and https://unpkg.com/vue@3/dist/vue.esm-browser.js 到服务器端的drawable文件夹下

```html
<head>
    <script src="vue.global.js"></script>
</head>
<body>
    <div id="app">
    	{{ msg }}  <!-- {{}}可获取下方return内数据进行渲染 -->
    </div>
    <script>
    	Vue.createApp({
            //setup选项 用于设置响应数据和方法
            setup(){
                return{  //return待被获取数据
                    msg:"success"
                }
            }
        }).mount("#app")  //#用于匹配id
    </script>
</body>
```

**注** 框选后Tab为整体向右缩进 Shift+Tab为整体向左缩进

**注** 可通过解构方式去除createApp前的Vue 即加入: 

```html
<body>
	<script>
		const {createApp} = {Vue}  //将Vue的createApp属性赋值给createApp变量 后面可直接使用createApp变量
	</script>
</body>
```

#### 模块化开发方式

```html
<body>
    <div id="app">
    	{{ msg }}
    </div>
    <script type="module">  //声明模块化类型
        import {createApp} from "./vue.esm-browser.js"  //使用局部构建版本Vue
    	createApp{  //这里因为之前的import不用写"Vue."
            setup(){
                return{
                    msg: "success"
                }
            }
        }.mount("#app")
    </script>
</body>
```

**注** 使用ES模块构建版本 在我们通过本地盘符的路径打开html时是不会有渲染效果的 只有在服务器形态下才能成功渲染

