##### avio_list_dir.c

​	使用 AVIOContext 功能展示文件夹中的文件

​	demo使用方法：avio_list_dir ${input_dir}

​	*该功能在Linux有标准库，暂时用不到ffmpeg的库*

##### avio_reading.c

​	使用AVIOcontext 功能重定义了读写函数，让FFmpeg操作实现对内存操作、而不是基于url或者文件

​	关键函数 avio_alloc_context **值得写一个专题**

​    demo使用方法：avio_reading ${input_file}

##### demuxing_decoding.c

​	解析并解码完整文件，已写专题

##### encode_audio.c

​	音频编码，自动生成音频数据并编码为MP2音频文件

​    demo使用方法：encode_audio ${output_file}

​	**待写专题**

##### encode_video.c

​	视频编码，自动生成YUV数据并根据输入编码器编码

​    已写专题

##### extract_mvs.c

​	视频解码基础上，导出了运动向量(motion vectors)。难道是运动方向检测？怀疑已经涉及CV内容了

​	关键函数：av_frame_get_side_data 提到了诸多边缘数据(side_data)，motion vectors仅是其中一个。其他还包括立体3D元数据

​	**非常值得写一个专题**

##### filter_audio.c

​	生成一个sin波形，再使用filter_graph进行音频处理。看得出是音量调节，不清楚有没有其他动作

​	**待写专题**

##### filtering_audio.c

​	音频解码+filtering，貌似通道、采样率、输出格式都变换了

​    **待写专题**

##### filter_video.c

​    视频解码+filter_graph，将stream变换为8bit灰度图、再根据8bit输出为字符画

​    **待写专题**

##### http_multiclient.c

​	使用avio接口完成http网络视频处理，转了一圈又发出去了，没有预览部分

​    **待写专题**

##### hw_decode.c

​	演示了硬件加速解码？关键结构体：AVHWDeviceType

​	**非常值得写一个专题**

##### metadata.c

​	展示metadata(元数据)内容。可能是视频中自带的设备类型之类的内容

​	**待写专题**

##### muxing.c

​	自动生成YUV视频帧和原始音频帧，编码+打包

​	**待写专题**

##### qsvdec.c

​	因特尔QSV-加速h264解码示例，和厂商有关的接口

​	**待写专题**

##### remuxing.c

​	转封装演示demo

​	**待写专题**

##### resampling_audio.c

​	音频重采样demo，程序生成正弦波当作音频数据。关键函数swr_convert

​	**待写专题**

##### scaling_video.c

​	仍然是libswscale库的内容，YUV图像缩放

​	**待写专题**

##### transcode_aac.c

​	解码、重采样、再编码成aac

​	**待写专题**

##### transcoding.c

​	解析、解码、重采样、编码、打包，音视频两者都有

​	**待写专题**

##### vaapi_encode.c

​	video acceleration视频加速接口 编码示例

​	关键函数：av_hwdevice_ctx_create创建时使用参数 AV_HWDEVICE_TYPE_VAAPI。pix_fmt也要填成专有的AV_PIX_FMT_VAAPI

​	解码也是有的，vaapi_decode.c包含在libavcodec里面

​	**待写专题**

##### vaapi_transcode.c

​	视频解码、再编码，主要是用硬件加速实现解码和编码

​	**待写专题**

#### 疑问：

​	swr和filter_graph有什么区别？