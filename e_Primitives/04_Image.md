원본: https://www.fusetools.com/docs/fuse/controls/image

I마법사 클래스

 고급 기능 표시
이동 :
목차
이미지를 표시합니다.

이미지는 퓨즈의 이미지 작업을위한 몇 가지 기능을 제공하며 몇 가지 간단한 예를 통해 살펴 보겠습니다.

파일 또는 URL에서 이미지 표시 :

<StackPanel>
    <이미지 파일 = "some_file.png"/>
    <이미지 Url = "some_url"/>
</ StackPanel>
파일에서 다중 밀도 이미지 표시 :

<StackPanel>
    <Image Files = "logo.png, logo@2x.png, logo@4x.png"/>
    <이미지>
        <MultiDensityImageSource>
            <FileImageSource Density = "1"File = "logo.png"/>
            <FileImageSource Density = "2"File = "logo@2x.png"/>
            <FileImageSource Density = "3"File = "logo@4x.png"/>
        </ MultiDensityImageSource>
    </ Image>
</ StackPanel />
URL에서 다중 밀도 이미지 표시 :

<StackPanel>
    <이미지>
        <MultiDensityImageSource>
            <HttpImageSource Density = "1"Url = "..."/>
            <HttpImageSource Density = "2"Url = "... @ 2x"/>
            <HttpImageSource Density = "3"Url = "... @ 4x"/>
        </ MultiDensityImageSource>
    </ Image>
</ StackPanel>
JavaScript에서 지정한 파일의 이미지 표시

우노는 경로가 자바 스크립트에서 정의 될 때 이미지를 자동으로 묶을 수 없습니다. 이 때문에 수동으로 unproj 파일에 수동으로 가져 와서 수동으로 묶어야합니다. 하나의 파일을 다음과 같이 묶을 수 있습니다 :

"포함": [
    "*",
    "image.jpg : 번들"
]
또는 와일드 카드를 사용하여 전체 폴더 또는 특정 유형의 모든 파일을 묶을 수 있습니다.

"포함": [
    "* .jpg : 번들"
]
여기에 프로젝트와 함께 파일 묶기에 대한 자세한 내용을 볼 수 있습니다.

이미지 파일을 번들로 묶어서 다음과 같이 자바 스크립트에서 참조 할 수 있습니다.

<자바 스크립트>
    module.exports = {
        image : "image.jpg"
    };
</ JavaScript>
<이미지 파일 = "{이미지}"/>
