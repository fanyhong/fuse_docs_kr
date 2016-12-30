원본: https://www.fusetools.com/docs/native-interop/native-js-modules

Uno에서 기본 JavaScript 모듈 만들기

Fuse.Reactive.JavaScript를 사용하면 Uno에서 원시 코드 모듈을 정의하고 JavaScript에서 사용할 수 있습니다. 이것은 FuseJS의 모든 기본 모듈 (예 : 카메라, 진동 등)이 구현되는 방식입니다.

모듈은 require 함수를 통해 얻을 수있는 JavaScript 객체입니다.

네이티브 모듈 정의하기

기본 모듈을 정의하려면 Fuse.Scripting.NativeModule의 인스턴스를 만듭니다. .unoproj 파일에 Fuse.Scripting에 대한 참조가 아직 추가되지 않은 경우 추가하는 것을 잊지 마십시오.

생성자에 대해 멤버 목록을 전달할 수 있습니다. AddMember () 메서드를 통해 생성자에 멤버를 추가 할 수도 있습니다.

현재 두 가지 유형의 구성원이 지원됩니다.

NativeFunction - JavaScript 함수로 Uno 대리자를 래핑합니다.
NativePromise - Uno Promise <T>를 EcmaScript 6 스타일 약속으로 포장합니다.
참고 : 당분간 네이티브 모듈을 자바 스크립트에 노출시키는 것은 약간의 해킹입니다. [UXGlobalModule] 특성으로 클래스를 표시하고 공용 생성자를 사용하여 해당 특성을 싱글 톤으로 만들고 Resource.SetGlobalKey ()를 사용하여 전역 리소스로 표시해야합니다. 우리는 이것을 해결하기 위해 더 좋은 방법으로 작업하고 있습니다.
다음은 간단한 로깅 기능을 보여주는 간단한 예입니다.

퓨즈 사용;
Fuse.Scripting을 사용하여;
Fuse.Reactive를 사용하여;
Uno.UX 사용;

[UXGlobalModule]
공용 클래스 MyLogModule : NativeModule
{
    정적 읽기 전용 MyLogModule _instance;

    public MyLogModule ()
    {
        // 우리가 모듈을 한 번만 초기화하는지 확인하십시오.
        if (_instance! = null) return;

        _ 인스턴스 =이;
        Resource.SetGlobalKey (_instance, "MyLogModule");
        AddMember (새 NativeFunction ( "로그", (NativeCallback) 로그));
    }

    정적 오브젝트 Log (Context c, object [] args)
    {
        foreach (args의 var arg)
            debug_log arg;

        null를 돌려 준다.
    }
}
그러면 아래와 같이 JavaScript에서 네이티브 모듈을 사용할 수 있습니다.

<App>
    <패널>
        <자바 스크립트>
            var LogModule = require ( "MyLogModule");

            LogModule.Log ( "Hello from JavaScript!");
        </ JavaScript>
    </ Panel>
</ App>
NativeFunction

네이티브 함수는 다음 서명이있는 Fuse.Scripting.NativeCallback 대리자를 제공하여 정의됩니다.

공용 위임 객체 NativeCallback (Context c, object [] args);
여기서 args는 함수에 전달 된 JavaScript 인수 목록입니다. 인수는 런타임에 함수에 전달되는 내용에 따라 다음 유형 중 하나가 될 수 있습니다.

인수가 JavaScript 수인 경우 double 유형의 숫자 ​​또는 int입니다. 우리는 Fuse.Scripting.Marshal 클래스를 사용하여 원하는 특정 값으로 정확하게 마샬링 할 수 있습니다.

var number = Marshal.ToInt (args [0]);

인수가 JavaScript bool 인 경우 부울입니다.

인수가 JavaScript 문자열이면 문자열입니다.
인수가 JavaScript 배열 인 경우 Array.Scripting.Array입니다.
인수가 JavaScript 함수 인 경우 Fuse.Scripting.Function.
인수가 배열이나 함수가 아닌 JavaScript 객체 인 경우 Fuse.Scripting.Object입니다.
인수가 박스형 Uno 객체 인 경우 Fuse.Scripting.External입니다.
JavaScript 객체가 null이거나 정의되지 않은 경우 null입니다.
콜백의 반환 값은 동일한 규칙을 따르며 JS 함수로 정렬되어야하는 Uno 콜백에 대해 Fuse.Scripting.Callbacks를 추가로 허용합니다. null을 반환하면 해당 JS 함수는 null을 반환합니다.

