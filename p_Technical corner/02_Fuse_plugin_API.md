원본: https://www.fusetools.com/docs/technical-corner/fuse-protocol

퓨즈 플러그인 API

당신이 좋아하는 편집자를위한 퓨즈 플러그인을 만들고, 퓨즈를위한 커스텀 툴을 작성하거나, 다른 방법으로 그것을 통합하고 싶습니까? fuse 데몬 - 클라이언트를 사용하면 사실상 모든 언어에서이를 수행 할 수 있습니다. JavaScript로 작성된 예제 플러그인은 물론 통신 프로토콜에 대한 설명을 미리 읽어보십시오.

이 가이드는 플러그인 API에 대한 기술적 인 소개부터 시작합니다. 그러나 기술적 인 설명을 원하지 않으면 사용법으로 뛰어들 수 있습니다. 여전히 우리는 그것을 읽으면서 장면 뒤에서 일어나는 일에 대해 이해할 것을 권합니다.

퓨즈 프로토콜

퓨즈 플러그인 API는 두 개의 추상화 레이어 위에 구축됩니다. 코어 레이어는 Fuse Protocol입니다. 퓨즈 프로토콜 (Fuse Protocol)은 최소한의 데이터 오버 헤드로 다양한 유형의 데이터를 보낼 수있는 단순한 프로토콜입니다.

퓨즈 프로토콜을 통해 전송 된 데이터는 헤더와 페이로드의 두 부분으로 구성됩니다. 그들은 함께 우리가 메시지라고 부르는 것을 나타냅니다. 헤더는 줄 바꿈으로 끝나는 두 개의 UTF-8 인코딩 된 문자열로 구성됩니다. 첫 번째 문자열은 메시지의 유형을 나타내며 두 ​​번째 문자열은 페이로드의 길이를 나타냅니다.

다음은 메시지 구조화의 예입니다.

내 메시지 유형 \ n
7 \ n
푸 바
데몬 프로토콜

퓨즈 프로토콜 위에는 데몬 프로토콜이 있습니다. 데몬 프로토콜은 이벤트, 요청 및 응답이라는 세 가지 유형의 퓨즈 프로토콜 메시지를 정의합니다. 이러한 각 유형의 페이로드는 UTF-8로 인코딩 된 JSON이며 다음과 같습니다.

행사

{
    "Name": "ExampleEvent",
    "SubscriptionId": 32, // 이벤트 구독의 ID (데몬에 의해 자동 설정 됨),
    "Data": {...}, // 이벤트 데이터가있는 이벤트 별 JSON 객체
}
의뢰

{
    "이름": "MyRequest",
    "Id": 242, // 각 요청에 대해 고유 한 번호로 지정하여 일치하는 응답 메시지를 인식 할 수 있습니다.
    "Arguments": {...}, // 요청이있는 요청 별 JSON 객체
}
응답

{
    "Id": 242, // 응답 인 요청의 ID입니다.
    "Status": "Success", // "Success", "Error"또는 "Unhandled"일 수 있음
    "Result": {...}, // 상태가 "Succsess"인 경우 응답이있는 요청 별 JSON 객체
    "Errors": [...], // 상태가 "Error"이면 더 많은 오류 정보를 포함하는 객체의 배열
}
요청 및 응답

요청 및 응답 시스템은 RPC (원격 프로 시저 호출)를 기반으로합니다. 퓨즈 데몬은 요청자와 응답자 사이의 전환 역할을합니다. 클라이언트에서 데몬으로 보낸 요청은 특정 요청 유형에 대한 처리기를 제공하는 마지막 클라이언트로 전달됩니다. 클라이언트는 PublishService 요청을 데몬에 전송하여 서비스를 제공한다는 것을 공개 할 수 있습니다 (요청 유형을 처리 함). 서비스 공급자는 요청을 응답으로 변환하여 하나 이상의 요청 유형을 처리해야합니다. 이 개념은 메서드 호출과 유사합니다. 요청 메시지는 메서드 이름과 메서드 호출에 대한 인수를 저장하며 메서드의 반환 값은 유효하거나 예외 기반 언어의 예외와 비슷한 오류입니다. turn은 요청자에 대한 응답으로 되돌려 보내집니다 (응답은 요청자에게 전달하는 데몬으로 보내집니다).

PublishService 요청

{
    "이름": "PublishService",
    "논쟁": {
        "RequestNames": [ "MyRequest", ...], // 우리가 응답 한 요청의 이름 배열
    }
}
이벤트

