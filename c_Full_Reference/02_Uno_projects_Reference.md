원본: https://www.fusetools.com/docs/basics/uno-projects

# Uno 프로젝트 (.unoproj) #

Uno는 Fuse의 핵심에 놓인 C# 의 파생문법으로 포터블하고 가벼우며, 여러분이 iOS 및 Android 용 네이티브 앱들로 내보내기 할 수 있게 합니다. Uno에 대해 더 알고 싶다면, [Native Interop](https://www.fusetools.com/docs/native-interop/native-interop) 섹션을 참조하십시오.

Fuse 프로젝트들은 기본적으로 Fuse 라이브러리들을 참조하는 Uno 프로젝트들입니다. 퓨즈 라이브러리들은 Uno 언어로 작성되었습니다.

Uno 프로젝트 파일 (.unoproj) 은, 이런 기본 구조를 가진 일반 JSON 텍스트 파일입니다.

```
{
    "Packages": [
        "Fuse",
        "FuseJS"
    ],
    "Includes": [
        "*"
    ]
}
```

Uno 프로젝트 파일은 여러분 프로젝트가 무엇에 의존하고, 무엇으로 구성되어 있으며, iOS 또는 Android의 네이티브 앱으로 어떻게 패키징할지를 선택적으로 지정하는 곳입니다.

