원본: https://www.fusetools.com/docs/native-interop/facebook-login

외부 코드를 통한 Facebook SDK를 사용한 Facebook 로그인

퓨즈의 외부 코드 기능을 사용하면 원시 코드를 Uno 코드와 통합 할 수 있습니다. 우리는 현재 Android에서 Java를 지원하고 iOS에서는 Objective-C를 지원합니다. 이 예제는이 기능을 사용하여 Facebook SDK를 Fuse와 통합하여 Facebook의 로그인 기능을 얻는 방법을 보여줍니다.

이 예제는 외국 코드에 대해 독립적으로 소개하기위한 것이지만, 예를 들어 Uno 유형과 기본 유형 간의 정렬을 자세히 설명하는 기술 문서를 참조하는 것이 유용 할 수 있습니다. 이 가이드는 또한 우리의 C # 사투리 인 우노 (Uno)를 유창하게 사용합니다. Uno에 대한 자세한 정보는 Uno 언어 참조 서를 참조하십시오.

기계적 인조 인간

우리는 페이스 북의 로그인 안드로이드 가이드를 면밀히 준수 할 것이므로 자신의 코드를 따라 가고 싶다면 자신의 필수 조건을 반드시 따라야합니다. LoginManager 클래스를 사용하여 퓨즈 버튼을 눌러 로그인을 시작합니다.

로그인 트리거 및 응답

기본 로그인 기능은 다음 서명을 사용하여 Uno 메소드에서 구현됩니다.

공개 무효 로그인 (액션 <AccessToken> onSuccess, 취소 된 액션, 액션 <string> onError)
이 메소드는 FacebookLogin 클래스에 포함되어 있으며 로그인 결과에 따라 호출되는 액션을 허용합니다.

Java에서 메소드 본문을 작성하려면 [Foreign (Language.Java)] 속성을 추가하고 일반 중괄호 대신 @ {및 @}을 사용하십시오. 특별한 중괄호는 Uno에게 코드를 일반적인 Uno 코드로 구문 분석하지 말라고 지시하며, 속성은 코드가 실제로 작성된 언어 (이 경우 Java)를 컴파일러에 알립니다. Android에서는이 버전의 메소드 만 사용할 수 있으므로 extern (Android)를 사용하여 Android를 타겟팅 할 때만 컴파일합니다.

[Foreign (Language.Java)]
public extern (Android) 무효 로그인 (액션 <AccessToken> onSuccess, 취소 된 액션, 액션 <string> onError)
@ {
@}
onSuccess 액션에 대한 인수로 사용되는 AccessToken 클래스는 Java 객체를 둘러싼 씬 래퍼입니다.

공용 클래스 AccessToken
{
    extern (Android) Java.Object _token;

    public extern (Android) AccessToken (Java.Object 토큰)
    {
        _ 토큰 = 토큰;
    }
}
Uno 클래스 Java.Object는 Uno 측에서 Java 객체를 유지하는 방법을 제공하여 Uno에서 Java 코드에서 얻은 결과를 저장하고 전달할 수 있습니다.

참고 : 여기서는 AccessToken 클래스가 반드시 필요한 것은 아니지만, 우리가 기대하는 Java 객체의 유형에 대한 문서로 사용됩니다. 또한 액세스 토큰에 포함 된 정보를 얻을 수있는 방법을 제공하는 구현의 기초가 될 수도 있습니다.

우리는 또한 주위 Uno 객체 (FacebookLogin)에 저장된 상태를 사용할 것입니다. 이 상태는 Facebook SDK의 CallbackManager가 포함 된 Java.Object _callbackmanager입니다. 다음 섹션에서는이 상태가 초기화되는 방법을 보여줍니다.

이제 Java에서 Login 메소드의 본문을 구현할 준비가되었습니다.

우선, 매크로를 사용하여 _this 객체에서 callbackManager를 가져옵니다.

@ {
    CallbackManager callbackManager = (CallbackManager) @ (FacebookLogin : Of (_this).) callbackManager : Get ()};
    ...
