원본: https://www.fusetools.com/docs/fusejs/observable

관찰 가능한 물건들

Observables는 Fuse를 효과적으로 이해하기위한 핵심 개념입니다. Observables는 데이터 모델이 변경 될 때마다 UI를 자동으로 업데이트합니다. 또한 뷰 모델을 설명하는 데 매우 유용합니다. 이는 데이터 모델을보기 쉬운 형식으로 매핑하는 것을 의미합니다. 중괄호 구문 ({<some_observable>})을 사용하여 Observable에 데이터 바인딩하면 Observable이 변경 될 때마다 UI를 업데이트하는 가입이 자동으로 생성됩니다. 이 기사는 FuseJS / Observable API를 이해하고 사용하는 방법을 간략히 소개합니다.

여기에 우리가 무엇을 볼 것인지에 대한 아이디어를 줄 수있는 아주 작은 예가 있습니다.

<App>
    <자바 스크립트>
        var Observable = require ( "FuseJS / Observable");
        module.exports.myValue = 관찰 가능 (1);
    </ JavaScript>

    <텍스트 값 = "{myValue}"/>
</ App>
비디오 자습서

관측 자료 작업에 대한 훌륭한 소개를 보려면 다음 비디오를보십시오.


관찰 대상 사용하기

Observable은 단일 값을 유지하거나 0 개 이상의 요소가있는 값 목록으로 처리 될 수 있습니다. Observable의 값이 변경되면 모든 가입자에게 자동으로 통보됩니다. 구독자 / 구독 개념에 대해서는이 기사 뒷부분에서 자세히 설명합니다.

함수를 반환하는 관찰 가능 모듈을 가져 와서 시작하십시오.

var Observable = require ( "FuseJS / Observable");
Observable은 Observable 함수를 0 개 이상의 초기 값으로 직접 호출하여 생성됩니다.

var emptyObservable = Observable ();
var singleValueObservable = 관찰 가능 (true);
var listObservable = 관찰 가능 (1,2,3,4);

var singleValueObservable = 관찰 가능 (10); // 이제는 .value == 10입니다.
singleValueObservable.value = 20; // 이제는 .value == 20입니다.
var theValue = singleValueObservable.value; // theValue는 이제 == 20입니다.
observable을 목록으로 사용할 때 .add 및 .remove 메소드를 사용하여 항목을 추가 및 제거 할 수 있습니다.

var multiValueObservable = 관찰 가능 (1,2);
multiValueObservable.add (5); // 이제 1,2,5가 포함되어 있습니다.
multiValueObservable.remove (2); // 이제 1,5가 들어 있습니다.
또한 우리는 집합 적으로 "반응 적 연산자"라고 부르는 일련의 방법을 사용하여 관측 대상에 다양한 변형을 적용 할 수 있습니다. 이 연산자는 새로운 관측 값을 반환하는 관측 가능 객체에서 호출 할 수있는 메소드입니다. 이 기사의 뒷부분에서 자세히 다룹니다. 다음 예제는 .map 연산자를 사용하여 observable someNumbers에있는 모든 숫자를 제곱합니다. Map은 제공된 함수를 사용하여 Observable의 모든 항목을 한 형식에서 다른 형식으로 변환하여 작동합니다.

var someNumbers = 관찰 가능 (1,2,3,4); // 이제 1,2,3,4가 포함되어 있습니다.
var someNumbersSquared = someNumbers.map (function (x) {return x * x;}); // someNumbersSquared는 결국 2,4,6,8을 포함하게됩니다.
모든 반응식 연산자는 새로운 관측 값을 반환하기 때문에 여러 개의 반응식을 연속적으로 배치하여 반응식 연산의 긴 체인을 만들 수 있습니다.

var someNumbers = 관찰 가능 (1,2,3,4);
var someTransformedNumbers = someNumbers
    .map (function (x) {return x * x;})
    .where (function (x) {return x <8;})
    .map (function (x) {return -x;}); // 결국 -2, -4, -6을 포함하게됩니다.
새로운 Fusers를 많이 잡는이 예제에 대해주의해야 할 점은 someTransformedNumbers는 실제로 선언 한 명령문 바로 다음에 데이터를 포함하지 않는다는 것입니다.

