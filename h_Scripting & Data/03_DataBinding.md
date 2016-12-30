원본: https://www.fusetools.com/docs/fuse/reactive/databinding_1

데이터 바인딩 DataBinding <T> 클래스

 고급 기능 표시
이동 :
목차
비고
데이터 바인딩을 사용하면 UX 마크 업 객체의 속성을 JavaScript 또는 기타 데이터 컨텍스트에서 가져온 값에 바인딩 할 수 있습니다.

데이터 바인딩은 {key} 구문을 사용하여 UX Markup에서 가장 쉽게 표현됩니다. 여기서 key는 바인딩 경로입니다.

<텍스트 값 = "{textKey}"/>
데이터 바인딩은 명시 적으로 선언 할 수도 있습니다. 명시 적 데이터 바인딩을 사용하면 데이터 바인딩이 해결되기 전에 사용되는 기본값을 지정할 수 있습니다.

<Panel ux : Name = "panel1"Width = "100"/>
<DataBinding Target = "panel1.Width"Key = "panelWidth"/>
위 코드는 panelWidth 데이터가 해석 될 때까지 panel1.Width의 기본값으로 100을 사용합니다.

데이터 바인딩의 인터페이스 <T>

첨부 된 UX 속성
GlobalKey (리소스에 의해 첨부 됨) : string UX
ux : Global 속성은 UX 마크 업에서 모든 곳에서 액세스 할 수있는 글로벌 리소스를 만듭니다.
비고

데이터 바인딩

퓨즈는 직접 바인딩, 반복 및 분기를 통해 UX 태그가있는 데이터 기반 응용 프로그램을 만드는 일류 지원을 제공합니다. UX는 복잡한 데이터 구조의 내부를 참조 할 수도 있으므로 코드에서 지루한 데이터 마술을 수행 할 필요가 없습니다.

DataContext

퓨즈 노드 트리의 어느 지점에서나 데이터 컨텍스트가 있습니다. 모든 노드의 데이터 바인딩은 노드의 현재 데이터 컨텍스트에 상대적입니다. 기본적으로이 데이터 컨텍스트는 null이며 모든 데이터 바인딩은 null 또는 빈 값만 반환합니다. 컨텍스트는 또한 트리 아래로 전달됩니다. 즉, 자식 노드가 데이터 컨텍스트를 제공하지 않으면 부모 노드의 데이터 컨텍스트를 사용합니다.

데이터 컨텍스트를 설정하려면 일반적으로 <JavaScript>와 같은 데이터 컨텍스트를 제공하는 노드에 비헤이비어를 추가합니다.

데이터 소스로 사용되는 JavaScript 모듈

데이터 소스를 만드는 가장 간단한 방법은 JavaScript를 사용하는 것입니다. 여기서는 데이터 바인딩 된 "Hello world"최소 예제가 있습니다.

<App>
    <자바 스크립트>
        module.exports = {
            인사말 : "Hello databound world!"
        };
    </ JavaScript>
    <텍스트 값 = "{인사말}"/>
</ App>
마찬가지로 컬렉션에 바인딩 할 수 있습니다.

<App>
    <자바 스크립트>
        var data = [ "1", "2", "3"];

        module.exports = {
            데이터 : 데이터
        };
    </ JavaScript>
    <StackPanel>
        <각 항목 = "{data}">
            <텍스트 값 = "{}"/>
        </ 각>
    </ StackPanel>
</ App>
이것은 예상대로 텍스트 문자열 1, 2 및 3을 나열합니다. 텍스트 값 {}을 바인딩하면이 데이터 컨텍스트를 의미합니다. 일반적으로 더 복잡한 데이터 소스에 바인딩하므로 각 요소는 바인딩 할 흥미로운 것을 갖게됩니다.

<App>
    <자바 스크립트>
        var Observable = require ( "FuseJS / Observable");

        var 데이터 = 관찰 가능 (
            {이름 : "Hubert Cumberdale", 나이 : 12},
            {이름 : "Marjory Stewart-Baxter", 나이 : 43},
            {이름 : "Jeremy Fisher", 나이 : 25});

        module.exports = {
            데이터 : 데이터
        };
    </ JavaScript>
    <StackPanel>
        <각 항목 = "{data}">
            <DockPanel>
                <텍스트 독 = "오른쪽"값 = "{연령}"/>
                <텍스트 값 = "{name}"/>
            </ DockPanel>
        </ 각>
    </ StackPanel>
</ App>
이 경우 데이터 소스 Observable을 만들었습니다. 이는 런타임시 데이터 소스에 대한 변경 사항 전파를 지원함을 의미합니다. 이 경우 컬렉션 자체는 Observable이지만 아이템은 그렇지 않습니다. 자식에 바인딩 할 수 있지만 변경하려는 경우 이러한 변경 사항은 UI에 반영되지 않습니다. 어린이가 UI에 변경 사항을 전파하도록하려면 Observable로 변경해야합니다.