@}
이 외부 코드의 객체는 메소드가 정의 된 Uno 객체에 해당합니다 (정적 메소드가 아니라고 가정). 말하자면, 매크로 (@ {...} 부분)는 "이 FacebookLogin 유형의 Uno 객체 인 _thall의 _callbackManager 필드를 얻습니다"라고 말합니다 (자세한 내용은 외부 코드 문서 참조). Uno의 Java.Objects는 Java 측에서 일반 Java 객체로 변환되므로, 우리가 알고있는 형인 CallbackManager에 캐스팅합니다.

다음으로 콜백을 Facebook LoginManager에 등록합니다.

LoginManager.getInstance (). registerCallback (callbackManager,
    새로운 FacebookCallback <LoginResult> ()
    {
        @보수
        public void onSuccess (LoginResult loginResult)
        {
            AccessToken accessToken = loginResult.getAccessToken ();
            객체 unoAccessToken = @ {AccessToken (Java.Object) : 새 (accessToken)};
            onSuccess.run (unoAccessToken);
        }

        @보수
        public void onCancel ()
        {
            onCancelled.run ();
        }

        @보수
        public void onError (FacebookException 예외)
        {
            onError.run (exception.toString ());
        }
    }
);
onSuccess 메서드에서는 먼저 LoginResult에서 AccessToken을 가져온 다음 UXL : New 매크로를 사용하여 UnoAccessToken (위에서 정의한 클래스) 인 unoAccessToken을 만듭니다.이 매크로는 UnoObject가됩니다. 우리는 자바 측에서 작업 할 때 Uno 객체를위한 UnoObject 객체를 얻습니다. UnoObject는 Uno에서 Java.Object의 반대입니다. Java에서 Uno 객체를 고정하고 전달하는 방법입니다.

대리자를 외부 메서드에 전달하는 특별한 지원이 있습니다. 델리게이트가 우노 액션 인 경우 자바에서 실행 파일로 자동 변환됩니다. 델리게이트가 매개 변수를 가지거나 값을 반환하면 델리게이트의 서명과 일치하는 run 메소드가있는 사용자 정의 클래스로 변환되고 인수 및 리턴 유형은 Java와 동등한 것으로 변환됩니다. 따라서 onSuccess 메서드에서는 run 메서드를 사용하여 방금 만든 unoAccessToken을 전달하여 onSuccess 대리자를 호출하기 만하면됩니다.

우리는 onCancel 및 onError 케이스를 비슷하게 처리합니다. onError.run에 대한 인수로 사용 된 Java String은 함수가 호출 될 때 자동으로 Uno 문자열로 변환됩니다.

설정

초기화

Facebook 문서는 우리 앱의 라이프 사이클의 여러 단계에서 전화를 걸도록 지시합니다. 퓨즈 응용 프로그램의 수명주기는 일반 Android 응용 프로그램과 완전히 똑같은 방식으로 처리되지 않으므로 Facebook 설명서와 같이 정확하게 수행 할 수 없습니다.

먼저, FacebookLogin 클래스를 연결하여 관련 이벤트가 트리거 될 때 콜백을받습니다. 이는 Fuse.Platform에서 Lifecycle 클래스를 사용하여 수행됩니다. FacebookLogin 생성자에서는 다음을 수행합니다.

public FacebookLogin ()
{
    Lifecycle.Started + = 시작됨;
    Lifecycle.EnteringInteractive + = OnEnteringInteractive;
    Lifecycle.ExitedInteractive + = OnExitedInteractive;
}
Started의 구현은 다음과 같습니다.

[Foreign (Language.Java)]
extern (Android) void 시작됨 (ApplicationState 상태)
@ {
    FacebookSdk.sdkInitialize (Activity.getRootActivity ());
    최종 CallbackManager callbackManager = CallbackManager.Factory.create ();
    @ {FacebookLogin : Of (_this) ._ callbackManager : Set (callbackManager)};
    Activity.subscribeToResults (새 Activity.ResultListener ())
    {
        @보수
        public boolean onResult (int requestCode, int resultCode, 의도 데이터)
        {
            return callbackManager.onActivityResult (requestCode, resultCode, data);
        }
    });