var someNumbers = 관찰 가능 (1,2,3,4);
var someNumbersSquared = someNumbers.map (function (x) {return x * x;});
console.log ( "SquaredNumbers length :"+ someNumbersSquared.length); // 0을 출력합니다.
그 이유는 Observables가 체인 끝에 가입자가 없으면 데이터를 전파하지 않기 때문입니다. 구독 방법에 대해 자세히 알아 보려면 계속 읽으십시오.

완전한 참조 문서는 Observable API 문서를 확인하십시오.

Observables의 변경 사항 구독

Observables에 대해 이해하는 것이 정말 중요합니다. Observables를 시작하기 전에 적어도 한 명의 가입자가 있어야한다는 것입니다. Observable에 가입하려면 다음과 같은 몇 가지 방법이 있습니다.

UX 마크 업에서 데이터 바인딩

중괄호 구문을 사용하여 Observable with UX에 데이터 바인딩 할 때마다 자동으로 Subscription을 만듭니다. 이것은 구독을 만드는 가장 일반적인 방법입니다. 데이터 바인딩에 대한 자세한 내용은이 기사를 참조하십시오.

.onValueChanged (module, func (item))

경우에 따라 Observable Change에 대한 응답으로 명령형 코드를 실행하는 데 관심이 있습니다. .onValueChanged (module, func (item)) 함수를 사용하여 구독하면됩니다. module 매개 변수를 사용하면이 구독의 수명을 모듈의 수명에 연결할 수 있습니다. 대부분의 경우 우리는 현재있는 모듈을 전달합니다.

var myObservable = 관찰 가능 (1,2,3);
myObservable.onValueChanged (module, function (item) {
    // 무언가를하십시오.
});
.addSubscriber (func)

마지막으로 .addSubscriber (func (item)) 메서드를 사용하여 구독을 명시 적으로 추가 할 수도 있습니다. .addSubscriber 사용의 단점은 .removeSubscriber를 사용하여 더 이상 구독이 필요하지 않을 때 수동으로 구독을 제거해야한다는 것입니다. 사용자 지정 반응 연산자를 정의하지 않는 한 .onValueChanged를 항상 선호합니다. 여기서는 완전성을 위해서만 언급합니다.

반응성 연산자

반응성 연산자는 새로운 관측 값을 생성하는 관측 가능 값에 대해 호출 할 수있는 메서드입니다. 그런 다음 리액턴스 연산자의 추가 어플리케이션을 구독하거나 변형 할 수 있습니다. 이 메소드에 대해 알아야 할 중요한 점은 선언적으로 작동한다는 것입니다. Observable에 반응 연산자를 적용한다고해서 실제로 변환이 적용되는 것은 아닙니다. Observable 소스에서 값이 제공 될 때 값이 통과하는 "파이프 라인"을 설정하기 만합니다.

관측 값은 "푸시 기반"모델을 사용한다고합니다. 푸시 기반 모델의 대안은 "풀 기반"입니다. 이 시나리오에서 UI는 자신을 업데이트하려고 할 때 데이터 소스에 최신 데이터를 요청해야합니다. 푸시 기반 접근 방식에서 데이터 모델은 변경 될 때마다 UI에 대한 값을 밀어 넣습니다.

이 "푸시 기반"접근 방식을 알고 있어야하는 것은 앞서 언급 한 것처럼 누군가가 이러한 푸시 알림에 가입하지 않으면 실제로 값을 푸시하지 않는 것입니다.

Observable이 가치를 생산한다는 것은 무엇을 의미합니까?

우리는 일반적으로 관측 대상을 출처 (또는 출처 집합)에서 대상 (또는 대상 집합)으로 흐르는 값의 흐름으로 생각합니다. 이 과정에서 .map, .where 및 .combine과 같은 반응 연산자를 사용하여 스트림을 변형하고 필터링하고 다른 스트림과 결합 할 수 있습니다 (여기에 모든 연산자에 대한 정보를 찾으십시오.

var source = Observable (2);

var destination = source.map (function (x) {
    return x * x;
}). 여기서 (function (x) {
    return x <50;
});
위의 코드에서 단일 초기 값으로 Observable 소스를 만듭니다. 숫자 2입니다. 그런 다음 반응 연산자 .map을 사용하여 숫자를 제곱하는 "매핑 함수"로 값을 변환합니다. 마지막으로 반응식 연산자 .where를 사용하여 관측 값을 필터링하면 50보다 작은 값만 더 전파됩니다. .where 연산자는 true 또는 false를 반환해야합니다. 값을 전파 할 수 있으면 true이고, 그렇지 않으면 false입니다.
