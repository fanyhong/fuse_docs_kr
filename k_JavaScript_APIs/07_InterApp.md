원본: https://www.fusetools.com/docs/fuse/reactive/fusejs/interapp

FuseJS / InterApp 모듈 (JS)

  고급 기능 표시
이동 :
목차
InterApp API를 사용하면 다른 앱을 실행하고 URI 스킴을 통해 실행되는 것에 응답 할 수 있습니다.

이 모듈은 EventEmitter이므로 EventEmitter의 메서드를 사용하여 이벤트를 수신 할 수 있습니다.

이 기능을 사용하려면 프로젝트 파일에서 "Fuse.Launcher"에 대한 참조를 추가해야합니다.

예

var InterApp = require ( "FuseJS / InterApp");

InterApp.on ( "receivedUri", function (uri) {
     console.log ( "URI로 시작 함", uri);
});

InterApp.launchUri ( "https://www.fusetools.com/");
receivedUri 이벤트가 트리거되도록하려면 여기에 표시된대로 앱에 대한 맞춤 URI 스키마를 등록해야합니다.

InterApp의 인터페이스
