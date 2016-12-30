원본: https://www.fusetools.com/docs/fusejs/observable-api

관찰 가능한 API

관측 가능한 가치

observable이 정확히 하나의 값을 보유하고 있다면, .value 속성을 사용하여 그 값을 얻거나 설정할 수 있습니다 :

var someString = Observable ( "foobar");
console.log (someString.value); // "foobar"를 인쇄합니다.
someString.value = "barfoo"; // 값을 설정하고 구독자에게 알립니다.
.value 등록 정보가 설정되면 모든 등록자에게 통지됩니다.

빈 관측 값은 .value === undefined 및 .length === 0입니다.
관찰 가능한 목록

observable을 값 목록으로 사용하려면 .add (item) 및 .remove (item)와 같은 메서드를 사용하여 observable을 조작합니다. .length 속성을 통해 목록의 값 수를 쿼리 할 수도 있습니다.

var friends = Observable ( "Jake", "Jane", "Joe");
console.log (friends.length); // 3을 인쇄합니다.

friends.add ( "Gina");
console.log (friends.length); // 4를 인쇄합니다.
관찰 가능한 목록으로 가능한 것을 확인하려면 전체 회원 목록을보십시오.

Observables가있는 데이터 유형

Observables는 모든 기본 유형을 제공하는 데 사용할 수 있습니다. 숫자, 문자열, 부울 및 벡터 유형이 포함됩니다. Number, string 및 boolean은 일반적인 JavaScript 리터럴을 사용하여 만듭니다.

var obsNumber = 관찰 가능 (10.5);
var obsString = Observable ( "hello");
var obsBool = 관찰 가능 (true);
벡터 유형 (예 : 색상)이 필요한 데이터 바인딩의 경우 자바 스크립트 배열을 사용할 수 있습니다.

var obsRedColor = 관찰 가능 ([1,0,0,1]);
var obsWhiteAndBlack = 관찰 가능 ([1,1,1,1], [0,0,0,1]);
관찰 가능한 기능

Observable이 유일한 인수로 함수를 사용하여 초기화되면 관찰 가능 함수의 .value는 함수를 평가하여 계산됩니다.

반응 종속성은 기능을 평가하는 동안 터치 된 다른 모든 관찰 가능 요소와 함께 자동으로 생성됩니다.

예:

var firstName = 관찰 가능 ( "John");
var lastName = 관찰 가능 ( "Doe");

var fullName = 관찰 가능 (function () {
    return firstName.value + ""+ lastName.value;
})
firstName 또는 lastName이 변경되면 fullName이 자동으로 업데이트됩니다.

예, 마술입니다.

회원

값

Observable의 현재 값을 가져 오거나 설정합니다.

value 속성은 getAt (0) 및 replaceAt (0)의 약자로 사용됩니다. 이것이 단일 요구 사항 Observable과 함께 사용되는 경우가 많지만, 이것이 필수 사항은 아닙니다.

if (isSomethingEnabled.value) {
    doSomething ();
}
isSomethingEnabled.value = false;
toArray ()

Observables 내부 값 배열의 단순 복사본을 반환합니다.

var obs = 관찰 가능 (1,2,3);
var obsArray = obs.toArray (); // obsArray == [1,2,3]
연산자 목록

추가 (값)

관찰 가능한 값 목록에 값을 추가합니다.

var colors = 관찰 가능 ( "Red", "Green");
colors.add ( "Blue");
addAll (항목)

관측 가능 항목 끝에 배열 항목을 추가합니다.

var val = new 관측 가능 (1, 2, 3);

val.addAll ([4, 5, 6]);

// val = Observable (1, 2, 3, 4, 5, 6);
명확한()

Observable에서 모든 값을 제거합니다.

var colors = 관찰 가능 ( "Red", "Green");
colors.clear ();
contains (value)

값이 var에 있으면 true를 반환합니다.

관측 가능 계절 = 관찰 가능 ( "여름", "가을", "겨울", "봄");
var winterExists = seasons.contains ( "Winter"); // true
forEach (func (item))

Observable의 모든 값에 대해 func을 호출합니다.

