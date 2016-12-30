우노 프로젝트 (.unoproj)

Uno는 Fuse의 중심에있는 C #의 휴대용 경량 사투리로, iOS 및 Android 용 기본 응용 프로그램을 내보낼 수 있습니다. Uno에 대한 자세한 내용은 Native Interop 섹션을 참조하십시오.

퓨즈 프로젝트는 기본적으로 퓨즈 라이브러리를 참조하는 Uno 프로젝트입니다. 퓨즈 라이브러리는 우노 (Uno) 언어로 작성되었습니다.

Uno 프로젝트 파일 (.unoproj)은이 기본 구조를 가진 일반 JSON 텍스트 파일입니다.

{
    "패키지": [
        "퓨즈",
        "퓨즈 (FuseJS)"
    ],
    "포함": [
        "*"
    ]
}
Uno 프로젝트 파일은 프로젝트가 의존하는 것, 구성 요소 및 iOS 또는 Android의 기본 앱으로 선택적으로 패키지하는 방법을 지정하는 곳입니다.

Uno 프로젝트는 다른 프로젝트에서 사용하기위한 자산과 구성 요소로 구성된 라이브러리 프로젝트이거나 실제 응용 프로그램 프로젝트 일 수 있습니다. 프로젝트에 App 클래스가 있으면 프로젝트가 앱 프로젝트임을 나타냅니다.

패키지

패키지 섹션은 프로젝트에서 참조 할 패키지 목록입니다. 패키지는 사전 컴파일 된 Uno 프로젝트로서 Uno 패키지 관리자에 정식 패키지 이름으로 등록되어 있습니다.

퓨즈 프로젝트를 만들려면 일반적으로 적어도 퓨즈 및 퓨즈 JS 패키지를 참조해야합니다. 그러나 응용 프로그램을 스캐 폴딩 할 때 Fuse에는 표준 패키지 세트가 포함되어 실행될 수 있습니다.

참조한 패키지가 앱에 필요한 권한에 영향을 미칠 수 있습니다.

{
    "패키지": [
        "퓨즈",
        "퓨즈 (FuseJS)"
    ]
}
매크로

Uno 프로젝트 형식은 기본 매크로 시스템을 지원합니다. 이 기능은 제목 및 버전과 같은 많은 최상위 속성을 구현하는 데 사용됩니다.

예를 들어 다음과 같이 Version 속성을 설정한다고 가정 해보십시오.

{
    "버전": "0.9.4"
}
기본적으로 Android.VersionName과 iOS.BundleVersion은 다음과 같이 설정됩니다.

{
    "Android": {
        "VersionName": "$ (Version)"
    },
    "iOS": {
        "BundleVersion": "$ (Version)"
    }
}
그러면 해당 속성이 최상위 Version 속성을 상속하게됩니다.

프로젝트

Projects 섹션은 선택 사항이며 로컬 파일 시스템에서 다른 Uno 프로젝트를 참조 할 수 있습니다. 다른 .unoproj 파일에 대한 상대 경로.

{
    "프로젝트": [
        "../../SomeOtherProject/SomeOtherProject.unoproj"
    ]
}
포함 및 제외

Includes 및 Excludes 속성을 사용하면 프로젝트에 포함 할 파일을 제어 할 수 있습니다. 그것들은 아래의 스 니펫 (snippet)과 같이 두 문자열 배열입니다.

{
    "포함": [
        "* .ux",
        "js / ** / *. js",
        "SomeUnoClass.uno : Source",
        "ForeignCode.java:Java:android"
    ],
    "제외": [
        "js / ExcludeThisFilePlease.js",
        "node_modules /"
    ]
}
포함 / 제외 항목은 다음 양식 중 하나를 사용할 수 있습니다.

GlobPattern

이 문서에서 더 자세히 설명하는 glob 패턴.

{
    "포함": [
        "* .ux",
        "js / ** / *. js",
        "Foo.uno"
    ]
}
다음 glob 기능이 지원됩니다.

가새 확장
확장 된 glob 매칭
Globstar 매칭
/를 포함하지 않는 패턴은 재귀 적으로 일치합니다.


