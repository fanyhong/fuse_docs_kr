
원본: https://www.fusetools.com/docs/features

# 특성 #
베타 버전을 통해 진화하는 현재 및 미래의 Fuse 기능을 추적하십시오.
> 이 목록은 완전한 것이 아니며 현재는 오래된 것입니다. 현재 진행중인 로드맵에 대한 의견을 교환하고 의견을 수집 할 수있는 새로운 전략을 결정 중입니다.
###### 최종업데이트: 2016년 8월 6일 ######

## 범례 ##

| 상태 | |
|---|---|
| Ready	| 이 기능은 프로덕션 응용 프로그램을 사용할 준비가되었습니다. Ready로 표시된 기능에 문제가 발생하면 Fuse 팀은 버그 보고서를 신속하게 처리합니다. |
| In progress | 이 기능은 적극적으로 작업 중입니다. |
| Planned | 기능이 계획 중이고 곧 작업이 시작됩니다. |
| Backlog | 이 기능은 미래에 대한 todo 목록에 있지만 구현은 아직 계획되지 않았습니다. |
| N/A | 이 기능은 대상 플랫폼이나 구성에 구현할 수 없거나 바람직하지 않습니다. |

## 크로스플랫폼 UX 컴포넌트 ##
이 UX 컴포넌트는 교차 플랫폼 응용 프로그램 화면을 만들기 위한 기본 구성 요소입니다. 이러한 컴포넌트는 `Native` 모드와 `Graphics` 모드 모두에서 모든 테마에서 작동합니다. 버튼과 같은 특정 컨트롤의 모양과 느낌은 테마에 따라 다를 수 있습니다.