var numbers = 관찰 가능 (10, 2, 50, 3);
numbers.forEach (function (number) {
    console.log (number + "nice number!");
});
forEach (func (item, index))

Observable의 모든 값에 대해 func을 호출합니다.

var numbers = 관찰 가능 (10, 2, 50, 3);
numbers.forEach (function (number, index) {
    console.log (숫자 + "색인 포함 :"+ 색인);
});
func가 두 개의 인수를 허용하면 두 번째 인수는 Observable에있는 해당 항목의 인덱스입니다.

getAt (index)

지정된 인덱스의 값을 돌려줍니다.

var seasons = Observable ( "여름", "가을", "겨울", "봄");
console.log (seasons.getAt (2)); // 출력 : "Winter"
정체()

식별 함수 (map) (function (x) {return x;})를 사용하여 map을 호출하는 것과 같습니다.

양방향 데이터 바인딩이 필요하지만 원본 Observable의 데이터를 훼손하고 싶지 않은 경우 유용합니다.

var originalData = Observable ( "이것은 내 원래 데이터입니다.");
var nonClobberedData = originalData.identity (); // 안전한 양방향 데이터 바인딩
indexOf (값)

최초로 나타나는 value의 인덱스를 돌려줍니다.

var seasons = Observable ( "여름", "가을", "겨울", "봄");
var index = seasons.indexOf ( "겨울"); // 2
insertAll (index, array)

배열의 내용을 지정된 인덱스에 삽입합니다.

var 구름 = Observable ( "Cirrus", "Alto", "Stratus");
clouds.insertAll (1, [ "Cumulus", "Mammatus"]);

// clouds = Observable ( "Cirrus", "Cumulus", "Mammatus", "Alto", "Stratus");
insertAt (index, value)

지정된 인덱스에 값을 삽입합니다.

var words = Observable ( "foo", "bar");
words.insertAt (1, "baz");

console.log (단어); // Observable ( "foo", "baz", "bar")
.길이

Observable의 값 수를 반환합니다.

var fruits = Observable ( "Orange", "Apple", "Pear");
console.log (fruits.length); // 출력 : 3
refreshAll (newValues, compareFunc, updateFunc, mapFunc)

Observable의 모든 항목을 newValues의 값으로 업데이트합니다. compareFunc는 두 항목이 같은지 여부를 확인하는 데 사용됩니다. updateFunc는 compareFunc에서 일치 항목을 찾으면 기존 항목을 새 값으로 업데이트하는 데 사용됩니다. mapFunc는 새 항목이 발견 될 때마다 호출되어 새 항목에 매핑되도록합니다.

var items = 관찰 가능 (
    {id : 1, text : "one"},
    {id : 2, text : "two"},
    {id : 3, text : "tres"},
);

var newItems = [
    {id : 3, text : "three"},
    {id : 4, text : "four"},
    {id : 5, text : "5"}
];

items.refreshAll (newItems,
    // ID 비교
    함수 (oldItem, newItem) {
        return oldItem.id == newItem.id;
    },
    // 텍스트 업데이트
    함수 (oldItem, newItem) {
        oldItem.text.value = newItem.text;
    },
    // 관찰 가능한 버전의 텍스트가있는 객체에 매핑합니다.
    함수 (newItem) {
        반환 {
            id : newItem.id,
            텍스트 : 관찰 가능 (newItem.text)
        };
    }
);
제거 (값)

관찰 가능한 값 목록에서 value의 첫 번째 항목을 제거합니다.

var shapes = Observable ( "Round", "Square", "Rectangular");
shapes.remove ( "직사각형");
removeAt (인덱스)

지정된 인덱스의 값을 삭제합니다.

var shapes = Observable ( "Round", "Square", "Rectangular");
shapes.removeAt (2);
removeRange (시작, 개수)

count 아이템을 start로부터 삭제합니다.

var letters = Observable ( "a", "b", "c", "d");
letters.removeRange (1, 2);
// letters = Observable ( "a", "d");
removeWhere (func)

func가 true 인 모든 값을 제거합니다.

