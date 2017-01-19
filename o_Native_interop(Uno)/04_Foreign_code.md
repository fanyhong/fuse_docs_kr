원본: https://www.fusetools.com/docs/native-interop/foreign-code

외부 코드로 작업하기

우리가 퓨즈에 넣을 수있는만큼의 기본 크로스 플랫폼 기능을 포장하려고 시도하지만 자연스럽게 아직 구현되지 않은 기능이 될 것입니다. 이 가이드는 Uno에서 외부 코드 기능을 사용하여 타겟 플랫폼의 기본 기능에 접근하는 방법을 설명합니다.

참고 :이 기능은 언어 바인딩을 통해 interop을 수행하는 기존 방식을 대체합니다. 이렇게하는 이유에 대한 자세한 내용은 해당 주제에 대한 독립 실행 형 게시물을 작성했습니다.

외국 코드 란 무엇입니까?

외국 코드를 ​​사용하면 Uno 메서드 본문을 다른 언어로 작성할 수 있습니다. 다음 예제를 고려하십시오.

Uno.Compiler.ExportTargetInterop;

클래스 예제
{
    [Foreign (Language.Java)]
    public static extern (Android) void 로그 (문자열 메시지)
    @ {
        android.util.Log.d ( "ForeignCodeExample", message);
    @}

    [Foreign (Language.ObjC)]
    public static extern (iOS) void 로그 (문자열 메시지)
    @ {
        NSLog (@ "% @", message);
    @}
}
여기서는 Java와 Objective-C의 로깅 기능을 공개하여 Uno에서 사용할 수 있도록했습니다. Uno 메서드를 Foreign 특성으로 태그 지정하면 특성에서 선택한 언어의 @ {& @} 중괄호 사이에 원하는 코드를 간단하게 작성할 수 있습니다.

Uno 컴파일러는 Uno로 코드를 읽으려고하지 않으므로 외부 메서드 본문에 특별한 중괄호를 사용합니다.

Google은 현재 [Foreign (Language.ObjC)] 및 [Foreign (Language.Java)] 속성을 사용하여 Objective-C (iOS) 및 Java (Android)로 작성된 외부 코드를 지원합니다.

Foreign 특성은 Uno.Compiler.ExportTargetInterop 네임 스페이스에 있으며 Uno 파일의 맨 위에 사용하여 가져올 수 있습니다.

동일한 클래스에 동일한 이름과 서명을 가진 두 개의 메소드가 있음에 유의하십시오. extern (...)은 컴파일 초기 단계에서 대상 플랫폼과 일치하지 않는 코드를 제거하기 때문에 허용됩니다.

유형 변환

외부 코드에서 인수와 반환 값은 Uno 표현과 해당 Java / Objective-C 표현 사이에서 자동으로 변환됩니다.

기본 유형

기본 형식 (int, float, char 등)은 Java에 원시 부호없는 바이트가 없다는 예외를 제외하고는 해당하는 외부 항목으로 직접 변환되므로 Java의 바이트는 Uno에서 sbyte입니다.

그래서 우리는 이것을 쓸 수 있습니다 :

[Foreign (Language.Java)]
public static extern (안드로이드) void Foo (double x, long y)
@ {
    android.util.Log.d ( "ForeignCodeExample", "이것 좀 봐 :"+ x + "and"+ y);
@}
그리고 우노에서 이것을 다음과 같이 부르십시오.

Foo (1.2, 12345678);
문자열

문자열도 자동으로 처리됩니다. 우노 문자열 매핑 대상 :

Java의 java.lang.String
Objective-C의 NSString *
소개 예제를 다시 사용하여 다음을 작성할 수 있습니다.

[Foreign (Language.ObjC)]
public static extern (iOS) void 로그 (문자열 메시지)
@ {
    NSLog (@ "% @", message);
@}
그리고 우노에서 이것을 다음과 같이 부르십시오.

로그 ( "모든 유형의 마법!");
우노에 전달 된 이물

그렇다면 외부 메소드에서 Java 또는 Objective-C 객체를 반환하려는 경우 어떻게됩니까? 대답은 우리가 이물질을 나타내는 불투명 한 객체를 얻는다는 것입니다.

