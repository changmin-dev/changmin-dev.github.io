---
layout: post
title: mac 시에라에서 OpenCV 사용하기 (Xcode)
tags:
- OpenCV
- Xcode
---
이번에 mac을 구매해서 OpenCV를 설치해 보았다. 3.x 보다는 2.4가 안정적이고 예전에 사용했던 소스도 사용할 수 있을 것 같아서 2.4로 설치하였다.
기본적인 소스는 잘되지만, 과거에 했던것들도 잘될지는 모르겠다.

## 다운로드 및 설치 
```
git clone https://github.com/opencv/opencv.git
git checkout 2.4
cd opencv
mkdir build
cd build
cmake ..
sudo make install
```

## Xcode 설정하기
프로젝트를 생성후 그룹을 만든다
. ./framework/opencv

add files를 통해서 빌드한 /usr/local/lib/의 libopencv_*.dylib파일들을 추가한다.

![dylib설정]({{ site.baseurl }}/images/OpenCVmac1.png)

Build Settings에서 다음과 같이 설정 

- Header Search Paths: 
```
/usr/local/include/ 
/usr/local/include/opencv/
```

- Library Search Paths: 
```
/usr/local/lib/
```

![path설정]({{ site.baseurl }}/images/OpenCVmac2.png)

## 간단한 파일 열기 예제
```cpp
#include <iostream>
#include <opencv2/core.hpp>
#include <opencv2/highgui.hpp>

int main(int argc, const char * argv[]) {
    
    cv::Mat image = cv::imread("당신의 이미지 경로");
    if(!image.empty()){
        cv::imshow("image", image);
        cv::waitKey(0);
    }
    else{
        std::cout<< "empty image" <<std::endl;
    }
    return 0;
}

```
core
2.4버전은 추가적인 설정없이도 문제 없이 실행된다.


## 참고한 자료 
https://mansoo-sw.blogspot.kr/2016/10/macos-xcode-opencv.html
