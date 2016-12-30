원본: https://www.fusetools.com/docs/fuse/controls/nativeviewhost

NativeViewHost 클래스

 고급 기능 표시
이동 :
목차
GraphicsView 위에 네이티브 컨트롤 레이어를 작성합니다. 하위 트리의 컨트롤은 OS가 제공하는 네이티브 컨트롤에 매핑됩니다.

퓨즈 앱에는 기본적으로 고성능 OpenGL 그래픽을 사용하여 UI 구성 요소가 렌더되도록 보장하는 루트 수준의 암시 적 GraphicsView가 포함되어 있습니다.

플랫폼 OS에 번들로 제공되는 기본 재고 컨트롤을 표시하려면 NativeViewHost를 사용할 수 있습니다. 일부 컨트롤은 네이티브 컨트롤 (예 : WebView 또는 MapView)에서만 사용할 수있는 반면 일부 컨트롤은 네이티브 컨트롤과 그래픽 컨트롤 (예 : ScrollView 또는 Rectangle)으로 사용할 수 있습니다.

참고 : 네이티브 컨트롤은 항상 그래픽 컨트롤 앞에 렌더링됩니다.

유일한 예외는 Fuse.Controls.NativeViewHost.RenderToTexture가 활성화 된 경우이며, 고유 한 제한 집합이 제공됩니다.
예제들

WebView는 기본보기로만 사용할 수 있습니다. 표시 방법은 다음과 같습니다.

<패널>
    <NativeViewHost>
        <WebView Url = "http://example.com"/>
    </ NativeviewHost>
</ Panel>
또한 일반적인 UX 마크 업과 마찬가지로 네이티브 컨트롤을 서로 겹치고 NativeViewHost 내에서 계층을 형성 할 수 있습니다.

<패널>
    <NativeViewHost>
        <Panel Alignment = "Top"패딩 = "15"Color = "# 0006">
            <Text>이 텍스트는 WebView 상단에 겹쳐 있습니다 </ Text>
        </ Panel>
        <WebView Url = "http://example.com"/>
    </ NativeviewHost>
</ Panel>
RenderToTexture 속성을 사용하여 네이티브 뷰를 텍스처로 렌더링하여 다른 그래픽 기반 비주얼에서 올바른 레이어 합성을 사용할 수 있습니다. 이것은 성능 비용으로 발생하며 네이티브 뷰는 텍스처로 렌더링되는 동안 상호 작용하지 않습니다.

<Text Alignment = "Center">이 텍스트는 NativeViewHost </ Text> 위에 레이어되어 있습니다.
<NativeViewHost RenderToTexture = "true">
    <사각형 색상 = "# 324"/>
</ NativeviewHost>
네이티브 구성 요소만으로 구성된 응용 프로그램을 만들려면 응용 프로그램의 루트 수준에 <NativeViewHost>를 배치하십시오.

<App>
    <NativeViewHost>
        <! - 전체 앱이 여기에옵니다. ->
    </ NativeviewHost>
</ App>
NativeViewHost의 인터페이스