이벤트는 클라이언트가 특정 이벤트 유형을 구독 할 수있는 전형적인 pub / sub 방식으로 메시지 버스에서 방송됩니다. 등록은 데몬에 등록 요청을 전송하여 수행됩니다. 클라이언트는 이벤트를 데몬으로 보내서 버스로 전달되는 이벤트를 보낼 수 있습니다.

우리 프로토콜에 대해 주목해야 할 것은 두 클라이언트 사이에 직접 연결이 없으며 서로에 대해 알지 못한다는 것입니다. 모든 통신은 스위치와 브로드 캐스터로 작동하는 데몬을 통과합니다.

구독 신청

{
    "이름": "구독",
    "논쟁": {
        "Filter": "<regex>", // 유형에 따라 수신 이벤트를 필터링하는 .Net 스타일 정규식
        "Replay": false, // 연결하기 전에 보낸 메시지를 받으려면 재생을 사용하십시오.
        "SubscriptionId": 32, //이 구독을 나타내는 로컬 고유 번호로, 들어오는 이벤트를 인식하고 구독을 취소 할 수 있습니다.
    }
}
데몬 프로토콜 자체는 일련의 메시지 유형을 정의하지 않습니다. 데몬 프로토콜의 사용자는 자유롭게 유형을 정의 할 수 있습니다. 다음은 맞춤 이벤트 메시지의 예입니다.

사용자 정의 이벤트

{
    "이름": "MyMouseEvent",
    '데이터': {
        "mouseX": 200,
        "MouseY": 100
    }
}
용법

fuse 데몬 - 클라이언트 <name-of-client> 명령은 데몬에 대한 연결을 만들고 stdin / stdout에 쓰고 읽는 모든 데이터를 전달합니다. 프로세스를 시작하기 위해 API에 액세스해야하기 때문에 모든 주요 언어에서 사용할 수 있어야합니다. 다시 말해 데몬에게 데이터를 보내 stdin에 쓰고 stdout에서 읽은 데이터를받는 것입니다. 또한 stderr에서 프로토콜 오류를 읽을 수 있습니다. 그 외에도 fuse 데몬 - 클라이언트는 연결이 끊어진 것으로 간주 될 때 종료 코드 1로 종료합니다. 다른 오류 코드는 치명적인 오류로 간주됩니다. 발생하지 않아야합니다 (발생하면 당사에보고하십시오).

요청 및 이벤트

데몬에 대한 연결을 설정 한 후 가장 먼저 수행 할 작업 중 하나는 요청을 보내는 것입니다. 가장 중요한 두 가지 요청 유형은 PublishService 및 Subscribe입니다. 자세한 내용은 api 참조에 나열되어 있습니다. 한 가지 예는 기록 된 모든 이벤트를 구독하는 것입니다.이 이벤트는 다음 데이터를 전송하여 수행 할 수 있습니다 (전체 테스트 예는 여기를 참조하십시오).

요청 / 메시지 타입이 맨 위에있는`Fuse Protocol` 단락에 설명 된대로 개행 문자로 끝납니다
106 // 페이로드의 크기 (bytes)는 맨 위에있는 'Fuse Protocol` 문단에서 설명한대로 개행 문자로 끝났다.
{ "Name": "Subscribe", "Id": 101, "Arguments": { "필터": "Fuse.BuildLogged", "Replay": false, "SubscriptionId": 42}}
위의 구독 요청을 보낸 후 Fuse.BuildLogged라는 모든 이벤트를 수신해야합니다. (이 이벤트를 트리거 할 프로젝트의 미리보기 실행). 받은 이벤트는 다음과 유사해야합니다.

행사
144
{ "Name": "Fuse.BuildLogged", "Data": { "BuildId": "6c7e6f55-74de-45d1-bdb5-9f9cf2bafbf7", "Message": "코드 및 데이터 생성 \ n"}, "SubscriptionId" 42}
또한 이벤트를 브로드 캐스팅 할 수도 있습니다. 이벤트는 데몬으로 전송됩니다. 데몬은 등록 된 모든 리스너에게 이벤트 브로드 캐스팅을 처리합니다. 예를 들어, Fuse.BuildLogged 이벤트를 직접 보낼 수도 있습니다 (기존 이벤트와 동일한 이름의 사용자 정의 이벤트를 브로드 캐스팅하는 것은 좋지 않습니다). 이것이 당신의 Fuse.BuildLogged 이벤트가 어떻게 생겼는지입니다 :

