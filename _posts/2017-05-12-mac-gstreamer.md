---
layout: post
title: osx에서 GStreamer사용하기(xcode)
---
원하는 버전을 다운(여기서는 1.12.0기준으로 설명)
https://gstreamer.freedesktop.org/data/pkg/osx/

다음 2개를 받아서 설치
gstreamer-1.0-1.12.0-x86_64.pkg
gstreamer-1.0-devel-1.12.0-x86_64.pkg

설치하면 /Library/Frameworks/GStreamer.framework 에 설치된다.

Build Phases - Link Binary With Libraries에 해당경로를 추가해 준다.(드래그가 편함)
Build Setting의 Header Search Paths
```

```


## 참고
https://gstreamer.freedesktop.org/documentation/installing/on-mac-osx.html#InstallingonMacOSX-Run




