원본: https://www.fusetools.com/docs/fuse/controls/text

텍스트 클래스

 고급 기능 표시
이동 :
UX
목차
비고
예제들
텍스트 블록을 표시합니다.

텍스트 UI 컨트롤은 읽기 전용 텍스트를 렌더링합니다.

트루 타입 글꼴을 포함하는 ttf 파일에서 글꼴을 가져올 수 있습니다. 일반적으로 글꼴은 응용 프로그램 전체에서 참조되므로 ux : Global을 사용하여 글꼴을 간단하게 작성하는 것이 가장 좋습니다. 이 글꼴 가져 오기 방법을 통해 글꼴을 전체 프로젝트에서 사용할 수 있으며 한 번만로드 할 수 있습니다.

노트

데스크톱 미리보기를 실행할 때 기본 다국어 평면 외부의 대체 글꼴이나 색칠 된 글리프 또는 유니 코드 문자도 지원되지 않습니다.

이 때문에 로컬 미리보기를 실행할 때 특정 텍스트 기능 (예 : 그림 이모티콘)이 지원되지 않습니다. 데스크톱 렌더링이 장치 렌더링 100 %와 일치하지 않으면 놀라지 마십시오. 이 문제가 해결 중입니다.
이 예제에서는 두 개의 글꼴을로드하고 사용 가능한 Text 속성 중 일부를 결합한 세 가지 의미 론적 클래스 인 Light, Medium 및 Warning을 만듭니다. 이 예제에서 글꼴은 관련 UX 파일과 동일한 디렉토리에 있습니다.

UX

<App>
    <Font File = "Roboto-Medium.ttf"ux : Global = "Medium"/>
    <Font File = "Roboto-Light.ttf"ux : Global = "Light"/>

    <텍스트 ux : Class = "Light"Font = "Light"/>
    <Text ux : Class = "Medium"Font = "Medium"TextWrapping = "줄 바꾸기"/>
    <텍스트 ux : Class = "경고"
        글꼴 = "보통"
        FontSize = "42"
        TextAlignment = "센터"
        색상 = "# f00"/>

    <StackPanel>
        <Light> 텍스트 만 </ Light>
        <Warning> 로봇들이오고 있습니다! </ Warning>
        <Medium> 이것은 중간 텍스트 일 ​​뿐이지 만 화면 가장자리에 도달하면 행복하게 감쌀 것입니다. </ Medium>
    </ StackPanel>
</ App>

## Interface of Text ##





예제들

예제들

텍스트 속성

<Text Color = "# 999"> 왼쪽 </ 텍스트>
<텍스트 TextAlignment = "센터"> 센터 </ 텍스트>
<Text FontSize = "24"TextAlignment = "Right"> 오른쪽 </ Text>
<Text LineSpacing = "10">
배수
윤곽
</ 텍스트>
이 예제에서 첫 번째 텍스트 요소는 기본값이므로 왼쪽 정렬되며 색상은 중간 밝은 회색으로 설정됩니다. 두 번째 텍스트는 가운데 정렬됩니다. 세 번째 글꼴은 오른쪽 정렬되고 큰 글꼴을 사용합니다. 네 번째 줄은 10 줄의 간격으로 두 줄에 걸쳐 있습니다.