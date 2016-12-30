원본: https://www.fusetools.com/docs/technical-corner/uxl-handbook

UXL 핸드북 (여기에 드래곤즈가 될 것입니다)

일반적으로 pure UX 및 JavaScript 만 있으면 퓨즈를 사용하여 전체 프론트 엔드 구성 요소 및 응용 프로그램을 빌드하는 데 필요한 모든 것을 할 수 있습니다. 그러나 때로는 Uno에 플랫폼 간 코드를 작성하여 원시 처리 능력이나 네이티브 라이브러리와의 상호 운용성을 활용해야합니다. 더 적은 경우에는 Uno를 사용하는 것만으로도 작업이 완료되지 않습니다. 예를 들어, 작성한 플랫폼 고유의 고유 구성 요소가 사용자 정의되어 있고 Uno에서 사용하고 /하거나 JavaScript에 노출해야하는 경우. 절대적으로 더 깊이 들어가야 만한다면 UXL에 대해 설명 할 수 있습니다.

UXL은 Uno eXtension Layer의 약자입니다. 이 용어는 Uno 컴파일러에 다양한 기능을 제공하기 위해 사용되는 다양한 기능을 포함하는 용어로, 표준 기능 집합을 넘어서는 작업을 수행해야 할 때 사용됩니다.

이 문서는 상당히 비공식적이고 (아직은 비교적 완성 된) UXL의 현재 상태를 보여줍니다. 그러나이 문서는 "Here be Dragons"제목과 같이 이러한 기능이 매우 많이 변경 될 수 있습니다. 너 경고 받았어 :)

이 물건들이 어떻게 작동하는지 보는 가장 좋은 방법은 코드에 있으므로 시작할 수 있습니다.

extern (조건)

extern ()은 조건에 맞는 경우에만 uno 코드를 빌드에 포함 할 수있게합니다.

문서의 나머지 부분에서 문구는 '조건을 충족합니다'또는 '조건이 충족 됨'은 '사실로 평가 된 조건'으로 읽을 수 있습니다.

예를 들어 다음 클래스는 iOS 용으로 빌드 할 때만 포함됩니다.

public extern (iOS) 클래스 Test0
{
    공개 무효 SayHi ()
    {
        debug_log "hi from ios";
    }
}
extern () 조건 내에서 간단한 부울 논리를 사용할 수도 있습니다.

public extern (! iOS) 클래스 Test0
{
    공개 무효 SayHi ()
    {
        debug_log "iOS 이외의 어떤 것에서도 안녕 :)";
    }
}
Extern ()의 스트라이핑은 컴파일 프로세스 초기 (AST 처리 중)에 발생하므로 조건이 충족되지 않으면 대부분의 구문 버그가 발견되지 않습니다. UXL 코드를 사용하여 모든 대상을 테스트하십시오.

extern () 스트리핑은 너무 일찍 끝나기 때문에 우노 (Uno) 규칙을 여러 번 위반할 수 있습니다. 예를 들어 위의 두 클래스는 이름 충돌을 일으키지 않고 동일한 네임 스페이스에있을 수 있습니다. 이것은 우리가 클래스의 플랫폼 특정 인스턴스를 인스턴스화하기위한 팩토리 메소드를 필요로하지 않는다는 것을 의미합니다. extern ()을 사용하여 조건부로 컴파일 할 수 있습니다.

extern ()은 메서드와 필드에도 사용할 수 있습니다.

class Test1 // 메소드에도 extern을 사용할 수 있습니다.
{
    public extern (ANDROID) void SayHi ()
    {
        debug_log "hi from android";
    }
    public extern (! ANDROID) 무효 SayHi ()
    {
        debug_log "hi from notroid";
    }
}
그리고 유형과 마찬가지로 메소드 서명이 동일하더라도 충돌은 발생하지 않습니다.

참고 : 자신의 건전성을 위해 다른 외부 조건에 대해 다른 공용 인터페이스를 사용하지 마십시오. 그것이 작동하는 동안, 그것은 결국 당신을 붙잡기 위해 되돌아 오는 경향이 있습니다.

