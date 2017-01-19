원본: https://www.fusetools.com/docs/fusejs/fusejs

소개

FuseJS는 크로스 플랫폼 모바일 앱 비즈니스 로직을 작성하기위한 JavaScript 프레임 워크입니다.

시작하기

FuseJS는 <JavaScript> 태그를 통해 @ (UX 마크 업)에서 다음과 같이 외부 JavaScript 파일을 가리켜 사용할 수 있습니다.

<JavaScript File = "SomeCode.js"/>
또는 다음과 같이 태그에서 JavaScript 코드를 인라이닝합니다.

<자바 스크립트>
    console.log ( "Hello, FuseJS!");
</ JavaScript>
모듈

FuseJS는 CommonJS 모듈 시스템을 구현합니다. 각 코드 파일 또는 인라인 스 니펫은 모듈입니다.

모듈 내부를 외부에서 볼 수있게하려면 module.exports-construct를 사용합니다.

<자바 스크립트>
    module.exports = {
        exportedSymbol : "여보세요, 나머지 세상!"
    };
</ JavaScript>
모듈에서 내보내기에 실패하면 모듈 내부의 정의 된 데이터에 도달 할 수 없습니다.

<자바 스크립트>
    var data = [1, 2, 3];
    var invisible = "나는 보이지 않는다";

    module.exports = {
        데이터 : 데이터
    };
</ JavaScript>
이는 다른 호출 JavaScript 모듈 및 UX 코드에서 구현 세부 사항을 숨기는 데 유용합니다.

모듈 가져 오기

각 코드 파일 (또는 인라인 스 니펫)은 모듈입니다.

require를 통해 다른 모듈에서 모듈을 사용할 수있게하려면 별개의 .js 파일에 모듈을 배치해야합니다.이 모듈은 .unoproj의 : 번들에 포함되어 있어야합니다 (예 :

"포함": [
    "MyLibrary / SomeModule.js : Bundle",
    ...
그런 다음 같은 프로젝트의 다른 모듈에서이 모듈에 액세스 할 수 있습니다.

var myModule = require ( 'MyLibrary / SomeModule');
require () 문자열에 .js 확장자를 포함시키는 것은 선택 사항입니다.
또한 glob을 사용하여 전체 폴더를 자동으로 묶을 수 있습니다 (예 : "MyLibrary / *. js : Bundle".

프로젝트의 모든 JavaScript 파일을 번들 파일로 포함하려면 다음을 수행하십시오.

"포함": [
    "**. js : 번들"
]
그러면 JavaScript 파일 이름을 요구할 수 있습니다.

var myModule = require ( '/ yourJavaScriptFile.js');
파일 이름 접두사 앞에 "/"가 있으면 프로젝트 루트 디렉토리를 기준으로 파일을 찾습니다. 현재 파일에 상대적인 파일 이름을 지정하려면 "./"접두어를 붙이십시오. 접두사를 생략함으로써 파일 이름은 프로젝트 루트 나 해당 모듈의 전역 모듈에 상대적입니다.

var relativeToProjectRoot = require ( '/ SomeComponent.js');
var relativeFile = require ( './ MainView.js');
var relativeToRootOrGlobalModule = require ( 'SomeOtherComponent.js');

디자인과 동기

FuseJS의 핵심 설계 목표는 JS 코드를 작고 깨끗하게 유지하는 것입니다.이 코드는 애플리케이션의 실제 기능에만 관심이 있습니다. 한편 레이아웃, 데이터 표시, 애니메이션 및 제스처 응답과 같은 UX 중심의 모든 것이 선언적 UX 마크 업과 기본 UI 구성 요소에 맡겨져 있습니다.

퓨즈가 JavaScript 비즈니스 로직과 UX 마크 업 프레젠테이션을 분리하는 방법은 몇 가지 확실한 이점이 있습니다.

성능 - 모든 성능에 중요한 비트는 네이티브 코드로 처리되며 네이티브 UI 구성 요소를 기반으로 처리됩니다.
쉬운 선언적 코드는 제한된 프로그래밍 지식으로도 읽고, 쓰고 이해하기 쉽습니다.
오류 발생 가능성이 적다 - 상태가 적어지면 잘못 될 가능성이 줄어 듭니다.
시각적 도구 - UX 마크 업은 검사자, 타임 라인 및 일반적으로 멋진 드래그 앤 드롭 피스와 같은 퓨즈 도구로 편집 할 수 있습니다.
퓨즈에는 JavaScript (즉, 명령형)에서 애니메이션을 제어해야하는 필요성을 대체하는 많은 수의 선언적 API (설계된 UX 마크 업)가 있습니다.

다른 많은 JavaScript 프레임 워크는 명령형 UI 코드, 애니메이션 및 성능 중요한 작업을 JavaScript로 섞습니다. 따라서 FuseJS를 처음 접하는 사람들은 처음부터 이러한 방식으로 작업을 시도하는 경향이 있습니다. 이러한 것들의 대부분은 기술적으로 FuseJS에서 가능하지만, 실망하고 있습니다. 새로운 방식으로 일하기위한 느낌을 얻기 위해 퓨즈 예제를 공부하는 데 시간을 할애하는 것이 좋습니다.

뷰와 로직을 UX 마크 업과 JavaScript로 분리하여 코드를 정제하면 코드 기반을 크게 줄이고 유지 관리가 용이하며 UX 디자이너와 개발자 간의 효과적인 공동 작업을 할 수 있습니다.

성능이 중요한 비즈니스 로직을 작성해야하는 경우 원시 코드 또는 JS 대신 Uno 코드로 수행하는 것이 좋습니다.