var hotPlaces = 관찰 가능 (
    {name : "Oslo", 온도 : 30},
    {name : "New York", 온도 : 24},
    {name : "California", 온도 : 27},
    {이름 : "시드니", 온도 : 10}
) .removeWhere (function (place) {
    return place.temperature <20;
}); // 목록에서 시드니를 제거합니다.
replaceAll (배열)

Observables 값을 배열의 값으로 바꿉니다.

var colors = 관찰 가능 ( "Red", "Green", "Blue");
colors.replaceAll ([ "Orange", "Cyan", "Pink"]);

replaceAt (index, value)

index의 값을 value로 바꿉니다.

var 구성 요소 = 관찰 가능 ( "설탕", "우유", "감자");
ingredients.replaceAt (2, "flour"); // "potato"를 "flour"로 바꿉니다.
tryRemove (값)

관찰 가능한 값 목록에서 첫 번째 값의 발생을 제거하려고 시도합니다. 성공하면 true를 반환하고 그렇지 않으면 false를 반환합니다.

var shapes = Observable ( "Round", "Square", "Rectangular");
if (shapes.tryRemove ( "Rectangular")) {
    console.log ( "성공");
}
반응성 연산자

FuseJS는 다른 Observables에서 Observables를 반환하는 반응 연산자 집합과 함께 제공됩니다. 즉, 원래 Observable이 변경되면 반응 연산자를 적용한 결과로 생성 된 Observables도 자동으로 변경됩니다.

임의의 (필터)

Observable가 불려 갔을지 어떨지를 나타내는 boolean 치를 포함한 새로운 Observable를 리턴합니다.

var 차량 = 관찰 가능 (
    {type : "car", 이름 : "SuperSpeeder 2000"},
    {type : "car", 이름 : "Initial Dash 2k00"},
    {type : "boat", 이름 : "Floaty McFloatface"}
);

var hasBoats = vehicles.any ({type : "boat"}); //참된
var hasAircraft = vehicles.any ({type : "aircraft"}); //그릇된
var hasCar = veichles.any (function (x) {return x.type === "car";}) // true
combine ([obs, ...], func)

현재 observable 또는 obs observables (통칭하여 "dependencies")가 변경 될 때마다 func을 호출합니다.

인수는 각 종속성의 현재 .value를 보유하며, 첫 번째 인수는 this.value이고, 순서대로 각 연속 Observable의 값이옵니다.

var foo = Observable (1);
var boo = Observable (2);
var moo = 관찰 가능 (3);

var res = foo.combine (boo, moo, function (f, b, m) {
    // f는 foo의 현재 .value를 유지합니다.
    // b는 boo의 현재 .value를 유지합니다.
    // m은 moo의 현재 .value를 유지합니다.

    f + b + m을 되 돌린다; // 결과로 나오는 관측 값은 값 6을 산출합니다.
})
결합에 제공된 함수가 값을 반환하면 그 값은 결과로 나오는 관측 가능 값의 .value를 대체합니다. 함수가 아무것도 반환하지 않으면 (정의되지 않음) func는 func에서 this 매개 변수를 수정하여 결과로 생성되는 observable을 업데이트해야합니다.

각 종속성마다 값을 사용할 수없는 경우에도 모든 변경 사항에 대해 화재를 결합하십시오. 이를 방지하려면 combineLatest를 참조하십시오.

결합은 각 의존성의 첫 번째 값 (.value)을 제공합니다 (심지어 이들 중 일부가 배열 인 경우에도). 각 종속성의 모든 값을 가져 오려면 .combineArrays를 참조하십시오.

combineLatest ([obs, ...], func)

결합과 동일하지만 모든 종속성에 대해 하나 이상의 값 (.length> 0)을 사용할 수있을 때까지 func를 실행하지 않습니다.

combineArrays ([obs, ...], func)

결합과 동일하지만 각 관찰 가능 값의 첫 번째 값 대신 각 관찰 값의 모든 값을 갖는 각 종속성에 대한 배열을 제공합니다.

var foo = Observable (1);
var boo = Observable ( "a", "b", c);
var moo = 관찰 가능 (3);