Objective-C 오브젝트 (id)는 ObjC.Object이고 Java 오브젝트 (Object)는 Uno의 Java.Object입니다.

Java에서 객체를 가져 오는 방법은 다음과 같습니다.

[Foreign (Language.Java)]
public static extern (Android) Java.Object 테스트 (int texName)
@ {
    새로운 android.graphics를 반환합니다. SurfaceTexture (texName);
@}
그리고 그것을 부른다 :

var stex = Test (1);
우리는 더 많은 유형에 대한 특별한 경우 지원을하지 못하는 이유를 궁금해 할 수 있습니다. 자동 변환을 편리하게 사용할 수있는 몇 가지 유형을 생각하는 것이 어렵지 않습니다. 이에 대한 답은 앞에서 언급 한 매체 기사에서 찾을 수 있습니다. 요약:

Java 및 Uno와 구문 학적으로 유사한 언어에서도 일반적인 경우의 객체 모델 매핑은 작동하지 않습니다. 번역에 필요한 차이점은 외부 코드를 사용하는 프로그래머에게 정신적 오버 헤드입니다. 대신 우리는 예측 가능한 인터페이스를 제공하고 외부 객체의 내부 작업은 외부 코드로 수행 할 것을 요구합니다.
그리고 다른 방법은? (Uno 객체가 외부 코드로 전달됨)

우리는 같은 접근법을 취합니다. Java 또는 Objective-C로 전달 된 Uno는 불투명 한 객체 내부에 박스로 표시됩니다.

Objective-C에서 해당 상자의 유형은 id <UnoObject>이고 Java에서는 com.uno.UnoObject입니다.

다음은 전달되는 Uno 객체의 예입니다.

[Foreign (Language.Java)]
공공 정적 extern (안드로이드) void Test (SomeFancyType soFancy)
@ {
    // 여기에서 soFancy의 타입은`UnoObject`입니다.
    android.util.Log.d ( "ForeignCodeExample", "여기 있습니다 :"+ soFancy);
@}
이 메서드를 호출하는 것은 간단합니다.

var v = Test (새로운 SomeFancyType (1, 2, 3));
Java.Objects를 Java 및 ObjC.Object로 다시 전달하여 Objective-C로 되돌림

우리는 이물질을 다시 꺼내므로 상자 상자에 대해 걱정할 필요가 없습니다.

그냥 명확하게 :

외부 Objective-C 메소드로 전달 된 ObjC.Object는 ID가됩니다.
외부 Java 메소드에 전달 된 Java.Object가 Object가됩니다.
이 작업이 끝나면 객체를 다시 원래 유형으로 캐스팅 할 수 있습니다.

배열

Uno 배열을 외부 코드에 전달하는 특별한 지원이 있습니다. 성능상의 이유로 우리는 즉시 데이터의 사본을 만들지 않습니다. 대신 우리는 외부 코드에서 사용할 수있는 Uno 배열에 핸들을 전달합니다. 데이터의 전체 복사본이 필요한 경우 arr.copyArray () (Java) 또는 [arr copyArray] (Objective-C)를 호출하여 해당 유형의 기본 버전을 가져옵니다. 전체 내용은 다음과 같습니다.

목표 -C :

배열은 Uno 배열을 감싸는 래퍼 인 id <UnoArray> 유형의 객체로 변환됩니다. 익숙한 arr [i] 구문을 사용하여 색인을 생성하고 업데이트 할 수 있으며 NSUInteger를 반환하는 count 메서드 ([arr count]라고 함)가 있습니다. 원래의 Uno 배열에 반영된 배열을 업데이트합니다. 이는 복사본이 아니라 래퍼입니다. 앞서 언급했듯이 [xs copyArray]를 호출하여 id <UnoArray>를 NSArray *로 복사 할 수도 있습니다.

Objective-C는 generics가 없으므로 id <UnoArray> 객체를 인덱싱하여 요소를 얻으면 Uno 측 배열의 요소 유형에 관계없이 id를 반환합니다. 이 ID는 다음 표에 따라 요소 유형의 박스형 표현입니다.