| | 그래픽스 모드 | 네이티브 iOS | 네이티브 Android |
|---|---|---|---|
| [Layout](https://www.fusetools.com/docs/layout/layout) - 앱 화면을위한 유연하고 반응이 빠른 레이아웃을 정의하십시오. | Ready | Ready | Ready |
| [Image](https://www.fusetools.com/docs/fuse/controls/image) - 모든 화면 크기 및 밀도에 적응할 수 있도록 다중 밀도를 지원하는 이미지 및 아이콘을 표시합니다. | Ready | Ready | Ready |
| [Video](https://www.fusetools.com/docs/fuse/controls/video) - 미디어 플레이어 또는 UI 디자인의 일부로 비디오를 재생하십시오. | Ready | Planned | Planned |
| [Navigation](https://www.fusetools.com/docs/fuse/navigation/navigation) - 범용, 크로스 플랫폼 네비게이션 시스템으로 사용자 정의 제스처 기반 애니메이션 내비게이션을 앱의 어느 곳에나 추가 할 수 있습니다. | Ready | Ready | Ready |
| [TextEdit](https://www.fusetools.com/docs/fuse/controls/text) - 테마 나 스타일이없는 일반 텍스트 입력 필드. | Ready | Ready | Ready |
| [TextInput](https://www.fusetools.com/docs/fuse/controls/textinput) - 테마의 Look & Feel로 장식 된 텍스트 입력 필드입니다. | Ready | Ready | Ready |
| [Button](https://www.fusetools.com/docs/fuse/controls/button) - 기본 클릭 가능한 컨트롤. | Ready | Ready | Ready |
| [Slider](https://www.fusetools.com/docs/fuse/controls/slider) - 트랙과 노브로 숫자 값을 지정할 수 있습니다. | Ready | Ready | Ready |
| [Switch](https://www.fusetools.com/docs/fuse/controls/switch) - on / off boolean 값을 지정할 수 있습니다. | Ready | Ready | Ready |
| CameraFeed - 장치 카메라의 라이브 피드를 UI 요소로 표시합니다. | Planned | N/A | N/A |


## 네이티브 UX 컴포넌트 ##
이 UX 컴포넌트들은  Fuse에서 `Native` 테마, 또는 `NativeViewHost` 를 통해 사용할 수 있습니다. `Graphics` 테마에서는 사용할 수 없으며 Fuse 효과 시스템과 상호 작용할 수 없습니다.
해당 기능, 기능 및 모양은 플랫폼에 따라 다를 수 있습니다.

| | iOS | 안드로이드 |
|---|---|---|
| [WebView](https://www.fusetools.com/docs/fuse/controls/webview) - 앱에 브라우저를 표시하거나 HTML5를 사용하여 앱의 특정 부분을 만들 수 있습니다. | Ready | Ready |
| [MapView](https://www.fusetools.com/docs/fuse/controls/mapview) - OS 네이티브 맵을 출력합니다. | Ready | Ready |
| Native Pickers - 기본 OS 선택 도구, 날짜 선택 도구, 값 선택 도구 등을 표시합니다. | Planned | Planned |
| Native UI Controls - 기타 기본 컨트롤 및 위젯. 모달 대화 상자, 탭 막대 등을 다룹니다. 기본 기술이 준비되었을 때 게시 할 세부 기능 목록입니다.  | In progress | In progress |
| Audio - UX 마크 업 트리거의 일부로 사운드를 재생합니다. | Planned | Planned |
| iOS Navigation - iOS 내장 네비게이터 컨트롤 및 네이티브 전환을 기반으로 네이티브 네비게이션 시스템을 만듭니다. | Planned | N/A |
| Android Material Design Navigation - Google에서 제공하는 Material Design 구성 요소 팩을 기반으로 기본 네비게이션 시스템을 만듭니다.  | N/A | Planned |

## FuseJS 기능들 ###

| | 네이티브 iOS | 네이티브 안드로이드 |
|---|---|---|
| [Observable](https://www.fusetools.com/docs/fusejs/observable) - Reactive 데이터 바인딩 | Ready | Ready |
| [XMLHttpRequest](https://www.fusetools.com/docs/fusejs/http) | Ready | Ready |
| [fetch()](https://www.fusetools.com/docs/fusejs/http) | Ready | Ready |
| [Push Notifications](https://www.fusetools.com/docs/fuse/pushnotifications/push) | Ready | Ready |
| [Local Notifications](https://www.fusetools.com/docs/fuse/localnotifications/localnotify) | Ready | Ready |
| [Take photo with device camera](https://www.fusetools.com/docs/fuse/camera/camera) | Ready | Ready |
| [Geolocation](https://www.fusetools.com/docs/fuse/geolocation/geolocation) | Ready | Ready |
| [Local Storage](https://www.fusetools.com/docs/fuse/storage/storagemodule) | Ready | Ready |
| WebSocket | In progress | In progress |

## 네이티브 상호운용 ##
퓨즈에는 네이티브 API, 기존 네이티브 코드 및 타사 SDK와 상호 작용할 수있는 여러 가지 방법이 있습니다.

| | 네이티브 iOS | 네이티브 안드로이드 |
|---|---|---|
| Foreign Code - 네이티브 Java, Objective-C 및 C++ 코드를 Fuse 프로젝트에 직접 입력하고 새로운 기능을 FuseJS에 표시하십시오. | Ready | Ready |
| Uno Inline Foreign Code - Uno 메소드를 네이티브 플랫폼 언어로 직접 구현하십시오: Java, Objective-C 또는 C++. | Ready | Ready |
| Uno Native Foreign Bindings - Uno에서 직접 Android 및 iOS API를 일반 구문으로 호출 할 수 있습니다. | Ready (삭제 예정) | Ready (삭제 예정) |
| [Uno Extension Layer (UXL)](https://www.fusetools.com/docs/technical-corner/uxl-handbook) - Uno와 네이티브 타겟 언어 사이에 고급 상호운용을 위한 낮은-수준의 다양한 메서드들 | Ready (삭제 예정) | Ready (삭제 예정) |

## 써드파티 라이브러리 래퍼 ##
Fuse는 가장 인기있는 타사 SDK를 사용하기 쉬운 UX / JS API로 포장합니다. 많은 타사 JavaScript SDK를 즉시 사용할 수 있습니다.
공식적으로 Fuse로 래핑되지 않은 SDK 및 API는 [Native Interop](https://www.fusetools.com/docs/native-interop/native-interop) 을 통해 계속 사용할 수 있습니다.

| | iOS | 안드로이드 |
|---|---|---|
| Parse (JavaScript SDK) - cliend-side JS를 통해 앱의 데이터, 푸시 및 분석. | 그대로 동작함 | 그대로 동작함 |
| Facebook SDK Wrapper - FuseJS API 및 UX 마크 업 구성 요소로 공개 된 Facebook SDK의 기본 기능. | Planned | Planned |
| [SQLite Wrapper](https://github.com/bolav/fuse-sqlite) - 디바이스 내 로컬 데이터베이스를 래핑. 현재 구현은 서트파티에 의해 제공된것이며, 완전한 통합을 계획 중입니다. | Ready | Ready |

## 실시간 그래픽스 효과들 ##
실시간 그래픽 효과는 실시간으로 UI 요소에 GPU 효과를 적용하기 위해 퓨즈의 강력한 그래픽 모드를 이용합니다. 이를 통해 디자이너는 시각적 표현, 애니메이션 및 전환을 형성 할 수있는 새로운 가능성을 제공합니다.

| | 그래픽스 모드 |
|---|---|
| Blur - 모든 UI 요소에 실시간 흐림을 적용합니다. | Ready |
| DropShadow - 모든 UI 요소에 실시간 소프트 그림자를 넣으십시오. | Ready |
| Desaturate - 전환 또는 영구 효과로 모든 요소를 ​​회색 음영으로 페이드인 합니다. | Ready |
| HalfTone - 구성 가능한 하프 톤 패턴을 모든 UI 요소에 적용합니다. | Ready |
| Glass - 모든 UI 요소의 배경에 실시간 흐림 및 색상 등급을 적용하여 유리 모양을 나타냅니다. | Planned |
| ColorCorrect - 모든 UI 요소에 색상 보정을 실시간으로 적용하십시오. | Planned |
| Custom Shader Effects - Fuse 이펙트 시스템은 일반화되어 있으며 Uno 코드로 사용자 정의 효과를 쉽게 만들 수 있습니다. 기존 효과 중 하나를 포크하고 [Effect](https://www.fusetools.com/docs/fuse/effects/effect) 클래스를 확장하여 GPU 셰이더 효과를 만듭니다.| Ready |

> GPU 효과에는 성능 비용이 있으며 모든 조합과 매개 변수가 모든 장치에 적합하지 않을 수 있습니다. 그러나 신중하게 적용된 모든 효과는 저가형 장치에서도 60 FPS로 처리 할 수 ​​있습니다. 덜 강력한 기기에서 앱을 사용 중지해야 할 수 있으므로 앱의 필수 기능에 효과를 사용하지 않는 것이 좋습니다.

## Fuse 도구들 ##

| | 상태 |
|---|---|
| Fuse CLI Tools - Fuse 프로젝트들을 생성하고 빌드하고 미리보기하기 위한 `fuse` 커맨드 | Ready |
| Fuse Desktop Preview - 부드럽고 60FPS의 성능으로 macOS 및 Windows에서 응용 프로그램을 미리 봅니다. iOS 또는 Android 에뮬레이터 / 시뮬레이터가 필요하지 않습니다. | Ready |
| Fuse On-Device Preview - 실제 iOS 및 Android 기기에서 앱을 미리 봅니다. | Ready |
| Fuse Monitor - 데스크톱, 장치 또는 에뮬레이터에서 미리보기로 실행되는 앱의 모든 출력 및 진단 정보를 수집합니다. | Ready |
| Sketch Importer - `fuse import` 명령은 .sketch 파일에서 UX 클래스 및 이미지 자원을 자동으로 만들고 업데이트합니다. | Ready |
| Fuse Package System - Fuse 패키지들, 컴포넌트들, 프로젝트 파일들을 더 쉽게 공유. | In progress |
| Fuse Profiler - UX 마크 업 노드의 실시간 성능 데이터를 시각화하여 UI 성능 병목 현상을 쉽게 식별 할 수 있습니다. | Planned |

## 뭔가 놓친것이 있습니까? ##
우리의 기능 로드맵에 없지만 당신이 있어야 한다고 느끼는 뭔가가 있다면, 부끄러워하지말고 [기능 요청](https://www.fusetools.com/community/forums/feature_requests) 을 통해 우리에게 알려주세요. 우리는 여러분 의견을 더 듣는것을 기쁘게 생각합니다. 

---