{
    "이름": "Fuse.BuildLogged",
    '데이터': {
        "BuildId": "1c7e6f55-74de-45d1-bdb5-9f9cf2bafbf7",
        "Message": "내 로그 기록 이벤트! \ n"
    }
}
퓨즈 프로토콜 헤더를 생략했음을 유의하십시오. 우리는 명확성을 위해 나머지 기사에서 퓨즈 프로토콜 헤더를 계속 수행 할 것입니다. 그러나 메시지에는 여기에 설명 된 헤더도 있습니다.

PublishService는 사용자 지정 요청 유형에 응답 할 때 보낼 것을 고려할 수있는 요청입니다. 즉, 다른 클라이언트가 요청할 수있는 기능을 제공합니다. 예를 들어 GetAgeOfStudent라는 기능을 제공한다고하면 다음과 같은 내용이 데몬으로 전송되어야합니다.

{
    "이름": "PublishService",
    "논쟁": {
        "RequestNames": [ "GetAgeOfStudent"]
    }
}

모든 GetAgeOfStudent 요청은이 시점부터 사용자에게 전달됩니다. 예를 들어 데몬에 연결된 클라이언트가 GetAgeOfStudent 요청을 보내면 응답을 받게되며 응답을 다시 보내는 것은 사용자의 책임입니다.

{
     "이름": "GetAgeOfStudent",
     "이드": 2,
     "논쟁": {
         "StudentName": "Bob"
     }
}
위와 같은 요청을받을 수 있습니다. 그러나 당신이 그것에 어떻게 반응하는지는 당신에게 달려 있습니다. 예를 들면 :

{
     "이드": 2,
     "상태": "성공",
     "결과": {
         "나이": "24"
     },
     "오류": null
}
GetAgeOfStudent를 요청한 클라이언트는 응답을 수신하고 응답을 수신하는 유일한 사람이됩니다.

예

이 기사는 단지 단어가있는 곳으로 향하고 있지 않으므로 여기에 Javascript / Node.js 예제가 있습니다. 데몬과 간단한 통신을 작성하는 방법을 보여주는 예입니다.이 로그는 빌드 로그를 뱉어냅니다. 코드를 복사하여 파일에 붙여넣고 노드 파일을 실행하고 nodej도 있는지 확인하십시오.

```
var spawn = require('child_process');

// Spawn daemon client
var fuseClient = spawn.spawn("fuse", ['daemon-client', 'Simple Client']);

var buffer = new Buffer(0);
fuseClient.stdout.on('data', function (data) {
  // Data is a stream and must be parsed as that
  var latestBuf = Buffer.concat([buffer, data]);
  buffer = parseMsgFromBuffer(latestBuf, function (message) {
    console.log("Message received: " + message);
  });
});

fuseClient.stderr.on('data', function (data) {
  console.log(data.toString('utf-8'));
});

fuseClient.on('close', function (code) {
  console.log('fuse: daemon client closed with code ' + code);
});

// This is how you may subscribe to all "Fuse.BuildLogged" events
// In other words you will receive build output, by sending this request
var subRequest = JSON.stringify({
  Name: "Subscribe",
  Id: 101, // Arbitrary identifier to map responses back to requests
  Arguments: {
    Filter: "Fuse.BuildLogged",
    Replay: false, // Use replay to receive messages sent before you connected
    SubscriptionId: 42 // Arbitrary ID to map events to a particular subscription
  }
});

console.log("Sending: " + subRequest);
send(fuseClient, "Request", subRequest);

function parseMsgFromBuffer(buffer, msgCallback) {
  var start = 0;

  while (start < buffer.length) {
    var endOfMsgType = buffer.indexOf('\n', start);
    if (endOfMsgType < 0)
      break; // Incomplete or corrupt data

    var startOfLength = endOfMsgType + 1;
    var endOfLength = buffer.indexOf('\n', startOfLength);
    if (endOfLength < 0)
      break; // Incomplete or corrupt data

    var msgType = buffer.toString('utf-8', start, endOfMsgType);
    var length = parseInt(buffer.toString('utf-8', startOfLength, endOfLength));
    if (isNaN(length)) {
      console.log('fuse: Corrupt length in data received from Fuse.');
      // Try to recover by starting from the beginning
      start = endOfLength + 1;
      continue;
    }

    var startOfData = endOfLength + 1;
    var endOfData = startOfData + length;
    if (buffer.length < endOfData)
      break; // Incomplete data

    var jsonData = buffer.toString('utf-8', startOfData, endOfData);
    msgCallback(jsonData);
    start = endOfData;
  }

  return buffer.slice(start, buffer.length);
}

function send(fuseClient, msgType, serializedMsg) {
  // Pack the message to be compatible with Fuse Protocol.
  // As:
  // '''
  // MessageType (msgType)
  // Length (length)
  // JSON(serializedMsg)
  // '''
  // For example:
  // '''
  // Event
  // 50
  // {
  //   "Name": "Test",
  //   "Data":
  //   {
  //     "Foo": "Bar"
  //   }
  // }
  // '''
  var length = Buffer.byteLength(serializedMsg, 'utf-8');
  var packedMsg = msgType + '\n' + length + '\n' + serializedMsg;
  try {
    fuseClient.stdin.write(packedMsg);
  }
  catch (e) {
    console.log(e);
  }
}
```