Uno Objective-C 박스형 배열 요소
int, bool, char 등 int, bool, char NSNumber *
문자열 NSString * NSString *
ObjC.Object id id
개체 ID <UnoObject> id <UnoObject>
Func <string, int> 등 ^ int (NSString *) ^ int (NSString *)
SomeType [] id <UnoArray> id <UnoArray>
대부분의 유형은 이미 boxed되어 있지만 int, bool 및 char와 같은 기본 유형은 줄 바꿈 된 배열에서 액세스 할 때 NSNumber *로 상자가 지정됩니다. 즉, Objective-C 측에서 Uno 배열 인수 int [] x를 업데이트하려면 예를 들어 다음과 같이 작성해야합니다. x [index] = @ 42 ;. 배열을 복사 할 때 결과 NSArray *의 요소는 동일한 규칙에 따라 boxed됩니다.

UXL 매크로를 사용하여 복싱 동작을 피할 수 있습니다. 다음 예제는 외부 Objective-C 코드에서 배열을 사용하는 두 가지 방법을 대조합니다.

[Foreign (Language.ObjC)]
공용 정적 extern (iOS) void ForeignIntArray (int [] xs)
@ {
    @ {int [] : Of (xs) .Set (3, 123)};
    for (int i = 0; i <@ {int [] : Of (xs) .Length : Get ()}; ++ i)
    {
        NSLog (@ "array [% d] = % d", i, @ {int [] : Of (xs) .Get (i)});
    }
@}
[Foreign (Language.ObjC)]
공용 정적 extern (iOS) void ForeignIntArray (int [] xs)
@ {
    xs [3] = @ 123;
    for (int i = 0; i <[xs count]; ++ i)
    {
        NSLog (@ "배열 [% d] = % @", i, xs [i]);
    }
@}

자바:

외국의 Objective-C와 마찬가지로, 배열을 내용을 복사하는 대신 박스형 Uno 객체로 전달합니다. 그런 다음 .copyArray ()를 호출하여 원시 배열 (데이터가 복사되는 시점)을 가져올 수 있습니다.

변환은 다음과 같이 보입니다 (참고 : Java는 기본 유형의 제네릭을 허용하지 않습니다).

Uno Type 박스형 자바 타입 Unboxed 자바 타입
bool [] com.uno.BoolArray bool []
sbyte [] com.uno.ByteArray byte []
char [] com.uno.CharArray char []
짧은 [] com.uno.ShortArray 짧은 []
int [] com.uno.IntArray int []
long [] com.uno.LongArray long []
float [] com.uno.FloatArray float []
double [] com.uno.DoubleArray double []
string [] com.uno.StringArray String []
anyOtherType [] com.uno.ObjectArray com.uno.UnoObject []
x.get (0)을 사용하여 int 배열에서 firstint를 가져옵니다. Intx의 첫 번째 요소를 10으로 설정하려면 Usex.set (0, 10)을 사용하십시오.

대표자들

외국 코드를 ​​사용하면 대리자를 외부 메서드에 전달할 수도 있습니다. Java와 Objective-C 사이의 세부 사항은 약간 다릅니다.

목표 -C :

대리자는 해당 유형의 Objective-C 블록으로 변환됩니다. 예를 들어, Action <string, int> 유형의 인수는 ^ void (NSString *, int) 유형의 블록이됩니다. 블록의 인수 및 반환 형식은 일반적으로 외부 함수에 대한 인수가 사용하는 것과 동일한 형식 변환을 사용합니다.

다음은 간단한 예입니다.

[Foreign (Language.ObjC)]
공용 static extern (iOS) double DelegateArgument (Func <int, double> f)
@ {
     // f는 여기서`^ double (int)`형이므로 모든 함수처럼 호출 될 수 있습니다
     return f (12);
@}
Uno가 ObjC를 Apple의 BOOL과 반대로 bool로 상자에 넣었다는 사실은 주목할 가치가 있습니다. 이것은 bool 값을 사용하여 Uno-created 블록을 관리 할 때 특정 의미를 갖습니다. 예를 들어 블록 void (^) (BOOL)은 Action <bool>을 통해 생성 된 void (^) (bool)과 같지 않으며 동일한 속성에 저장할 수 없습니다.

자바:

다음과 같이 델리게이트를 정의하면 :

네임 스페이스 Foo
{
    공용 대리자 void Bar (float x, float y, float z);
}
다음과 같이 외부 Java에서 사용할 수 있습니다.

