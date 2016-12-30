원본: https://www.fusetools.com/docs/fuse/controls/navigator

네비게이터 클래스

 고급 기능 표시
이동 :
목차
주문형 인스턴스 생성 및 페이지 재활용 기능이있는 범용 탐색 컨테이너입니다.

참고 : Fuse의 네비게이션 시스템 전체를 보려면 먼저 네비게이션 가이드를 읽는 것이 좋습니다.
페이지

네비게이터는 템플릿 모음을 자식으로 사용합니다. 이를 통해 필요에 따라 페이지를 인스턴스화하고 재활용 할 수 있습니다.

ux : Template 속성을 지정하여 노드를 템플릿으로 선언 할 수 있습니다. 경로 경로는 ux : Template 값과 일치하여 템플릿을 선택합니다.

<Page ux : Template = "matchPath">
템플릿에 대한 자세한 내용은 여기를 참조하십시오.

템플릿이 아닌 페이지도 사용할 수 있습니다. 페이지 이름이 경로와 일치하는 데 사용됩니다.

<Page Name = "matchPath">
이 페이지는 항상 하나의 인스턴스 만 갖고, 항상 재사용되며 절대로 제거되지 않습니다. 그렇지 않으면 템플릿 페이지와 동일하게 작동합니다.

다음은 템플릿 또는 비 템플릿 페이지 중 어느 것을 사용할지 결정하는 데 도움이되는 몇 가지 일반적인 규칙입니다.

같은 경로이지만 매개 변수가 다른 페이지간에 전환이 필요한 경우 템플릿을 사용하십시오.
비활성 상태 일 때도 성능에 영향을주는 페이지가 있거나 사용하지 않을 때 다른 이유로 페이지를 제거해야하는 경우 템플리트를 사용하십시오.
상태를 유지하기 위해 항상 존재해야하거나 매우 자주 탐색되는 페이지가있는 경우 템플릿이 아닌 템플릿을 사용하십시오.
하나의 네비게이터 내에서 템플릿 및 비 템플릿을 혼합 할 수 있습니다.

전환

네비게이터에는 push (), goBack () 및 goto ()의 동작과 일치하는 일련의 기본 전환이 있습니다.

페이지 전환을 완벽하게 제어하려면 PageView 클래스를 사용하십시오. 네비게이터처럼 작동하지만 표준 전환이나 상태 변경이 없습니다.

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
    <네비게이터 DefaultPath = "firstPage">
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
내비게이션 주문

네비게이터는 탐색하는 동안 개별 페이지 진행률 변경 사항을 사용합니다. 활성 페이지는 진행률이 0입니다. 페이지가 푸시되면 1에서 시작하여 즉시 0으로 전환됩니다. 이전에 활성화 된 페이지는 -1이됩니다. "뒤로"작업은 변환을 되돌립니다.

-1, 0 및 1 만 진행됩니다. 더 먼 거리는 계산되지 않으며 부분 값도 가능하지 않습니다.

내비게이션 주문을 참조하십시오.

네비게이터의 인터페이스