@}
첫 번째 줄에서는 Activity.getRootActivity ()를 사용하여 응용 프로그램의 Activity를 가져옵니다. 이 방법은 com.fuse.Activity라는 퓨즈 관련 클래스에서 가져온 것입니다. 다음으로 Facebook SDK의 CallbackManager를 만들고 UXL 매크로를 사용하여 나중에 사용할 수 있도록이 객체에 값을 저장합니다. Facebook SDK 설명서에 나와 있듯이 Fuse에서 onActivityResult 메서드를 직접 재정의 할 수는 없지만 com.fuse.Activity에서 해당 콜백을 얻기 위해 수신기를 추가 할 수 있습니다. 이것이 subscribeToResults가 호출하는 것입니다.

수입품 및 Gradle 의존성

Uno 클래스의 외부 Java 코드는 모두 동일한 파일로 컴파일됩니다. 이 경우 Java 코드에 가져 오기가 필요한 경우 해당 클래스의 ForeignInclude 속성을 사용하여 생성 된 Java 파일에 추가 할 수 있습니다. FacebookLogin 클래스의 경우 다음과 같이 보입니다.

[ForeignInclude (Language.Java, "android.content.Intent")]
[ForeignInclude (Language.Java, "com.facebook. *")]
[ForeignInclude (Language.Java, "com.facebook.appevents.AppEventsLogger")]
[ForeignInclude (Language.Java, "com.facebook.login. *")]
[ForeignInclude (Language.Java, "com.fuse.Activity")]
[ForeignInclude (Language.Java, "com.fusefacebook.ActivityResultListener")]
public class FacebookLogin
{
...
}
또한 Facebook SDK 자체를 추가해야합니다. Uno는 Android에서 Gradle 빌드 시스템을 지원하며, -DGRADLE 플래그로 빌드하고 앱이 빌드 될 때마다 Gradle 빌드 파일을 생성합니다. 클래스에 다음 특성을 추가하여 컴파일러에서 종속성 및 저장소 섹션에 항목을 추가하도록 지시 할 수 있습니다.

