원본: https://www.fusetools.com/docs/fuse/controls/button

버튼 클래스

 고급 기능 표시
이동 :
목차
버튼을 표시합니다.

퓨즈의 기본 버튼입니다. 그 외관은 파란색 글씨로 투명합니다. 모양을 변경하거나 의미 론적 특정 버튼을 만들려면이 클래스의 서브 클래스를 만듭니다. NativeViewHost 내부에서 사용할 때이 버튼은 플랫폼 기본 모양을 가지므로 추가 스타일이 필요할 수도 있습니다. 예를 들어, iOS에서 버튼의 기본 기본 모양은 흰색 파란색 텍스트입니다.

예제들

기본적으로 Button은 투명한 배경 위에 파란색 텍스트로 그려집니다.

<Button Text = "Click me"/>
그러나 Button은 가능할 때마다 플랫폼 기본 단추 컨트롤을 렌더링하는 데 사용할 수도 있습니다. 아래 그림과 같이 NativeViewHost에 Button을 래핑하면됩니다.

<NativeViewHost>
    <Button Text = "네이티브 버튼"/>
</ NativeViewHost>
그러나 우리는 일반적으로 우리 자신의 모양과 느낌을 가진 버튼을 원합니다. 이 경우 Button이 아니라 Panel을 하위 클래스로 만드는 것이 좋습니다. Clicked 핸들러를 모든 요소에 연결할 수 있으므로 Panel을 기본 클래스로 사용하면 많은 유연성을 제공하면서 실제 Button 클래스의 불필요한 복잡성을 제거 할 수 있습니다.

다음은 Panel에서 자신 만의 버튼 컨트롤을 만드는 예제입니다.

<Panel ux : Class = "MyButton"HitTestMode = "LocalBounds"Margin = "4"Color = "# 25a">
    <string ux : Property = "Text"/>
    <텍스트 값 = "{ReadProperty 텍스트}"Color = "# fff"정렬 = "중앙"여백 = "30,15"/>

    <잠시 후>
        <변경 this.Color = "# 138"Duration = "0.05"DurationBack = ".2"/>
    </ WhilePressed>
</ Panel>

<MyButton Text = "Click me"/>
그러나 비 모바일 장치에서 사용자 정의 모양으로 되돌아가는 플랫폼 기본 단추를 원하면 Button을 하위 클래스에 추가해야합니다.

<Button ux : Class = "MyNativeButtonWithFallback"Margin = "2">
    <Panel ux : Template = "GraphicsAppearance"HitTestMode = "LocalBounds">
        <Text Value = "{ReadProperty Text}"Color = "# fff"맞춤 = "가운데"TextAlignment = "중앙"여백 = "10"/>
        <Rectangle CornerRadius = "4"Layer = "배경"Color = "# 25a"/>
    </ Panel>
</ Button>
NativeViewHost에 배치하면 버튼이 기본 버튼 컨트롤을 초기화하려고 시도합니다. 이것이 불가능할 경우 (예 : 바탕 화면에서 실행중인 경우) ux : Template = "GraphicsAppearance"로 지정된 템플리트로 되돌아갑니다.

<NativeViewHost>
    <! - 가능한 경우 기본입니다. ->
    <MyNativeButtonWithFallback Text = "일부 버튼"/>
</ NativeviewHost>
NativeViewHost 안에 Button을 배치하지 않으면 GraphicsAppearance 템플릿이 항상 버튼을 그리는 데 사용됩니다.

<MyNativeButtonWithFallback />
버튼 인터페이스