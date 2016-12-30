원본: https://www.fusetools.com/docs/assets/bundle

번들 파일

미리보기를 실행하는 동안 프로젝트를 빌드하거나 파일을 저장할 때 컴파일러는 UX 파일에서 참조 된 파일을 검색하여 해당 파일을 응용 프로그램 번들에 복사합니다. 그러나 JavaScript에서 액세스하는 파일 (예 : 다른 JavaScript 모듈, 정적 JSON 데이터 또는 UX에서 직접 사용되지 않는 다른 유형의 파일)이있을 수 있습니다. 이것들은 자동적으로 복사되지 않을 것입니다. 왜냐하면 컴파일러는 그것들을 포함 할 것인지 알 수있는 방법이 없기 때문입니다.

번들 파일은 프로젝트 파일에 포함될 파일을 명시 적으로 정의함으로써이 문제를 해결합니다. 또한 JavaScript에서이 파일을 읽을 수있는 JavaScript API도 제공합니다.

번들 파일을 포함 시키려면 .unoproj의 Includes 섹션에 다음 형식으로 추가하십시오.

<filename 또는 glob 패턴> : 번들
다음은 js / 디렉토리에있는 모든 JavaScript 파일과 묶음 파일 인 dogs.json 파일을 포함하는 예제입니다.

"포함": [
    ... 기타 포함 ...

    "dogs.json : Bundle",
    "js / *. js : Bundle"
]
이제 번들로 제공되는 JavaScript 모듈 ()을 require 할 수 있습니다.

var foo = require ( "./ js / foo.js");

foo.bar ();
번들 파일의 내용을 문자열로 읽을 수도 있습니다. 아래 예제는 번들 된 파일에서 JSON을로드하고 구문 분석합니다.

var 번들 = require ( "FuseJS / Bundle");

Bundle.read ( "dogs.json")
    .then (function (dogsJson) {
        var parsedDogs = JSON.parse (dogsJson);

        // ... parsedDogs로 무언가를하십시오 ...
    });
추가 읽기

퓨즈 / 번들
퓨즈 소개
우노 프로젝트