var res = foo.combineArrays (boo, moo, function (f, b, m) {
    // f [1]
    // b는 [ "a", "b", "c"]를 유지합니다.
    // m 보유 [3]

    return [f [0], m [0], b.length]; // 결과 관측 값은 (1, 3, 3) 값을 유지합니다.
})
combineArray에 제공된 func이 배열을 반환하면 배열의 요소가 결과로 생성되는 observable의 모든 요소를 ​​대체합니다. 함수가 아무것도 반환하지 않으면 (정의되지 않음) func는 func에서 this 매개 변수를 수정하여 결과로 생성되는 observable을 업데이트해야합니다.

카운트()

Observable의 값 수를 관찰 가능 숫자로 반환합니다. Observable에서 항목을 추가하거나 제거 할 때마다 개수가 변경됩니다.

책 = 관찰 가능 (
    "UX와 당신"
    "관찰자 관찰"
    "문서 센터 문서화"
);

numBooks = books.count (); // 결과 : 3
수 (조건)

condition이 함수이면 조건이 참인 관측 가능한 값 수를 반환합니다. 조건이 객체이면 조건에 속성이있는 값의 양을 포함하는 관찰 가능 객체를 반환합니다.

var tasks = 관찰 가능 (
    {title : "퓨즈 배우기", isDone : true},
    {title : Observables에 대해 알아보기, isDone : true},
    {title : "멋진 앱 만들기", isDone : false}
);
var tasksDone = tasks.count (function (x) {
    return x.isDone;
}); // 2
var tasksDone = tasks.count ({isDone : true}); // 2

확장 (func)

Observable에 단일 배열 만 들어 있으면 expand는 해당 배열의 값을 포함하는 Observable을 반환합니다.

Observable ([1,2,3]) expand () -> Observable (1,2,3)

필터 (조건)

지정된 조건을 통과하는 값만 전파하는 관찰 가능 객체를 반환합니다. 그렇지 않으면 이전 값을 유지합니다.

이 방법은 관찰 가능 항목의 첫 번째 (단일) 값만을 고려합니다.

먼저()

호출 한 관찰 가능 객체의 첫 번째 항목 값을 포함하는 새로운 Observable을 반환합니다.

var values ​​= 관찰 가능 (1, 2, 3);
var firstEntry = values.first (); // 관찰 가능 (1)
flatMap (func (item))

Observable의 모든 항목에 대해 func를 호출 한 다음 모든 func 호출 항목을 하나의 Observable 배열로 병합합니다.

var numbers = 관찰 가능 (
    [1, 2, 3],
    [4, 5, 6]
);
var counts = numbers.flatMap (function (item) {
    return [item, item + 1];
});
// counts = Observable ([1, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7])
안의()

두 Observable이 중첩 될 때 내부 값을 반영하는 새로운 Observable을 반환합니다 (Observable의 .value는 Observable 임). Observable이 호출되면 Observable이 중첩되지 않으면 .inner ()는 단순히 값을 반영합니다.

현재 Observable이 중첩되지 않은 경우 반환 된 관찰 가능 객체는 현재 관찰 가능 객체의 값을 반영합니다.

var foo = Observable (관찰 가능 (4))
var bar = foo.inner (); // bar.value = 4
foo.value.value = 9; // bar.value = 9
foo.value = 3; // bar.value = 3
이것은 ux : Property를 통해 전달 된 Observables를 처리 할 때 특히 유용합니다.

마지막()

호출 한 관찰 가능 객체에서 마지막 항목의 값을 포함하는 새로운 Observable을 리턴합니다.

var values ​​= 관찰 가능 (1, 2, 3);
var lastEntry = values.last (); // 관찰 가능 (3)
지도 (func (item)))

Observable의 모든 값에 대해 func을 호출하여 새로운 Observable에 결과를 반환합니다.

var numbers = 관찰 가능 (1, 4, 9);
var roots = numbers.map (function (number) {
    return Math.sqrt (number);
});
뿌리의 값은 숫자의 제곱근이됩니다. 숫자의 값은 변경되지 않습니다.

