원본: https://www.fusetools.com/docs/fuse/navigation/router

라우터 클래스

 고급 기능 표시
이동 :
목차
퓨즈 앱의 일부 또는 전체에 대한 라우팅 및 내비게이션 기록을 관리합니다.

참고 : Fuse의 네비게이션 시스템 전체를 보려면 먼저 네비게이션 가이드를 읽는 것이 좋습니다.
라우터 클래스는 네비게이터 및 PageControl과 같은 라우터 출력과 함께 퓨즈에서 탐색의 기초를 형성합니다. 퓨즈 앱에서 이동하려면 경로가 라우터 인스턴스로 전송됩니다. 이 경로는 탐색 할 대상을 식별하는 문자열 경로와 탐색 할 때이 대상에 보낼 데이터 (선택 사항)로 구성되는 하나 이상의 부분으로 구성됩니다.

라우터는 경로를 수신하면 경로의 다른 부분에 대한 탐색을 재귀 적으로 수행합니다. 각 부분에 대해, 즉각적인 UX 트리를 검색하여이 부분의 문자열 경로를 사용하여 응용 프로그램의 일부로 이동할 라우터 콘센트를 찾습니다. 예를 들어 네비게이터의 템플릿 템플릿 키 또는 PageControl의 페이지 이름을 나타낼 수 있습니다.

라우터는 goto가있는 경로간에 직접 이동하거나 push 및 goBack을 사용하여 계층 적으로 탐색 할 수 있습니다.

일반적으로 앱은 App 루트에서 작동하는 단일 글로벌 라우터 인스턴스를 사용하며 전체 앱에 대한 단일 탐색 컨텍스트를 나타냅니다. 그러나 UX 트리의 다른 현지화 된 부분에 대해 별도의 라우터를 만드는 것이 가능합니다. 예를 들어 앱의 일부분에 다른 기록을 보관해야하는 경우 유용 할 수 있습니다.

예

다음 예제는 라우터 및 네비게이터를 사용하는 기본 탐색 설정을 보여줍니다. 퓨즈의 네비게이션 시스템에 대한 전체 소개 및 적절한 예를 보려면 네비게이션 가이드를 참조하십시오.

<자바 스크립트>
    module.exports = {
        gotoFirst : function () {router.goto ( "firstPage"); },
        gotoSecond : function () {router.goto ( "secondPage"); }
    };
</ JavaScript>

<라우터 ux : Name = "라우터"/>

<DockPanel>
    <네비게이터 DefaultTemplate = "firstPage">
        <Page ux : Template = "firstPage">
            <Text Alignment = "Center"> 첫 번째 페이지입니다. </ Text>
        </ Page>
        <Page ux : Template = "secondPage">
            <Text Alignment = "Center"> 두 번째 페이지입니다. </ Text>
        </ Page>
    </ Navigator>

    <Grid Dock = "Bottom"Columns = "1 *, 1 *">
        <Button Text = "첫 페이지"Padding = "20"Clicked = "{gotoFirst}"/>
        <Button Text = "두 번째 페이지"Padding = "20"Clicked = "{gotoSecond}"/>
    </ Grid>
</ DockPanel>
페이지 탐색 순서

라우터의 역사는 표준 히스토리 순서를 따르고, 최신 경로는 역사의 앞에 있고, 오래된 경로는 뒤쪽에 있습니다.

그러나 라우터는 탐색 순서에 설명 된대로 개별 컨트롤에서 페이지의 탐색 순서를 결정하지 않습니다. 이것은 사용되는 각 콘센트에 의해 제어됩니다.

라우터의 인터페이스