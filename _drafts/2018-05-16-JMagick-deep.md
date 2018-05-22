# JMagick 기능들
[문서](http://www.jmagick.org/jmagick-doc/)와 [테스트](https://github.com/techblue/jmagick/blob/master/test/magicktest/TestJMagick.java)[코드](https://github.com/techblue/jmagick/blob/master/test/magicktest/OldSimpleTest.java)들이 있어서 필요한 것은 파악이 될듯하다.

## ImageInfo
생성자는 파일로 부터 정보를 가지고 온다. 생성자에서 네이티브 부분을 보면 이미지의 정보를 가지고 오고 있다. 객체만 생성하고 값을 설정하는 것으로도 사용이 가능하다.
ImageInfo 각종 값을 가지고 올 수 있고 설정할 수 있다.

## MagickImage
생성자는 ImageInfo, MagickImage의 배열등에서 이미지를 가져온다. BLOB(binary large object)으로 이미지 정보도 가지고 올 수 있는거 같다.
ImageToBlob 메서드로 Blob(byte[])으로 바꿀수 있다. 각종 영상 처리(샤프닝 등)가 가능하고 값들을 설정하고 가져올 수 있다.

## xxType, xxMode 등
인터페이스의 상수 값으로 타입과 모드 같은 정보를 표현한다. 문서에는 값들이 모두 나와있지 않아서 IDE에서 정의를 찾아가서 확인해야 했다.

## MagickWindow
화면을 미리 보는데 사용하기 좋다. gif에니메이션은 안되는듯 하다.