원본: https://www.fusetools.com/docs/fuse/localnotifications/localnotify

FuseJS / LocalNotifications 모듈 (JS)

 고급 기능 표시
이동 :
목차
로컬에서 생성 된 알림을 작성, 예약 및 이에 대응합니다.

앱이 포 그라운드에서 실행되지 않는 경우에도 앱의 일정에 대해 사용자에게 경고해야하는 경우가 있습니다. 이를 위해 대부분의 모바일 장치에는 알림이라는 개념이 있습니다. 이 문서에는 응용 프로그램 자체에서 예약 한 알림 인 '로컬 알림'이 포함되어 있습니다. '푸시 알림'은 다른 곳에서 서버로 전송되는 알림이며 여기에서 다룹니다.

OS 기능에 대한 많은 바인딩과 마찬가지로 가벼운 API로 시작하여 구축하는 것이 좋습니다. 우리는 의견 및 요청에 매우 관심이 있으므로 포럼에 들려서 알려주십시오.

설정하기

.unoproj 파일에 다음을 추가하여 Fuse 로컬 알림 라이브러리를 포함시킵니다.

"패키지": [
    ...
    "Fuse.LocalNotifications",
    ...
],
앱에서이 기능을 사용하기에 충분합니다. 이제 살펴 보겠습니다.

앱 예제

로컬 알림을 사용하는 완전한 퓨즈 앱입니다.

<App>
    <자바 스크립트>
        var LocalNotify = require ( "FuseJS / LocalNotifications");

        LocalNotify.on ( "receivedMessage", function (payload) {
            console.log ( "수신 된 지역 알림 :"+ payload);
            LocalNotify.clearAllNotifications ();
        });

        함수 sendLater () {
            LocalNotify.later (4, "마침내!", "4 초는 긴 시간", "hmm?", true);
        }

        함수 sendNow () {
            LocalNotify.now ( "Boom!", "Just like that", "payload", true);
        }

        module.exports = {
            sendNow : sendNow,
            sendLater : sendLater
        };
    </ JavaScript>
    <DockPanel>
        <TopFrameBackground DockPanel.Dock = "Top"/>
        <ScrollView>
            <StackPanel>
                <Clicked Button = "{sendNow}"Text = "지금 알림 보내기"Height = "60"/>
                <클릭 한 버튼 = "{sendLater}"텍스트 = "4 초 후에 알림 보내기"높이 = "60"/>
            </ StackPanel>
        </ ScrollView>
        <BottomBarBackground DockPanel.Dock = "Bottom"/>
    </ DockPanel>
</ App>
여기서 일어나는 일을 정리합시다.

어떻게 작동 하는가?

DockPanel 내부에서 module.exports와 stuff를 건너 뛸 것입니다. 다른 안내서에서 더 잘 설명되어 있습니다. 대신 JS를 살펴 보겠습니다.

우리 모듈을 정상적으로 요구 한 후, 우리는 4 초후에 알림을 전달할 함수를 설정합니다.

함수 sendLater () {
    LocalNotify.later (4, "마침내!", "4 초는 긴 시간", "hmm?", true);
}
이후 함수는 다음 매개 변수를 사용합니다.

secondsFromNow : 알림이 시작될 때까지의 시간 (초)
title : 장치의 알림 표시 줄에 알림이 표시 될 때 제목이 될 문자열입니다.
body : 알림 표시 줄에 표시 될 때 알림 본문이 될 문자열입니다.
payload : 알림 자체에는 표시되지 않지만 콜백에는 표시되는 문자열입니다.
소리 : 알림 표시 줄에 장치가 표시 될 때 장치가 기본 알림 소리를 내야하는지 여부를 지정하는 부울입니다.
badgeNumber : iOS에서만 사용되는 선택적 매개 변수로 앱 아이콘에 배지 번호를 표시합니다. 이것은 종종 사용자의주의가 필요한 '사물'의 양을 보여주기 위해 사용됩니다. 예를 들어 이메일 앱은 읽지 않은 이메일 수를 표시 할 수 있습니다.
함수 sendNow () {
    LocalNotify.now ( "Boom!", "Just like that", "payload", true);
}

now 함수는 secondFromNow 매개 변수를 가지고 있지 않다는 것을 제외하고는 나중에 함수와 거의 동일합니다.

두 가지 모두에 대한 최신 정보는 앱이 열려있는 경우 사용자에게 알림을 전달하지 않는다는 것입니다. 대신 receiveMessage 이벤트를 자동으로 트리거합니다.

마지막으로 우리는 알림을받을 때마다 호출 될 함수를 설정합니다.

LocalNotify.on ( "receivedMessage", function (payload) {
    console.log ( "수신 된 지역 알림 :"+ payload);
    LocalNotify.clearAllNotifications ();
    LocalNotify.clearBadgeNumber ();
});
이 함수는 앱이 열려있는 동안 또는 사용자가 선택한 알림에서 앱이 시작될 때 알림이 전달 될 때마다 호출됩니다.

페이로드는 다음 키가 포함 된 JSON 형식의 문자열입니다.

'title': 문자열로 된 알림 제목
'body': 문자열의 알림 본문
'payload': 알림과 함께 전송 된 데이터 문자열입니다.
clearAllNotifications ()는 이미 전달 된 앱의 모든 알림을 지 웁니다. 클릭하면 유사한 알림을 제거하는 데 사용할 수 있습니다.

마지막으로, clearBadgeNumber ()는 홈 화면의 앱 아이콘 옆에 표시된 작은 숫자를 지우고 앱에 알림의 양을 표시합니다.

라이프 사이클 행동

OS가 알림을 처리하는 방법은 앱의 상태에 따라 다릅니다. 앱이 대화 형 인 경우 알림이 표시되지 않고 대신 실행중인 앱에 바로 전달됩니다. 대화식이 아닌 경우 OS는 사용자가 나중에 제공 한 매개 변수 또는 사용자가 제공하지 않은 기능을 기반으로 알림을 작성합니다. 대화 형은 앱이 포 그라운드에 있음을 의미 할뿐만 아니라 다른 창에 의해 가려지지 않는다는 것을 의미합니다. 대화 형이 아닌 전경에있는 한 가지 예는 상태 표시 줄을 스 와이프하여 '알림 센터 / 서랍'을 열 때입니다.

위의 예제 응용 프로그램을 사용하여이 작업을 시도 할 수 있습니다. 4 초 후에 알림 보내기 버튼을 클릭하고 '알림 센터 / 서랍'

비고

이 모듈은 EventEmitter이므로 EventEmitter의 메서드를 사용하여 이벤트를 수신 할 수 있습니다.

이 기능을 사용하려면 프로젝트 파일에서 "Fuse.LocalNotifications"에 대한 참조를 추가해야합니다.

LocalNotify의 인터페이스

