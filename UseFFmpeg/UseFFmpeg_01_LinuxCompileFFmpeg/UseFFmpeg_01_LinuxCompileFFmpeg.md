## UseFFmpeg 01: Linux操作系统下编译FFmpeg



### 1. 安装

a)   登录FFmpeg官网下载Ubuntu最新release包即可。下载、解压后执行相关命令：

```shell
./configure –prefix=/usr/local/ffmpeg –enable-shared

make

make install
```

即可将ffmpeg库安装到/usr/local/ffmpeg位置。后续开发将会直接调用动态库实现

b)  安装后，执行命令：

```shell
echo “/usr/local/ffmpeg/lib” > /etc/ld.so.conf.d/ffmpeg-linux.conf
```

将ffmpeg的动态库路径直接放入默认搜索路径

### 2. 编码

a)   编写简单的代码：

```c++
#include <iostream>
extern “C” {
#include “libavcodec/avcodec.h”
}

using namespace std;

void main() {
    cout << avcodec_configuration() << endl;
}
```

b)   编写CMakeLists.txt：

```cmake
project(Use_FFmpeg_01)

cmake_minimum_required(VERSION 3.16)

 

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_PREFIX_PATH /usr/local/ffmpeg/include)

 

include_directories(/usr/local/ffmpeg/include)

link_libraries(/usr/local/ffmpeg/lib)

 

add_executable(Use_FFMpeg use_ffmpeg.cpp)

target_link_libraries(Use_FFMpeg avcodec avutil swresample pthread)
```

详细CMakeLists.txt见截图，防止手抄失误



### 3. 排错

上述CMakeLists.txt不是一蹴而就的，实际经历过几次编译失败。报错的内容包括：

```
undefined reference to ‘pthread_once’

undefined reference to ‘swr_close’

undefined reference to ‘av_log’
```

#### 解决方法

一方面通过ldd命令查看主要的库libavcodec.so链接了哪些ffmpeg库，得知avutil和swresample被链接到了，后两个报错解决。



另一方面通过上网搜索，得知pthread_once是标准库libpthread的内容，并且ldd其实已经提示过了。

三个库链接，编译通过、问题解决
