# Build

#### Select a Project Template

Empty Activity - 现代jetpack compose(声明式UI)  **推荐** 但不支持java

Empty View Activity - xml布局文件(传统命令式UI)

#### Configure Your Project

Minimum SDK = API 33 (Android 13)  //自用版本

**注** 项目构建完成后 file->project structure->project SDK选择Android API 35  //自用

#### Gradle

推荐kotlin风格

```
dependencies{
	implementation()
}
```

**注** 根目录下build.gradle通常只用来定义插件版本或全局配置 app文件夹下build.gradle可定义依赖

##### 使用jitpack获取github包

全局settings.gradle

```
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven(){
            setUrl("https://jitpack.io")  //声明使用jitpack
        }
    }
}
```

局部build.gradle

```
//build.gradle(:app)
dependencies{
	implementation("com.github.ywl5320:wlmedia:4.2.0")
}
```

##### 使用NDK加入其它语言的编译环境

```
defaultConfig {
   ...
   ndk {
   		//或abiFilters.addAll(listOf("armeabi-v7a", "arm64-v8a", "x86", "x86_64"))
       abiFilters.add("armeabi-v7a")
       abiFilters.add("arm64-v8a")
       abiFilters.add("x86")  //用于适配模拟器环境
       abiFilters.add("x86_64")  //同上
       }
...
}
```