정의 된 경우 ()

defined ()가 extern ()이 유형, 메소드 및 필드에서 수행 한 명령문에 대해 동일한 작업을 수행하는 경우.

Test2 클래스
{
    공공 무효 SomeMethod ()
    {
        정의 된 경우 (ANDROID)
        {
            debug_log는 "android에서 작동합니다";
        } 정의 된 경우 (IOS) {
            debug_log는 "ios에서 작동합니다";
        } else {
            debug_log "anything else";
        }
    }
}
구문은 정의 된 경우 (CONDITION)가 아니라 (CONDITION) 인 경우에 유의하십시오. 이것이 평가 될 때의 차이를 강조하기위한 디자인입니다.

extern ""

extern ""( "extern string"이라고 발음 함)은 사용자의 uno 코드에서 원래 타겟 언어 인라인으로 표현식을 작성하는 방법입니다. 예 :

public class Test
{
    // extern은 접근 수정 자 앞에 올 수 있음을 주목하라.
    extern (MOBILE) // MOBILE은 (IOS || ANDROID)와 같습니다.
    공공 무효 SomeMethod ()
    {
        extern "Xli :: Console :: WriteLine (\"null deref 예외가 있습니다. ")";
    }

    extern (JAVASCRIPT)
    공공 무효 SomeMethod ()
    {
        extern "Console.log (\"I have have Ï € \ ")";
    }

    extern (! (모바일 || JAVASCRIPT))
    공공 무효 SomeMethod ()
    {
        debug_log "왜 처음에는하지 않습니까?"
    }
}
extern ""을 사용할 때는 올바른 백엔드에서만 사용되도록하십시오. 예를 들어 위의 코드에서 MOBILE 플랫폼은 C ++을 의미합니다.

extern ""은 정말 멋지며 네이티브 코드가 필요한 모든 장소에서 사용해야합니다. 이 경우 우리는 타겟 별 UXL 파일로 돌아갈 것이지만, 나중에 설명하겠다.

extern ""은 값을 반환 할 수 있습니다.

extern (CPLUSPLUS)
public int AddOne ()
{
    var result = extern <int> "10 + 1"; // 'extern' "은 표현식이며 문자열 안에`;`가 필요하지 않음을 주목하라.
    반환 결과;
}
generic-style 유형 지정자는 유효한 Uno 유형 일 수 있습니다.

extern ""도 값을 전달할 수 있습니다.

extern (CPLUSPLUS)
공용 int AddOne (int x)
{
    return extern <int> (x) "$ 0 + 1";
}
$ 0은 폼에 전달되는 첫 번째 인수의 이름을 지정하는 UXL 매크로입니다.

extern ""은 여러 인자를 가질 수 있습니다 :

extern (CPLUSPLUS)
공용 int AddIntAndFloat (int x, float y)
{
    return extern <int> (x, y) "$ 0 + (int) $ 1";
}
$ 0이 첫 번째 인수에 이름을 붙이는 것과 마찬가지로 $ 1은 두 번째 이름을 지정하며, 이는 원하는만큼 많은 인수에 적용됩니다.

extern 문법에서 int를 말할 때, 우리는 C ++ int를 말하는 반면, <int>는 Uno int입니다.

UXL 매크로

위의 $ 0 및 $ 1을 사용하여 첫 번째 UXL 매크로를 보았습니다. 더 많은 것들이 있으며 그 중 일부는 아래에 설명되어 있습니다.

@ {type} 구문을 사용하여 Uno 유형 지정

네이티브 코드 안에 Uno 유형의 변수를 정의 할 수 있습니다. 또는 기본 유형을 Uno 유형으로 변환 할 수도 있습니다.

이 작업은 다음과 같이 수행 할 수 있습니다.

extern (CPLUSPLUS)
공용 int AddIntAndFloat (long x, float y)
{
    extern (x) "@ {long} z = $ 0";
    return extern <long> (x, y) "z + (@ {long}) $ 1";
}
여기서 우리는 extern의 몇 가지 흥미로운 특징을 볼 수 있습니다 :