[Foreign (Language.Java)]
public static extern (안드로이드) void ForeignDelegate (바 x)
@ {
    x.run (1.0f, 2.0f, 3.0f);
@}
버전 8까지는 Java가 lambdas를 가지지 않았고 Runnable과 Callables는 인수를 취하지 않으므로 Uno 컴파일러는 배후에서 public void run (float x, float y)을 사용하여 com.foreign.Foo.Bar라는 Java 클래스를 작성합니다 , float z) {...} 메소드.

프리미티브, 문자열, 객체 및 배열에 대한 외부 Java 유형 변환은 대리자의 인수에 적용됩니다.

Uno 액션을 Java에 전달할 수도 있습니다. Java 대리자는 프리미티브를 지원하지 않으므로이 클래스는 Wellk 클래스로 생성됩니다.

유형 변환은 다음 패턴을 따릅니다.

액션 -> com.foreign.Uno.Action
액션 <int> -> com.foreign.Uno.Action_int
액션 <int [], int> -> com.foreign.Uno.Action_IntArray_int
출력 / 참조 매개 변수

Out 및 ref 매개 변수는 외부 Objective-C 메소드에서 지원됩니다. 이러한 매개 변수의 Objective-C 유형은 Objective-C / Uno 유형 변환 규칙에 따라 매개 변수의 Objective-C 유형에 대한 포인터입니다.

다음 두 예제는 작동 방식을 보여줍니다.

[Foreign (Language.ObjC)]
extern (iOS) void PrimitiveOutParam (ref int m, 출력 int n)
@ {
    // m과 n은`int *`형이다.
    * m = 222;
    * n = 123;
@}

[Foreign (Language.ObjC)]
extern (iOS) void StringOutParam (참조 문자열 m, 출력 문자열 n)
@ {
    // m과 n의 타입은 NSString **입니다.
    * m = @ "Out1";
    * n = @ "Out2";
@}
Java는 out / ref 파라미터를 가지지 않기 때문에 우리는 외부 자바에서 이것을 지원하지 않습니다.

우노에게 말하기

외국 코드에서 Uno로 값을 반환 할 수있는 것은 아닙니다. 때로는 외부 코드에서 Uno와 상호 작용하는 것이 더 합리적입니다. 이를 위해 우리는 UXL (Uno Extension Layer) 매크로를 사용합니다.

UXL을 사용하여 우리는 외국 코드에서 두 가지 주요한 일을 할 수 있습니다 :

Uno 필드 액세스
Uno 메소드 호출
UXL에 대한 자세한 내용은 UXL 핸드북을 확인하십시오. 이 가이드에서는 몇 가지 작은 예를 보여줍니다.

정적 Uno 필드 가져 오기 및 설정

Get 및 Set 매크로를 사용하여 속성 값을 가져 오거나 설정할 수 있습니다.

UXL Get 표현식의 해부학은 다음과 같습니다.

`@ {``}`문법은 그것의 UXL 매크로를 의미합니다
@ {Foo : Get ()}
   ~ ^
 필드 이름
그리고 Set 표현식은 다음과 같이 보입니다 :

          이것은 외래 값이다.
@ {Foo : Set (20)}
   ~ ^
 필드 이름
예제 코드에서 이것을 보자 :

Uno.Compiler.ExportTargetInterop;

public class Example
{
    공공 정적 intValue = 7;

    [Foreign (Language.Java)]
    public extern (Android) void Doubler ()
    @ {
        int originalVal = @ {SomeValue : Get});
        @ {SomeValue : Set (originalVal * 2)};
    @}

    [Foreign (Language.ObjC)]
    public extern (iOS) void Doubler ()
    @ {
        int originalVal = @ {SomeValue : Get});
        @ {SomeValue : Set (originalVal * 2)};
    @}
}
Uno 인스턴스 필드 가져 오기 및 설정

인스턴스 메소드를 호출하려면 this 포인터가 있어야합니다. 대상 언어의 고유 한 의미와 충돌하지 않기 위해 외부 인스턴스 메소드는 호출 된 객체를 참조하는 UninObject 또는 id <UnoObject>로 래핑 된 _this에 대한 액세스 권한을 갖습니다.

