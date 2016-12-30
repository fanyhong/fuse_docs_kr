원본: https://www.fusetools.com/docs/assets/iconfont-import

아이콘 글꼴

FontAwesome과 같은 아이콘 글꼴은 문자 대신 벡터 아이콘이 포함 된 특수 글꼴입니다. MultiDensityImageSource를 사용할 필요없이 앱에서 선명한 아이콘을 제공합니다.

아이콘 글꼴 가져 오기

아이콘 글꼴은 다른 글꼴과 똑같이 작동합니다. 이것을 사용하려면 다음과 같이 Font 요소를 만들어야합니다.

<Font File = "assets / fontawesome-webfont.ttf"ux : Global = "FontAwesome"/>
이 작업을 수행하면 Font의 전역 이름이 무엇이든 Font를 설정하여 모든 구성 요소의 글꼴을 사용할 수 있습니다. 이 경우 FontAwesome이됩니다.

아이콘 글꼴 사용

아이콘 글꼴은 특수 유니 코드 문자를 사용하기 때문에 이스케이프 된 문자 리터럴을 사용해야합니다. 이들은 JavaScript와 UX에서 다르게 이스케이프 처리됩니다.

UX

UX에서는 보통 xml에서와 같이 유니 코드 리터럴을 이스케이프 처리합니다. 다음 예제는 FontAwesome의 톱니 바퀴 아이콘이 포함 된 텍스트를 보여줍니다.

<Text Font = "FontAwesome"> & # xf013; </ Text>
이 글꼴의 전역 이름을 FontAwesome 의존하는 유의하십시오.

자바 스크립트

JavaScript에서는 두 가지 경우가 있습니다. 사용하고자하는 문자 코드가 65536 이하일 경우 (또는 4 자리의 16 진수로 작성 될 수있는 경우) 다음과 같이 이스케이프 처리 할 수 ​​있습니다.

var cog = '\ uF013';
그러나 문자 코드가 65535보다 높으면 문자에 대해 UCS-2 인코딩을 사용하는 JavaScript 때문에 각 문자가 16 비트 너비라는 의미에서 약간의 해결 방법을 수행해야합니다. 이러한 사례를 처리하는 트릭은 서로 게이트 쌍입니다. 서로 변환하는 방법을 포함하여 서로 게이트 쌍에 대해 더 많은 내용을 읽을 수 있습니다.

캐릭터를 JavaScript 측에서 문자열로 이스케이프하면 UX에서 데이터 바인딩과 같이 사용할 수 있습니다.

<자바 스크립트>
    var Observable = require ( "FuseJS / Observable")
    var cog = Observable ( '\ uF013');
    module.exports = {
        장부 : 톱니 바퀴
    };
</ JavaScript>
<텍스트 값 = "{cog}"Font = "FontAwesome"/>
