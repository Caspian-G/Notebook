# Compose 框架

#### 与xml对比

传统xml 一个xml代表一个页面对应一个activity.java或.kt

Compose方式 一个@Composable函数代表一个页面

#### 基本执行函数

onCreate() 第一次创建时执行 (仅执行一次)  需要接收保存的状态 所以onCreate(savedInstanceState: Bundle?)需要参数
onStart() 用户重新可见时执行
onResume() 用户重新可交互时执行
onPause() 离开屏幕时执行
onStop() 用户完全不可见时执行
onDestroy() 销毁时执行

#### 基本组件

Text：用于显示文本内容。
Image：用于显示图片。
Button：用于创建按钮。
TextField：用于接收用户输入的文本。
Column：用于垂直排列多个组件。
Row：用于水平排列多个组件。
Box：用于在屏幕上创建一个矩形的区域。
Surface：用于绘制一块可交互的区域。
Card：用于显示一个卡片式的 UI 元素。
Divider：用于在 UI 中添加分隔线。

#### 基于函数构建UI

@Composable：这是一个注解，用于标记 Compose 中的函数。使用该注解后，该函数将被视为一个 Compose 组件。
Column 和 Row：这两个组件用于创建垂直和水平排列的 UI 元素。它们都是一个函数，接收一个 Lambda 表达式作为参数，该表达式包含了需要排列的组件。
Modifier：该组件用于修改其他组件的行为和样式。它本质上是一个函数，可以接收多个参数，用于修改其他组件的行为和样式。
State：该组件用于在 Compose 中管理状态。它本质上是一个可变的数据结构，可以通过修改它的值来更新 UI。
setContent：该函数用于将 Compose 组件渲染到屏幕上。它接收一个 Lambda 表达式作为参数，该表达式包含了需要渲染的 Compose 组件。

#### 优化体验

```
override fun onCreate(savedInstanceState: Bundle?){  //kotlin函数无@Override注解
	window.insetsController?.hide(WindowInsets.Type.statusBars() or WindowInsets.Type.navigationBars())  //隐藏顶部状态栏和底部导航栏
	actionBar?.hide()  //隐藏标题栏
}
```

#### 定义函数以创建UI组件

```
@Composable
fun test(){
	column{
		Row{
			Text("test1")
			Text("test2")
		}
		Text("test3")
	}
}

override fun onCreate(savedInstanceState: Bundle?){
	setContent{
		test()  //将UI组件呈现在画面上
	}
}
```

#### 预览

```
@Preview(
    name = "预览名称",                    // 显示在预览窗口的名称
    showBackground = true,              // 是否显示背景
    backgroundColor = 0xFFF0F0F0,       // 背景色（十六进制，前两位是透明度）
    widthDp = 360,                      // 预览宽度（dp）
    heightDp = 640,                     // 预览高度（dp）
    device = "id:pixel_6",              // 指定设备
    locale = "zh-rCN",                  // 语言区域
    uiMode = Configuration.UI_MODE_NIGHT_YES,  // UI模式
    fontScale = 1.2f,                  // 字体缩放
    showSystemUi = true,               // 是否显示状态栏/导航栏
    apiLevel = 33,                     // Android API 级别
    wallpaper = true,                  // 显示壁纸背景
    group = "分组名",                   // 分组
)
@Composable
fun GreetingPreview() {  //随意取名
    VLTheme {  //android提供的应用主题预设无需更改
        test()
    }
}
```

