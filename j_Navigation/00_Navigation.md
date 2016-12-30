원본: https://www.fusetools.com/docs/navigation/navigation

항해

이 튜토리얼에서는 라우터, 네비게이터 및 PageControl과 같은 클래스를 사용하여 Fuse의 기본 네비게이션 시스템이 어떻게 작동하는지에 대해 간략하게 설명합니다.

완전한 애플리케이션 예제

이 응용 프로그램 예제에서는 퓨즈에서 내비게이션 시스템을 사용하는 방법을 보여줍니다.

Cattr - 퓨즈의 기본 멀티 뷰 앱 예입니다.
페이지 만들기

다중 페이지 응용 프로그램을 디자인 할 때 각 페이지를 다음과 같이 ux : Class로 자체 UX 파일에 정의하는 것이 좋습니다.

LoginPage.ux

<Page ux : Class = "LoginPage">
    <라우터 ux : 종속성 = "라우터"/>
    <JavaScript File = "LoginPage.js"/>
    ...
</ Page>
SettingsPage.ux

<Page ux : Class = "SettingsPage">
    <라우터 ux : 종속성 = "라우터"/>
    <JavaScript File = "SettingsPage.js"/>
    ...
</ Page>
그리고 각 페이지마다.

각 페이지가 해당 페이지의 내부 논리를 처리하는 자체 <JavaScript> 태그를 가질 수있는 방법에 유의하십시오.

대부분의 페이지는 앱의 라우터에 액세스해야하므로 ux : Dependency = "router"로 선언합니다. 즉, 해당 페이지의 JavaScript가 해당 라우터에 액세스 할 수 있습니다. 페이지에 다른 종속성이있는 경우 동일한 방식으로 선언 할 수 있습니다.

앱 어셈블하기

주요 UX 파일의 기본 구조는 다음과 같아야합니다.

<App>
    <라우터 ux : Name = "라우터"/>
    <네비게이터 DefaultPath = "로그인">
        <LoginPage ux : Template = "login"router = "router"/>
        <HomePage ux : Template = "home"router = "router"/>
        <SettingsPage ux : Template = "settings"router = "router"/>
        <UserProfilePage ux : Template = "user"router = "router"/>
    </ Navigator>
</ App>
ux : Template 속성은 이것이 실제 객체 인스턴스가 아니라 필요에 따라 인스턴스화 될 수있는 템플릿임을 나타냅니다. 네비게이터는 필요에 따라 각 템플릿의 인스턴스를 인스턴스화하고 더 이상 필요없는 인스턴스를 재활용합니다.

DefaultPath는 응용 프로그램이 시작될 때 경로가 아직 설정되지 않은 경우 기본적으로 어떤 템플릿을 인스턴스화해야하는지 지정합니다.

우리가 속성과 마찬가지로 의존성을 주입하는 방법에 주목하십시오. 이름과 같이 종속성 이름을 소문자로 유지하는 것이 좋습니다. 이렇게하면 개체의 이름을 바꾸지 않고 구성 요소를 추출 할 수 있으며 종속성과 속성 간의 차이를 쉽게 파악할 수 있습니다.

모든 종속성을 지정해야합니다. 그렇지 않으면 컴파일 시간 오류가 발생합니다.

페이지는 Page 클래스의 자손 일 필요는 없습니다. Panel과 같은 모든 시각적 객체를 사용할 수 있습니다.
둘러보기 (평면 탐색)

위의 예에서 로그인 템플릿 (LoginPage 클래스)이 시작 페이지가됩니다.

LoginPage에 버튼을 제공합니다.

<버튼 텍스트 = "로그인"클릭 = "{로그인 클릭}"/>
LoginPage.js에서 Clicked 이벤트를이 함수까지 연결합니다.

function loginClicked () {
    // TODO : 로그인 자격 증명 확인

    router.goto ( "home");
}
이것은 당신이 기대하는 것을 할 것이며, 네비게이터는 로그인 스크린을 움직이게 할 것이고, bam! 우리는 홈 페이지에 있습니다.

귀하의 module.exports에 loginClicked를 추가하십시오.
계층 적 탐색

계층 적 탐색은 임시 페이지를 탐색 ( "푸시") 한 다음 온 스크린 단추 또는 장치의 실제 뒤로 단추를 사용하여 사용자가 어디에서 왔는지 나중에 "팝"(되돌아 감)합니다.