네이티브 EventEmitter 모듈

NativeModule의 하위 클래스 인 NativeEventEmitterModule을 사용하여 JavaScript 클래스 EventEmitter의 NativeModules 인스턴스를 자동으로 만들 수 있습니다. EventEmitters는 명명 된 이벤트를 내보내고들을 수 있으며 NativeEventEmitterModules는 Uno에서 이러한 이벤트를 방출 할 수 있습니다.

다음 예제에서는 이벤트를 사용하여 Send 함수를 사용하여 보낸 모든 메시지를 반향하는 NativeEventEmitterModule을 만드는 방법을 보여줍니다.

퓨즈 사용;
Fuse.Scripting을 사용하여;
Fuse.Reactive를 사용하여;
Uno.UX 사용;

[UXGlobalModule]
공용 클래스 채팅 : NativeEventEmitterModule
{
    static 읽기 전용 Chat _instance;

    공개 채팅 ()
        : base (true, "messageReceived")
    {
        if (_instance! = null) return;

        _ 인스턴스 =이;
        Resource.SetGlobalKey (_instance, "Chat");

        AddMember (새 NativeFunction ( "send", (NativeCallback) SendMessage));
    }

    개체 [] SendMessage (Fuse.Scripting.Context 컨텍스트, 개체 [] args)
    {
        var arg = args.Length> 0? args [0] as string : null;

        if (arg == null)
            Emit ( "messageReceived", "----");
        그밖에
            Emit ( "messageReceived", arg);

        null를 돌려 준다.
    }
}
NativeEventEmitterModule을 만들 때 명시 적으로 기본 클래스 생성자를 호출해야합니다. 첫 번째 인수는 앱이 완전히 초기화되기 전에 발생하는 이벤트가 캐시되고 재생되는지 여부를 결정하는 bool입니다. 다른 인수는 허용 된 이벤트 이름입니다. 이 경우에는 "messageReceived"라는 이벤트 하나만 있습니다.

우리는 Emit ( "messageReceived", arg)를 호출하여 JavaScript 측에서 "messageReceived"이벤트를 수신하는 모든 리스너에게 arg를 전달합니다.

채팅 모듈은 자바 스크립트에서 다음과 같이 사용할 수 있습니다.

var chat = require ( "채팅");

function send () {
    chat.send (새 Date (). toString ());
}

chat.on ( "messageReceived", function (message) {
    console.log ( "메시지 수신"+ 메시지);
});
자세한 정보는 EventEmitter의 문서를 참조하십시오.

NativePromise

약속은 나중에 사용할 수있게 될 수있는 개체를 나타내는 개체이며 해당 개체를 검색 할 때 성공하거나 실패 할 수 있습니다. HTTP 요청, 대화 상자 또는 카메라로 사진을 찍은 결과가 그 예입니다.

네이티브 모듈은 래핑을 지원합니다. Uno는 NativePromise 클래스를 통해 JS 약속을 자동으로 약속합니다.

구문은 다음과 같습니다.

새로운 NativePromise <TUno, TJavaScript> (name, futureFactory [, converter])
또는

새로운 NativePromise <TUno, TJavaScript> (name, resultFactory [, converter])
어디에:

TUno는 Uno에서 약속이 생산하는 유형입니다.
TJavaScript는 JavaScript에서 약속이 생성하는 유형입니다. converter가 생략되면 TUno와 TJavaScript는 같은 유형 (문자열, 숫자 또는 부울)이어야합니다.
name은 모듈에있는 멤버의 이름입니다.
futureFactory는 JavaScript (NativeFunction에 대한 인수와 동일한 규칙에 따라)에서 전달되는 인수 집합 인 객체 []에서 Future <TUno>를 생성하는 대리자입니다. 서명:

public delegate Future <TUno> FutureFactory (object [] args);

resultFactory는 객체 []로부터 TUno를 생성하는 델리게이트입니다.

공개 위임 TUno ResultFactory (object [] args);

converter (옵션)는 Fuse.Scripting.Context가 주어진 경우 TUno를 TJavaScript로 변환하는 메서드입니다. 서명:

공용 델리게이트 TJavaScript 변환기 (컨텍스트 c, TUno 값);

첫 번째 버전은 기존 Future <TUno>를 래핑하고 선택적으로 해당 JavaScript 유형으로 변환합니다.