@ {someUnoType}은 해당 Uno 유형의 기본 버전을 확장합니다.
extern ""의 반환 유형을 무효로 선언 할 수 있습니다. 이것은 암시 적이기 때문에, 이것을 쓰지 않아도됩니다.
extern ""은 컴파일 된 출력 내에서 순서를 유지합니다. 즉, 첫 번째 외부에서 z를 선언하고 두 번째 외부에서 z를 사용할 수 있음을 의미합니다.
가능한 모든 곳에서 @ {type} 구문을 사용해야합니다. 정말 추적하기 어려운 미묘한 버그를 잡을 것입니다. 예를 들어, Uno는 C ++ long long입니다.
@ {type} 구문은 모든 Uno 유형에서 작동합니다. 위의 코드에서 Uno의 long 타입을 사용했다.

우리는 UXL을 사용하여 새로운 Uno 객체를 만드는 방법을 다루지 않았습니다.
원시 형이 반드시 동일한 이름을 가진 원시 형에 매핑되는 것은 아니라는 사실을 잊기 쉽습니다.
비즈니스용 Google 번역:번역사 도구웹사이트 번역기해외 시장 정보

UXL의 정적 필드 참조

우리는 UXL에서 쉽게 유형의 정적 필드에 액세스 할 수 있습니다. 아래에서는 Test0 클래스의 _name 필드에 액세스하는 방법을 보여줍니다.

public class Test0 {
    public static string _name = "Jeff";
}

public class Test1 {
    public string getTest0Name ()
    {
        extern "@ {string} tmp = @ {Test0._name}";
        extern을 반환합니다. <string> "tmp";
    }
}
보시다시피 @ {type} 구문은 클래스에도 적용됩니다.

: Of 매크로를 사용하여 유형의 인스턴스에있는 필드를 참조하십시오.

UXL은 C ++ 변수의 유형을 추론 할 수 없기 때문에 인스턴스 데이터에 액세스해야 할 경우 UXL에 유형을 알려야합니다. 우리는 다음을 사용합니다.

public class Test0 {
    공용 문자열 _instanceName = "Jeff";
}

public class Test1 {
    공용 문자열 getTest0Name (Test0 x)
    {
        extern (x) "@ {Test0} tmp = @ 0";
        extern "@ {string} tmp2 = @ {Test0 : Of (tmp) ._ instanceName}"
        extern을 반환합니다. <string> "tmp2";
    }
}
이것은 다소 복잡한 예이지만 요점을 보여 주어야합니다. 물론, UXL에서 필드의 값이 정말로 필요하다면 Uno에서 필드를 전달해야합니다 :

extern (x._instanceName) ".. 여기 $ 0을 사용하는 일부 코드";
다음과 함께 UXL을 사용하여 정적 메서드 호출 : Call 매크로

public class Test0 {
    public static string AgeJeff (int x) {return "Jeff is"+ x; }
}

public class Test1 {
    public string getTest0Name ()
    {
        return extern <string> "@ {Test0.AgeJeff (int) : Call (253)}";
    }
}
호출 할 메소드를 정확하게 지정하려면 서명을 제공해야합니다 (유형 만 필요함).

그러면 우리는 실제적인 주장을 제시합니다.

UXL을 사용하여 인스턴스 메소드 호출하기

여러분이 짐작할 수 있듯이 다음과 같이 사용합니다 : 필드의 매크로와 같습니다 :

public class Test0 {
    공용 문자열 AgeJeff (int x) {return "Jeff is"+ x; }
}

public class Test1 {
    공용 문자열 getTest0Name (Test0 x)
    {
        return extern <string> (x) "@ {Test0 : Of ($ 0) .AgeJeff (int) : Call (253)}";
    }
}
액세스 한정자에 대한 참고 사항

UXL은 모두 (경고 포함)를 봅니다. 액세스 수정자를 따르지 않습니다. 네이티브가이를 참조 할 수 있다면 그렇게 할 수 있습니다. 분명히 반대도 마찬가지이므로 C #으로 컴파일 할 때 모든 것을 액세스 할 수 없을 가능성이 높습니다.

