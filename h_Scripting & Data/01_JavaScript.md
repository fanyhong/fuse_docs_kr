원본: https://www.fusetools.com/docs/fuse/reactive/javascript

자바 스크립트 자바 스크립트 클래스

 고급 기능 표시
이동 :
목차
JavaScript 태그는 JavaScript를 실행하는 데 사용되며 parent.video의 데이터 컨텍스트로 module.export를 할당합니다.

시작하기

JavaScript는 JavaScript 클래스를 통해 UX 마크 업에서 다음과 같이 외부 JavaScript 파일을 가리켜 사용할 수 있습니다.

<JavaScript File = "SomeCode.js"/>
또는 다음과 같이 태그에서 JavaScript 코드를 인라이닝합니다.

<자바 스크립트>
    console.log ( "Hello, FuseJS!");
</ JavaScript>
퓨즈에 대하여

FuseJS는 크로스 플랫폼 모바일 앱 비즈니스 로직을 작성하기위한 JavaScript 프레임 워크입니다. 네이티브 모바일 앱을 만드는 데 필요한 기본 기능뿐만 아니라 기능적으로 반응적인 방식으로 UI에 데이터를 표시 할 수있는 Observable 클래스를 다루는 일련의 클래스로 구성되어 있습니다.

모듈

FuseJS는 CommonJS 모듈 시스템을 구현합니다. 각 코드 파일 또는 인라인 스 니펫은 모듈입니다.

데이터와 함수를 다른 모듈에 공개하기 위해 module.exports 객체에 추가 할 수 있습니다.

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

각 코드 파일 (또는 인라인 스 니펫)은 모듈을 정의합니다.

자바 스크립트 모듈을 파일 이름으로 가져올 수 있습니다. 이렇게하려면 JavaScript 파일이 .unoproj 파일에 "번들"파일로 포함되어 있는지 확인하십시오.

"포함": [
    "yourJavaScriptFile.js : Bundle"
    .. 다른 파일 ..
]
또는 모든 JavaScript 파일을 묶음 파일로 포함 시키려면 다음을 수행하십시오.

"포함": [
    "**. js : 번들"
]
그런 다음 JavaScript 파일 이름을 사용하도록 요구할 수 있습니다.

var myModule = require ( '/ someJavaScriptFile.js');
파일 이름 접두사 앞에 "/"가 있으면 프로젝트 루트 디렉토리를 기준으로 파일을 찾습니다. 현재 파일에 상대적인 파일 이름을 지정하려면 "./"접두어를 붙이십시오. 접두사를 생략함으로써 파일 이름은 프로젝트 루트 나 해당 모듈의 전역 모듈에 상대적입니다.

var relativeToProjectRoot = 필요 ( '/ SomeComponent');
var relativeFile = require ( './ MainView');
var relativeToRootOrGlobalModule = require ( 'SomeOtherComponent.js');
원하는 경우 파일 이름에 .js 파일 확장자를 생략 할 수 있습니다.
예 :

파스 백엔드 예제가있는 TODO 앱
모듈 인스턴스

퓨즈의 <JavaScript> 태그 처리는 CommonJS 모듈 시스템에서 모듈이 작동하는 것과 중요한 차이점이 있습니다.

<JavaScript> 태그 안의 모듈 (또는 외부 파일이 가리키는)은 주변 UX 범위가 인스턴스화 될 때마다 한 번씩 인스턴스화됩니다. 즉, <JavaScript> 태그가 구성 요소의 일부인 경우 해당 구성 요소의 각 인스턴스는 코드를 초기화하고 별도의 로컬 변수 및 내보내기 집합을 갖습니다.

모듈 정리

퓨즈에서 자바 스크립트 모듈은 즉시 생성되고 파괴되는 여러 모듈 인스턴스에 해당 할 수 있습니다. 모듈이 명시적인 Observable 구독 만들기와 같이 수동 정리가 필요한 리소스를 할당하는 경우 처리기를 module.disposed에 할당하고 거기서 직접 정리할 수 있습니다.

예:

var foo = getSomeGlobalObservable ();

함수 fooChanged () {...}

foo.addSubscriber (fooChanged);

...

module.disposed = function () {
    foo.removeSubscriber (fooChanged)
}
디자인과 동기

FuseJS의 핵심 설계 목표는 JavaScript 코드를 작고 깨끗하게 유지하는 것입니다.이 코드는 애플리케이션의 실제 기능에만 관심이 있습니다. 한편 레이아웃, 데이터 표시, 애니메이션 및 제스처 응답과 같은 UX 관련 모든 것이 선언적 UX 마크 업 및 기본 UI 구성 요소로 남습니다.

퓨즈가 JavaScript 비즈니스 로직과 UX 마크 업 프레젠테이션을 분리하는 방법은 몇 가지 확실한 이점이 있습니다.

성능 - 모든 성능에 중요한 비트는 네이티브 코드로 처리되며 네이티브 UI 구성 요소를 기반으로 처리됩니다.
쉬운 선언적 코드는 제한된 프로그래밍 지식으로도 읽고, 쓰고 이해하기 쉽습니다.
오류가 발생하기 쉬움 - 상태가 적 으면 오류가 줄어들 수 있음을 의미합니다.
시각적 도구 - UX 마크 업은 검사자, 타임 라인 및 일반적으로 멋진 드래그 앤 드롭 피스와 같은 퓨즈 도구로 편집 할 수 있습니다.
퓨즈에는 자바 스크립트 (즉, 명령형)에서 애니메이션을 제어해야하는 필요성을 대체하는 수많은 API (UX 마크 업용으로 설계된)가 있습니다.

다른 많은 JavaScript 프레임 워크는 명령형 UI 코드, 애니메이션 및 성능 중요한 작업을 JavaScript로 섞습니다. 따라서 FuseJS를 처음 접하는 사람들은 처음부터 이러한 방식으로 작업을 시도하는 경향이 있습니다. 이러한 것들의 대부분은 기술적으로 FuseJS에서 가능하지만, 실망하고 있습니다. 새로운 방식으로 일하기위한 느낌을 얻기 위해 퓨즈 예제를 공부하는 데 시간을 할애하는 것이 좋습니다.

뷰와 로직을 UX 마크 업과 JavaScript로 분리하여 코드를 정제하면 코드 기반을 크게 줄이고 유지 관리가 용이하며 UX 디자이너와 개발자 간의 효과적인 공동 작업을 할 수 있습니다.

성능이 중요한 비즈니스 로직을 작성해야하는 경우 원시 코드 또는 JavaScript 대신 Uno 코드로 작성하는 것이 좋습니다.

자바 스크립트 인터페이스