[Require ( "Gradle.Dependency", "compile ( 'com.facebook.android:facebook-android-sdk:4.8.+') {제외 모듈 : 'support-v4'}})]
[Require ( "Gradle.Repository", "mavenCentral ()")]
Android 매니페스트 변경

Facebook SDK는 AndroidManifest.xml 파일을 일부 변경해야합니다. 이것은 우리가 앱을 만들 때 Uno가 생성하는 또 다른 파일입니다. Gradle 종속성을 추가 한 것과 비슷한 방식으로 변경 사항을 추가 할 수 있습니다. 이번에는 변경 사항이 너무 크기 때문에 속성으로 추가하기가 쉽지 않으므로 .uxl 파일을 사용하여 다음과 같이 지정합니다.

<Extensions Backend = "CPlusPlus">
  <Set Facebook.AppID = "YOUR-FACEBOOK-APP-ID"/>
  <Require Condition = "Android"Android.ResStrings.Declaration>
    <! [CDATA [
      <string name = "FACEBOOK_APP_ID"> @ (Facebook.AppID) </ string>
    ]]>
  </ Require>
  <Require Condition = "Android"AndroidManifest.ApplicationElement>
    <! [CDATA [
      <meta-data android : name = "com.facebook.sdk.ApplicationId"android : value = "@ string / FACEBOOK_APP_ID"/>
      <activity android : name = "com.facebook.FacebookActivity"
              android : configChanges =
                     "keyboard | keyboardHidden | screenLayout | screenSize | orientation"
              android : theme = "@ android : style / Theme.Translucent.NoTitleBar"
              android : label = "com.uno.test"/>
    ]]>
  </ Require>
</ Extensions>
이 첫 번째 행은 Facebook 응용 프로그램 ID로 변수를 생성합니다. 나중에 @ (Facebook.AppID)를 참조하십시오.

또한 몇 가지 요소가 필요합니다. 기본적으로 생성 된 빌드 파일의 특정 위치에 추가 할 사항을 Uno 컴파일러에 알리는 것입니다. 예를 들어 AndroidManifest.ApplicationElement 요소를 사용하면 AndroidManifest.xml의 application 요소에 하위 요소를 추가 할 수 있습니다. 이러한 종류의 설정에 대한 자세한 내용은 빌드 설정 설명서를 참조하십시오.

iOS

페이스 북 로그인 iOS 가이드를 자세히 읽으므로 자신의 코드를 따르고 싶다면 필수 조건을 모두 거쳐야합니다. 여기에서는 FBSDKLoginManager를 사용하여 퓨즈 버튼을 눌러 로그인을 시작합니다.

로그인 트리거 및 응답

우리는 Android 구현과 동일한 고리를 사용하여 라이프 사이클 이벤트를 가져옵니다. iOS 구현은 주변 Uno 객체에 상태를 저장할 필요가 없기 때문에 조금 더 쉽습니다.

login 메소드는 다음과 같이 foreign-Object-C를 사용하여 구현됩니다.

[Foreign (Language.ObjC)]
public extern (iOS) void 로그인 (액션 <AccessToken> onSuccess, 취소 된 액션, 액션 <string> onError)
@ {
    FBSDKLoginManager * login = [[FBSDKLoginManager alloc] init];
    [로그인
        logInWithReadPermissions : @ [@ "public_profile"]
        fromViewController : [[[[UIApplication sharedApplication] 키 윈도우] rootViewController]
        처리기 : ^ (FBSDKLoginManagerLoginResult * 결과, NSError * 오류)
        {
            if (error)
            {
                onError ([오류 localizedDescription]);
                반환;
            }
            if (result.isCancelled)
            {
                onCancelled ();
                반환;
            }
            id <UnoObject> unoAccessToken = @ {AccessToken (ObjC.Object) : New (result.token)};
            onSuccess (unoAccessToken);
        }
    ];
@}
먼저 FBSDKLoginManager를 만들고이를 사용하여 로그인을 시작하고 적절한 위임자를 다시 호출합니다.

Objective-C에 전달 된 Uno 대리자는 자동으로 해당 서명이있는 블록으로 변환되므로 Objective-C 쪽에서 일반 함수처럼 호출 할 수 있습니다. onError 콜백에 대한 문자열 인수는 NSString *에서 자동으로 변환되므로 [error localizedDescription]을 사용하여 오류의 경우 오류 문자열을 가져 와서 콜백에 전달합니다.

UXL : New 매크로를 사용하여 onSuccess 콜백에 사용되는 Uno AccessToken 객체를 만듭니다. id <UnoObject> 유형을 사용하여 Objective-C에서 Uno 객체를 유지할 수 있습니다.

설정

초기화

iOS의 초기화는 Android의 경우와 비슷하지만 SDK에 대한 Objective-C 인터페이스 만 사용합니다.

응용 프로그램 Started hook에서 우리는 다음을 수행합니다 :

[Foreign (Language.ObjC)]
extern (iOS) void 시작됨 (ApplicationState state)
@ {
    [[FBSDKApplicationDelegate sharedInstance]
        응용 프로그램 : [UIApplication sharedApplication]
        didFinishLaunchingWithOptions : nil];
@}
비슷하게 interactive를 입력 할 때 activateApp를 호출합니다.

[Foreign (Language.ObjC)]
정적 extern (iOS) void OnEnteringInteractive (응용 프로그램 상태)
@ {
    [FBSDKAppEvents activateApp];
@}
iOS에서 로그인이 성공했음을 알기 위해받은 URI를 Facebook SDK에 전달해야합니다. 우리는 Fuse.Platform.InterApp.ReceivedURI를 사용하여 이러한 이벤트에 연결할 수 있으므로 생성자에 추가합니다.

public FacebookLogin ()
{
    ...
    InterApp.ReceivedURI + = OnReceivedUri;
}
fb로 시작하는 URI로 콜백을 받으면 Facebook에 전달합니다.

정적 무효화 OnReceivedUri (string uri)
{
    if (uri.StartsWith ( "fb"))
    {
        OpenFacebookURL (uri);
    }
}

[Foreign (Language.ObjC)]
정적 extern (iOS) void OpenFacebookURL (string url)
@ {
    [[FBSDKApplicationDelegate sharedInstance]
        응용 프로그램 : [UIApplication sharedApplication]
        openURL : [NSURL URLWithString : url]
        sourceApplication : @ "com.apple.mobilesafari"
        주석 : 없음];
@}
프레임 워크 추가 및 수입 얻기

여기서는 Uno 프로젝트가있는 폴더의 FacebookSDKs-iOS라는 폴더에서 iOS 용 Facebook SDK를 다운로드하여 압축을 푼 것으로 가정합니다.

Uno는 iOS 용으로 빌드 할 때마다 Xcode 프로젝트를 생성합니다. 프로젝트에 추가 된 Facebook SDK 프레임 워크를 얻으려면 Uno 클래스에 다음 속성을 추가합니다.

[Require ( "Xcode.FrameworkDirectory", "@ ( 'FacebookSDKs-iOS': 경로) ')]
[Require ( "Xcode.Framework", "@ ( 'FacebookSDKs-iOS / FBSDKCoreKit.framework': 경로)")]
[Require ( "Xcode.Framework", "@ ( 'FacebookSDKs-iOS / FBSDKLoginKit.framework': 경로)")]
public class FacebookLogin
{
...
}
@ ( 'relative / path': Path) 구문을 사용하여 현재 파일과 관련된 경로를 지정합니다. 기본적으로 iPhone SDK 시스템 경로에서 프레임 워크를 찾는 것이므로 (SDK와 Apple 번들 프레임 워크에서만 작동 함)이 작업을 수행합니다. 또한 Xcode가 해당 디렉토리의 헤더를 검색하도록 Xcode.FrameworkDirectory를 요구해야합니다.

(참고로 [Require ( "Xcode.EmbeddedFramework", "...")]를 사용하여 프레임 워크를 임베드 할 수도 있습니다. 이러한 설정에 대한 자세한 내용은 빌드 설정 설명서를 참조하십시오.

다음으로, 외부 코드에서 Facebook SDK를 사용하는 Uno 클래스에서 생성 된 파일에 필수 포함 항목이 있는지 확인해야합니다. CoreKit과 LoginKit의 엔티티를 사용하기 때문에 다음과 같이 할 수 있습니다.

[ForeignInclude (Language.ObjC, "FBSDKCoreKit / FBSDKCoreKit.h")]
[ForeignInclude (Language.ObjC, "FBSDKLoginKit / FBSDKLoginKit.h")]
목록

Facebook SDK는 Xcode 프로젝트의 plist 파일에 몇 가지 요소를 추가해야합니다. 빌드시 생성되므로 UIManifest에 요소를 추가 할 때 사용한 파일과 동일한 파일 인 UXL에서이 작업을 수행합니다.

<Require Condition = "iOS"Xcode.Plist.Element>
<! [CDATA [
  <key> CFBundleURLTypes </ key>
  <배열>
    <dict>
      <key> CFBundleURLSchemes </ key>
      <배열>
        <string> fb @ (Facebook.AppID) </ string>
      </ array>
    </ dict>
  </ array>
  <key> FacebookAppID </ key>
  <string> @ (Facebook.AppID) </ string>
  <key> FacebookDisplayName </ key>
  <string> 퓨즈 테스트 앱 </ string>

  <key> NSAppTransportSecurity </ key>
  <dict>
    <key> NSExceptionDomains </ key>
    <dict>
      <key> facebook.com </ key>
      <dict>
        <key> NSIncludesSubdomains </ key> <true />
        <key> NSThirdPartyExceptionRequiresForwardSecrecy </ key> <false />
      </ dict>
      <key> fbcdn.net </ key>
      <dict>
        <key> NSIncludesSubdomains </ key> <true />
        <key> NSThirdPartyExceptionRequiresForwardSecrecy </ key> <false />
      </ dict>
      <key> akamaihd.net </ key>
      <dict>
        <key> NSIncludesSubdomains </ key> <true />
        <key> NSThirdPartyExceptionRequiresForwardSecrecy </ key> <false />
      </ dict>
    </ dict>
  </ dict>

  <key> LSApplicationQueriesSchemes </ key>
  <배열>
    <string> fbauth2 </ string>
  </ array>
  ]]>
</ Require>
전체 프로젝트 얻기

iOS와 Android에서 모두 작동하는 전체 프로젝트를 여기에서 다운로드 할 수 있습니다.

전체 프로젝트는 NativeModule의 로그인 기능을 연결하여 JavaScript에서 로그인 기능을 사용할 수 있도록하여 약속을 반환합니다.