다음을 사용하여 UXL에서 클래스의 새 인스턴스를 만듭니다. 새 매크로

클래스의 새 인스턴스를 만드는 것은 다음과 매우 유사합니다. 호출. 구문은 다음과 같습니다.

@ {Test2} someVar = @ {Test2 (int, float) : New (1,1.2f)}

어디에:

Test2는 유형입니다.
(int, float)는 생성자 중 하나의 유형 시그니처이며
(1,1.2f)는 생성자에 대한 인수입니다.
늘 그렇듯이, 가능한 경우 Uno에서 new를 사용하여 다음과 같이 extern 인수로 UXL에 전달해야합니다.

extern (new Test2 (1,1.2f)) ".. 일부 원시 코드"

다음을 사용하여 Uno Type 객체를 가져옵니다. TypeOf 매크로

@ {Android.android.accessibilityservice.AccessibilityService : TypeOf}

다음과 동등합니다.

typeof (Android.android.accessibilityservice.AccessibilityService)

Get 및 Set 매크로를 사용하여 속성 가져 오기 또는 설정

UXL을 사용하여 속성 값을 얻는 방법은 다음과 같습니다.

@ {Uno.Exception : Of ($ 0) .Message : Get ()}

세트는 매우 유사합니다.

@ {Uno.Exception : Of ($ 0) .Message : Set ($ 1)}

그리고 더...

더 많은 매크로가 있지만 UXL 파일에 대한 소개가 먼저 필요하다고 생각합니다.

UXL 파일 및 [TargetSpecificImplementation] 속성

extern ""구문은 Uno를위한 비교적 새로운 기능입니다. 이 전에는 모든 원시 코드를 별도의 파일에 정의해야했습니다. 지금도 이러한 파일이 필요한 경우가 많이 있습니다.

주어진 유형이 네이티브 구현을 가지고 있다는 것을 Uno에게 알리기 위해 우리는 [TargetSpecificImplementation] 속성을 사용합니다.

우리는 네이티브 코드로 구현하고자하는 메소드에 대해서도 속성을 사용합니다.

다음은 예제 클래스입니다.

[TargetSpecificImplementation]
Test3 클래스
{
    [TargetSpecificImplementation]
    public extern float FirstMethod (int a);

    [TargetSpecificImplementation]
    public extern float SecondMethod (int a) // SecondMethod에 기본 구현이 있습니다.
    {
        return 0f;
    }

    공공 정적 부동 소수점 도우미 (int a)
    {
        return (float) a * a;
    }

    public float NonStaticHelper (int a)
    {
        return (float) a * a;
    }
}
여기에서 알아야 할 몇 가지 사항이 있습니다.

네이티브 구현이 필요한 각 메서드에는 extern 한정자가 있습니다.
본문없이 추상 스타일 정의를 사용하거나 기본 구현을 제공 할 수 있습니다.
FirstMethod와 SecondMethod의 차이점은 FirstMethod가 모든 백엔드에 구현되어야한다는 것입니다. 반면에 SecondMethod가 특정 백엔드에 대해 구현되지 않으면 정의 된 구현에 기본값이됩니다.

이러한 TargetSpecific 메서드를 구현하려면 UXL 파일을 만들어야합니다.

UXL 파일

UXL 파일은 TargetSpecific 메소드 및 기타 다양한 조작 및 메타 데이터의 구현을 보유하는 XML 문서입니다.

전통적으로, 우리는 컴파일 될 언어에 특정한 별도의 UXL 파일을 만들었습니다.

C ++의 경우 someName.cpp.uxl 파일을 만들 것입니다.
JavaScript의 경우 someName.js.uxl 파일을 만듭니다.
CIL의 경우 someName.cil.uxl 파일을 만듭니다.
컴파일러는이를 강제하지 않습니다.

위에서 보았던 Test3 클래스의 UXL 파일은 다음과 같습니다.