실세계의 예를 보려면 공식 오픈 소스 Atom Plugin을 참조하십시오.

퓨즈 API 참조

이 목록은 휴즈에있는 메시지의 예입니다. 그러나 이러한 메시지 중 일부는 변경 될 수 있음을 유의하십시오 (그러나 제거하기 전에 더 이상 사용되지 않습니다). 또한 PublishService 및 Subscribe를 살펴 봐야합니다.

짓다

예를 들어 오류 목록을 만들 때 구독 할 수있는 빌드 이벤트가 몇 가지 있습니다.

퓨즈. 빌드 시작 - 이벤트

빌드가 시작된 후 보내고 빌드와 관련된 정보를 포함합니다.

{
    "이름": "Fuse.BuildStarted",
    "SubscriptionId": 32, // 구독 응답에서 제공된 ID입니다.
    "데이터":
    {
        "BuildType": "FullCompile", // 이것은 전체 프로젝트가 빌드되었음을 의미합니다. 또한 "LoadMarkup"일 수 있습니다. 즉, 이미 빌드 된 앱을 실시간으로 다시로드하고 있음을 의미합니다.
        "BuildId": "a19de798-6d22-45f8-b453-0a44ea606025", //이 빌드를 식별하는 GUID,이 빌드와 관련된 빌드 이벤트가 intereseted 인 경우 계속 유지하십시오.
        "BuildTag": "DaveWasHere", // Build-tag은 자신이 시작한 빌드에서 빌드 이벤트를 인식하는 방법입니다. `fuse preview -name = <YourTag>`를 사용하여 설정할 수 있습니다. 기본값은 emtpy 또는 null입니다.
        "PreviewId": "cceb4dbc-e515-4dda-bff2-46cda79a9696", // 빌드 유형이 "LoadMarkup"인 경우 미리보기에서 사용 된 초기 전체 컴파일의 BuildId
        "ProjectPath": "C : \\ FuseProjects \\ MyProject.unoproj", // 빌드중인 프로젝트 파일의 기본 경로
        "Target": "DotNetDll", // 빌드 타겟. 지원되는 대상은 "DotNetDll"(로컬 미리보기), "iOS"및 "Android"입니다. 다른 대상으로는 "DotNetExe", "CMake", "MSVC12", "WebGL"및 "Unknown"이 있습니다.
    }
}
Fuse.BuildLogged - 이벤트

빌드가 기록 된 후에 보냈습니다.

{
    "이름": "Fuse.BuildLogged",
    "SubscriptionId": 32,
    "데이터":
    {
        "BuildId": "a19de798-6d22-45f8-b453-0a44ea606025",
        "Message": "...", // 빌드 중에 기록 된 사람이 읽을 수있는 메시지
    }
}
퓨즈. 빌드 사고 감지 - 이벤트

빌드 문제가 감지되면 전송됩니다.

{
    "이름": "Fuse.BuildIssueDetected",
    "SubscriptionId": 32,
    "데이터":
    {
        "BuildId": "a19de798-6d22-45f8-b453-0a44ea606025",
        "IssueType": "Error", // "Unknown", "FatalError", "Error", "Warning"또는 "Message"일 수 있음
        "Path": "C : \\ FuseProjects \\ MyClass.uno", // 오류가 발생한 위치의 파일 경로. 선택적으로 비어있을 수 있습니다 (선택 사항).
        "StartPosition": 오류가 발생한 파일의 { "Line": 1, "Character": 1}, // 1 인덱싱 된 시작 위치 (선택 사항)
        "EndPosition": 오류가 발생한 파일의 { "Line": 1, "Character": 1}, // 1-indexed end position (선택 사항)
        "ErrorCode": "E0001", // Uno 컴파일러가 생성 한 오류 코드
        "Message": "..."// 사람이 읽을 수있는 이슈 설명
    }
}
퓨즈. 빌드 완료 - 이벤트

