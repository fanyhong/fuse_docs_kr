원본: https://www.fusetools.com/docs/fusejs/lifecycle

라이프 사이클 모듈 (JS)

 고급 기능 표시
이동 :
목차
JS에서 애플리케이션 라이프 사이클 모니터링

앱의 라이프 사이클은 앱이 시작된 후 종료 될 때까지의 시간입니다. 이 시간 동안 앱은 여러 주를 통과합니다.

라이프 사이클 모듈을 사용하면 현재 상태를 쿼리하고 앱이 상태를 변경하면 경고를받을 수 있습니다.

미국

시작 중
배경
전경
대화식
시작 중

앱 시작 이벤트는 암시 적입니다. 자바 스크립트가 처음 평가 될 때입니다.

배경

앱이 사용자가 지금 대화 형 앱이 아니기 때문에 운영체제가 '잠자기'상태에 빠져 있습니다.

앱이이 상태에있는 동안 코드를 실행할 수는 없지만 JS / UX가해야 할 일이 아니더라도 퓨즈가 JS / UX를 보장하지는 않으므로 걱정할 필요가 없습니다.

전경

앱이 사용자 기기의 전면 중앙에 위치하지만 아직 앱과 상호 작용할 수 없습니다. 이 상태가되는 주된 이유는 사용자가 iOS 또는 Android에서 알림 표시 줄을 열었 기 때문입니다.

대화식

이제 앱이 포 그라운드에 있으며 사용자의 입력을 수락하고 있습니다.

상태 변경

앱이 무작위로 주를 뛰어 넘을 수 있다면 앱 수명주기를 사용하는 것이 어려울 것입니다. 대신에 우리는 다음과 같은 흐름을 주를 통해 전달합니다.

시작 ↓ 배경 ⟷ 전경 ⟷ 대화 형 ↓ 종료 중

종료 이벤트 없음

왜 종결 사건이 없는지 궁금해 할 수도 있습니다. 그 이유는 모바일 플랫폼에서 OS가 앱을 종료 할 때 전화하지 않는다고 약속하지 않기 때문입니다. 어떤 경우에는 (기억력 부족, 긴급 전화 통화 등) 어떤 경우에는 그렇지 않을 수도 있습니다.

이 때문에 모바일 플랫폼 용 가이드는 앱이 종료된다는 신호로 종료 이벤트를 사용하지 말 것을 강력히 권유합니다. 대신 정기적으로 앱의 '검사 점'을 지정해야 모든 종류의 종료를 복구 할 수 있습니다.

우리가 그것을 사용할 의도가 없다는 것을 감안할 때, 우리는 그 사건을 폭로하지 않기로 결정했다.

이 모듈은 EventEmitter이므로 EventEmitter의 메서드를 사용하여 이벤트를 수신 할 수 있습니다.

예

<자바 스크립트>
    var 라이프 사이클 = 필수 ( 'FuseJS / Lifecycle');

    Lifecycle.on ( "enteringForeground", function () {
        console.log ( "onForeground에");
    });
    Lifecycle.on ( "enteringInteractive", function () {
        console.log ( "onInInactive");
    });
    Lifecycle.on ( "exitedInteractive", function () {
        console.log ( "on exitedInteractive");
    });
    Lifecycle.on ( "enteringBackground", function () {
        console.log ( "on enteringBackground");
    });
    Lifecycle.on ( "stateChanged", function (newState) {
        console.log ( "on stateChanged"+ newState);
    });
    module.exports = {lifecycleState : Lifecycle.observe ( "stateChanged")}
</ JavaScript>
<StackPanel>
    <Text TextWrapping = "Wrap"> 퓨즈 모니터를 열어 로그보기 </ Text>
    <텍스트> 현재 수명주기 : </ Text>
    <텍스트 값 = "{lifecycleState}"/>
</ StackPanel>
라이프 사이클 인터페이스