<Extensions Backend = "CPlusPlus">
    <유형 이름 = "Test3">
        <메서드 서명 = "FirstMethod (int) : float">
            <Expression> ((@ {float}) $ 0) </ Expression>
        </ Method>

        <메소드 서명 = "SecondMethod (int) : float">
            <Require Entity = "Test3.NonStaticHelper (int)"/>
            <Body> <! [CDATA [
                @ {float} tmp = @ {Test3.Helper (int) : Call ($ 0)};
                return tmp + @ {Test3 : Of ($$). NonStaticHelper (int) : Call ($ 0)};
            ]]> </ Body>
        </ Method>
    </ Type>
</ Extensions>

여기서 우리는 인식 할 수있는 매크로와 그렇지 않은 매크로를 볼 수 있습니다.

$$ 매크로는 인스턴스의이 변수입니다.

확장 태그는 내용이 유효한 백엔드를 지정합니다.

Type 태그는 구현중인 Uno 유형을 지정합니다. 이 유형에는 [TargetSpecificImplementation] 속성이 있어야합니다.

그런 다음 메소드 구현이 있습니다. 서명은 : Call 매크로에서 사용 된 것과 거의 동일하지만 여기서는 콜론 다음에 반환 유형을 지정해야합니다 (단 반환 유형이있는 경우에만 해당).

FirstMethod의 구현은 Expression 태그 내에 있습니다. 이것은 코드가 인라인 될 것이므로 extern ""처럼 세미콜론이 필요 없다는 것을 의미합니다.

SecondMethod의 구현은 Body 태그 내부에 있습니다. 즉, 코드가 callsite에 인라인되지 않고 대신 C ++의 메서드 본문이됩니다. 또한, 몸 안에 여러 줄을 가질 수 있습니다. CDATA의 사용은 필수는 아니지만 많은 문자를 벗어날 필요가 없으며 일반적으로 프로그래머로서의 삶을 편하게 만듭니다.

SecondMethod에서는 새로운 엔티티 태그 인 Require Entity 태그를 볼 수 있습니다. 이것은 Uno가 코드를 제거하는 방법에 관해 간단한 간략한 설명이 필요합니다.

코드 스트리핑 및 UXL

Uno는 프로그램의 일부분에서 사용되지 않는 모든 것을 제거하려고합니다. 이렇게하기 위해 컴파일러는 우노 (Uno) 코드를 따라 가며 '터치 된'모든 것에 대한 참조를 추가합니다. 도보의 끝에서 메소드, 필드 또는 유형의 참조 수가 0이면 제거됩니다.

참조가 UXL에서 사용되는 경우가 아니라 Uno에서 무언가가 사용되는 경우에만 추가된다는 점에 유의하십시오. 즉, '엔터티'가 제거되지 않도록하려면 해당 엔터티에 대한 참조를 추가해야합니다. 우리는 Require Entity 태그를 사용하여이 작업을 수행합니다.

<Require Entity = "Test3.NonStaticHelper (int)"/> 메소드에 대한 참조를 추가합니다.
<Require Entity = "Test3._someField"/>는 필드에 대한 참조를 추가합니다.
<Require Entity = "Test3"/>은 형식에 대한 참조를 추가합니다.
우리가 이것을 요구하는 곳은 그것이 효과를 발휘할 때 영향을 미치기 때문에 중요하다.

Method 블록 안에 넣으면 메서드가 제거되지 않으면 참조가 추가됩니다.
Type 블록 안에 넣으면 해당 유형이 제거되지 않으면 참조가 추가됩니다.
Extensions 블록 안에 넣으면 항상 참조를 추가합니다.
코드 스트리핑은 Uno의 강점 중 하나이므로 참조를 추가 할 위치를 선택할 때 가능한 한 재 시도하십시오. 대략적인 규칙 : 방법> 유형> 확장.

이제 스누커에게 돌아가 ...

더 많은 UXL 태그

생성 된 네이티브 코드의 파일 확장명을 설정합니다.

UXL에서 Objective C를 사용하는 경우 파일 확장자가 .cpp가 아니라 .mm가되도록해야합니다. 그렇지 않으면 컴파일 및 컴파일시 Xcode에서 매우 기괴한 포함 오류가 발생합니다.

