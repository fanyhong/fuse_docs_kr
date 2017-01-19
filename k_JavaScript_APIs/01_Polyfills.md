원본: https://www.fusetools.com/docs/fusejs/polyfills

폴리 필

FuseJS는 지원되는 모든 플랫폼에서 (최소) EcmaScript 5.1 환경에서 실행됩니다. 웹 브라우저가 없으므로 FuseJS는 타사 라이브러리를 작동시키는 데 필요한 일부 브라우저 기능에 대해 polyfill을 제공합니다. 아직 구현이 완료되지 않았으므로 포럼에서 버그 및 기능 요청을보고하십시오.

현재 지원되는 polyfills

fetch - HTTP 요청을하는 기본 방법 (MDN 문서)
XMLHttpRequest - 서버와의 데이터 전송에 더 많은 기능을 제공하는 API (MDN docs)
Promise - 비동기 JavaScript 프로그래밍에서 매우 일반적인 개념입니다. 우리는 A + 약속 표준을 준수합니다.
setTimeout / clearTimeout - 미래에 실행될 기능을 예약합니다 (MDN 문서).
setInterval / clearInterval - 함수가 간격으로 실행되도록 예약합니다 (MDN 문서).
localStorage - 존재하는 로컬 저장소 (MDN 문서)
atob / btoa - base64와의 데이터 인코딩 (MDN 문서)
FileReader - JavaScript의 파일 내용로드 (MDN 문서)
EventTarget - 자바 스크립트에서 이벤트를 수신 할 수 있습니다 (MDN 문서).
