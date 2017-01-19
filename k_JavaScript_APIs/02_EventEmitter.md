원본: https://www.fusetools.com/docs/fusejs/eventemitter

EventEmitters

EventEmitter는 우리가 이벤트를 내보내고들을 수있게 해주는 클래스입니다. Fuse의 자바 스크립트 모듈 대부분은 EventEmitter 클래스의 인스턴스입니다. 즉,이 모듈에 정의 된 메서드를 사용하여 모듈에서 정의한 이벤트에 대한 작업을 수행 할 수 있습니다.

가져 오는 방법

var EventEmitter = require ( "FuseJS / EventEmitter");
이것에 의해 EventEmitter가 임포트되어 클래스의 독자적인 인스턴스를 작성할 수 있습니다.

기본 사용

다음 코드는 두 이벤트로 EventEmitter를 만들고 이벤트 중 하나에 리스너를 추가 한 다음 해당 이벤트를 내 보냅니다.

var myEmitter = new EventEmitter ( "myEvent1", "myEvent2");

myEmitter.on ( "myEvent1", function (arg) {
    console.log ( "myEvent1"+ arg);
});

myEmitter.emit ( "myEvent1", "arg");
이 코드는 "myEvent1가 arg로 시작됨"을 인쇄합니다.

이벤트에서 Observables 만들기

때때로 이벤트는 시간이 지남에 따라 변하는 값을 나타냅니다. 이 경우 일반 콜백 대신 Observable을 사용하는 것이 더 편리 할 수 ​​있습니다. EventEmitters는 이것을 쉽게 만듭니다.

var obs = myEmitter.observe ( "myEvent");
"myEvent"가 발생하면 obs는 이벤트에 전달 된 인수를 포함하도록 업데이트됩니다. 인수가 여러 개인 경우 obs는 관찰 가능 목록입니다.

일단 청취자

다음 코드는 한 번만 발생하는 "myEvent"에 리스너를 추가합니다.

myEmitter.once ( "myEvent", function () {console.log ( "한 번만 호출됩니다");});
이벤트로부터 약속 만들기

한 번 리스너를 사용하는 대신 promiseOf를 사용하면 이벤트 이름에서 JavaScript Promise를 만들 수 있습니다.

var prom = myEmitter.promiseOf ( "myEvent");
prom.then (function (args) {
    // "myEvent"가`args`와 함께 출력되었습니다.
}). catch (함수 거부 (이유) {
    // "reason"이벤트와 함께 "error"이벤트가 발생했습니다.
});
promiseOf 메서드는 추가로 "error"이벤트 (아래 참조)를 수신하고 약속이 방출되면이를 거부합니다.

오류 처리

모든 EventEmitters에는 "error"라는 특수 내장 이벤트가 있습니다.

myEmitter.emit ( "오류", "오류가 발생했습니다 :("));
「error」가 출력되었을 때에 청취자가없는 경우, 예외를 슬로우합니다. 그 외에도 "오류"는 일반적인 이벤트처럼 작동합니다.

회원

EventEmitter ([... eventNames]) 생성자

지정된 인수 명을 허가하는 새로운 EventEmitter를 구축합니다.

var myEmitter = new EventEmitter ( "anEventName", "anotherEventName");
on (eventName, func ([... eventArgs]))

func를 리스너로 eventName 이벤트에 추가합니다.