<Set Source.FileExtension = "mm"/>
UXL 사용에 해당하는 것

정규화 된 유형을 작성하면 지루할 수 있습니다. Using 태그는 동일한 작업을 수행합니다. Uno는 using 문을 사용합니다.

<Namespace = "iOS.Foundation"/> 사용
추가는 Require 태그와 : Include 매크로를 사용하여 포함합니다.

UXL에서는 다른 Uno 유형을 참조 할 수 있습니다 (예 : 메소드 중 하나를 호출하여). 그러나 일부 대상의 경우 사용하기 전에 해당 유형의 헤더 파일을 포함시켜야합니다. require 태그를 사용하여이 작업을 수행합니다.

<Require Source.Include = "@ {Android.Base.AndroidBindingMacros : Include}"/>
<Require Header.Include = "@ {Android.Base.AndroidBindingMacros : Include}"/>
위에서 우리는 : Include 매크로를 사용하여 우리가 필요로하는 .h 파일의 경로를 얻는다.

Source.Include를 사용하여 #include가 해당 형식의 .cpp 파일에 추가됩니다.

Header.Include를 사용하여 #include가 해당 형식의 .h 파일에 추가됩니다.

가능한 경우 Source.Include와 Header.Include를 사용해야합니다.

경로를 명시 적으로 제공하여 일반 C / C ++ 헤더 파일을 포함 할 수도 있습니다.

<Require Header.Include = "Uno / Platform2.h"/>
<Require Header.Include = "Uno / Platform2.h"/>
Require Source.Declaration 태그를 사용하여 원시 파일에 임의의 소스 추가

이 기능은 완전성을 위해 문서화되어 있으며 아주 가끔씩 사용하는 것이 옳다. 그러나 사용하는 경우에는 다른 옵션이 없기 때문에 사용해야한다.

    <Source.Declaration 필요> <! [CDATA [
        void JNICALL 안드로이드 NativeActivityHelper_onActionModeFinished (JNIEnv * env, jclass clazz, jobject arg)
        {
            uAutoReleasePool 풀;
            @ {Activity.OnActionModeFinished : Call (arg)};
        }
    ]]> </ Require>
Source.Declaration은 네이티브 코드 블록을 해당 형식의 컴파일 된 C ++ 코드 맨 위에 추가합니다. 포함 이후이지만 네임 스페이스가 열리기 전입니다.

Header.Declaration은 .h 파일에도 동일하게 적용됩니다.

단일 클래스에 여러 개의 Source.Declaration 블록을 추가하면 최종 C ++에서 순서가 무엇인지 보장 할 필요가 없습니다.

조건을 사용하는 UXL 조건 =

UXL은 extern (CONDITION)과 같습니다.

<Entity = "Uno.Platform2.Display.OnTick (Uno.Platform2.TimerEventArgs)"조건 = "MOBILE"/>
예를 들어 여기에서는 모바일 장치 용으로 컴파일하는 경우 OnTick 메서드에 대한 참조 만 추가합니다.

이는 ANDROID 및 iOS (예 :)에 대해 서로 다른 구현을 제공 할 수 있도록 Type 및 Method 태그에서도 사용할 수 있습니다.

ProcessFile 태그를 사용하여 임의 유형의 매크로 확장

비 UXL 블록에서 일부 UXL을 확장하려는 경우가 있습니다. readme 파일에 프로젝트 버전 = @ (Project.Version)을 넣는 것만 큼 간단 할 수도 있고 특정 Uno 메서드가 제거 된 경우 Java 메서드의 동작을 변경하는 것과 같이 좀 더 복잡 할 수도 있습니다.

UXL 프로세서에서 파일을 처리하게하려면 다음 구문을 사용합니다.

<ProcessFile HeaderFile = "Uno / Platform2.h"/> 네이티브 헤더 파일 용
<ProcessFile SourceFile = "Uno / Platform2.h"/> 네이티브 소스 파일 용
다른 종류의 파일의 경우 <ProcessFile Name = "Camera.java"TargetName = "src / com / fuse / Native / Camera.java"/>
마지막 경우에는 프로젝트의 출력 빌드 디렉토리에 파일을 저장할 위치를 정의해야합니다.

