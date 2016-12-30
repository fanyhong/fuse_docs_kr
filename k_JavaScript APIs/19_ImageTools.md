원본: https://www.fusetools.com/docs/fuse/imagetools/imagetools

퓨즈 JS / ImageTools 모듈 (JS)

 고급 기능 표시
이동 :
목차
일반적인 이미지 조작을위한 유틸리티 메소드.

퓨즈는 이미지를 고정 된 JavaScript 이미지 객체로 나타내며 경로, 파일 이름, 너비 및 높이로 구성됩니다. 이미지를 생성하거나 획득하면 이미지를 다른 API로 전달하여 기본 데이터를 사용, 가져 오거나 변경할 수 있습니다. 모든 이미지는 CameraRoll 또는 기타 게시를 통해 저장 공간이 지정 될 때까지 임시 "스크래치 이미지"입니다.

이 API를 사용하는 Android에서는 WRITE_EXTERNAL_STORAGE 및 READ_EXTERNAL_STORAGE 권한을 요청합니다.

예

<자바 스크립트>
    var ImageTools = require ( "FuseJS / ImageTools");
    var Observable = require ( "FuseJS / Observable");

    var imagePath = Observable ();
    var base64Image = "iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPAQMAAAABGAcJAAAABlBMVEX9 // wAAQATpOzaAAAAH0l"+
                        "EQVQI12MAAoMHIFLAAYSEwIiJgYGZASrI38AAAwBamgM5VF7xgwAAAABJRU5ErkJggg ==";
    ImageTools.getImageFromBase64 (base64Image)
    .then (function (image) {
        imagePath.value = image.path;
    });

    module.exports = {테스트 : new Date (). toString (), image : imagePath};
</ JavaScript>
<이미지 파일 = "{이미지}"/>
ImageTools의 인터페이스