myEmitter.on ( "anEventName", function (arg) {
    console.log ( ""+ arg "로 시작된 anEventName);
});
한 번 (eventName, func ([... eventArgs]))

func를 한 번 리스너로 eventName에 추가합니다. 즉, func는 리스너를 추가 한 후 처음 이벤트가 방출 될 때만 호출됩니다.

myEmitter.once ( "myEvent1", function (arg) {
    console.log ( "myEvent1"+ arg);
});
removeListener (eventName, func ([... eventArgs]))

eventName의 리스너에서 func를 제거합니다.

var listener = function () {console.log ( "Hello"); };

myEmitter.on ( "anEventName", listener);
// 여기서 "anEventName"에 대한 리스너가 호출됩니다.
myEmitter.removeListener ( "anEventName", listener);
// "anEventName"에서 더 이상 리스너가 호출되지 않습니다.

emit (eventName, [... eventArgs])

eventName을 트리거하여 eventArgs로 모든 리스너를 호출합니다.

myEmitter.on ( "anEvent", function (arg) {
    console.log ( "+ arg"로 시작된 anEvent);
});

myEmitter.emit ( "anEvent", "arg");
eventNames ()

이 EventEmitter가 지원하고있는, 편입을 포함한, 모든 허가 이벤트의 배열을 돌려줍니다.

var myEmitter = new EventEmitter ( "a", "b", "c");
var eventNames = myEmitter.eventNames ();

// 이벤트 이름에 "a", "b", "c", "error", "newListener"및 "removeListener"
관찰 (eventName)

eventName를 수신하는 observable을 작성합니다.

var obs = myEmitter.observe ( "anEvent");
promiseOf (eventName)

eventName 및 "error"를 수신하는 약속을 만듭니다.

var prom = myEmitter.promiseOf ( "anEvent");
prom.then (function (args) {
    // "args"로 "anEvent"가 발생했습니다.
}). catch (함수 거부 (이유) {
    // "reason"이벤트와 함께 "error"이벤트가 발생했습니다.
});
고급 회원

다음 기능은 기본 사용에는 필요하지 않지만 호환성 및 고급 사용 시나리오를 위해 제공됩니다.

addListener (eventName, func ([... eventArgs]))

on과 동의어.

myEmitter.addListener ( "anEvent", function (arg) {
    console.log ( "anEvent 방출 됨")
});
prependListener (eventName, func ([... eventArgs]))

on의 버전은 func가 eventName의 리스너 목록에서 첫 번째가되도록합니다. 즉, (prependListener 또는 prependOnceListener에 대한 나중에 호출 금지) func은 eventName이 발생할 때 먼저 호출됩니다.

myEmitter.prependListener ( "anEvent", function (arg) {
    console.log ( "anEvent가 방출 될 때 처음 호출됩니다");
});
prependOnceListener (eventName, func ([... eventArgs])

한 번 실행하면 func가 eventName의 리스너 목록에서 첫 번째 버전이됩니다. 즉, (prependListener 또는 prependOnceListener에 대한 나중에 호출 금지) func은 eventName이 발생할 때 먼저 호출됩니다.

myEmitter.prependOnceListener ( "anEvent", function (arg) {
    console.log ( "처음에는 호출되며, 한 번만, anEvent가 발생합니다");
});
registerEvent (eventName)

이 EventEmitter가 지원하는 허가 된 이벤트 세트에 eventName를 추가합니다.

myEmitter.registerEvent ( "newEventName");
removeAllListeners ([eventName])

인수와 함께 호출되어 eventName을 수신하는 모든 리스너를 제거합니다.

myEmitter.removeAllListeners ( "anEvent");
인수없이 호출되면 모든 이벤트를 수신하는 모든 리스너를 제거합니다.

myEmitter.removeAllListeners ();
이 메소드는 일반적으로 EventEmitter를 수신하는 코드의 다른 부분에 영향을주기 때문에 사용하지 않아야합니다.

기본 제공 이벤트 이름

위에서 언급 한 "오류"이벤트 외에도 EventEmitter의 모든 인스턴스는 다음 이벤트를 제공합니다.

"newListener"(eventName, listener)

새로운 청취자가 EventEmitter에 추가되면 (자) 트리거됩니다.

myEmitter.on ( "newListener", function (eventName, listener)) {
    console.log ( "listener"+ eventName에 추가됨;
});
"removeListener"(이벤트 이름, 리스너)

리스너가 EventEmitter에서 제거되면 트리거됩니다.

myEmitter.on ( "removeListener", function (eventName, listener)) {
    console.log ( "리스너가"+ eventName에서 제거됨);
});