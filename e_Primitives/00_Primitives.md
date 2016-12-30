원본: https://www.fusetools.com/docs/primitives/primitives

프리미티브

퓨즈는보다 복잡한 시각적 요소의 기본 빌딩 블록 인 기본 요소 세트를 제공합니다. 이러한 프리미티브에는 텍스트, 사각형, 원, 이미지 및 비디오가 포함됩니다.

도형

앱에서 가장 많이 사용되는 두 가지 모양은 직사각형과 원입니다. 그들은 각각 퓨즈에 자신의 유형이 있습니다 :

<패널>
    <Circle Color = "# bbfdff"/>
    <사각형 색상 = "# ff6eb4"/>
</ Panel>
직사각형과 원에 대한 전체 설명서를 확인하십시오.

색

퓨즈의 모든 시각적 요소에는 Color 속성이 있습니다. 이 속성은 문제의 요소의 주 색상으로 직관적으로 간주되는 요소로 매핑됩니다.

직사각형과 원과 같은 도형의 경우, Color 속성이 해당 색상의 SolidColor 브러시로 변환해야합니다. 이것은 다음 샘플을 의미합니다.

<사각형 색상 = "# f0f"/>
실제로

<직사각형 채우기 = "# f0f"/>
Fill 속성은 브러시 유형이기 때문에 Fill = "# f0f 부분은 실제로 다음 동등한 항목으로 더 확장됩니다.

<직사각형>
    <SolidColor Color = "# f0f"/>
</ Rectangle>
SolidColor가 직사각형 채우기 속성에 "자동 바인딩"되는 위치입니다.

브러쉬 및 선

퓨즈의 색상은 브러쉬로 표현됩니다. 일반적인 단색을 얻으려면 SolidColor 브러시를 사용할 수 있습니다. 모든 셰이프에는 브러시 형식의 채우기 속성이 있습니다. 모양에 단색을 설정하는 것이 일반적이므로 특수한 Color 속성이 있습니다.

본문

Text 클래스는 정적 텍스트를 표시하는 기본 기본 객체입니다.

다음은 다양한 텍스트 관련 컨트롤 간의 차이점에 대한 개요입니다.

텍스트 - 여러 줄을 넘길 수있는 상호 작용이 불가능한 텍스트를 표시합니다.
TextInput - 한 줄로 사용자가 조작 할 수있는 텍스트 컨트롤입니다.
텍스트 만 - 특별한 장식이 없습니다.
DockLayout을 가지고 있습니다. 즉, DockPanel.Dock 연결된 속성을 사용하여 자식을 배치 할 수 있습니다.
IsPassword를 부르세요.
TextInputActionHandler 액션 트리거 됨
TextBox - TextInput과 비슷하지만 기본 장식을 사용하므로 비어있는 경우 (예 : 프로토 타이핑)
TextView

여러 줄 TextInput
장식되지 않은
비밀번호가 될 수 없다.
특별한 행동을 취할 수 없다.
Basic.TextInput - Fuse.BasicTheme 패키지의 장식 된 TextInput입니다.

이미지 / 비디오

이미지 및 비디오 요소를 사용하여 이미지 및 비디오를 쉽게 포함 할 수 있습니다.

<Image File = "someImage.jpg"/>
<Video File = "someVideo.mp4"/>
이미지와 비디오 모두 로컬 파일 대신 URL을 가져 오는 것을 지원합니다.

<Image Url = "http://www.some.com/image.jpg"/>
<Video Url = "http://www.some.com/video.mp4"/>
이미지 및 비디오 문서 자세한 설명서를 확인하십시오.