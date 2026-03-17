# Integrate Cpp into Android Project

#### Step 1

NDK env

```
//build.gradle.kts
android{
	ndkPath = "D:\\AndroidSDK\\ndk\\27.0.12077973"
	externalNativeBuild{
		cmake{
			path = file("src/main/cpp/CMakeLists.txt")
		}
	}
	defaultConfig {
		ndk {
			abiFilters.add("arm64-v8a")
		}
	}
}
//CMakeLists.txt 告诉ndk待编译的native-lib在哪以及.so文件位置
cmake_minimum_required(VERSION 3.22.1)
project("ffmpeg_jni")
set(ffmpeg_lib_dir ${CMAKE_SOURCE_DIR}/jniLibs/arm64-v8a)
set(ffmpeg_include_dir ${CMAKE_SOURCE_DIR}/include)
set(ffmpeg_libs avcodec avformat avutil swresample swscale avfilter)
foreach(lib ${ffmpeg_libs})
    add_library(${lib} SHARED IMPORTED)
    set_target_properties(${lib} PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/lib${lib}.so)
endforeach()
include_directories(${ffmpeg_include_dir})
add_library(native-lib SHARED native-lib.cpp)
target_link_libraries(native-lib ${ffmpeg_libs} log android)
```

#### Step 2

在linux或win上编译你的cpp源代码 生成.so文件 放至src\main\cpp\jniLibs\arm64-v8a  生成include文件夹 放至src\main\cpp下

#### Step 3

src\main\cpp\native-lib.cpp

```
#include <jni.h>
#include <string>
#include <android/log.h>

extern "C" {
#include "libavcodec/avcodec.h"
#include "libavformat/avformat.h"
#include "libavutil/avutil.h"
}

#define LOG_TAG "FFmpegLog"
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, LOG_TAG, __VA_ARGS__)

extern "C" JNIEXPORT jstring JNICALL
Java_com_Fch_vl_presentation_view_MainActivity_stringFromJNI(JNIEnv* env, jobject /* this */) {
    std::string hello = "FFmpeg 集成成功！";
    return env->NewStringUTF(hello.c_str());
}

extern "C" JNIEXPORT jstring JNICALL
Java_com_Fch_vl_presentation_view_MainActivity_getFFmpegVersion(JNIEnv* env, jobject /* this */) {
    std::string version = av_version_info();
    LOGI("FFmpeg version: %s", version.c_str());
    return env->NewStringUTF(version.c_str());
}
```

MainActivity.java

```
static {  //注意根据相互依赖关系进行顺序引入
    System.loadLibrary("avutil");
    System.loadLibrary("avcodec");
    System.loadLibrary("avformat");
    System.loadLibrary("avfilter");
    System.loadLibrary("swresample");
    System.loadLibrary("swscale");
    System.loadLibrary("native-lib");
}
//声明上述native-lib.cpp中方法
private native String stringFromJNI();
private native String getFFmpegVersion();
```