Uno 프로젝트는 다른 프로젝트들에서 사용하기 위한 에셋과 컴포넌트들로 구성된 *라이브러리 프로젝트* 이거나 실제 *앱 프로젝트* 일 수 있습니다. 프로젝트에 [App](https://www.fusetools.com/docs/fuse/app) 클래스가 있다는 것은, 해당 프로젝트가 하나의 앱 프로젝트임을 나타냅니다.

## 패키지들 ##

`Packages` 섹션은 프로젝트에서 참조 할 패키지들 목록입니다. 패키지는 사전 컴파일된 Uno 프로젝트로서 Uno 패키지 관리자에 정식 패키지 이름으로 등록되어 있습니다.

Fuse 프로젝트를 만들려면, 보통 기본적으로 `Fuse` 및 `FuseJS` 패키지들을 참조해야 합니다. 그러나 여러분 앱을 스캐폴딩 할 때 Fuse 는 표준 패키지 세트를 포함하여 실행할 것입니다.

여러분이 참조한 패키지들은 여러분 앱이 요구하는 권한들에 영향을 줄 것입니다.

```
{
    "Packages": [
        "Fuse",
        "FuseJS"
    ]
}
```

## 매크로 ##

Uno 프로젝트 형식은 기본 매크로 시스템을 지원합니다. 이 기능은 [Title](https://www.fusetools.com/docs/basics/uno-projects#title) 이나 [Version](https://www.fusetools.com/docs/basics/uno-projects#version) 같은 다양한 최상위 속성들을 구현하는데 사용됩니다. 예를 들어 다음과 같이 `Version` 속성을 설정한다고 가정 해보십시오.:

```
{
    "Version": "0.9.4"
}
```

기본적으로, [Android.VersionName](https://www.fusetools.com/docs/basics/uno-projects#android-versionname) 과 [iOS.BundleVersion](https://www.fusetools.com/docs/basics/uno-projects#ios-bundleversion) 은 다음과 같이 설정됩니다.

```
{
    "Android": {
        "VersionName": "$(Version)"
    },
    "iOS": {
        "BundleVersion": "$(Version)"
    }
}
```

그러면 해당 속성들은 최상위 `Version` 속성을 상속할 것입니다.

## 프로젝트들 ##

`Projects` 섹션은 선택 사항이며, 로컬 파일 시스템에서 다른 Uno 프로젝트들을 참조 할 수 있도록 합니다. 다른 `.unoproj` 파일들에 대한 상대 경로로 작성합니다.

```
{
    "Projects": [
        "../../SomeOtherProject/SomeOtherProject.unoproj"
    ]
}
```

## Includes 및 Excludes ##

`Includes` 및 `Excludes` 속성들을 사용하여 여러분 프로젝트에 포함할 파일들을 제어할 수 있습니다. 그것들은 아래의 스니펫(snippet)과 같이, 문자열들의 배열들입니다.

```
{
    "Includes": [
        "*.ux",
        "js/**/*.js",
        "SomeUnoClass.uno:Source",
        "ForeignCode.java:Java:android"
    ],
    "Excludes": [
        "js/ExcludeThisFilePlease.js",
        "node_modules/"
    ]
}
```

Include/Exclude 항목은 아래의 양식들 중 하나를 사용할 수 있습니다.

## Glob 패턴 ##

이 문서에서 glob 패턴에 대해 자세히 살펴봅니다.

```
{
    "Includes": [
        "*.ux",
        "js/**/*.js",
        "Foo.uno"
    ]
}
```

다음 glob 기능들이 지원됩니다.:

- Brace 확장
- 확장 된 glob 매칭
- Globstar(**) 매칭

`/` 를 포함하지 않는 패턴들은 재귀적으로 매칭됩니다.

| Pattern | Matches |
|-|-|
| `*` | 모든 파일들을 매칭합니다. |
| `*.foo` | 이름이 `.foo` 로 끝나는 모든 파일들을 매칭합니다. |
| `*.+(bar|foo)` | 이름이 `.bar` 또는 `.foo` 로 끝나는 모든 파일들을 매칭합니다. |
| `foobar` | `foobar` 라는 이름의 모든 파일들을 매칭합니다. |

재귀를 비활성화 하기위해 `/` 또는 `./` 를 추가합니다.:

| Pattern | Matches |
|-|-|
| `/*.png` | 프로젝트 디렉토리에서 직접 찾은 모든 PNG 파일들을 매칭합니다. |
| `Foo/*.png` | `Foo` 디렉토리에 있는 모든 PNG 파일들을 매칭합니다. |

명시적인 재귀를 위해서는 globstar(**) 를 사용하십시오:

| Pattern | Matches |
|-|-|
| `Foo/**/*.uno` | `Foo` 디렉토리와 하위 디렉토리들에 있는 모든 Uno 파일들을 매칭합니다. |

## FileName:Type ##

이것은 Uno 컴파일러가 하나의 파일을 포함하여, 특정 [Type](https://www.fusetools.com/docs/basics/uno-projects#allowed-include-types) 으로 해석하도록 지시합니다.

```
{
    "Includes": [
        "MainView.ux:UX"
    ]
}
```

## FileName:Type:Condition  ##

특정 타입의 파일을 추가해 해당 파일이 포함될지 포함되지 않을지 여부를 결정하는 조건을 지정할 수 있습니다.

```
{
    "Includes": [
        "AndroidOnly.java:Java:Android",
        "iOSOnly.hh:ObjCHeader:iOS",
        "iOSOnly.mm:ObjCSource:iOS"
    ]
}
```

> 허용된 Include Types

- UX - UX 마크업 파일을 정의합니다 (확장명이 .ux 인 파일).
- Source - Uno 소스 파일 (확장자가 .uno 인 파일).
- Bundle - 앱 번들에 포함될 파일이며 런타임에 액세스 할 수 있습니다. 예를 들어 PNG 또는 JPEG와 같은 이미지 파일 또는 JSON 또는 XML 같은 데이터 파일 일 수 있습니다. 또한 독립형 JavaScript 파일을 필요로 할 때 필요합니다.
- CSource - C 소스 파일.
- CHeader - C 헤더 파일.
- ObjCSource - Objective-C 소스 파일.
- ObjCHeader - Objective-C 헤더 파일.
- Java - .java 로 끝나는 파일로 Java 코드 파일
- Extensions - UXL 확장 파일.
- File - 프로젝트의 필수 부분으로 하나의 파일을 정의하는데 사용되지만, 해당 어플리케이션에서 컴파일되거나 번들될 필요는 없습니다.

# 모든 속성들 #

## Title ##

사용자가 읽을 수 있는 여러분 앱의 제목입니다. 기본값은 `$(Name)` 입니다.

*unrealisticallyLongNameForAnApp.unoproj* :

```
{
    "Title": "ShorterName"
}
```

## Description ##

사용자가 볼 수 있는 여러분 앱에 대한 설명입니다. 기본값은 비어있습니다.

```
{
    "Description": "An app that helps you organize, track and analyze cat pictures on the internet."
}
```

### Copyright ##

여러분 앱의 저작권 고지입니다. 기본값은 `Copyright (C) <current year> $(Publisher)` 입니다.

```
{
    "Copyright": "Copyright © 2003-2016 $(Publisher)"
}
```

## Publisher ##

해당 어플리케이션을 퍼블리싱하는 책임을 가진 법적주체 입니다. 기본값은 `$(Publisher)` 입니다.

```
{
    "Publisher": "VeryBusinessCorp Inc."
}
```

## Version ##

앱의 현재 버전이며 기본값은 `0.0.0` 입니다.

```
{
    "Version": "1.2.9"
}
```

## VersionCode ##

일부 플랫폼들 (현재는 Android 만 해당)에는 version 문자열 외에 추가적인 version code 가 필요합니다. 추가 문서는 [여기](http://developer.android.com/tools/publishing/versioning.html#appversioning) 에서 찾을 수 있습니다. 기본값은 `0` 입니다.

```
{
    "VersionCode": 125
}
```

## RootNamespace ##

사용할 루트 네임 스페이스입니다. 기본값은 `$(QIdentifier)` 입니다.

```
{
    "RootNamespace": "MyCompany.MyApp"
}
```

## BuildDirectory ##

다양한 빌드 설정들 및 빌드 타겟들에 대한 임시 파일들과 실행 가능 파일들을 배치 할 곳입니다. 기본값은 `build` 입니다.

```
{
    "BuildDirectory": "output"
}
```

## OutputDirectory ##

*현재 빌드 타겟* 및 *설정* 에 대한 임시 파일 및 실행 파일을 배치할 위치입니다. 기본값은 `$(BuildDirectory)/@(Target)/@(Configuration)` 입니다.

```
{
    "OutputDirectory": "$(BuildDirectory)/@(Target)/@(Configuration)"
}
```

## CacheDirectory ##

Uno의 캐시 파일들을 놓을 위치입니다. 기본값은 `.uno` 입니다.

```
{
    "CacheDirectory": ".cache"
}
```

> UnoCoreReference

UnoCore 가 참조되어져야 하는지 여부입니다. 내부적으로만 사용되므로, 여러분은 아마도 이 기능이 전혀 필요 하지 않을 것입니다. 기본값은 `true` 입니다.

```
{
    "UnoCoreReference": true
}
```

## Mobile ##

모든 모바일 타겟들에 적용되는 옵션들에 대한 딕셔너리(dictionary) 입니다.

## Mobile.UriScheme ##

여러분 앱을 실행하는데 사용될 수 있는 URI 스키마를 지정합니다.

예를 들어, `"UriScheme": "abcd"` 를 설정하면, `abcd://` URI 가 시작될때 여러분 앱을 시작될 것입니다.

```
{
    "Mobile": {
        "UriScheme": "abcd"
    }
}
```

## Mobile.KeepAlive ##

`true` 로 설정하면, 앱이 열리는 동안 화면이 어두워지지 않고 결국 꺼지게 됩니다. 기본값은 `false` 입니다.

```
{
    "Mobile": {
        "KeepAlive": false
    }
}
```

## Mobile.ShowStatusbar ##

상태표시줄을 표시하거나 표시하지 않습니다. 기본값은 `true` 입니다.

```
{
    "Mobile": {
        "ShowStatusbar": true
    }
}
```

## Mobile.RunsInBackground ##

사용자가 홈 버튼을 눌렀을때 여러분 앱이 계속 실행되어야하는지 여부를 제어합니다. 기본값은 `true` 입니다.

```
{
    "Mobile": {
        "RunsInBackground": false
    }
}
```

## Mobile.Orientations ##

허용되는 화면 방향들을 지정합니다.

다음 값들 중 하나 일 수 있습니다.

- Auto (기본값, 모든 방향이 가능)
- Portrait
- PortraitUpsideDown
- Landscape
- LandscapeLeft
- LandscapeRight

```
{
    "Mobile": {
        "Orientations": "Portrait"
    }
}
```

### Android ###

안드로이드 타겟에만 적용되는 옵션들에 대한 딕셔너리(dictionary) 입니다.

## Android.ApplicationLabel ##

Android 전용 응용 프로그램에 대해 사용자가 볼 수 있는 레이블입니다. 이것은 최상 위 레벨 [Title](https://www.fusetools.com/docs/basics/uno-projects#title) 속성과 동일한 Android 입니다. 기본값은 `$(Title)` 입니다.

```
{
    "Android": {
        "ApplicationLabel": "MyFancyApp"
    }
}
```

## Android.Description ##

[Description](https://www.fusetools.com/docs/basics/uno-projects#description) 과 동일하지만 Android 에만 해당됩니다. 기본값은 `$(Description)` 입니다.

```
{
    "Android": {
        "Description": "This is an Android-specific description!"
    }
}
```

## Android.VersionCode ##

[VersionCode](https://www.fusetools.com/docs/basics/uno-projects#versioncode) 와 동일하지만 Android 에만 해당됩니다. 기본값은 `$(VersionCode)` 입니다.

```
{
    "Android": {
        "VersionCode": 412
    }
}
```

## Android.VersionName ##

[Version](https://www.fusetools.com/docs/basics/uno-projects#version) 과 동일하지만 Android 에만 해당됩니다. 기본값은 `$(Version)` 입니다.

```
{
    "Android": {
        "VersionName": "0.5.2"
    }
}
```

## Android.Package ##

Android 내보내기(exports) 에 사용할 Java 패키지의 이름입니다. 기본값은 `$(QIdentifier)` 입니다.

```
{
    "Android": {
        "Package": "com.mycompany.myapp"
    }
}
```

## Android.PreviewPackage ##

미리보기 모드에서 Android 내보내기(export) 에 사용할 Java 패키지의 이름입니다. 기본값은 `Android.Package` 입니다. `fuse preview -t=android` 로 사용되는 동안에만, 일반 패키지와 미리보기 패키지를 구별합니다. 디바이스에 여러분 앱의 미리보기 버전과 내보내기된 버전을 동시에 설치하려면 이 설정을 사용하십시오.

```
{
    "Android": {
        "PreviewPackage": "com.mycompany.myapp.preview"
    }
}
```

## Android.Icons ##

모든 플랫폼들에서 사용될 수 있는 균일한 크기의 아이콘을 제공하는 대신, Android 별로 다양한 화면 밀도에 대해 서로 다른 아이콘들을 지정할 수 있습니다. 이들은 안드로이드의 [일반화된 밀도(generalized desities)](http://developer.android.com/guide/practices/screens_support.html#range) 에 따라 분류됩니다. 모두 기본값은 `$(Icon)` 입니다.

참고: 이것은 Android에만 적용되지만 [iOS.Icons](https://www.fusetools.com/docs/basics/uno-projects#ios-icons) 를 사용하여 iOS에서도 동일한 결과를 얻을 수 있습니다.

```
{
    "Android": {
        "Icons": {
            "LDPI": "Icon-ldpi.png",
            "MDPI": "Icon-mdpi.png",
            "HDPI": "Icon-hdpi.png",
            "XHDPI": "Icon-xhdpi.png",
            "XXHDPI": "Icon-xxhdpi.png",
            "XXXHDPI": "Icon-xxxhdpi.png"
        }
    }
}
```

## Android.Key ##

[signing](https://www.fusetools.com/docs/preview-and-export/signing) 을 참조하십시오.

## Android.Geo.ApiKey ##

[MapView](https://www.fusetools.com/docs/fuse/controls/mapview) 와 함께 사용하기위한 Google Maps API 키입니다. 자세한 정보는 아래에서 찾을 수 있습니다.

```
{
    "Android": {
        "Geo": {
            "ApiKey": "<your Google Maps API key>"
        }
    }
}
```

## Android.NDK.PlatformVersion ##

사용할 NDK 플랫폼 버전입니다. 기본값은 `9` 입니다.

```
{
    "Android": {
        "NDK": {
            "PlatformVersion": 9
        }
    }
}
```

## Android.SDK ##

빌드 할 Android SDK의 버전 제약 조건을 지정합니다. 다음 예제에서는 각 속성의 기본값을 보여줍니다.

```
{
    "Android": {
        "SDK": {
            "BuildToolsVersion": "23.0.0",
            "CompileVersion": 19,
            "MinVersion": 10,
            "TargetVersion": 19
        }
    }
}
```

## iOS.BundleIdentifier ##

iOS 번들 식별자입니다. 기본값은 `$(QIdentifier)` 입니다.

[CFBundleIdentifier](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-102070) 에 해당합니다.

```
{
    "iOS": {
        "BundleIdentifier": "com.mycompany.myapp"
    }
}
```

## iOS.PreviewBundleIdentifier ##

미리보기 모드의 iOS 번들 식별자입니다. 기본값은 `iOS.BundleIdentifier` 입니다. `fuse preview -t=iOS` 동안에만 사용되어 일반 번들과 미리보기 번들을 구별합니다. 디바이스에 여러분 앱의 미리보기 버전과 내보내기된(exported) 버전을 동시에 설치하려면 이 설정을 사용하십시오.

```
{
    "iOS": {
        "PreviewBundleIdentifier": "com.mycompany.myapp.preview"
    }
}
```

## iOS.BundleName ##

iOS용 응용 프로그램에서 사용자가 볼 수 있는 레이블입니다. 이것은 [Android.ApplicationLabel](https://www.fusetools.com/docs/basics/uno-projects#android-applicationlabel) 에 해당하는 iOS입니다. [CFBundleName](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-109585) 에 해당합니다. 기본값은 `$(Title)` 입니다.

```
{
    "iOS": {
        "BundleName": "MyAwesomeApp"
    }
}
```

## iOS.BundleVersion ##

[Version](https://www.fusetools.com/docs/basics/uno-projects#version) 과 동일하지만 iOS에만 적용됩니다. 기본값은 `$(Version)` 입니다.

[CFBundleVersion](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/20001431-102364) 에 해당합니다.

```
{
    "iOS": {
        "BundleVersion": "0.5.2"
    }
}
```

## iOS.DeploymentTarget ##

앱을 실행할 수있는 최소 iOS 버전입니다. 기본값은 8.0입니다.

```
{
    "iOS": {
        "DeploymentTarget": "8.0"
    }
}
```

## iOS.Icons ##

모든 플랫폼들에서 사용될 수 있는 균일한 크기의 아이콘을 제공하는 대신, iOS에 특정한 다양한 크기들 및 화면 밀도들에 대해 다른 아이콘들을 지정할 수 있습니다. 모두 기본값은 `$(Icon)` 입니다.

[CFBundleIconFiles](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW10) 에 해당합니다.

참고 : 이것은 iOS에만 적용되지만 Android에서는 [Android.Icons](https://www.fusetools.com/docs/basics/uno-projects#android-icons) 를 사용하여 동일한 결과를 얻을 수 있습니다.

```
{
    "iOS": {
        "Icons": {
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
```

## iOS.LaunchImages ##

iOS에서 사용할 다양한 크기들의 시작 이미지들을 지정합니다. [UILaunchImages](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) 에 해당합니다.

```
{
    "iOS": {
        "LaunchImages": {
            "iPhone_Portrait_2x": "...",   // 640x960
            "iPhone_Portrait_R4": "...",   // 640x1136
            "iPhone_Portrait_R47": "...",  // 750x1334
            "iPhone_Portrait_R55": "...",  // 1242x2208
            "iPhone_Landscape_R55": "...", // 2208x1242
            "iPad_Portrait_1x": "...",     // 768x1024
            "iPad_Portrait_2x": "...",     // 1536x2048
            "iPad_Landscape_1x": "...",    // 1024x768
            "iPad_Landscape_2x": "..."     // 2048x1536
        }
    }
}
```

## iOS.PList ##

iOS 번들의 `Info.plist` 파일에 포함될 항목들의 닥셔너리(dictionary) 입니다. 모든 속성 목록 항목이 작동하지는 않습니다. - 여기에 포함된 항목만 작동합니다.

### iOS.PList.MKDirectionsApplicationSupportedModes ###

앱이 지도의 방향을 제공할 수 있도록 하는 교통 수단의 모드를 지정하는 문자열 배열입니다. 참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW33) 에서 찾을 수 있습니다.

```
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
```

### iOS.PList.UIRequiredDeviceCapabilities ###

앱이 작동하기 위해 필요한 디바이스 관련 기능들을 지정하는 문자열 배열입니다. 참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UIRequiredDeviceCapabilities": [
                "camera-flash",
                "gps"
            ]
        }
    }
}
```

### iOS.PList.NSHealthShareUsageDescription ###

앱이 HealthKit 데이터를 요청할때 사용자에게 표시되는 메시지입니다.

```
{
    "iOS": {
        "PList": {
            "NSHealthShareUsageDescription": "We need access to your HealthKit data because..."
        }
    }
}
```

### iOS.PList.UIApplicationExitsOnSuspend ###

`true` 로 설정하면, 사용자가 홈 버튼을 누를 때 앱을 백그라운드로 보내지 않고 종료합니다. 기본값은 `false` 입니다.

참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW23) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UIApplicationExitsOnSuspend": false
        }
    }
}
```

### iOS.PList.UIFileSharingEnabled ###

컴퓨터에 연결되었을때 iTunes를 통해 파일을 공유할 수 있는 앱을 지정합니다. 기본값은 `false` 입니다.

참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW20) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UIFileSharingEnabled": true
        }
    }
}
```

### iOS.PList.UINewsstandApp ###

iOS 뉴스 스탠드 앱에서 앱의 콘텐츠를 표시하도록 지정합니다. 기본값은 `false` 입니다.

참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW25) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UINewsstandApp": true
        }
    }
}
```

### iOS.PList.UIPrerenderedIcon ###

{
    "iOS": {
        "PList": {
            "UIPrerenderedIcon": true
        }
    }
}

### iOS.PList.UISupportedExternalAccessoryProtocols ###

앱이 통신할 수 있는 외부 액세서리 프로토콜을 지정하는 문자열 배열입니다. 기본값은 `[]` 입니다.

참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW4) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UISupportedExternalAccessoryProtocols": [
                "my-external-accessory-protocol"
            ]
        }
    }
}
```

### iOS.PList.UIViewEdgeAntialiasing ###

픽셀 경계들에 정렬되지 않은 레이어를 그리는 경우, Core Animation 레이어들에서 안티앨리어싱(antialiasing) 을 사용할지 여부를 지정합니다. 기본값은 `false` 입니다.

참고 자료는 [여기](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW5) 에서 찾을 수 있습니다.

```
{
    "iOS": {
        "PList": {
            "UIViewEdgeAntialiasing": true
        }
    }
}
```
