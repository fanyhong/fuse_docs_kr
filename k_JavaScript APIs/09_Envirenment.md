원본: https://www.fusetools.com/docs/fusejs/environment

퓨즈 JS / 환경 모듈 (JS)

 고급 기능 표시
이 페이지에는 고급 퓨즈 기능에 대한 설명서가 포함되어 있으므로 위의 "고급 기능 표시"체크 상자를 선택하여 몇 가지 추가 정보를 제공 할 수 있습니다.
이동 :
목차
환경 API를 사용하면 현재 실행중인 플랫폼을 확인할 수 있습니다.

이 기능을 사용하려면 프로젝트 파일에 "FuseJS"에 대한 참조를 추가해야합니다.

예제들

다음 부울 속성을 사용하여 앱이 실행중인 플랫폼을 확인할 수 있습니다.

var 환경 = require ( 'FuseJS / Environment');

if (Environment.ios) console.log ( "iOS에서 실행 중");
if (Environment.android) console.log ( "Android에서 실행 중");
if (Environment.preview) console.log ( "미리보기 모드에서 실행 중");
if (Environment.mobile) console.log ( "iOS 또는 Android에서 실행 중");
if (Environment.desktop) console.log ( "데스크탑에서 실행 중");
mobileOSVersion 속성을 사용하여 현재 모바일 OS의 버전을 사람이 읽을 수있는 문자열로 가져올 수도 있습니다.

console.log (Environment.mobileOSVersion);
노트

Android에서 mobileOSVersion은 Build.VERSION.RELEASE를 반환합니다 (예 : 1.0 또는 3.4b5). iOS에서는 <major>. <minor>. <patch> (예 : 9.2.1)의 형식으로 문자열을 반환합니다. 다른 모든 플랫폼에서 빈 문자열을 반환합니다.
위치

네임 스페이스
퓨즈
묶음
퓨즈 JS 0.42.2
환경 인터페이스

