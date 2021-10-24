## UseFFmpeg_视频编码

​	学习的源码来自ffmpeg源码路径：doc/examples下的encode_video.c

### 0. 使用说明

​	该demo演示了ffmpeg视频编码功能的用法，在demo中使用生成的YUV数据、使用入参设置编码格式，最后编码出没有封装的ES流(Elementary Stream)并写入设定好的输出文件。

​	执行编译程序：./UseFFmpeg_04_video_encode ${编码视频文件} ${编码器名称}   就可以得到编好的码流。

### 1. 代码结构

##### 	初始化

 1. 查找编码器：`AVCodec * = avcodec_find_encoder_by_name(char * encodec_name)`

    不知道本机支持哪些编码器的话，可以通过执行ffmpeg标准库自带的ffmpeg可执行程序，运行：

    `ffmpeg -encoders` 查看已有的编码器。

    比如下图就显示了我本机支持的编码器。libx265只是没截出来，本地实际是支持的。

    ![image-20211025004719356](C:\Users\lovig\AppData\Roaming\Typora\typora-user-images\image-20211025004719356.png)

 2. 创建解码器 AVCodecContext * = avcodec_alloc_context3(AVCodec *);

 3. AVPacket * = av_packet_alloc() 申请包结构体

 4. 编码器参数设置：

    ```c++
    // AVCodecContext *code_ctx = nullptr;
    code_ctx->bit_rate = 400000;
    code_ctx->width = 352;
    code_ctx->height = 288;
    code_ctx->time_base = (AVRational){1, 25};
    code_ctx->framerate = (AVRational){25, 1};
    
    code_ctx->gop_size = 10;
    code_ctx->max_b_frames = 1;
    code_ctx->pix_fmt = AV_PIX_FMT_YUV420P;
    if (code_c->id == AV_CODEC_ID_H264) {
        av_opt_set(code_ctx->priv_data, "preset", "slow", 0);
    }
    ```

 5. avcodec_open2(AVCodecContext *, AVCodec *, nullptr); 打开编码器

 6. AVFrame * = av_frame_alloc(); 申请帧结构体

 7. 更新帧信息：

    ```
    frame->format = code_ctx->pix_fmt;
    frame->width = code_ctx->width;
    frame->height = code_ctx->height;
    ```

 8. av_frame_get_buffer(frame, 0) 零对齐申请buffer

##### 	循环编码

​		上面已设定编码帧率为25，此次使用循环送帧25*60次、达到编码1分钟码流的效果。

​		循环内：

 1. 手动构造YUV数据：

    ```
    // 手动构造YUV数据
    uint32_t y = 0;
    uint32_t x = 0;
    for (y = 0; y < code_ctx->height; y++) {
        for (x = 0; x < code_ctx->width; x++) {
            frame->data[0][y * frame->linesize[0] + x] = x + y + i * 3;
        }
    }
    for (y = 0; y < code_ctx->height / 2; y++) {
        for (x = 0; x < code_ctx->width; x++) {
            frame->data[1][y * frame->linesize[1] + x] = 128 + y + i * 2;
            frame->data[2][y * frame->linesize[2] + x] = 64 + y + i * 5;
        }
    }
    ```

 2. 送帧编码

     	1. avcodec_send_frame(AVCodecContext *, AVFrame *)
     	2. while (ret >= 0) 循环取包
          	1. ret = avcodec_receive_packet(AVCodecContext *, AVPacket *)
          	2. 如果返回码等于AVERROR(EAGAIN)或AVERROR_EOF，说明读完数据、需要退出while
          	3. 否则将包数据写入${编码视频文件}
          	4. av_packet_unref 释放包

##### 	释放资源

 	1. avcodec_free_context(AVCodecContext *)
 	2. av_frame_free(AVFrame **)
 	3. av_packet_free(AVPacket **)