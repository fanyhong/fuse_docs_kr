원본: https://www.fusetools.com/docs/fuse/cameraroll/cameraroll

FuseJS / CameraRoll 모듈 (JS)

 고급 기능 표시
이동 :
목차
시스템 이미지 갤러리에 이미지를 추가하거나 가져올 수 있습니다.

퓨즈는 이미지를 고정 된 JavaScript 이미지 객체로 나타내며 경로, 파일 이름, 너비 및 높이로 구성됩니다. 이미지를 생성하거나 획득하면 이미지를 다른 API로 전달하여 기본 데이터를 사용, 가져 오거나 변경할 수 있습니다. 모든 이미지는 CameraRoll 또는 기타 게시를 통해 저장 공간이 지정 될 때까지 임시 "스크래치 이미지"입니다.

Android에서이 API를 사용하면 WRITE_EXTERNAL_STORAGE 및 READ_EXTERNAL_STORAGE 권한을 요청합니다.

참고 :이 API를 사용하려면 Fuse.CameraRoll에 대한 패키지 참조를 추가해야합니다.
예제들

카메라 롤에서 이미지 요청 :

var cameraRoll = require ( "FuseJS / CameraRoll");

cameraRoll.getImage ()
    .then (function (image) {
        // 사용자가 이미지를 성공적으로 선택하면 호출됩니다.
    }, function (오류) {
        // 사용자가 선택을 중단하거나 오류가 발생하면 호출됩니다.
    });
카메라로 사진 찍고 카메라 롤에 추가하기 :

var cameraRoll = require ( "FuseJS / CameraRoll");
var camera = require ( "FuseJS / Camera");

camera.take 사진 (640, 480)
    .then (function (image) {
        return cameraRoll.publishImage (image);
    })
    .then (function () {
        // 이미지가 성공적으로 카메라 롤에 추가 된 경우 호출됩니다.
    }, function (오류) {
        // 오류가 발생하면 호출됩니다.
    });
참고 : 위 예제가 작동하려면 Fuse.Camera에 대한 패키지 참조를 추가해야합니다.
CameraRoll의 인터페이스