<자바 스크립트>
    var Observable = require ( "FuseJS / Observable");
    var 데이터 = 관찰 가능 (
        {이름 : Observable ( "Hubert")},
        {이름 : Observable ( "Marjory")}));
    module.exports = {
        데이터 : 데이터
    };
</ JavaScript>
<StackPanel>
    <각 항목 = "{data}">
        <텍스트 값 = "{name}"/>
    </ 각>
</ StackPanel>
다음 경로에 바인딩 할 수도 있습니다.

<자바 스크립트>
    var complex = {
        사용자: {
            userinfo : {
                이름 : "Bob"
            }
        }
    };
    module.exports = {
        복합체 : 복합체
    };
</ JavaScript>
<텍스트 값 = "{complex.user.userinfo.name}"/>
이는 REST 서비스에서 JSON으로 반환되는 것과 같은 임의의 데이터 소스에 바인딩 할 때 매우 유용합니다. 코드에서 먼저 데이터를 처리하지 않고 복잡한 데이터에 직접 바인딩 할 수 있기 때문입니다. 이 심층적 인 예를 참조하십시오.

바인딩 방향

양방향 바인딩 (기본값)

기본적으로 데이터 바인딩은 가능한 경우 양방향입니다. 즉, 변경된 이벤트를 방출하는 특성이 사용자 입력 또는 애니메이션과 같은 데이터 바인딩 이외의 다른 수단에 의해 변경되면 바인딩 오브젝트는 Observable 인 경우 새 값을 소스에 다시 기록합니다.

<TextInput Value = "{text}"/>
위의 예에서 사용자가 텍스트 입력을 조작하고 텍스트가 바운드 관측 가능이면 관측 가능 항목이 업데이트됩니다.

읽기 전용 바인딩 (단방향)

데이터 원본에서 읽고 속성을 업 그레 이드하는 단방향 바인딩을 만들려면 읽기 바인딩 별칭을 사용합니다.

<TextInput Value = "{Read text}"/>
위의 예에서, 사용자가 텍스트 입력을 조작하면 바운드 관찰 가능 텍스트가 업데이트되지 않습니다.

쓰기 전용 바인딩 (단방향)

외부 액터에 의해 속성이 변경 될 때 데이터 소스에 쓰는 단방향 바인딩을 엄격하게 만들려면 Write 바인딩 별칭을 사용합니다.

<TextInput Value = "{텍스트 쓰기"} />
위의 예에서 Value는 JavaScript에서 가져 오는 텍스트의 값을 따르지 않지만 사용자가 텍스트 상자를 조작하면 관찰 할 수있는 텍스트를 업데이트합니다.

JavaScript 함수에 대한 이벤트 바인딩

다음과 같은 구문을 사용하여 JavaScript 함수를 호출하도록 이벤트 핸들러를 연결할 수 있습니다.

<자바 스크립트>
    module.exports = {
        clickHandler : function (args) {
            console.log ( "나는 클릭되었습니다 :"+ JSON.stringify (args));
        }
    };
</ JavaScript>
<Clicked Clicked = "{clickHandler}"Text = "Click me!" />
클리어 바인딩

때로는 포함 페이지가 제거되거나 탐색 된 후 다른 데이터 컨텍스트로 다시 추가되거나 다시 탐색 될 때 바인딩이 루틴되지 않을 때 데이터 바인딩에서 대상 속성 값을 지우는 것이 바람직합니다. 일부 시나리오에서는 새로운 데이터가로드되는 동안 원하지 않는 오래된 데이터가 페이지에 표시됩니다.

이를 방지하기 위해 소위 clear-bindings를 사용할 수 있습니다.

<텍스트 값 = "{Clear foo}"/>
이렇게하면 포함 된 페이지가 루핑되지 않은 경우 텍스트 값이 null (빈 문자열)로 재설정되므로 나중에 페이지를 다시 사용할 경우 이전 텍스트가 남아 있지 않습니다.

읽기 전용 및 쓰기 전용 바인딩을위한 명확한 binidng의 변종도 있습니다 :

<텍스트 값 = "{ReadClear foo}"/>
<텍스트 값 = "{WriteClear foo}"/>
주 : 클리어 바인딩은, 프로퍼티의 디폴트 치는 아니고, 루 박트가 아닌 경우, 항상 디폴트 (T) (null, false, 0 또는 0.0)를 푸쉬합니다.

마다

각각은 컬렉션의 각 항목에 대해 UX 마크 업 부분을 반복하는 데 사용됩니다.

각 비헤이비어는 Items 컬렉션의 항목 당 하위 트리의 사본 한 개를 유지 관리하고 그에 따라 상위 노드에서 추가 및 제거합니다. Items 컬렉션은 동적으로 변경할 수있는 Observable 일 수 있습니다.

