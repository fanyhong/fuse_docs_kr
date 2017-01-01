원본: https://www.fusetools.com/docs/ux-markup/properties

속성 (ux : Property)

UX Markup의 ux : Property 속성은 포함하는 ux : Class에 대한 새 속성을 정의합니다.

사용자 정의 특성을 사용하여 사용자 정의 구성 요소에 대해 구성 가능 또는 애니메이션 가능 매개 변수를 표시 할 수 있습니다.

속성은 {Property PropertyName} 또는 {Property objectName.PropertyName} 형식으로 속성 바인딩을 사용하여 UX Markup 내에서 참조 할 수 있습니다.

UX 마크 업에서 클래스 (구성 요소)를 만들 때 기본 클래스의 모든 속성을 자동으로 상속합니다.
통사론

<type ux : Property = "property_name"/>
여기서 type은 Uno 유형의 이름 (예 : Panel 또는 string)이며 property_name은 유효한 Uno 식별자입니다.

기본값

속성의 defaul 값은 포함하는 ux : Class의 일반 속성으로 지정할 수 있습니다.

예:

<Panel ux : Class = "MyComponent"NumberOfThings = "13">
    <int ux : Property = "NumberOfThings"/>
    ...
</ Panel>
비고

속성 바인딩

속성은 {Property PropertyName} 또는 {Property objectName.PropertyName} 형식으로 속성 바인딩을 사용하여 UX Markup 내에서 참조 할 수 있습니다.

속성 바인딩은 양방향 (기본값), 읽기 전용 ({ReadProperty ...}) 또는 쓰기 전용 ({WriteProperty ...}) 일 수 있습니다. 자세한 내용은 PropertyBinding의 문서를 참조하십시오.

예

다음 예제에서 컨트롤은 BackgroundColor라는 새 속성을 정의하며 기본값은 루트 노드에 설정된 # f00입니다. 이 색은 둥근 모서리가있는 배경 사각형의 Color 속성에 속성 바인딩됩니다.

<Panel ux : Class = "MyButton"BackgroundColor = "# f00">
    <float4 ux : Property = "BackgroundColor"/>
    <텍스트> 제출 </ 텍스트>
    <Rectangle Layer = "배경"Color = "{Property BackgroundColor}"CornerRadius = "10"/>
</ Panel>
구성 요소가 인스턴스화되면 사용자는 기본 BackgroundColor를 그대로 두거나 새 BackgroundColor를 설정할 수 있습니다.

<MyButton /> <! - 기본 색상 유지 ->
<MyButton BackgroundColor = "# 0f0"/> <! - 이것은 녹색을 대신 사용합니다 ->
<Change> 애니메이터를 사용하여 속성을 애니메이트 할 수도 있습니다.

<MyButton ux : Name = "mb1"/>
<잠시 후>
    <변경 mb1.BackgroundColor = "# 00f"기간 = "1"/>
</ WhilePressed>
속성에 적합한 유형 선택하기

속성은 강력한 형식이며 Uno 형식 이름으로 정규화되어야합니다. 모든 일반 UX 태그 이름을 포함하여 모든 Uno 유형을 사용할 수 있습니다.

종종 프로퍼티는 숫자, 텍스트, 색상, 크기 (잠재적으로 % 또는`px "와 같은 단위)와 같은 값 유형을 가지며, 유효한 값의 유효 범위를 가장 잘 포착하는 속성 유형을 선택하는 것이 중요합니다. 속성.

대부분의 경우, 우리는 속성을 기존 속성에 직접 속성 바인딩합니다. 이 경우 기존 속성에 대한 설명서를 참조하고 동일한 유형을 사용하는 것이 좋습니다. 위의 예에서는 float4를 유형으로 사용했습니다. 이는 Panel의 Color 속성 유형이기 때문입니다.

다음은 사용할 유형에 대한 몇 가지 지침입니다.

정수 - 정수 사용
십진수 - 이중 (또는 부동)
10 % 같은 단위로 숫자 - 사용 크기
2D 벡터 - 값이 hs이면 float2를 사용하십시오.
ux : Property와 ux : Dependency의 차이점은 무엇입니까?

속성은 종속성 (ux : Dependency)과 비슷하지만 다음과 같은 차이점이 있습니다.

속성은 변경할 수 있습니다. 즉, 언제든지 변경하고 애니메이션으로 만들 수 있습니다.
구성 요소는 기본값을 제공 할 수 있지만 종속성은 항상 사용자가 주입해야합니다.
이 속성은 속성 바인딩을 사용하여 속성 바인딩 될 수 있습니다.
종속성 대신 속성을 사용하는 한 가지 단점은 속성 변경으로 인해 속성으로 전달 된 객체를 UX 마크 업에서 이름으로 참조 할 수 없다는 것입니다 (예 : <변경> 애니메이터.

종속성과 달리 속성은 JavaScript에서 명명 된 객체로 직접 사용할 수 없으며 모듈의 루트 범위에서이 객체에 대한 Observable 속성으로 사용할 수 있습니다.

JavaScript에서 속성 사용.

모든 사용자 정의 속성 (ux : Property를 통해 정의 됨)은 JavaScript 모듈의 Observable 객체로 속성 범위 내에서 자동으로 반영됩니다.

다음은 JavaScript의 속성 변경에 응답하는 방법의 예입니다.

<패널 ux : Class = "RgbDisplayer"RGB = "# A00">
    <float4 ux : Property = "RGB"/>
    <자바 스크립트>
        var Observable = require ( "FuseJS / Observable");
        var rgbString = this.RGB.map (function (val) {
            "R :"+ (val [0] * 255) .toFixed (1) +
                "G :"+ (val [1] * 255) .toFixed (1) +
                "B :"+ (val [2] * 255) .toFixed (1);
        });
        module.exports = {rgbString : rgbString};
    </ JavaScript>
    <텍스트 값 = "{rgbString}"/>
</ Panel>
JavaScript의 속성에 대한 중요 사항

즉, JavaScript 코드가 실행될 때 속성의 값이 반드시 최신 상태가 아닙니다. 이것은 혼란의 일반적인 원인입니다.

observable의 .value 속성을 읽는 대신 .map () 또는 .onValueChanged ()와 같은 반응 연산자를 사용하십시오. 또한 속성 값이 변경되거나 애니메이션이 적용될 때 구성 요소가 업데이트되도록합니다.

모듈이 실행될 때 바로 사용할 수있는 속성 값이 필요하면 ux : Dependency를 대신 사용해보십시오.

속성을 통해 관찰 가능 항목 전달하기

Observables는 Properties를 사용하여 맞춤형 구성 요소로 전달할 수도 있습니다. Observables를 받아들이는 속성을 추가 할 수 있습니다.

<Panel ux : Class = "CoolPanel">
    <object ux : Property = "ObservableProperty"/>
    <자바 스크립트>
        var passedInObservable = this.ObservableProperty.inner ();
    </ JavaScript>
</ Panel>
<자바 스크립트>
    var Observable = require ( "FuseJS / Observable");
    module.exports = {
        valueToPass : 관찰 가능 ( "123")
    };
</ JavaScript>
<CoolPanel ObservableProperty = "{valueToPass}"/>
거의 항상 속성을 통해 전달 된 관찰 가능 항목을 가져올 때 inner ()를 사용하는 데 관심이 있습니다. 이것은 javascript 값 this.PropertyName이 PropertyName에 포함 된 것으로 관찰 가능하기 때문입니다. Observable을 전달하면 this.PropertyName은 우리가 관측 한 관측 값을 가진 관측 가능 값을 포함하게됩니다.
