#### Document

[Documentation](https://ffmpeg.org/documentation.html)

#### 经典http测试视频

```
"http://vjs.zencdn.net/v/oceans.mp4"
"https://prod-streaming-video-msn-com.akamaized.net/a8c412fa-f696-4ff2-9c76-e8ed9cdffe0f/604a87fc-e7bc-463e-8d56-cde7e661d690.mp4"
```

#### ffmpeg (encoding and decoding)

```
ffmpeg -version
```

#### ffplay

```
ffplay file.mp4(.mp3...)
//可跟图片 或 url  命令行输出中第一个值是计数器
ffplay -fs ....mp4  //全屏播放
ffplay -window_title "..." ....mp4  //窗口标题
ffplay -ss 30 ....mp4  //从第30秒开始播放
ffplay -t 10 ....mp4  //只播放10秒
ffplay -an ....mp4  //只放视频无声
ffplay -vn ....mp3  //只有声音无画面
ffplay -loop 0 ....mp4  //循环播放-loop 0表示无限循环
ffplay -volume 50 ....mp4  //设置音量0-100
//播放时快捷键  space  <-/->  up/down  Esc/q  f(全屏)
```

#### 原理

解复用 -> 解码 -> 处理 -> 编码 -> 复用

#### Android Studio(java)中调用FFmpeg

include

```
#include <jni.h>
#include <string>
#include <android/log.h>

extern "C" {
#include "libavcodec/avcodec.h"
#include "libavformat/avformat.h"
#include "libavutil/avutil.h"
}

//定义日志宏
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, "FFmpegNative", __VA_ARGS__)
```

编写playVideo作为JNI函数

```
extern "C" JNIEXPORT ...JNICALL  //void声明输出类型 jstring声明输出类型为java格式风格string
//JNIEnv* env JNI环境指针 用于调用JNI提供的方法(如将c字符串转java字符串) 
java_com_Fch_vl_viewmodel_Vlplayer_playVidoe(JNIEnv* env, jobject /* this */, jstring filePath){
	
}
```

编写解复用器demuxer函数(非JNI函数)

```
//返回int表示成功/失败
//AVFormatContext.duration的类型是int64_t
int myDemuxer(const char* filePath, int* pStreamCount, int64_t* pDuration) {
    LOGI("执行解复用: %s", filePath);
    //分配并初始化一个AVFormatContext结构体 它包含了文件的所有信息
    AVFormatContext* fmtCtx = avformat_alloc_context();
    if (!fmtCtx) {
        LOGI("分配上下文失败");
        return -1;
    }
    //打开文件并读取文件头  &fmtCtx是指向fmtCtx的二级指针 函数内部可能需要修改fmtCtx这个指针  nullptr是c++里的null
    int ret = avformat_open_input(&fmtCtx, filePath, nullptr, nullptr);
    if (ret < 0) {
        char errBuf[128];
        //将错误码转化成错误信息 sizeof(errBuf)告知数组大小防止写溢出
        av_strerror(ret, errBuf, sizeof(errBuf));
        LOGI("打开文件失败: %s", errBuf);
        //释放avformat_alloc_context()申请占用的内存
        avformat_free_context(fmtCtx);
        return -2;
    }
    //读取文件中流信息并填充到fmtCtx中 avformat_find_stream_info()依赖avformat_open_input()
    ret = avformat_find_stream_info(fmtCtx, nullptr);
    if (ret < 0) {
        LOGI("获取流信息失败");
        //关闭文件并释放所有相关资源
        avformat_close_input(&fmtCtx);
        return -3;
    }
    //打印信息
    LOGI("文件格式: %s", fmtCtx->iformat->long_name);
    LOGI("时长: %lld 秒", fmtCtx->duration / AV_TIME_BASE);
    LOGI("流数量: %d", fmtCtx->nb_streams);
    //通过输出参数返回信息
    if (pStreamCount) *pStreamCount = fmtCtx->nb_streams;
    if (pDuration) *pDuration = fmtCtx->duration;
    //读取数据包
    AVPacket* pkt = av_packet_alloc();
    int packetCount = 0;
    LOGI("开始读取数据包...");
    while (av_read_frame(fmtCtx, pkt) >= 0 && packetCount < 30) {
        packetCount++;
        AVStream* stream = fmtCtx->streams[pkt->stream_index];
        //获取流类型
        const char* type = "未知";
        if (stream->codecpar->codec_type == AVMEDIA_TYPE_VIDEO) type = "视频";
        else if (stream->codecpar->codec_type == AVMEDIA_TYPE_AUDIO) type = "音频";
        //时间戳处理
        if (pkt->pts != AV_NOPTS_VALUE) {
            double pts_sec = pkt->pts * av_q2d(stream->time_base);
            LOGI("包 #%d: %s, 大小=%d, PTS=%.3f秒", 
                 packetCount, type, pkt->size, pts_sec);
        } else {
            LOGI("包 #%d: %s, 大小=%d, PTS无效", 
                 packetCount, type, pkt->size);
        }
        //判断关键帧
        if (pkt->flags & AV_PKT_FLAG_KEY) {
            LOGI("  → 关键帧");
        }
        av_packet_unref(pkt);
    }
    LOGI("共读取 %d 个数据包", packetCount);
    //清理
    av_packet_free(&pkt);
    avformat_close_input(&fmtCtx);
    LOGI("解复用完成");
    return 0;  // 返回成功
}
```

**注** avformat_close_input()关闭文件及所有资源同时将传入指针置空 avformat_free_context()仅释放流信息

编写解码器decoder函数

```
java_com_Fch_vl_viewmodel_VlPlayer_decoder(){
	
}
```