각각을 사용할 때 일반적으로 Items 속성을 배열 데이터 소스에 데이터 바인딩하여 데이터 소스의 객체 당 하나의 비주얼 노드를 생성합니다.

<각 항목 = "{items}">
    <Rectangle Width = "{width}"높이 = "{height}"채우기 = "# 808"/>
</ 각>
AddingAnimation, RemoveAnimation 및 LayoutAnimation을 사용하여 Items 컬렉션에서 관찰 가능한 추가 / 제거 작업을 애니메이션으로 만들 수 있습니다.

각 동작을 중첩 할 수도 있습니다.

<자바 스크립트>
    var Observable = require ( "FuseJS / Observable");
    module.exports = {
        항목 : [
            {
                내부 : [
                    {child : "John"},
                    {아이 : "폴"}
                ]
            },
            {
                내부 : [
                    {아이 : "링고"},
                    {아이 : "조지"}
                ]
            }
        ]
    };
</ JavaScript>
<ScrollView>
    <StackPanel>
        <각 항목 = "{items}">
            <StackPanel Orientation = "Horizontal">
                <각 항목 = "{inner}">
                    <텍스트 값 = "{자식}"여백 = "10"/>
                </ 각>
            </ StackPanel>
        </ 각>
    </ StackPanel>
</ ScrollView>
Count 속성을 사용하여 반복되는 항목 수를 가져올 수 있습니다.

또한 각각을 단순한 중계기로 사용할 수도 있습니다.

<Grid ColumnCount = "3">
    <각 Count = "9">
        <사각형 여백 = "10"채우기 = "# 610"/>
    </ 각>
</ Grid>
WhileCount 및 WhileEmpty

WhileEmpty 및 WhileCount Triggertriggers)를 사용하여 컬렉션의 항목 수를 조정할 수 있습니다.

<각 항목 = "{친구}">
    <! - ... 친구 목록에 ... ->
</ 각>
<WhileEmpty Items = "{friends}">
    <Text> 친구가 없습니다. : (</ 텍스트>
</ WhileEmpty>
WhileEmpty는 EqualTo 속성이 0으로 설정된 WhileCount의 특별한 경우입니다. WhileCount는 다음 속성을 허용합니다.

EqualTo - 콜렉션 수가 제공된 값과 같을 때 활성화됩니다.
GreaterThan - 콜렉션 수가 제공된 값보다 클 경우 활성화됩니다.
LessThan - 콜렉션 수가 제공된 값보다 작은 경우 활성화됩니다.
예 :

<WhileCount Items = "{things}"EqualTo = "3"GreaterThan = "3">
    <Text> 3 가지 이상 있습니다. </ Text>
</ WhileCount>
고르다

복잡한 데이터 컨텍스트가 있고 데이터 컨텍스트를 축소하려는 경우 Select를 사용할 수 있습니다.

<자바 스크립트>
    module.exports = {
        복합체 : {
            item1 : {
                부제 1 : {이름 : "Spongebob", 나이 : 32}
            }
        }
    };
</ JavaScript>
<Select Data = "{complex.item1.subitem1}">
    <텍스트 값 = "{name}"/>
    <텍스트 값 = "{연령}"/>
</ 선택>
경기와 사건

Match and Case를 사용하여 활성화 할 서브 트리를 구동 할 수 있습니다 :

<자바 스크립트>
    module.exports = {
        활성 : "파란색"
    };
</ JavaScript>
<일치 값 = "{활성}">
    <Case String = "red">
        <직사각형 채우기 = "# f00"높이 = "50"너비 = "50"/>
    </ Case>
    <Case String = "blue">
        <직사각형 채우기 = "# 00f"높이 = "50"너비 = "50"/>
    </ Case>
</ Match>
Case의 유효한 일치 속성은 다음과 같습니다.

문자열 - 문자열 일치
번호 - 숫자와 일치합니다.
Bool - 부울 일치
IsDefault - 다른 대 / 소문자가 일치하지 않는 경우의 기본 대소 문자

참고 : Match.Value는 모든 JavaScript 유형에 데이터 바인딩 될 수 있지만 속성 바인딩을 사용하는 경우 String, Number, Integer 또는 Bool이라는 특수 속성을 사용해야합니다. 이것은 특성 바인딩이 유형이 동일해야하기 때문입니다.

DataToResource

DataToResource를 사용하여 정의 된 리소스에 바인딩 할 수 있습니다.

<FileImageSource ux : Key = "picture"File = "Pictures / Picture1.jpg"/>
<자바 스크립트>
    module.exports = {
        그림 : "그림"
    }
</ JavaScript>
<이미지 원본 = "{DataToResource 그림}"/>

