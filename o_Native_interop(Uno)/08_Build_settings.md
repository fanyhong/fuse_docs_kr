원본: https://www.fusetools.com/docs/native-interop/build-settings

빌드 설정

Uno는 빌드 타겟에 따라 다른 포맷으로 프로젝트를 생성하기 때문에 헤더와 링크 라이브러리를 포함하여 생성 된 프로젝트의 다양한 측면을 제어 할 수 있어야 할 때가 있습니다. 네이티브 라이브러리와 상호 운용 할 때 특히 그렇습니다. 이 문서는 사용 가능한 설정에 대한 참조입니다.

이러한 설정 중 일부를 외부 코드와 함께 사용하는 방법에 대한 예는 Facebook 로그인 자습서를 참조하십시오. 도움이 될 수있는 또 다른 문서는 UXL 핸드북으로 일부 빌드 설정을 변경해야하는 기능을 간략하게 설명합니다.

설정을 변경하는 두 가지 방법

이 문서의 설정은 Uno 클래스 또는 메서드에 첨부 된 특성으로 설정 될 수 있습니다. 이러한 속성을 속성으로 사용하면 클래스가 사용될 때 (Uno는 사용되지 않는 코드를 제거하기 때문에) 효과가 있으며, 일반적으로 원하는 것입니다.

Uno 클래스에서 설정을 사용하려면 다음을 추가하십시오.

Uno.Compiler.ExportTargetInterop;
파일의 맨 위로 이동하여 예를 들어 다음 코드 :

[Require ( "LinkDirectory", "my / path")]
공용 클래스 MyClass
{
    ...
}
설정이 많아 지거나 긴 문자열이 필요할 경우 별도의 파일로 저장하는 것이 편리 할 수 ​​있습니다. 우리는 이것을하기 위해 unoproj 파일 타입 확장자와 함께 .uxl 파일을 사용합니다.

Xcode plist 파일에 추가되는 uxl 파일의 예는 다음과 같습니다.

<Extensions Backend = "CPlusPlus"Condition = "iOS">
    <Require Xcode.Plist.Element>
        <! [CDATA [
            <key> NSLocationUsageDescription </ key>
            <string> </ string>
            <key> NSLocationWhenInUseUsageDescription </ key>
            <string> </ string>
            <key> NSLocationAlwaysUsageDescription </ key>
            <string> </ string>
        ]]>
    </ Require>
</ Extensions>
여기서 우리는 백엔드 (보통 CPlusPlus)와 (선택적으로) 타겟 조건을 지정합니다. 즉, 코드는이 경우 iOS에서만 사용됩니다. 그런 다음 Xcode.Plist.Element가 필요하고 XML 문자 데이터 (CDATA)를 사용하여 Plist 파일에 삽입 할 XML을 이스케이프하지 않아도됩니다.

C ++ 타겟 설정

대부분의 빌드 설정은 C ++ 타겟에만 적용됩니다. 데스크톱 미리보기 용으로 제작 된 앱은 CIL 대상을 사용하는 반면 iOS 및 Android는 물론 기본 데스크톱 대상도 여기에 포함됩니다.

빌드 플래그

[Require ( "LinkDirectory", "my / path")]
내 / 경로를 라이브러리 검색 경로에 추가합니다. 이것은 -Lmy / path를 C ++ 컴파일러에 전달하는 것과 같습니다. 경로로 "@ ( 'my / relative / path': Path)"를 사용하면 상대 경로라는 의미입니다.

[Require ( "LinkLibrary", "mylibrary")]
mylibrary와 응용 프로그램을 연결하십시오. 이것은 -lmylibrary를 C ++ 컴파일러에 전달하는 것과 같습니다.

참고 : Android에서 라이브러리를 링크하려면 아래의 JNI. * 설정을 사용하십시오.

