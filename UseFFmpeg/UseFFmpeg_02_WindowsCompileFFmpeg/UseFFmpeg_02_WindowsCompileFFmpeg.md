## UseFFmpeg-Windows下编写FFmpeg代码

Ubuntu虚拟机上播放视频发现没声音，Linux这条路还是走不通，上一节白搞了。

### 1. 准备素材

​	下载FFmpeg在Windows平台的标准库，移动include、lib到工程目录下

### 2. 编写简单验证代码

​	在Clion中编写下面代码

```c++
#include <iostream>
extern “C” {
#include “libavcodec/avcodec.h”
}

int main () {
	std::cout << avcodec_configuration();
}
```

### 3. 编写CMakeLists.txt

```cmake
cmake_minimum_required(version 3.16)
project(UseFFmpeg_Win)
set(CMAKE_CXX_STANDARD 11)

include_directories(${PROJECT_SOURCE_DIR}/include)
link_directories(${PROJECT_SOURCE_DIR}/lib)

add_executable(UseFFmpeg_Win main.cpp)
target_link_libraries(UseFFmpeg_Win avcodec avutil swresample)
```

 

直接运行，居然通过了：

![WindowsRunningRes.png](https://github.com/lanseie/lanseie.github.io/blob/main/UseFFmpeg/UseFFmpeg_02_WindowsCompileFFmpeg/WindowsRunningRes.png)
​                               