CopyFile 태그를 사용하여 매크로를 확장하지 않고 파일 복사

이 파일은 ProcessFile과 비슷하게 파일을 빌드 출력으로 복사하지만 UXL 매크로를 구문 분석 할 필요가 없으므로 훨씬 빠릅니다. 가능할 때마다 이것을 사용하십시오.

<CopyFile Name = "Camera.java"TargetName = "src / com / fuse / Native / Camera.java"/>

타겟 유형

이름이 TargetSpecificImplementation과 매우 비슷하지만 TargetSpecificType의 목적은 매우 다릅니다.

TargetSpecificType을 사용하면 Uno에서 네이티브 유형을 유형에 관계없이 전달할 수 있습니다. Uno 메쏘드는 그 타입에서 작동하며, 일반적으로 편리한 방식으로 사용합니다. 최종 형식 정의가 래핑 할 네이티브 형식의 오버 헤드가 아니므로 추가 오버 헤드가 발생하지 않습니다.

다음은 TargetSpecificType의 가장 간단한 가능한 예입니다.

[TargetSpecificType]
public extern (ANDROID) 구조체 UJObject {}

<유형 이름 = "UJObject"조건 = "ANDROID"
     TypeName = "jobject"
     포함 = "jni.h"/>
여기서 우리는 JNI jobject 유형을 나타내는 Uno 유형을 정의합니다. 알아 둘 사항 :

TargetSpecificTypes는 구조체 여야합니다. 우리는 그 (것)들을 종류 인 허용했다, 그러나 이것은 비난됩니다.
TargetSpecificTypes는 Uno에서 아무 문제없이 박스로 묶을 수 있습니다.
UXL에 정의 된 Include는 TargetSpecificType을 사용하는 모든 파일에 추가됩니다.
구조체는 값 유형이므로 Uno의 null과 비교할 수 없습니다.
TargetSpecificType에 Uno 메소드 정의하기

이것은 가능한 한 많은 논리를 Uno로 옮기는 정말 좋은 방법입니다. 다음은 이러한 유형 중 하나의 간단한 예제입니다.

```
[TargetSpecificType]
public extern(ANDROID) struct AndroidNativeView
{
    public static AndroidNativeView Null
    {
        get { return extern<AndroidNativeView> "NULL"; }
    }

    public static bool operator ! (AndroidNativeView handle)
    {
        return extern<bool> "!$0";
    }

    public static implicit operator bool (AndroidNativeView handle)
    {
        return extern<bool> "$0";
    }

    public static bool operator == (
        AndroidNativeView lhs, AndroidNativeView rhs)
    {
        return IsSameView(lhs, rhs);
    }

    public static bool operator != (
        AndroidNativeView lhs, AndroidNativeView rhs)
    {
        return !(lhs == rhs);
    }

    public override bool Equals(object obj)
    {
        return obj is AndroidNativeView && (AndroidNativeView)obj == this;
    }

    [TargetSpecificImplementation]
    public override extern int GetHashCode();

    [TargetSpecificImplementation]
    private static bool IsSameView(AndroidNativeView handle1, AndroidNativeView handle2)
    {
        return extern<bool> "$0 == $1";
    }
}
```
```
<Type Name="AndroidNativeView" Condition="Android"
    TypeName="jobject"
    Include="jni.h">

    <Method Signature="GetHashCode():int">
        <Require Source.Include="Xli/Traits.h" />
        <Expression>::Xli::DefaultTraits::Hash($$)</Expression>
    </Method>
    <Method Signature="IsSameView(AndroidNativeView,AndroidNativeView):bool">
        <Body>
            JniHelper jni;
            jni->IsSameObject($0, $1);
        </Body>
    </Method>
</Type>
```

UXL에서 사용할 수있는 값은 무엇입니까?

이 섹션은 곧 제공 될 예정입니다.

템플릿

