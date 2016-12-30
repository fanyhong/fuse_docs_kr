원본: https://www.fusetools.com/docs/fuse/filesystem/filesystemmodule

FuseJS / 파일 시스템 모듈 (JS)

 고급 기능 표시
이동 :
목차
파일 시스템에 대한 인터페이스를 제공합니다.

var FileSystem = require ( "FuseJS / FileSystem");
비동기 Promise 기반 함수를 사용하면 UI를 응답 성있게 유지하는 것이 좋습니다. 동기식 변형도 선호하는 경우 사용할 수 있습니다.

응용 프로그램에 파일을 비공개로 저장하면 dataDirectory 속성을 기본 경로로 사용할 수 있습니다.

이 기능은 미리보기 기능이며 일부 세부 정보가 변경 될 수 있습니다. 특히 dataDirectory와 cacheDirectory가 다른 플랫폼에 매핑되는 방식이 바뀔 가능성이 있습니다.
예

이 예제는 텍스트를 파일에 쓴 다음 다시 읽습니다.

var FileSystem = require ( "FuseJS / FileSystem");
var 경로 = FileSystem.dataDirectory + "/"+ "testfile.tmp";

FileSystem.writeTextToFile ( "hello world"경로)
    .then (function () {
        return FileSystem.readTextFromFile (path);
    })
    .then (function (text) {
        console.log ( "읽은 파일 내용 :"+ text);
    })
    .catch (function (error) {
        console.log ( "오류로 인해 파일을 읽을 수 없습니다 :"+ 오류);
    });
FileSystemModule의 인터페이스