버튼이 설정 페이지를 여는 것으로 가정하면 뒤로 버튼은 우리가 어디서 왔는지에 관계없이 다시 돌아와야합니다.

router.push ( "settings");
진정해? 이제 안드로이드에있는 뒤로 버튼은 우리를 다시 데려 갈 것입니다.

로컬 미리보기에서 Cmd / Ctrl + B를 사용하여 뒤로 버튼을 에뮬레이트하십시오. iOS에서는 3 손가락을 1 초 동안 쥐고 뒤로 버튼을 에뮬레이션 할 수있는 메뉴를 표시합니다.
자바 스크립트에서 go-back을 트리거 할 수도 있습니다.

router.goBack ();
push ()를 여러 번 사용하면 "더 깊숙이"탐색 할 수 있습니다. 그런 다음 뒤로 버튼을 여러 번 눌러서 모든 것을 되 찾을 수 있습니다.

푸시 된 페이지의 깊은 스택 내부에서 goto ()를 사용하여 스택을 삭제하고 해당 페이지로 이동하기 만하면됩니다. 그 후에 뒤로 버튼을 사용하면 뒤로 물러 가지 않습니다.

페이지에 매개 변수 전달하기

때로는 동일한 페이지 템플리트를 사용하지만 다른 매개 변수로 열려고하는 경우가 있습니다.

예를 들어, 사용자 페이지는 실제로 프로필을 표시 할 사용자의 ID를 알면 도움이됩니다.

이것은 인수 객체를 라우터에 전달함으로써 쉽습니다.

router.push ( "user", {userId : id});
그러면 제공된 객체를 매개 변수로 사용하여 사용자 템플릿의 새 인스턴스가 일시적으로 열립니다.

UserPage.js에서 Observable Parameter를 사용하여 매개 변수를 읽을 수 있습니다. Observable Parameter를 사용하면 매개 변수의 변경 사항에 대응할 수 있습니다.

var userId = this.Parameter.map (function (param) {
    return param.userId;
});

module.exports = {
    userId : userId
};
this.Parameter가 Observable이라는 사실을 유의하십시오. 즉, 모듈을 평가할 때 해당 값을 사용할 수있는 것은 아닙니다. 위와 같이 관찰 가능한 연산자를 사용하여 노출 시키거나 onValueChanged 함수를 사용하여 처리기를 추가하십시오.

this.Parameter.onValueChanged (module, function (param) {
    //이 시점에서 새로운 매개 변수 값을 알 수 있습니다.

    // 모듈은 모듈의 수명을 연결하는 데 사용됩니다.
    // 모듈의 수명에 대한 가입
});
UserPage.js의 루트 범위에서 UserPage 클래스의 현재 인스턴스를 참조합니다.

JavaScript에서는 this 키워드의 의미가 함수 범위에 따라 다릅니다. 모듈의 루트에서 올바른 인스턴스에 대한 참조를 가져 왔는지 확인하십시오.
같은 UserPage 인스턴스가 네비게이터에 의해 재사용되어 시간 경과에 따라 다른 사용자를 표시 할 수 있습니다. 매개 변수는 새 사용자를 표시해야하는 경우에만 업데이트됩니다.

다단계 항법

때로는 한 수준의 탐색 만 있으면 충분하지 않을 수도 있습니다. 예를 들어, 홈 화면에는 자체 내에 여러 페이지가있을 수 있습니다.

라우터는이 문제를 쉽게 처리 할 수 ​​있습니다. 우리 HomePage.ux가 다음과 같이 보입니다.

<Page ux : Class = "HomePage">
    <라우터 ux : 종속성 = "라우터"/>
    <JavaScript File = "HomePage.js"/>
    <PageControl>
        <SocialFeedPage ux : Name = "socialfeed"/>
        <DirectChatsPage ux : Name = "chats"/>
        <PinnedMessagesPage ux : Name = "pinned"/>
        <RecentNotificationsPage ux : Name = "recent"/>
    </ PageControl>
</ Page>
참고 : PageControl은 템플릿이 아닌 실제 페이지 객체를 가져옵니다. PageControl은 모든 페이지를 활성 상태로 유지하고 각각의 인스턴스를 하나만 유지합니다. 그들 사이에 스 와이프 네비게이션을 허용합니다.
Navigator와 PageControl은 모두 소위 라우터 콘센트입니다. 이것은 그들이 라우팅에 참여한다는 것을 의미합니다. .goto () 또는 .push ()를 사용할 때 각 수준에 대한 매개 변수가있는 다중 수준 경로를 제공 할 수 있습니다.

