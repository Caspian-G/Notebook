#### AndroidManifest.xml

//app户口本

```
<?xml version="1.0" encoding="utf-8"?>
//声明 命名空间 此处标有android:或tools:开头的属性都要遵循Android官方规范
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    //请求使用权限
    <uses-permission android:name="android.permission.INTERNET" />  //网络权限
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>  //读取本地存储权限
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>  //写入本地存储权限
    <application
        //备份
        android:allowBackup="true"  //允许系统备份应用数据
        android:dataExtractionRules="@xml/data_extraction_rules"  //引用一个 XML 文件，详细规定哪些数据可以被提取备份
        android:fullBackupContent="@xml/backup_rules"  //详细规定哪些数据不可以被备份
        //应用外观与辅助
        android:label="@string/app_name"  //定义应用显示名称
        android:supportsRtl="true"  //允许“从右到左”的布局
        android:theme="@style/Theme.VL">  //整个应用的皮肤
        //定义用户能看到的界面
        <activity
            android:name=".MainActivity"
            android:exported="true">  //声明系统有权启动这个界面(如果需将该页作为启动页需true)
            //启动意图
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />  //标记应用主入口

                <category android:name="android.intent.category.LAUNCHER" />  //将此应用的图标放在系统桌面上
            </intent-filter>
        </activity>
    </application>

</manifest>
```

#### activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
//constraintLayout是一个大幕布
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".view.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"  //文本
        //上下左右都是"parent"对齐所以会居中
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

	//这里是一个wlmedia播放器的案例 使用Surface机制
    <com.ywl5320.wlmedia.widget.WlSurfaceView
        android:id="@+id/wlsurfaceview"  //写上id给javaorkotlin识别
        android:layout_width="match_parent"  //铺满整个width
        android:layout_height="match_parent" />  //铺满整个height
</androidx.constraintlayout.widget.ConstraintLayout>
```

**注** 后面的代码会覆盖前面的 如果后面的代码声明铺满整个constraintlayout则会覆盖前面的text文本

wrap_content 包裹内容

match_parent 占满父母

