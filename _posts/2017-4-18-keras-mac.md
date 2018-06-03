---
layout: post
title: Anaconda를 이용한 mac에서 Keras설치
tags:
- Anaconda
- Keras
- Python
---

리눅스에는 Keras가 Anaconda로 바로 설치되었으나 맥에서는 안되었다. 다행히 패키지를 찾을 수 있어서 쉽게 설치할수 있었다.

다음의 명령어로 가상환경을 만든다.

```
conda create -n 환경이름
conda install -c conda-forge keras=2.0.2
```

conda-forge라는 커뮤니티에서 제공하는 패키지 인데 다음과 같은 방법으로도 각종 패키지가 설치 가능하다.

```
conda config --add channels conda-forge
conda install <package-name>
```
참조 
https://conda-forge.github.io