외부 코드의 표현식 유형은 일반적으로 Uno 컴파일러에서 자동으로 추론 할 수 없으므로 다음을 사용합니다. Of 매크로는 _this와 상호 작용합니다.

@ {MyClass : Of (_this)}
위 코드는 "Uno 유형의 _이 MyClass입니다"라고 말합니다.

다음 코드는 작동 방식을 보여줍니다.

Uno.Compiler.ExportTargetInterop;

public class Example
{
    공용 문자열 MyString = "이것은 모두 기본입니다";

    [Foreign (Language.Java)]
    public extern (Android) 무효 AddExcitement ()
    @ {
        String originalString = @ {예 : Of (_this) .MyString : Get ()};
        문자열 newString = originalString + "!!!";
        @ {예 : Of (_this) .MyString : Set (newString)};
    @}

    [Foreign (Language.ObjC)]
    public extern (iOS) 무효 AddExcitement ()
    @ {
        NSString * originalString = @ {예 : Of (_this) .MyString : Get ()};
        NSString * newString = [originalString stringByAppendingString : @ "!!!"];
        @ {예 : Of (_this) .MyString : Set (newString)};
    @}
}

정적 Uno 메서드 호출

UXL의 Call 매크로는 Uno 메소드 (및 다른 외부 메소드)를 외부 메소드 내부에서 호출 할 수있게 해줍니다.

약간의 해부학으로 시작합시다.

 Uno 메소드 이름 인수로 전달 될 외부 값
      v v v
@ {Foobernator (int, string) : Call (1, "jam")}
               ^ ^
     인수의 Uno 유형
        방법의 'Foobernator'
이것을 보자.

Uno.Compiler.ExportTargetInterop;

public class Example
{
    public static void Angrify (문자열 str)
    {
        debug_log "ARGHHH!"+ str + "ARGGHHH!";
    }

    [Foreign (Language.Java)]
    공개 extern (안드로이드) void Rage ()
    @ {
        @ {Angrify (문자열) : Call ( "JAVA!")};
    @}

    [Foreign (Language.ObjC)]
    공개 extern (iOS) void Rage ()
    @ {
        @ {Angrify (string) : (@ "OBJECTIVE-C!")};를 호출합니다. // : p
    @}
}
Uno 인스턴스 메소드 호출하기

Uno 인스턴스 메소드를 호출하기 위해 Of 매크로를 다시 사용합니다 :

Uno.Compiler.ExportTargetInterop;

클래스 예제
{
    string deviceModel;

    정적 void 로그 (문자열 메시지)
    {
        debug_log (메시지);
    }

    void SetDeviceModel (문자열 모델)
    {
        deviceModel = 모델;
    }

    [Foreign (Language.Java)]
    extern (Android) void LogDeviceModel ()
    {
        String deviceModel = android.os.Build.MODEL;

        // 인스턴스 메소드 호출
        @ {예제 : Of (_this) .SetDeviceModel (string) : Call (deviceModel)};

        // 정적 메서드 호출
        @ {Example.Log (string) : Call (deviceModel)};
    }

    [Foreign (Language.ObjC)]
    extern (iOS) void LogDeviceModel ()
    {
        NSString * deviceModel = [[UIDevice currentDevice] model];

        // 인스턴스 메소드 호출
        @ {예제 : Of (_this) .SetDeviceModel (string) : Call (deviceModel)};

        // 정적 메서드 호출
        @ {Example.Log (string) : Call (deviceModel)};
    }
}
외부 소스 파일

퓨즈의 많은 철학은 '올바른 직업에 적합한 도구 사용'이라는 아이디어를 중심으로 전개됩니다. 외국 코드를 ​​사용하면 빠르게 Java를 호출하여 빠르게 처리 할 수 ​​있습니다. 또한 많은 Java를 작성해야하는 경우 Java로 수행하는 것이 가장 좋습니다. 이를 위해 .java, .mm 및 .hh 파일을 .unoprojs에 추가하여 빌드에 포함되도록 지원합니다.

다음과 같이 Java와 Objective-C 파일을 추가합니다.

