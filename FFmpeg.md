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

头文件(mp4为例)：文件类型 文件时长 文件大小 创建时间 修改时间 比特率 流的数量

流信息(mp4为例)：编码格式(H.264) 分辨率 帧率 像素格式 色彩空间 编码器需要的额外参数

三种帧类型  (随机播放、快进快退、剪辑等操作都需要找到最近的关键帧)

| 帧类型  | 名称       | 含义                 | 依赖            |
| :------ | :--------- | :------------------- | :-------------- |
| **I帧** | 关键帧     | 完整的独立画面       | 不依赖任何帧    |
| **P帧** | 前向预测帧 | 只记录和前一帧的差异 | 依赖前面的I/P帧 |
| **B帧** | 双向预测帧 | 记录和前后帧的差异   | 依赖前后帧      |

#### Android Studio(java)中调用FFmpeg

include

```
#include <jni.h>
#include <string>
#include <android/log.h>
#include <android/native_window.h>  //ANativeWindow
#include <android/native_window_jni.h>  //ANativeWindow_fromSurface
#include <SLES/OpenSLES.h>  //处理音频
#include <SLES/OpenSLES_Android.h>
#include <unistd.h>

extern "C" {
#include "libavcodec/avcodec.h"
#include "libavformat/avformat.h"
#include "libavutil/avutil.h"
#include "libswscale/swscale.h"  //SwsContext
#include "libswresample/swresample.h"
}

//定义日志宏
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, "FFmpegNative", __VA_ARGS__)

//全局变量window 用于装入surface
static ANativeWindow* window = nullptr;
//用于将解码后的YUV视频帧转换为Android能显示的RGB/RGBA格式
static SwsContext* sws_ctx = nullptr;
//音频格式转换器
static SwrContext* swr_ctx = nullptr;
static int audio_sample_rate = 0;  // 采样率
static int audio_channels = 0;  // 声道数
//OpenSL ES播放器
static SLObjectItf engineObject = nullptr;      // 引擎对象
static SLEngineItf engineEngine = nullptr;      // 引擎接口
static SLObjectItf outputMixObject = nullptr;   // 混音器
static SLObjectItf playerObject = nullptr;      // 播放器对象
static SLPlayItf playerPlay = nullptr;          // 播放接口
static SLAndroidSimpleBufferQueueItf bufferQueue = nullptr;  // 缓冲区队列
//音频缓冲区（2个交替使用）
#define AUDIO_BUFFER_SIZE 8192
static uint8_t audio_buffer[2][AUDIO_BUFFER_SIZE];
static int current_buffer = 0;
static bool is_playing = true;  //播放控制
```

编写playVideo作为JNI函数

```
extern "C" JNIEXPORT ...JNICALL  //void声明输出类型 jstring声明输出类型为java格式风格string
//JNIEnv* env JNI环境指针 用于调用JNI提供的方法(如将c字符串转java字符串) 
Java_com_Fch_vl_viewmodel_VlPlayer_playVideo(JNIEnv* env, jobject thiz, jstring filePath) {
    //将jstring转为C字符串
    const char* path = env->GetStringUTFChars(filePath, nullptr);
    if (!path) {
        return;
    }
    //调用解复用器
    int streamCount = 0;
    int64_t duration = 0;
    int ret = demuxer(path, &streamCount, &duration);
    if (ret == 0) {
        //播放完成
    } else {
        //播放失败
    }
    //释放字符串
    env->ReleaseStringUTFChars(filePath, path);
}
```

编写解复用器demuxer函数(非JNI函数)

```c++
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
    //检查调用者是否想要这些参数，并通过输出参数返回信息
    if (pStreamCount) *pStreamCount = fmtCtx->nb_streams;
    if (pDuration) *pDuration = fmtCtx->duration;
    //初始化解码器
    AVCodecContext* video_codec_ctx = init_video_decoder(avformatcontext);
    AVCodecContext* audio_codec_ctx = init_audio_decoder(avformatcontext);
    //初始化数据包结构体 并将所有变量初始化 pkt里可能存一帧视频或一帧音频或字幕(压缩格式)
    AVPacket* pkt = av_packet_alloc();
    int packetCount = 0;
    LOGI("开始读取数据包...");
    //初始化一片空间以保留解码后的帧
    AVFrame* frame = av_frame_alloc();
    //av_read_frame()将数据读进pkt里
    while (av_read_frame(fmtCtx, pkt) >= 0 && packetCount < 30) {
        packetCount++;
        //pkt->stream_index是指引用视频流、音频流还是字幕流信息
        AVStream* stream = fmtCtx->streams[pkt->stream_index];
        //获取流类型
        const char* type = "未知";
        if (stream->codecpar->codec_type == AVMEDIA_TYPE_VIDEO) type = "视频";
        else if (stream->codecpar->codec_type == AVMEDIA_TYPE_AUDIO) type = "音频";
        //时间戳处理  pkt->pts != AV_NOPTS_VALUE判断时间戳是否(有效)不等于无效
        if (pkt->pts != AV_NOPTS_VALUE) {
        	//pkt->pts是对应帧的显示时刻 原始时间戳*时间基  av_q2d()将时间基转double
            double pts_sec = pkt->pts * av_q2d(stream->time_base);
            LOGI("包 #%d: %s, 大小=%d, PTS=%.3f秒", 
                 packetCount, type, pkt->size, pts_sec);
        } else {
            LOGI("包 #%d: %s, 大小=%d, PTS无效", 
                 packetCount, type, pkt->size);
        }
        //判断关键帧  pkt->flags是每帧的位掩码 关键帧的pkt->flags是0x000n(n可能为1 2 4 8) 我们只需要判断最低位是否为1即可判断是否为关键帧  P、B帧最低位一定为0
        if (pkt->flags & AV_PKT_FLAG_KEY) {
            LOGI("  → 关键帧");
        }
        //送包给初始化后的解码器  注意一个包可能有多帧所以需要while处理(一般情况是一包一帧)
        if(stream->codecpar->codec_type == AVMEDIA_TYPE_VIDEO && video_codec_ctx){
            if(avcodec_send_packet(video_codec_ctx, pkt) < 0) break;
            while(avcodec_receive_frame(video_codec_ctx, frame) == 0){
                //渲染帧
                render_video_frame(frame);
                av_frame_unref(frame);
            }
        }else if(stream->codecpar->codec_type == AVMEDIA_TYPE_AUDIO && audio_codec_ctx){
            if(avcodec_send_packet(audio_codec_ctx, pkt) < 0) break;
            while(avcodec_receive_frame(audio_codec_ctx, frame) == 0){
                //渲染帧
                av_frame_unref(frame);
            }
        }
        //释放当前包占用数据单保留包本身
        av_packet_unref(pkt);
    }
    LOGI("共读取 %d 个数据包", packetCount);
    //彻底销毁pkt
    av_packet_free(&pkt);
    avformat_close_input(&fmtCtx);
    LOGI("解复用完成");
    return 0;  // 返回成功
}
```