템플릿은 다른 UXL에서 사용할 수있는 하나의 이름으로 UXL 태그를 그룹화하는 방법입니다. 예를 들어 두 클래스 중 하나를 사용하는 경우 많은 메소드에 대한 참조를 추가해야 할 수도 있습니다. 다음과 같이 작성할 수 있습니다.

<템플릿 이름 = "예">
    <Require Entity = "Uno.Platform2.Application.Start ()"/>
    <엔티티 필요 = ""Uno.Platform2.Application.EnterForeground () "/>
    <Require Entity = "Uno.Platform2.Application.EnterInteractive ()"/>
    <Require Entity = "Uno.Platform2.Application.ExitInteractive ()"/>
    <엔티티 필요 = ""Uno.Platform2.Application.EnterBackground () "/>
    <Require Entity = "Uno.Platform2.Application.Terminate ()"/>
    <Require Entity = "Uno.Platform2.Application.OnReceivedLowMemoryWarning ()"/>
</ Template>
그런 다음 간단히 추가하십시오.

<Require Template = "예"/>
...이 방법을 필요로하는 두 클래스에.

템플릿에 넣을 수있는 유효한 UXL 태그는 다음과 같습니다.

<CopyFile>
<ProcessFile>
<Require>
일반적인 문제

C ++로 컴파일 할 때 UXL에서 Objective C를 사용하여 많은 미친 오류가 발생했습니다.

다음은 빌드 로그입니다.

```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSDictionary.h:5:
In file included from /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObject.h:7:
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObjCRuntime.h:400:1: error: expected unqualified-id
@class NSString, Protocol;
^
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObjCRuntime.h:413:53: error: format argument not an NSString
FOUNDATION_EXPORT void NSLog(NSString *format, ...) NS_FORMAT_FUNCTION(1,2);
                            ~~~~~~~~~~~~~~~~       ^                  ~
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObjCRuntime.h:98:49: note: expanded from macro 'NS_FORMAT_FUNCTION'
       #define NS_FORMAT_FUNCTION(F,A) __attribute__((format(__NSString__, F, A)))
                                                      ^
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObjCRuntime.h:414:63: error: format argument not an NSString
FOUNDATION_EXPORT void NSLogv(NSString *format, va_list args) NS_FORMAT_FUNCTION(1,0);
                             ~~~~~~~~~~~~~~~~                ^                  ~
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObjCRuntime.h:98:49: note: expanded from macro 'NS_FORMAT_FUNCTION'
       #define NS_FORMAT_FUNCTION(F,A) __attribute__((format(__NSString__, F, A)))
                                                      ^
In file included from /Users/erik/src/fuselibs/Tests/NativeTextPrototype/.build/iOS-Debug/src/app/-.CanvasTexture.cpp:8:
In file included from include/iOSCanvasTexture.h:5:
In file included from /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSDictionary.h:5:
In file included from /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSObject.h:8:
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS8.4.sdk/System/Library/Frameworks/Foundation.framework/Headers/NSZone.h:8:1: error: expected unqualified-id
@class NSString;
```

대답:

'일반'C ++로 컴파일되는 파일에서 Objective C를 사용하고 있습니다. 이 작업을하려면 파일 확장명이 .mm이어야합니다. UXL 파일에 다음을 추가하십시오.

<Set Source.FileExtension = "mm"/>
즉, 작은 UXL 파일을 작성해야하는 인라인 extern 표현식에있는 경우 ""입니다.

이후 버전에서는 Uno 기반의 방법으로이를 수행 할 것입니다.

우노는 확실히 거기에 있어도 내 메소드를 찾을 수 없으며 완전한 유형을 부여한다고해도 작동하지 않습니다.

대답:

Uno 네임 스페이스 해상도가 사용자와 관련이있을 수 있습니다. Uno는 C #과 같은 네임 스페이스를 해석하므로이 사항이 적용됩니다.

TLDR : 정규화 된 형식의 시작 부분에 global :: 고정하십시오.

이러한 모든 경로를 작성하는 것은 매우 지루합니다. 사용하는 것과 동등한가?

대답:

예 :)

<Namespace = "iOS.Foundation"/> 사용
