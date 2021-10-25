## UseFFmpeg-解析和解码

### 1. 解码范例

​	翻阅ffmpeg源码的文件目录：FFmpeg(源码根目录)/doc/examples发现一系列功能的示例代码。此次查看的是demuxing_decoding.c

### 2. 代码结构

#### 	初始化

1.  avformat_open_input 根据输入文件，初始化AVFormatContext结构体

      假如是直接在内存中存储的stream码流、不是url或者文件，可以用av_parser_parse2接口直接把AVPacket解析出来，然后继续send_packet。
      此时就不avformat_open_input了，而是应该av_parser_init初始化一个解析器。
      详细的做法可以参考 doc/examples/decode_video.c

2.  avformat_find_stream_info使用上一步获取的AVFormatContext，再一步初始化获取码流信息

3.  开始分别打开视频流和音频流，两者初始化动作相同，都整合到了open_codec_context函数：

   输入参数：

   AVFormatContext *fmt_ctx – 前两步已经初始化过的重要结构体

   AVMediaType type – 决定当前是AVMEDIA_TYPE_VIDEO还是AUDIO

   输出参数：

   int stream_idx – 码流序号，解码器初始化完成后idx传出

   AVCodecContext **dec_ctx 申请的解码器结构体

​        i.     stream_idx = av_find_best_stream 重要参数(fmt_ctx, type)查找码流，查到了更新到fmt_ctx结构体中

​       ii.     AVCodec = avcodec_find_decoder(fmt_ctx->streams[stream_idx]->codecpar->codec_id) 查找解码器并返回

​       iii.     AVCodecContext = avcodec_alloc_context3(AVCodec) 根据解码器信息AVCodec申请真实解码器AVCodecContext

​       iv.     avcodec_parameters_to_context(AVCodecContext, fmt_ctx->streams[stream_idx]->codecpar) 将解码器信息回写到码流结构体AVCodecParameters

​       v.     avcodec_open2(AVCodecContext, AVCodec)正式打开码流

​       vi.     将前面find_best_stream获取的stream_idx传给输出参数

4. 使用open_codec_context打开视频后，根据宽高和输出像素格式调用video_buffer_size = av_image_alloc(ptr[4], linesize_ptr[4])申请输出buffer

5. 把整体码流信息打印出来：av_dump_format(fmt_ctx, 0, 原始码流文件,0)

6. 申请AVFrame * = av_frame_alloc()

7. 申请AVPacket * = av_packet_alloc()

#### 	循环读帧

1. av_read_frame(fmt_ctx, frame)返回码是否不小于零、仍然存在码流
2. 进入音视频共用函数decode_packet：入参AVCodecContext解码器、AVPacket包
   1. avcodec_send_packet(AVCodecContext*, AVPacket*)送去解码
   2. while等 avcodec_receive_frame(解码器, AVFrame*)取帧
      1. receive到解码后的帧，视频、音频各自写入输出文件。
      2. 音频写文件时有个获取当前帧在视频中所处时长的操作：
         av_q2d(AVCodecContext->time_base) * frame->pts
         获取出来是个double型，整数位是秒

#### 	退出流程

1. av_codec_free_context(AVCodecContext音视频解码器)关闭解码器
2. avformat_close_input(AVFormatContext)关闭大结构体
3. av_packet_free(AVPacket &)
4. av_frame_free(AVFrame &)
5. av_free(视频输出buffer 缓冲区ptr[4])

#### 	参考文献

1. [《FFMPEG开发快速入坑——音频转换处理》](https://zhuanlan.zhihu.com/p/345880400) 

   ffmpeg源码提到了PCM存在planar存储方式的概念，该引用讲的很明晰