빌드가 완료된 후에 전송됩니다 (성공적이든 아니든).

{
    "이름": "Fuse.BuildEnded",
    "SubscriptionId": 32,
    "데이터":
    {
        "BuildId": "a19de798-6d22-45f8-b453-0a44ea606025",
        "Status": "Success", // 빌드 상태. 유효한 동상은 "InternalError", "Error"및 "Success"입니다.
    }
}
코드 완성

Fuse.GetCodeSuggestions - 메서드

요청 예

{
    "Id": 42, // 고유 요청 ID
    "이름": "Fuse.GetCodeSuggestions",
    "논쟁":
    {
        "SyntaxType": "UX", // 일반적으로 "UX"또는 "Uno"
        "Path": "C : \\ FuseProjects \\ MainView.ux", // 제안이 요청 된 문서의 경로
        "텍스트": "<응용 프로그램> \ n \ t <버튼 /> \ n </ 앱>", // 문서의 전체 소스는 제안이 요청되는 경우
        "CaretPosition"제안이 요청 텍스트 내의 { "선": 2, "문자"9} // 1 인덱스 텍스트 위치
    }
}
응답 예

{
    "Id": 42, // 요청 ID
    "상태": "성공",
    "결과":
    {
        "IsUpdatingCache": false, // 참이면 나중에 다시 시도하는 것을 고려해야합니다.
        "CodeSuggestions":
        [
            {
                "제안": "<제안>",
                "PreText": "<pretext>",
                "PostText": "<posttext>",
                "유형": "<데이터 유형>",
                "ReturnType": "<datatype>",
                "AccessModifiers": [ "<accessmodifier>", ...],
                "FieldModifiers": [ "<fieldmodifier>", ...],
                "MethodArguments":
                [
                    { "이름": "<이름>", "ArgType": "<데이터 유형>", "IsOut": "<거짓 | 진정한>"},
                    ...
                ],
            },
            ...
            ],
    }
}
감시 장치

Fuse.LogEvent - 이벤트

debug_log, console.log를 호출하거나 DebugAction을 사용하여 미리보기 인스턴스에서 보냅니다.

{
    "이름": "Fuse.LogEvent",
    "데이터":
    {
        "클라이언트 ID": "b564e85a-fd37-46aa - a8d8-e601e08fe4f5은"빌드하는 동안 생성 된 클라이언트의 고유 ID를, //
        "ClientName": "MyPhone 5", // 친숙한 클라이언트 이름
        "Message": "...", // 기록 된 메시지 (debug_log 사용)
        "ConsoleType": "DebugLog", // 예약 됨
    },
}
Fuse.ExceptionEvent - 이벤트

미리보기 인스턴스에서 예외가 발생한 후에 보냅니다.

{
    "이름": "Fuse.ExceptionEvent",
    "데이터":
    {
        "클라이언트 ID": "b564e85a-fd37-46aa - a8d8-e601e08fe4f5은"빌드하는 동안 생성 된 클라이언트의 고유 ID를, //
        "ClientName": "MyPhone 5", // 친숙한 클라이언트 이름
        "Type": "Uno.NullReferenceException", // 예외 이름 Uno-type
        "Message": "객체 참조가 객체의 인스턴스로 설정되지 않았습니다.", // 사람이 읽을 수있는 이슈 설명
        "StackTrace": "...", // 예외가 발생한 곳의 스택 추적을 포함하는 문자열입니다. iOS와 Android에서 stacktrace를 원한다면`fuse preview --enable-cppstacktrace`를 실행하십시오.
    }
}
시사

Fuse.SelectionChanged - 이벤트

미리보기에서 선택을 설정하려면이 이벤트를 보냅니다.

{
    "이름": "Fuse.Preview.SelectionChanged",
    "데이터":
    {
        "Path": "C : \\ FuseProjects \\ MainView.ux", // 선택이 변경된 파일의 경로
        "텍스트": "<App> \ n \ t <Button /> \ n </ App>", // 문서의 전체 소스
        "CaretPosition": { "Line": 2, "Character": 9} // 선택 영역이 변경된 텍스트 내의 1- 색인 텍스트 위치
    }
}
