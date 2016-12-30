원본: https://www.fusetools.com/docs/fuse/resources/imagesource

ImageSource 클래스

  고급 기능 표시
이동 :
목차
파일, URL 또는 기타 소스와 같은 소스의 이미지를 제공합니다.

예

다음 예제에서는 FileImageSource의 Image를 표시합니다.

<이미지>
     <FileImageSource File = "fuse.png"/>
</ Image>
일반적인 패턴은 ImageSource를 전역 리소스로 선언하는 것입니다 (아래 그림 참조).

<FileImageSource ux : Global = "FuseLogo"File = "fuse.png"/>

<이미지 소스 = "FuseLogo"/>
사용 가능한 이미지 소스 유형 :