예를 들어, 우리의 로그인 화면은 SocialFeedPage 대신에 RecentNotificationsPage로 직접 보내고 싶을 수도 있습니다. 이것은 다음과 같이 수행됩니다.

router.goto ( "집", {}, "최근", {scrollOffset : 300});
그리고 그게 다야! 먼저 네비게이터가 홈 템플리트로 이동하면 해당 템플리트 내의 PageControl이 최근 페이지로 이동합니다.

이 경우 홈 템플릿은 매개 변수를 사용하지 않으므로 빈 객체 ({})를 전달합니다.

한편, 최근 템플릿은 매개 변수를 페이지에 전달할 수있는 방법의 예로서 매개 변수 Observable에 {scrollOffset : 300} 매개 변수 객체를 가져옵니다.

사용자 정의 애니메이션 / 전환

기본적으로 페이지 객체에는 기본적으로 Transition = "Default"가 내장되어 있습니다. 즉, 포함 된 컨트롤이 의미있는 기본 전환을 정의한다는 의미입니다. 예를 들어 PageControl은 기본적으로 페이지를 가로로 슬라이드하여 입력 및 종료하고, Navigator에서는 왼쪽에서 밀어 넣고 축소하고 배경으로 페이드 아웃합니다.

변환을 사용자 정의하려면 먼저 기본 변환을 먼저 비활성화해야합니다.

<Page ux : Class = "SettingsPage"Transition = "None">
전환 = "없음"을 지정하지 않으면 사용자 정의 애니메이션이 기본 전환에 "추가"/ "추가"됩니다.

네비게이터는 Fuse의 표준 네비게이션 애니메이션 시스템과 함께 작동합니다. 즉, 페이지 전환은 다음 트리거에 의해 정의됩니다.

EnteringAnimation - 입력 페이지에 대해 역방향으로 재생됩니다 (페이지가 활성 페이지가 됨).
ExitingAnimation - 푸시로 대체 된 페이지에 대한 전달 재생
RemoveAnimation - 이동으로 인해 제거 된 페이지에 대한 전달을 재생했습니다.
너 돌아올 때 (), 반대가 일어난다. '

EnteringAnimation - goBack에 의해 삭제 된 페이지의 전달을 재생합니다.
ExitingAnimation - 복원중인 페이지에서 뒤로 재생 bt a goBack
이 시스템은 머리를 감싸는 데 약간 까다로울 수 있지만 사실 네 가지 경우 모두를 완전히 사용자 정의 할 수 있어야합니다.

예 :

<Page ux : Class = "SettingsPage"Transition = "None">
    <입장 입력>
        <Move X = "1"RelativeTo = "Size"Duration = "0.4"Easing = "BackOut"/>
    </ EnteringAnimation>
    <종료 애니메이션>
        <이동 Y = "1"RelativeTo = "크기"지속 시간 = "0.4"Easing = "CubicIn"/>
    </ ExitingAnimation>
    ...
</ Page>
이렇게하면 설정 페이지가 왼쪽에서부터 들어가고 화면 하단에서 빠져 나옵니다. <Change this.Opacity = "0"Duration = "0.3"../>, 크기 조절, 회전 또는 어떤 느낌이든 추가하여 마음껏 즐기십시오.

많은 페이지가 있고 전환 코드를 공유하기를 원한다면 공통 기본 클래스를 만들 수 있습니다.

<Page ux : Class = "FancyTransitionPage"Transition = "None">
    <입장 입력>
        <Move X = "1"RelativeTo = "Size"Duration = "0.4"Easing = "BackOut"/>
    </ EnteringAnimation>
    <종료 애니메이션>
        <이동 Y = "1"RelativeTo = "크기"지속 시간 = "0.4"Easing = "CubicIn"/>
    </ ExitingAnimation>
</ Page>
그런 다음이를 페이지의 기본 클래스로 사용하십시오.

<FancyTransitionPage ux : Class = "SettingsPage">
    ...
</ FancyTransitionPage>
ActivatingAnimation, WhileActive 등과 같은 다른 탐색 기반 트리거도 PageControl 및 Navigator에서 예상대로 작동합니다.

끝났어!

이제 앱 탐색 및 구조화를위한 우아하고 확장 가능한 모델을 만들었습니다.