**注** avformat_close_input()关闭文件及所有资源同时将传入指针置空 avformat_free_context()仅释放占用内存

**注** av_read_frame()可读取帧 对于视频而言 假设packetCount的循环条件为<300 那么仅能读取约150个视频帧和音频帧(交替读取)

init_video_decoder()

```
AVCodecContext* init_video_decoder(AVFormatContext* avformatcontext){
    int video_stream_index = -1;
    for(int i = 0; i < avformatcontext->nb_streams; i++){
        if(avformatcontext->streams[i]->codecpar->codec_type == AVMEDIA_TYPE_VIDEO){
            video_stream_index = i;
            break;
        }
    }
    if(video_stream_index == -1) return nullptr;
    AVStream* video_stream = avformatcontext->streams[video_stream_index];
    const AVCodec* decoder = avcodec_find_decoder(video_stream->codecpar->codec_id);
    if(!decoder) return nullptr;
    AVCodecContext* video_codec_ctx = avcodec_alloc_context3(decoder);
    avcodec_parameters_to_context(video_codec_ctx, video_stream->codecpar);
    if(avcodec_open2(video_codec_ctx, decoder, nullptr) < 0){
        avcodec_free_context(&video_codec_ctx);
        return nullptr;
    }
    return video_codec_ctx;
}
```

init_audio_decoder()

```
AVCodecContext* init_audio_decoder(AVFormatContext* avformatcontext){
    int audio_stream_index = -1;
    for(int i = 0; i < avformatcontext->nb_streams; i++){
        if(avformatcontext->streams[i]->codecpar->codec_type == AVMEDIA_TYPE_AUDIO){
            audio_stream_index = i;
            break;
        }
    }
    if(audio_stream_index == -1) return nullptr;
    AVStream* audio_stream = avformatcontext->streams[audio_stream_index];
    const AVCodec* decoder = avcodec_find_decoder(audio_stream->codecpar->codec_id);
    if(!decoder) return nullptr;
    AVCodecContext* audio_codec_ctx = avcodec_alloc_context3(decoder);
    avcodec_parameters_to_context(audio_codec_ctx, audio_stream->codecpar);
    if(avcodec_open2(audio_codec_ctx, decoder, nullptr) < 0){
        avcodec_free_context(&audio_codec_ctx);
        return nullptr;
    }
    return audio_codec_ctx;
}
```

setSurface()

```
extern "C" JNIEXPORT void JNICALL
Java_com_Fch_vl_viewmodel_VlPlayer_setSurface(JNIEnv* env, jobject thiz, jobject surface) {
    //释放旧的Surface
    if (window) {
        ANativeWindow_release(window);
        window = nullptr;
    }
    //从Java Surface创建ANativeWindow
    window = ANativeWindow_fromSurface(env, surface);
}
```

render_video_frame()

```
void render_video_frame(AVFrame* frame) {
    if (!window || !frame) return;
    // 初始化颜色空间转换器
    if (!sws_ctx) {
        sws_ctx = sws_getContext(
            frame->width, frame->height, (AVPixelFormat)frame->format,
            frame->width, frame->height, AV_PIX_FMT_RGBA,
            SWS_BILINEAR, NULL, NULL, NULL
        );
        // 设置窗口缓冲区
        ANativeWindow_setBuffersGeometry(window, frame->width, frame->height, WINDOW_FORMAT_RGBA_8888);
    }
    // 锁定窗口缓冲区
    ANativeWindow_Buffer buffer;
    if (ANativeWindow_lock(window, &buffer, NULL) < 0) return;
    // 准备目标数据（RGBA）
    uint8_t* dst_data[4] = { (uint8_t*)buffer.bits, NULL, NULL, NULL };
    int dst_linesize[4] = { buffer.stride * 4, 0, 0, 0 };
    // YUV 转 RGBA
    sws_scale(sws_ctx, frame->data, frame->linesize, 0, frame->height, dst_data, dst_linesize);
    // 解锁并提交到屏幕
    ANativeWindow_unlockAndPost(window);
}
```

音频渲染略 因Android对音频格式要求较严苛 所以音频播放较复杂
