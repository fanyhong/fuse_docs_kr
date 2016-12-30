원본: https://www.fusetools.com/docs/fuse/font

글꼴 클래스

  고급 기능 표시
이동 :
목차
특정 서체를 나타냅니다.

전역 리소스 글꼴은 Text 및 TextInput 객체에 직접 지정할 수 있습니다.

<텍스트 글꼴 = "PlatformDefault"/>
또는 .otf 또는 .ttf 파일을 기반으로 인라인 :

<텍스트 값 = "Hello!">
     <Font File = "arial.ttf"/>
</ 텍스트>
파일에서 전역 자원 글꼴을 만들려면 ux : Global 속성을 사용하십시오.

<Font File = "arial.ttf"ux : Global = "MyDefaultFont"/>
<텍스트 글꼴 = "MyDefaultFont"/>
글꼴 인터페이스