패턴 일치
* 모든 파일과 일치합니다.
* .foo 이름이 .foo로 끝나는 모든 파일과 일치합니다.
*. + (bar | foo) 이름이 .bar 또는 .foo로 끝나는 모든 파일과 일치합니다.
foobar foobar라는 이름의 모든 파일과 일치합니다.
재귀를 비활성화하려면 / 또는 ./를 추가하십시오.

패턴 일치
/*.png 프로젝트 디렉토리에서 직접 찾은 모든 PNG 파일을 찾습니다.
Foo / *. png Foo 디렉토리에있는 모든 PNG 파일을 찾습니다.
명시적인 재귀를 위해 globstar (**)를 사용하십시오 :

패턴 일치
Foo / ** / *. uno Foo 디렉토리와 하위 디렉토리에있는 모든 Uno 파일과 일치합니다.
파일 이름 : 유형

Uno 컴파일러가 단일 파일을 포함하여 특정 유형으로 해석하도록 지시합니다.

{
    "포함": [
        "MainView.ux : UX"
    ]
}
FileName : 유형 : 조건

특정 유형의 파일 외에도 파일 포함 여부를 결정하는 조건을 지정할 수 있습니다.

{
    "포함": [
        "AndroidOnly.java:Java:Android",
        "iOSOnly.hh : ObjCHeader : iOS",
        "iOSOnly.mm:ObjCSource:iOS"
    ]
}
허용 된 포함 유형

UX - UX 마크 업 파일을 선언합니다 (확장명이 .ux 인 파일을 추가 할 때 자동).
소스 - Uno 소스 파일 (확장자가 .uno 인 파일을 추가 할 때 자동).
번들 - 앱 번들에 포함될 파일이며 런타임에 액세스 할 수 있습니다. 예를 들어 PNG 또는 JPEG와 같은 이미지 파일 또는 JSON 또는 XML 같은 데이터 파일 일 수 있습니다. 또한 독립형 JavaScript 파일을 필요로 할 때 필요합니다.
CSource - C 소스 파일.
CHeader - C 헤더 파일.
ObjCSource - Objective-C 소스 파일.
ObjCHeader - Objective-C 헤더 파일.
Java - .java 파일이 끝나는 Java 코드 파일
Extensions - UXL 확장 파일.
파일 - 파일을 프로젝트의 필수 부분으로 선언하는 데 사용되지만 응용 프로그램에서 컴파일하거나 번들 할 필요가 없습니다.

모든 속성

표제

사용자가 읽을 수있는 앱 제목입니다. 기본값은 $ (Name)입니다.

비현실적으로 LongNameForAnApp.unoproj :

{
    "제목": "짧은 이름"
}
기술

사용자가 읽을 수있는 앱 설명. 기본적으로 비워 둡니다.

{
    "설명": "인터넷에서 고양이 사진을 정리, 추적 및 분석하는 데 도움이되는 앱입니다."
}
저작권

앱의 저작권 고지. 기본값은 Copyright (C) <현재 연도> $ (게시자)입니다.

{
    "저작권": "저작권 © 2003-2016 $ (발행인)"
}
발행자

신청서를 게시하는 법인입니다. 기본값은 $ (게시자)입니다.

{
    "발행인": "VeryBusinessCorp Inc."
}
번역

앱의 현재 버전이며 기본값은 0.0.0입니다.

{
    "버전": "1.2.9"
}
버전 코드

일부 플랫폼 (현재 Android 만 해당)에는 버전 문자열 외에 정수 버전 코드가 필요합니다. 추가 문서는 여기에서 찾을 수 있습니다. 기본값은 0입니다.

{
    "VersionCode": 125
}
RootNamespace

사용할 루트 네임 스페이스입니다. 기본값은 $ (QIdentifier)입니다.

{
    "RootNamespace": "MyCompany.MyApp"
}
BuildDirectory

다양한 빌드 구성 및 대상에 대한 임시 파일 및 실행 파일을 배치 할 위치입니다. 빌드 할 기본값.

{
    "BuildDirectory": "출력"
}
OutputDirectory

현재 빌드 대상 및 구성에 대한 임시 파일 및 실행 파일을 둘 위치. 기본값은 $ (BuildDirectory) / @ (Target) / @ (Configuration)입니다.

{
    "OutputDirectory": "$ (BuildDirectory) / @ (대상) / @ (구성)"
}
CacheDirectory

Uno의 캐시 파일을 저장할 위치. .uno 기본값.

{
    "CacheDirectory": ".cache"
}
UnoCoreReference

Wether 또는 UnoCore는 참조되어야합니다. 내부적으로 만 사용되므로이 기능은 절대로 필요하지 않습니다. 기본값은 true입니다.

{
    "UnoCoreReference": true
}
변하기 쉬운

모든 모바일 타겟에 적용되는 옵션 사전.

Mobile.UriScheme

앱을 실행하는 데 사용할 수있는 URI 스키마를 지정합니다.

예를 들어, "UriScheme": "abcd"를 설정하면 abcd : // URI가 시작될 때 앱이 시작됩니다.

{
    '모바일': {
        "UriScheme": "abcd"
    }
}
Mobile.KeepAlive

true로 설정하면 앱이 열리는 동안 화면이 어두워지지 않고 결국 꺼지게됩니다. 기본값은 false입니다.

{
    '모바일': {
        "KeepAlive": false
    }
}
Mobile.ShowStatusbar

상태 표시 줄을 표시하거나 표시하지 않습니다. 기본값은 true입니다.

{
    '모바일': {
        "ShowStatusbar": true
    }
}
Mobile.RunsInBackground

사용자가 홈 버튼을 누르면 앱이 계속 실행되어야하는지 여부를 제어합니다. 기본값은 true입니다.

{
    '모바일': {
        "RunsInBackground": false
    }
}
Mobile.Orientations

허용되는 화면 방향을 지정합니다.

다음 값 중 하나 일 수 있습니다.

자동 (기본값, 가능한 모든 방향)
초상화
세로 방향 다운
경치
LandscapeLeft
가로 방향
{
    '모바일': {
        "오리엔테이션": "세로"
    }
}
기계적 인조 인간

안드로이드 타겟에만 적용되는 옵션 사전.

Android.ApplicationLabel

Android 전용 응용 프로그램에 대한 사용자가 읽을 수있는 레이블입니다. 이것은 Android의 최상위 Title 속성과 동일합니다. 기본값은 $ (제목)입니다.

{
    "Android": {
        "ApplicationLabel": "MyFancyApp"
    }
}
Android.Description

설명과 동일하지만 Android에만 해당됩니다. 기본값은 $ (설명)입니다.

{
    "Android": {
        '설명': 'Android 관련 설명입니다.'
    }
}
Android.VersionCode

VersionCode와 동일하지만 Android에만 해당됩니다. 기본값은 $ (VersionCode)입니다.

{
    "Android": {
        "VersionCode": 412
    }
}
Android.VersionName

버전과 동일하지만 Android에만 해당됩니다. 기본값은 $ (Version)입니다.

{
    "Android": {
        "VersionName": "0.5.2"
    }
}
Android.Package

Android 내보내기에 사용할 Java 패키지의 이름입니다. 기본값은 $ (QIdentifier)입니다.

{
    "Android": {
        "패키지": "com.mycompany.myapp"
    }
}
Android.PreviewPackage

미리보기 모드에서 Android 내보내기에 사용할 Java 패키지의 이름입니다. 기본값은 Android.Package입니다. 퓨즈 미리보기 -t = android에서만 사용되어 일반 패키지와 미리보기 패키지를 구별합니다. 앱에 미리보기 버전과 내 보낸 버전을 동시에 설치하려면이 설정을 사용하십시오.

{
    "Android": {
        "PreviewPackage": "com.mycompany.myapp.preview"
    }
}
Android.Icons

모든 플랫폼에서 사용할 수 있도록 균일 한 크기의 아이콘을 제공하는 대신 Android별로 다양한 화면 밀도에 대해 서로 다른 아이콘을 지정할 수 있습니다. 이들은 안드로이드의 일반화 된 밀도에 따라 분류됩니다. 모두 기본값은 $ (Icon)입니다.

참고 : 이것은 Android에만 적용되지만 iOS.Icons를 사용하여 iOS에서도 동일한 결과를 얻을 수 있습니다.

{
    "Android": {
        "아이콘": {
            "LDPI": "Icon-ldpi.png",
            "MDPI": "Icon-mdpi.png",
            "HDPI": "Icon-hdpi.png",
            "XHDPI": "Icon-xhdpi.png",
            "XXHDPI": "Icon-xxhdpi.png",
            "XXXHDPI": "Icon-xxxhdpi.png"
        }
    }
}
Android.Key

서명을 참조하십시오.

Android.Geo.ApiKey

MapView와 함께 사용하기위한 Google Maps API 키입니다. 자세한 정보는 아래에서 찾을 수 있습니다.

{
    "Android": {
        "Geo": {
            "ApiKey": "<귀하의 Google지도 API 키>"
        }
    }
}
Android.NDK.PlatformVersion

사용할 NDK 플랫폼 버전. 기본값은 9입니다.

{
    "Android": {
        "NDK": {
            "PlatformVersion": 9
        }
    }
}

Android.SDK

빌드 할 Android SDK의 버전 제약 조건을 지정합니다. 다음 예제에서는 각 속성의 기본값을 보여줍니다.

{
    "Android": {
        "SDK": {
            "BuildToolsVersion": "23.0.0",
            "컴파일 버전": 19,
            "MinVersion": 10,
            "TargetVersion": 19
        }
    }
}
iOS.BundleIdentifier

iOS 번들 식별자입니다. 기본값은 $ (QIdentifier)입니다.

CFBundleIdentifier에 해당합니다.

{
    "iOS": {
        "BundleIdentifier": "com.mycompany.myapp"
    }
}
iOS.PreviewBundleIdentifier

미리보기 모드의 iOS 번들 식별자입니다. 기본값은 iOS.BundleIdentifier입니다. 퓨즈 미리보기 -t = iOS 중에 만 사용되어 일반 번들과 미리보기 번들을 구별합니다. 앱에 미리보기 버전과 내 보낸 버전을 동시에 설치하려면이 설정을 사용하십시오.

{
    "iOS": {
        "PreviewBundleIdentifier": "com.mycompany.myapp.preview"
    }
}
iOS.BundleName

iOS 전용 응용 프로그램에 대한 사용자가 읽을 수있는 레이블입니다. 이것은 Android.ApplicationLabel에 해당하는 iOS입니다. CFBundleName에 해당합니다. 기본값은 $ (제목)입니다.

{
    "iOS": {
        "BundleName": "MyAwesomeApp"
    }
}
iOS.BundleVersion

버전과 동일하지만 iOS에만 적용됩니다. 기본값은 $ (Version)입니다.

CFBundleVersion에 해당합니다.

{
    "iOS": {
        "BundleVersion": "0.5.2"
    }
}
iOS.DeploymentTarget

앱을 실행할 수있는 최소 iOS 버전입니다. 기본값은 8.0입니다.

{
    "iOS": {
        "DeploymentTarget": "8.0"
    }
}
iOS.Icons

모든 플랫폼에서 사용할 균일 한 크기의 아이콘을 제공하는 대신 iOS에 고유 한 다양한 크기 및 화면 밀도에 대해 다른 아이콘을 지정할 수 있습니다. 모두 기본값은 $ (Icon)입니다.

CFBundleIconFiles에 해당합니다.

참고 : 이것은 iOS에만 적용되지만 Android에서는 Android.Icons를 사용하여 동일한 결과를 얻을 수 있습니다.

{
    "iOS": {
        "아이콘": {
            "iPhone_29_2x": "Icon-iPhone-29@2x.png",
            "iPhone_29_3x": "Icon-iPhone-29@3x.png",
            "iPhone_40_2x": "Icon-iPhone-40@2x.png",
            "iPhone_40_3x": "Icon-iPhone-40@3x.png",
            "iPhone_60_2x": "Icon-iPhone-60@2x.png",
            "iPhone_60_3x": "Icon-iPhone-60@3x.png",
            "iPad_29_1x": "Icon-iPad-29@1x.png",
            "iPad_29_2x": "Icon-iPad-29@2x.png",
            "iPad_40_1x": "Icon-iPad-40@1x.png",
            "iPad_40_2x": "Icon-iPad-40@2x.png",
            "iPad_76_1x": "Icon-iPad-76@1x.png",
            "iPad_76_2x": "Icon-iPad-76@2x.png"
        }
    }
}
iOS.LaunchImages

iOS에서 사용할 다양한 크기의 이미지를 시작하도록 지정합니다. UILaunchImages에 해당합니다.

{
    "iOS": {
        "LaunchImages": {
            "iPhone_Portrait_2x": "...", // 640x960
            "iPhone_Portrait_R4": "...", // 640x1136
            "iPhone_Portrait_R47": "...", // 750x1334
            "iPhone_Portrait_R55": "...", // 1242x2208
            "iPhone_Landscape_R55": "...", // 2208x1242
            'iPad_Portrait_1x': '...', // 768x1024
            'iPad_Portrait_2x': '...', // 1536x2048
            "iPad_Landscape_1x": "...", // 1024x768
            "iPad_Landscape_2x": "..."// 2048x1536
        }
    }
}
iOS.PList

iOS 번들의 Info.plist 파일에 포함될 항목 사전입니다. 모든 속성 목록 항목이 작동하지는 않습니다. 여기에 포함 된 항목 만 작동합니다.

iOS.PList.MKDirectionsApplicationSupportedModes

앱이지도 방향을 제공 할 수있는 교통 수단의 모드를 지정하는 문자열 배열입니다. 참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "MKDirectionsApplicationSupportedModes": [
                "MKDirectionsModeCar",
                "MKDirectionsModeBus"
            ]
        }
    }
}
iOS.PList.UIRequiredDeviceCapabilities

앱이 작동하기 위해 사용할 수있는 장치 관련 기능을 지정하는 문자열 배열입니다. 참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UIRequiredDeviceCapabilities": [
                "카메라 플래시"
                "gps"
            ]
        }
    }
}
iOS.PList.NSHealthShareUsageDescription

앱이 HealthKit 데이터를 요청할 때 사용자에게 표시되는 메시지입니다.

{
    "iOS": {
        "PList": {
            "NSHealthShareUsageDescription": "HealthKit 데이터에 액세스해야하기 때문에 ..."
        }
    }
}
iOS.PList.UIApplicationExitsOnSuspend

true로 설정하면 사용자가 집 버튼을 누를 때 앱이 백그라운드로 보내지 않고 종료됩니다. 기본값은 false입니다.

참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UIApplicationExitsOnSuspend": false
        }
    }
}
iOS.PList.UIFileSharingEnabled

컴퓨터에 연결되었을 때 iTunes를 통해 파일을 공유 할 수있는 응용 프로그램을 지정합니다. 기본값은 false입니다.

참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UIFileSharingEnabled": true
        }
    }
}
iOS.PList.UINewsstandApp

iOS 뉴스 스탠드 앱에서 앱의 콘텐츠를 표시하도록 지정합니다. 기본값은 false입니다.

참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UINewsstandApp": 사실
        }
    }
}
iOS.PList.UIPrerenderedIcon

{
    "iOS": {
        "PList": {
            "UIPrerenderedIcon": true
        }
    }
}
iOS.PList.UISupportedExternalAccessoryProtocols

앱이 통신 할 수있는 외부 액세서리 프로토콜을 지정하는 문자열 배열입니다. 기본값은 []입니다.

참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UIS가 지원하는 ExternalAccessoryProtocols": [
                "내 외부 액세서리 프로토콜"
            ]
        }
    }
}
iOS.PList.UIViewEdgeAntialiasing

픽셀 경계에 정렬되지 않은 레이어를 그리는 경우 Core Animation 레이어에서 에일리어싱을 사용할지 여부를 지정합니다. 기본값은 false입니다.

참고 자료는 여기에서 찾을 수 있습니다.

{
    "iOS": {
        "PList": {
            "UIViewEdgeAntialiasing": true
        }
    }
}