지도 (func (item, index)))

Observable의 모든 값에 대해 func을 호출하여 새로운 Observable에 결과를 반환합니다.

var numbers = 관찰 가능 ( "this", "item", "is");
var roots = numbers.map (function (item, index) {
    리턴 아이템 + "는 인덱스 nr :"+ index;
});
Observable.map이 두 개의 인수를 취하는 함수와 함께 사용될 때 두 번째 인수는 Observable에서 해당 항목의 인덱스입니다.

아니()

호출하고있는 Observable의 역 값을 가진 Observable을 반환합니다. Observable이 true 인 경우 반환되는 객체는 false가되며 그 반대의 경우도 마찬가지입니다.

falseValue = 관찰 가능 (false);
trueValue = falseValue.not ();
선택 (색인)

호출 한 관찰 가능 객체의 모든 항목의 색인 색인에 항목을 포함하는 새로운 Observable을 반환합니다.

var values ​​= Observable ([1, 2], [3, 4], [5,6]);
var picked_values ​​= values.pick (1); // 관측 가능 (2, 4, 6);
setInnerValue (value)

inner ()에 의해 반환 된 관찰 대상에서 호출 될 때 호출 된 관찰 대상을 업데이트하지 않고 관찰 가능 원본 값을 설정합니다.

var foo = 관찰 가능 ( "123");
var bar = foo.inner ();
// foo == "123", bar == "123"

bar.setInnerValue ( "456");
// foo == "456", bar == "123"
slice ([begin [, end]])

현재 관찰 가능 요소의 슬라이스를 반영하는 새로운 observable을 반환합니다.

이 함수의 인수에 대한 설명은 Array.slice () 설명서를 참조하십시오.

var foo = 관찰 가능 (1,2,3,4);
var bar = foo.slice (1,2) // bar = Observable [2, 3]
힌트:

관측 가능 객체의 첫 번째 요소를 관측하려면 .slice (0, 1)을 사용하십시오.
관측 가능 객체의 마지막 요소를 관찰하려면 .slice (-1)를 사용하십시오.
twoWayMap (getter, setter)

양방향 바인딩 가능한 관찰 가능 객체를 생성하기위한 인터페이스를 제공합니다.

getter는 소스가 관찰 가능한 경우 관찰 가능 값을 반환하는 함수를 원합니다.
setter는 소스의 값을 관찰 가능으로 설정하는 함수를 원합니다.
twoWayMap은 inner에 의해 반환되는 관찰 가능 객체에서만 호출 될 수 있습니다.

var date = Observable (새 Date ()). inner ();
var day = date.twoWayMap (function (dt) {return dt.getDate ();}, function (d, dt) {dt.setDate (d); return dt;})
날짜 선택 도구를 만드는 방법에 대한 전체 예제는 https://github.com/Duckers/FusePlayground/tree/master/Components/DateEditor를 참조하십시오.

여기서 (조건)

조건은 함수 또는 객체가 될 수 있습니다. condition이 함수이면 조건이 true를 반환하는 값만있는 새 Observable을 반환합니다. 그러나 condition이 객체이면 strict equality (===)를 사용하여 검사 한 매개 변수가 조건의 매개 변수와 일치하는 값만 가진 새 Observable을 반환합니다.

새로운 Observable은 이전 Observable을 관찰하므로 원본 관찰 가능 값이 변경 될 때마다 업데이트됩니다.

과일 = 관찰 가능 (
    {name : "Apple", color : "red"},
    {name : "Lemon", color : "yellow"},
    {name : "배", 색상 : "녹색"},
    {name : "Banana", color : "yellow"});

goodFruits = fruits.where ({color : "yellow"});

goodFruits = fruits.where (function (e) {
    e.color를 반환 === "노랑";
});
노트! 당신은 조건 자체를 관찰 가능하게 만들기 위해 .where 연산자로 Observable 함수를 반환 할 수 있어야했습니다. 더 이상 허용되지 않습니다. 동일한 효과를 얻는 가장 좋은 방법은 대신 flatMap ()을 사용하는 것입니다.
관찰 가능한 조건에 따라 목록을 필터링하려면 .flatMap과 .where의 조합을 사용하십시오.

