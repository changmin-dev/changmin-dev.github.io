---
layout: post
title: CLion에서 FFmpeg사용하기(OS X)
---
최근에는 C/C++을 사용때는 CLion을 사용하고 있다. 맨날 보고하지만, CMAKEList에도 익숙해지는듯

## FFmpeg가 없다면 설치
```
brew install ffmpeg
```

## CMAKELists.txt는 다음과 같이 설정한다.

/usr/local/Cellar/ffmpeg/3.3.1/ 부분은 본인의 컴퓨터에 맞게 설정 
FFmpeg_tutorial은 본인의 프로젝트이름이다.

```
cmake_minimum_required(VERSION 3.7)
project(FFmpeg_tutorial)

set(CMAKE_CXX_STANDARD 11)

include_directories(/usr/local/Cellar/ffmpeg/3.3.1/include/)
link_directories(/usr/local/Cellar/ffmpeg/3.3.1/lib/)

set(SOURCE_FILES main.cpp)
add_executable(FFmpeg_tutorial ${SOURCE_FILES})

target_link_libraries (
        FFmpeg_tutorial
        avcodec
        avdevice
        avfilter
        avformat
        avresample
        avutil
        postproc
        swresample
        swscale
)
```

## 다음의 코드가 실행되는지 획인한다.
```
extern "C" {
#include<libavformat/avformat.h>
#include<libavcodec/avcodec.h>
}
using namespace std;

int main() {
    cout << "result" << endl;
    cout << avcodec_configuration();
    return 0;
}
```

## 출처
http://blog.csdn.net/biezhihua/article/details/52734662