{
    ...

    "포함": [
        "*",
        "Example.hh : ObjCHeader : iOS",
        "Example.mm:ObjCSource:iOS",
        "Example.java:Java:Android"
    ]
}
이것은 또한 Objective-C 코드가 UXL 매크로를 사용하여 외부 코드 블록처럼 Uno와 다시 대화 할 수있게합니다. 그러한 파일에서 매크로를 사용하려면 매크로 확장이 Objective-C 및 C ++의 기능을 사용하여 Uno 코드와 상호 작용하고 파일에 uObjC가 포함되어야하므로 Objective-C ++ 파일 (.mm)이어야합니다. Foreign.h. 매크로에 사용 된 Uno 클래스를 포함하려면 UXL 매크로 @ {The.Uno.Class : IncludeDirective}를 사용할 수 있습니다. UXL 매크로를 사용하지 않는다면 대신 CSource 및 CHeader 파일 유형을 사용하는 것으로 충분합니다. Java 파일 처리는 아직 가능하지 않지만 곧 제공 될 예정입니다.

프로젝트의 와일드 카드 (*) 패턴은 퓨즈 파일이 아닌 파일의 파일 유형을 자동으로 설정하지 않습니다. 와일드 카드를 사용하면 외국어로 된 파일이 프로젝트에 포함되지만 익명의 파일 형식으로 포함됩니다. 즉, 프로젝트에 외부 파일을 명시 적으로 추가하고 프로젝트에 처리되고 추가되도록 올바른 파일 형식을 설정해야합니다.

자바

퓨즈는 포함 된 Java 파일에서 패키지 문을 구문 분석하고 컴파일러에서 생성 한 프로젝트에서 폴더 계층 구조가 올바른지 확인하므로 Java를 사용할 때 폴더 구조에 대해 걱정할 필요가 없습니다.

ForeignInclude 속성을 사용하여 Java에서 가져 오기를 추가 할 수 있습니다. 클래스에서만 사용할 수 있습니다. includes는 Uno 클래스의 모든 외부 메소드에 영향을줍니다.

[ForeignInclude (Language.Java, "java.lang.Runnable", "java.lang.Boolean", "android.app.Activity")]
공용 클래스 SomeUnoClass : Uno.Application
{
    ...
}
목표 -C

외부 Objective-C 헤더를 사용하려면 클래스에 다음과 같이 헤더를 포함시킵니다.

[ForeignInclude (Language.ObjC, "Example.hh")]
클래스 예제
{
    ...
}
참고 : 이름 충돌에주의하십시오! Objective-C 클래스는 전역 이름 공간에서 Uno 클래스와 같은 이름을 가질 수 없습니다.

빠른

Swift 파일을 우리의 unoprojs에 추가하는 것이 가능합니다. Uno에서 Swift 코드 블록을 직접 사용하는 것은 아직 지원하지 않지만 외국의 Objective-C에서 Swift 코드를 호출 할 수 있습니다.

다음 예제에서는이 기능을 사용하는 방법을 보여줍니다.

안녕하세요. 스위프트 :

수입 재단

공용 클래스 HelloSwiftWorld : NSObject {
    public func hello () {
        print ( "Hello world!");
    }
}
unoproj :

{
    "포함": [
        "Hello.swift : Swift : iOS",
        ...
    ]

}
Swift는 Objective-C에서 사용할 수 있기 때문에 다음과 같이 Foreign Objective-C를 사용하여 Swift 코드를 호출합니다.

[ForeignInclude (Language.ObjC, "@ (Project.Name) -Swift.h")]
public class Example
{
    [Foreign (Language.ObjC)]
    공공 정적 무효 DoIt ()
    @ {
        HelloSwiftWorld * x = [[HelloSwiftWorld alloc] init];
        [안녕하세요.]
    @}
}
사용 된 Swift 버전은 iOS.SwiftVersion 프로젝트 속성을 사용하여 구성 할 수 있습니다.

"iOS": {
"SwiftVersion": 3.0,
},
Swift 파일은 현재 ObjCSource 파일에있는 외부 매크로 확장을 가져 오지 않습니다.

루트 활동 얻기

대부분의 Android 관련 작업에는 앱의 루트 활동이 어떤 방식 으로든 관련되어 있습니다. 다음 메소드를 호출하여 루트 활동을 얻을 수 있습니다.

com.fuse.Activity.getRootActivity ()