[Require ( "IncludeDirectory", "my / path"]
내 / 경로를 포함 검색 경로에 추가합니다. 이것은 -Imy / path를 C ++ 컴파일러에 전달하는 것과 같습니다. 경로로 "@ ( 'my / relative / path': Path)"를 사용하면 상대 경로라는 의미입니다.

외부 소스 파일 포함

다음과 같이 Objective-C, Java 및 C 소스 파일을 Uno 프로젝트에 추가합니다.

{
...

"포함": [
    "*",
    "Example.hh : ObjCHeader : iOS",
    "Example.mm:ObjCSource:iOS",
    "Example.java:Java:Android"
    "Example.h : CHeader",
    "Example.c : CSource",
]
}

자세한 내용은 외부 코드 설명서를 참조하십시오.

Android 관련 설정

JNI

Android에서 공유 및 정적 네이티브 라이브러리를 다르게 처리하므로 두 가지 모두에 대해 LinkLibrary를 사용하지 않고 대신 다음 두 요소를 사용합니다.

[Require ( "JNI.SharedLibrary", "mylib.so")]
응용 프로그램이 시작될 때 System.loadLibrary를 사용하여 mylib.so를로드하고 응용 프로그램에 mylib.so를 포함합니다.

[Require ( "JNI.SystemLibrary", "systemlib")]
응용 프로그램이 시작될 때 System.loadLibrary를 사용하여 systemlib을로드합니다 (응용 프로그램에 포함하지 않고 로그와 같은 시스템 라이브러리라고 가정).

[Require ( "JNI.StaticLibrary", "mylib.a")]
mylib.a와 응용 프로그램을 정적으로 연결합니다.

자원

[Require ( "Android.ResStrings.Declaration", "<string name = \"hello \ "> Hello! </ string>")]
<string name = "hello"> Hello! </ string>를 문자열 리소스 파일 res / values ​​/ strings.xml에 추가합니다.

명백한

[Require ( "AndroidManifest.ActivityElement", "<an-activity-element />")]
AndroidManifest.xml 파일의 응용 프로그램 활동 섹션에 <an-activity-element />를 추가합니다.

[Require ( "AndroidManifest.ApplicationElement", "<an-application-element />")]
<an-application-element />를 AndroidManifest.xml 파일의 응용 프로그램 섹션에 추가합니다.

[Require ( "AndroidManifest.Permission", "android.permission.A_PERMISSION")]
AndroidManifest.xml 파일에 <uses-permission android : name = "android.permission.A_PERMISSION"/>을 추가합니다.

[Require ( "AndroidManifest.RootElement", "<a-root-element />")]
AndroidManifest.xml 파일의 매니페스트 태그 아래 루트 수준에 <a-root-element />를 추가합니다.

[Require ( "AndroidManifest.Activity.ViewIntentFilter",
    "android : host = \"sites.google.com \ "android : pathPrefix = \"/ site / appindexingex / home / main \ "android : scheme = \"https \ "")]
응용 프로그램보기의 intent-filter에 지정된 문자열을 추가합니다.

요람

이 설정은 -DGRADLE Uno 플래그를 사용하여 빌드 할 때 사용할 수있는 예비 Gradle 실험을 사용하여 Android 용으로 구축 할 때 적용됩니다. Gradle을 사용한 종속성 관리에 대한 자세한 내용은이 항목을 참조하십시오.

[Require ( "Gradle.Dependency", "compile ( 'myDependency') {transitive = true}")]
컴파일 된 ( 'myDependency') {transitive = true}를 앱의 생성 된 build.gradle 파일의 종속성에 추가합니다.

[Require ( "Gradle.Dependency.ClassPath", "myDependency")]
classpath 'myDependency'를 최상위 레벨 생성 된 build.gradle 파일의 종속성에 추가합니다.

[Require ( "Gradle.Dependency.Compile", "myDependency")]
컴파일 'myDependency'를 앱의 생성 된 build.gradle 파일의 종속성에 추가합니다.

[Require ( "Gradle.BuildFile.End", "stuff")]
생성 된 build.gradle 파일의 맨 끝에 물건을 추가합니다.

[Require ( "Gradle.Repository", "mavenCentral ()")]
mavenCentral ()을 앱의 생성 된 build.gradle 파일에있는 저장소에 추가합니다.

iOS 관련 설정

Xcode 설정

[Require ( "Xcode.EmbeddedFramework", "my.framework")]
my.framework를 생성 된 Xcode 프로젝트에 임베디드 프레임 워크로 추가합니다. 경로로 "@ ( 'my / relative.framework': Path)를 사용하면 상대 경로라는 것을 의미합니다.

[Require ( "Xcode.Framework", "a.framework")]
생성 된 Xcode 프로젝트에 a.framework를 프레임 워크로 추가합니다. 프레임 워크 경로가 절대적이지 않은 경우 SDKROOT / System / Library / Frameworks와의 상대 경로로 간주되므로 iPhone SDK의 프레임 워크에 사용할 수 있습니다.

경로로 "@ ( 'my / relative.framework': Path)를 사용하면 상대 경로라는 것을 의미합니다.

[Require ( "Xcode.FrameworkDirectory", "@ ( 'my / relative / framework / path': 경로")]
my / relative / framework / path를 프레임 워크 검색 경로에 추가합니다. 이는 -Fmy / relative / framework / path를 C ++ 컴파일러에 전달하는 것과 같습니다. 참고 : Xcode.Framework 또는 Xcode.EmbeddedFramework를 사용하여 프레임 워크를 포함 할 때이 플래그는 자동으로 설정되므로이 경우 설정할 필요가 없습니다.

[Require ( "Xcode.Plist.Element", "<plist-element />")]
<plist-element />를 응용 프로그램의 plist 파일에 추가합니다.

[Require ( "Xcode.ShellScript", "someScript")]
생성 된 Xcode 프로젝트의 .pbxproj 파일에있는 PBXProjShellScriptBuildPhase에 someScript를 추가합니다.

CocoaPods

이 설정은 -DCOCOAPODS Uno 플래그를 사용하여 빌드 할 때 사용할 수있는 예비 CocoaPod 지원을 사용하여 iOS 용으로 빌드 할 때 적용됩니다.

[Require ( "Cocoapods.Platform.Name", "platformName")]
추가

플랫폼 : platformName, ''
생성 된 Podfile에 추가합니다.

[Require ( "Cocoapods.Platform.Version", "platformVersion")]
Cocoapods.Platform.Name이 platformName으로 설정된 경우

platform : platformName, 'platformVersion'
생성 된 Podfile에 추가합니다.

[Require ( "Cocoapods.Podfile.Target", "pod 'Firebase'")]
생성 된 Podfile에서 응용 프로그램의 대상 구성에 'Firebase'창을 추가합니다.

[Require ( "Cocoapods.Podfile.Pre", "myPreStatement")]
생성 된 Podfile의 시작 부분에 myPreStatement를 추가합니다.

[Require ( "Cocoapods.Podfile.Post", "myPostStatement")]
생성 된 Podfile의 끝에 myPostStatement를 추가합니다.
