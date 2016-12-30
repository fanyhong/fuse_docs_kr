원본: https://www.fusetools.com/docs/fuse/controls/pagecontrol

PageControl 클래스

 고급 기능 표시
이동 :
목차
기본 선형 탐색을위한 표준 전환, 사용자 상호 작용 및 페이지 처리 기능을 제공합니다.

예제들

다음 예제는 PageControl의 기본 동작을 보여줍니다.이 동작은 스 와이프 동작에 대한 응답으로 페이지를 슬라이드합니다.

<PageControl>
    <패널 배경 = "빨간색"/>
    <패널 배경 = "파란색"/>
</ PageControl>
PageControl은 라우터 콘센트로, 라우터에서 제어 할 수 있습니다. IsRouterOutlet 속성을 false로 설정하여이 동작을 비활성화 할 수 있습니다.

<자바 스크립트>
    module.exports = {
        gotoPage1 : function () {router.goto ( "page1"); },
        gotoPage2 : function () {router.goto ( "page2"); },
        gotoPage3 : function () {router.goto ( "page3"); }
    };
</ JavaScript>

<라우터 ux : Name = "라우터"/>

<PageControl>
    <패널 ux : Name = "page1"Color = "# e74c3c"Clicked = "{gotoPage2}"/>
    <패널 ux : Name = "page2"Color = "# 2ecc71"Clicked = "{gotoPage3}"/>
    <패널 ux : Name = "page3"Color = "# 3498db"Clicked = "{gotoPage1}"/>
</ PageControl>
데이터 바인딩을 사용하면 Active 속성을 사용하여 현재 활성 페이지를 이름으로 설정할 수 있습니다. 다음 예제에서는 3 개의 페이지와 사용자를 첫 번째 페이지로 되돌려주는 버튼이 있습니다.

<DockPanel>
    <자바 스크립트>
        var Observable = require ( "FuseJS / Observable");
        var currentPage = 관찰 가능 ( "page1");
        function clickHandler () {
            currentPage.value = "page1";
        }
        module.exports = {
            clickHandler : clickHandler,
            currentPage : currentPage
        };
    </ JavaScript>
    <PageControl Active = "{현재 페이지}">
        <Panel Name = "page1"Background = "Red"/>
        <Panel Name = "page2"Background = "Green"/>
        <Panel Name = "page3"Background = "Blue"/>
    </ PageControl>
    <Button Text = "Home"Clicked = "{clickHandler}"Dock = "Bottom"/>
</ DockPanel>
JavaScript를 사용하는 Pages 예제를 실제로 사용하는 방법을 살펴보십시오.

내비게이션 주문

PageControl의 페이지는 앞에서 뒤쪽으로 정렬되며 첫 번째 자식이 앞에 있습니다. 앞으로가는 것은 첫 번째 자녀를 향해가는 것을 의미하고 뒤로가는 것은 마지막 자녀를 향해가는 것을 의미합니다.

PageControl은 불연속적인 변경이 아닌 페이지 간의 지속적인 탐색을 사용합니다.

내비게이션 주문을 참조하십시오.

PageControl의 인터페이스