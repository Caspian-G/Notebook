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