var items = conditionObservable.flatMap (function (v) {
    return itemsObservable.where (function (x) {return v! = x;});
});
우리는 매핑 함수에서 관측 값을 반환하기 때문에 .map () 대신 .flatMap ()을 사용합니다.

노트! 위의 접근법은 빠르지 만 관찰 할 수있는 조건이 변경되는 경우 관찰 가능 항목에서 항목 순서를 보존하지 않습니다. 이는 Observable 항목이 효과적으로 삭제되고 처음부터 채워지는 것을 의미합니다. 이 문제가없는 .where를 가진 조건을 관찰하기위한 두 번째 패턴이 있지만 큰 목록에서는 잠재적으로 느릴 수 있습니다. 여기에는 완전성을 위해 포함됩니다.
var filteredItems = conditionObservable.flatMap (function (cond) {
    return items.where (function (item) {
        return cond.value;
    });
});
완전한 샘플을 보려면 여기를보십시오.

이렇게하면 조건이나 데이터가 변경 될 때마다 변경 사항을 푸시하는 Observable을 만들 수 있습니다.

업데이트 구독

onValueChanged (module, func)

Observable의 변경 사항에 수동으로 대응하기 위해 onValueChanged 메소드를 사용할 수 있습니다. 자동으로 구독을 생성하고 수명을 모듈과 연결합니다. Observable이 변경 될 때마다 func가 호출됩니다.

someObservable.onValueChanged (module, function (x) {
    console.log ( "새로운 값을 얻었습니다 :"+ x);
});
addSubscriber (func)

Observable에 대한 구독을 수동으로 만들 수 있습니다. 적절한 시간에 removeSubscriber를 사용하여 구독을 수동으로 제거해야합니다. 이런 이유로 사용자 정의 관찰 가능 연산자를 구현하지 않는 한 항상 onValueChanged를 사용하는 것이 좋습니다.

removeSubscriber (func)

Observable에서 값을 소비 한 후에는 구독을 제거하여 정리하는 것이 중요합니다. 이렇게하지 않으면 시간이 지남에 따라 메모리 쓰레기가 누적 될 수 있습니다. addSubscriber를 사용하여 수동으로 구독을 만든 경우에만 필요합니다. onValueChanged가 자동으로이 정리 작업을 처리한다는 점을 기억하십시오.

username.removeSubscriber (usernameLogger);
Observables로 비동기 데이터 가져 오기

Observable 변경의 값에 대한 응답으로 일부 데이터를 비동기 적으로 요청하고 결과를 다른 Observable로 가져와야하는 경우가 있습니다. 우리는 map ()과 inner ()의 조합을 사용하여 우아한 방법으로이 작업을 수행 할 수 있습니다.

아래 예제에서, 우리는 입력 된 Observable의 각 항목 (이 경우 하나)을 매핑하고 Observable을 반환하며, Observable은 가져온 데이터가 포함될 때 업데이트됩니다. 이것은 우리의 미래 데이터를 관찰 할 수있는 Observable - 거의 우리가 원하는 것 -을 가져올 것입니다. 그러므로 우리는 inner ()를 사용하여 외부 Observable 대신에 Observable을 "unwrap"하고, 외부를 관찰합니다. 우리는 입력이 변경 될 때마다 비동기 적으로 가져온 데이터로 업데이트 될 Observable로 끝납니다.

var inputUrl = 관찰 가능 ( "https://example.com/");

var output = inputUrl.map (function (url) {
    var resultObservable = 관찰 가능 ( "자리 표시 자 값");

    가져 오기 (URL)
        .then (function (response) {return response.text ();})
        .then (function (result) {
            resultObservable.value = result;
        });

    return resultObservable;
}).안의();
다른

toString ()

Observable과 그 내용의 문자열 표현을 반환합니다.

var testObservable = 관찰 가능 (1, "two", "3");
testObservable.toString (); // "(관찰 가능) 1, 2, 3"

