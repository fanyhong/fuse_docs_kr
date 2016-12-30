원본: https://www.fusetools.com/docs/fuse/reactive/fusejs/timermodule

퓨즈 JS / 타이머 모듈 (JS)

  고급 기능 표시
이동 :
목차
Timer API를 사용하면 지정된 시간 후에 실행할 함수를 예약 할 수 있습니다.

var Timer = require ( "FuseJS / Timer");

Timer.create (function () {
     console.log ( "3 초 후에 한 번 실행됩니다");
}, 3000, false);

Timer.create (function () {
     console.log ( "영원히 10 초마다 실행됩니다");
}, 10000, true);
TimerModule의 인터페이스