두 번째 버전은 TUno를 생성하는 간단한 함수를 래핑합니다. 이 함수는 별도의 스레드에서 자동으로 실행되며 결과는 완료되면 JavaScript 스레드로 다시 전달됩니다. 이것은 간단한 약속을 구현하는 간단한 방법을 제공합니다. 약속을 거부하려면 resultFactory에서 예외를 throw하십시오.

Future <T> 및 Promise <T>는 현재 Experimental.Threading 패키지에 정의 된 Uno 유형입니다.

아래는 아주 간단한 예제입니다.

퓨즈 사용;
Fuse.Scripting을 사용하여;
Fuse.Reactive를 사용하여;
Uno.UX 사용;
우노를 사용하여;

[UXGlobalModule]
Public 클래스 PromiseExample : NativeModule
{
    정적 읽기 전용 PromiseExample _instance;

    공개 PromiseExample ()
    {
        if (_instance! = null) return;

        _ 인스턴스 =이;
        Resource.SetGlobalKey (_instance, "PromiseExample");

        AddMember (새 NativePromise <string, Fuse.Scripting.Object> ( "promiseMe", PromiseMe, Converter));
    }

    정적 문자열 PromiseMe (object [] args)
    {
        if (args.Length! = 1)
        {
            // 예외 메시지와 함께 약속을 거부합니다.
            새로운 예외를 던지십시오 ( "promiseMe ()는 정확히 1 개의 매개 변수가 필요합니다.");
        }

        // 이것은 약속을 해결할 것입니다.
        args [0]을 문자열로 반환;
    }

    정적 Fuse.Scripting.Object 변환기 (컨텍스트 컨텍스트, 문자열 str)
    {
        var wrapperObject = context.NewObject ();
        래퍼 객체 [ "foo"] = str;
        return wrapperObject;
    }
}
우리는 할 수있다.


var PromiseExample = require ( "PromiseExample");
PromiseExample.promiseMe ( "bar")
    .then (function (result) {
        console.dir (result);
        // {foo : "bar"}
    });

PromiseExample.promiseMe ()
    .then (function () {
        // 이것은 결코 일어나지 않을 것이다.
    }, function (err) {
        console.log ( "오류 :"+ 오류);
        // 오류 : promiseMe ()에는 정확히 1 개의 매개 변수가 필요합니다.
    })
NativeEvent

참고 : 기본 이벤트를 수행하는 권장 방법은 위에서 설명한 NativeEventEmitterModule을 사용하는 것입니다. NativeEvent는 단계적으로 제거되는 이전 기능입니다.

네이티브 이벤트를 사용하여 Uno 코드에서 JavaScript를 다시 호출 할 수 있습니다. NativeModule에 NativeEvent를 추가하면됩니다. 그런 다음 NativeEvent.RaiseAsync (args)를 사용하여 NativeEvent를 호출 할 수 있습니다.

다음 예제에서는 send 함수와 onMessageReceived 이벤트를 사용하여 보낸 모든 메시지를 반향 출력하는 NativeModule을 만드는 방법을 보여줍니다.

퓨즈 사용;
Fuse.Scripting을 사용하여;
Fuse.Reactive를 사용하여;
Uno.UX 사용;

[UXGlobalModule]
공용 클래스 채팅 : NativeModule
{
    static 읽기 전용 Chat _instance;

    NativeEvent _nativeEvent;

    공개 채팅 ()
    {
        if (_instance! = null) return;

        _ 인스턴스 =이;
        Resource.SetGlobalKey (_instance, "Chat");

        AddMember (새 NativeFunction ( "send", (NativeCallback) SendMessage));
        _nativeEvent = 새 NativeEvent ( "onMessageReceived");
        AddMember (_nativeEvent);
    }

    오브젝트 SendMessage (Fuse.Scripting.Context context, object [] args)
    {
        var arg = args [0] as string;
        if (arg! = null)
            _nativeEvent.RaiseAsync (arg);
        그밖에
            _nativeEvent.RaiseAsync ( "----");

        null를 돌려 준다.
    }
}
채팅 모듈은 자바 스크립트에서 다음과 같이 사용할 수 있습니다.

var chat = require ( "채팅");

function send () {
    chat.send (새 Date (). toString ());
}

chat.onMessageReceived = function (message) {
    console.log ( "onMessageReceived"+ message);
};
