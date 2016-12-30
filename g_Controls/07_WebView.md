원본: https://www.fusetools.com/docs/fuse/controls/webview

WebView 클래스

 고급 기능 표시
이동 :
목차
안드로이드 및 iOS에 기본적으로 웹 콘텐츠를 표시합니다.

WebView는 네이티브 전용이므로 NativeViewHost에 포함되어야합니다.

WebView는 HTML 프로토콜을 통해 또는 HTML을 문자열로로드하여 웹 컨텐츠를 표시하고 PageBeginLoading, WhilePageLoading 및 PageLoaded와 같은 사용자 정의 된 탐색 환경을 구축하는 데 유용한 트리거에 연결합니다. GoBack 및 GoForward와 같은 탐색 트리거는 Reload, LoadUrl 및 LoadHtml과 같은 WebView 관련 항목으로 보완됩니다. 또한 ProgressAnimation을 구동하는 데 사용할 수 있습니다.

EvaluateJS 트리거는 WebView 컨텍스트에서 임의의 JavaScript를 실행하고 결과 데이터를 Fuse로 다시 제공 할 수 있으므로 주목할만한 기능입니다.

<App Background = "# 333">
    <자바 스크립트>
            module.exports = {
                onPageLoaded : function (res) {
                    console.log ( "WebView 도착"+ JSON.parse (res.json) .url);
            }
        };
    </ JavaScript>
    <DockPanel>
        <StatusBarBackground Dock = "위쪽"/>
        <NativeViewHost>
            <WebView Dock = "채우기"Url = "http://www.google.com">
                <PageLoaded>
                    <EvaluateJS 처리기 = "{onPageLoaded}">
                        var result = {
                            url : document.location.href
                        };
                        반환 결과;
                    </ EvaluateJS>
                </ PageLoaded>
            </ WebView>
        </ NativeviewHost>

        <BottomBarBackground Dock = "Bottom"/>
    </ DockPanel>
</ App>
WebViews는 HTML 노드를 래핑하거나 LoadHtml 트리거 액션을 통해 표시 할 원시 HTML을 제공받을 수도 있습니다.

<LoadHtml TargetNode = "myWebView"BaseUrl = "http : //my.domain"Source = "{html}"/>

WebView의 인터페이스
