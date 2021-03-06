원본: [https://www.fusetools.com/docs/features](https://www.fusetools.com/docs/features)

# 기능 상태

베타 버전을 통해 진화하는 현재 및 미래의 Fuse 기능을 추적하십시오.

> 이 목록은 완전한 것이 아니며, 현재는 좀 오래되었입니다. 현재 진행중인 로드맵에 대한 의견을 교환하고 수집 할 수있는 새로운 전략을 결정 중입니다.

###### 최종업데이트: 2016년 8월 6일

## 범례

| 상태 |  |
| --- | --- |
| Ready | 앱 제작을 위해 사용 될 준비가 된 기능입니다. Ready로 표시된 기능에 문제가 발생하면 Fuse 팀은 버그 보고서를 신속하게 처리합니다. |
| In progress | 적극적으로 작업 중인 기능입니다. |
| Planned | 계획 중에 있으며 곧 작업이 시작될 기능입니다. |
| Backlog | 미래에 대한 todo 목록에 있지만, 구현은 아직 계획되지 않은 기능입니다. |
| N/A | 대상 플랫폼이나 환경에 구현할 수 없거나 바람직하지 않은 기능입니다. |

## 크로스 플랫폼 UX 컴포넌트

이 UX 컴포넌트는 크로스 플랫폼 앱 화면들을 만들기 위한 기본 컴포넌트들입니다. 이러한 컴포넌트들은 `Native` 모드와 `Graphics` 모드 모두에서, 모든 테마들이 작동합니다. 특정 컨트롤\(버튼과 같은\) 의 look and feel\(모양과 느낌\) 은 테마에 따라 서로 다를 수 있습니다.

|  | 그래픽스 모드 | 네이티브 iOS | 네이티브 Android |
| --- | --- | --- | --- |
| [Layout](https://www.fusetools.com/docs/layout/layout) - 앱 화면들에 대해 유연한, 반응형 레이아웃을 정의합니다. | Ready | Ready | Ready |
| [Image](https://www.fusetools.com/docs/fuse/controls/image) - 모든 화면 크기 및 밀도에 적용할 수 있도록 다중 밀도를 지원하는 이미지 및 아이콘을 표시합니다. | Ready | Ready | Ready |
| [Video](https://www.fusetools.com/docs/fuse/controls/video) - 미디어 플레이어 또는 UI 디자인의 일부로 비디오를 재생합니다. | Ready | Planned | Planned |
| [Navigation](https://www.fusetools.com/docs/fuse/navigation/navigation) - 범용 크로스 플랫폼 네비게이션 시스템으로, 커스텀 제스처-기반으로 애니메이션된 내비게이션을 앱의 어느 곳에나 추가 할 수 있도록 합니다. | Ready | Ready | Ready |
| [Text](https://www.fusetools.com/docs/fuse/controls/text)Edit - 테마나 스타일링 없는 일반 텍스트 입력 필드입니다. | Ready | Ready | Ready |
| [TextInput](https://www.fusetools.com/docs/fuse/controls/textinput) - 테마의 look and feel 로 장식된 텍스트 입력 필드입니다. | Ready | Ready | Ready |
| [Button](https://www.fusetools.com/docs/fuse/controls/button) - 클릭 가능한 기본 컨트롤입니다. | Ready | Ready | Ready |
| [Slider](https://www.fusetools.com/docs/fuse/controls/slider) - 트랙과 노브로 숫자 값을 지정할 수 있습니다. | Ready | Ready | Ready |
| [Switch](https://www.fusetools.com/docs/fuse/controls/switch) - on/off boolean 값을 지정할 수 있습니다. | Ready | Ready | Ready |
| CameraFeed - 디바이스 카메라의 live feed 를 UI 요소로 표시합니다. | Planned | N/A | N/A |

## 네이티브 UX 컴포넌트들

이 UX 컴포넌트들은  Fuse에서 `Native` 테마, 또는 `NativeViewHost` 를 통해 사용할 수 있습니다. `Graphics` 테마에서는 사용할 수 없으며, Fuse 효과 시스템과 상호 작용할 수 없습니다.

해당 기능, 함수 및 외형은 플랫폼에 따라 서로 다를 수 있습니다.

|  | iOS | 안드로이드 |
| --- | --- | --- |
| [WebView](https://www.fusetools.com/docs/fuse/controls/webview) - 앱에 브라우저를 표시하거나, HTML5를 사용하여 앱의 특정 부분을 만들 수 있습니다. | Ready | Ready |
| [MapView](https://www.fusetools.com/docs/fuse/controls/mapview) - OS 네이티브 맵들을 표시합니다. | Ready | Ready |
| Native Pickers - 기본 OS 선택도구, 날짜 선택도구, 값 선택도구 등을 표시합니다. | Planned | Planned |
| Native UI Controls - 기타 네이티브 컨트롤들과 위젯들. 모달 대화상자, 탭바 등을 다룹니다. 기본 기술이 준비되었을 때 게시할 세부 기능-목록 입니다. | In progress | In progress |
| Audio - UX 마크 업 트리거들의 일부로 사운드를 재생합니다. | Planned | Planned |
| iOS Navigation - iOS 내장 네비게이터 컨트롤들 및 네이티브 트랜지션들을 기반으로 네이티브 네비게이션 시스템을 만듭니다. | Planned | N/A |
| Android Material Design Navigation - Google에서 제공하는 Material Design 컴포넌트 팩을 기반으로 기본 네비게이션 시스템을 만듭니다. | N/A | Planned |

## FuseJS 기능들

|  | 네이티브 iOS | 네이티브 안드로이드 |
| --- | --- | --- |
| [Observable](https://www.fusetools.com/docs/fusejs/observable) - Reactive 데이터 바인딩 | Ready | Ready |
| [XMLHttpRequest](https://www.fusetools.com/docs/fusejs/http) | Ready | Ready |
| [fetch\(\)](https://www.fusetools.com/docs/fusejs/http) | Ready | Ready |
| [Push Notifications](https://www.fusetools.com/docs/fuse/pushnotifications/push) | Ready | Ready |
| [Local Notifications](https://www.fusetools.com/docs/fuse/localnotifications/localnotify) | Ready | Ready |
| [Take photo with device camera](https://www.fusetools.com/docs/fuse/camera/camera) | Ready | Ready |
| [Geolocation](https://www.fusetools.com/docs/fuse/geolocation/geolocation) | Ready | Ready |
| [Local Storage](https://www.fusetools.com/docs/fuse/storage/storagemodule) | Ready | Ready |
| WebSocket | In progress | In progress |

## 네이티브 상호운용

Fuse에는 기존 네이티브 코드와 써드파티 SDK들이 네이티브 API들과 상호 작용할 수 있는 여러 방법들이 있습니다.

|  | 네이티브 iOS | 네이티브 안드로이드 |
| --- | --- | --- |
| Foreign Code - 네이티브 Java, Objective-C 및 C++ 코드를 Fuse 프로젝트에 직접 넣고, FuseJS 의 새로운 기능으로 노출합니다. | Ready | Ready |
| Uno Inline Foreign Code - Uno 메소드들을 네이티브 플랫폼 언어로 직접 구현합니다: Java, Objective-C 또는 C++. | Ready | Ready |
| Uno Native Foreign Bindings - Uno에서 직접 Android 및 iOS API를 일반 구문으로 호출 합니다. | Ready \(삭제 예정\) | Ready \(삭제 예정\) |
| [Uno Extension Layer \(UXL\)](https://www.fusetools.com/docs/technical-corner/uxl-handbook) - Uno 와 네이티브 타겟 언어 사이에 고급 상호운용을 위한 낮은-수준의 다양한 메서드들입니다. | Ready \(삭제 예정\) | Ready \(삭제 예정\) |

## 써드파티 라이브러리 래퍼

Fuse는 가장 인기있는 써드파티 SDK들을, 사용하기 쉬운 UX/JS API 들로 래핑 합니다. 많은 써드파티 JavaScript SDK들을 즉시 사용할 수 있습니다.  

공식적으로 Fuse 에 의해 래핑되지 않은 SDK 및 API 들은 [Native Interop](https://www.fusetools.com/docs/native-interop/native-interop) 을 통해 계속 사용될 수 있습니다.

|  | iOS | 안드로이드 |
| --- | --- | --- |
| Parse \(JavaScript SDK\) - client-side JS를 통한 여러분 앱의 푸시 및 분석 데이터 | 그대로 동작함 | 그대로 동작함 |
| Facebook SDK Wrapper - FuseJS API들 및 UX 마크업 컴포넌트들로 공개된 Facebook SDK의 기본 기능들 | Planned | Planned |
| [SQLite Wrapper](https://github.com/bolav/fuse-sqlite) - 디바이스 내 로컬 데이터베이스 래퍼. *현재 구현은 서트파티에 의해 제공된것이며, 완전한 통합은 계획 중입니다.* | Ready | Ready |

## 실시간 그래픽스 효과들

실시간 그래픽스 효과는, 실시간으로 UI 요소들에 GPU 효과를 적용하기 위한 Fuse 의 강력한 그래픽스 모드의 이점을 활용합니다. 이를 통해 디자이너가 시각적 표현들, 애니메이션들 및 트랜지션들을 갖출 수 있도록 새로운 가능성의 새 영역을 제공합니다.

|  | 그래픽스 모드 |
| --- | --- |
| Blur - 모든 UI 요소에 실시간 흐림을 적용합니다. | Ready |
| DropShadow - 모든 UI 요소에 실시간 소프트 그림자를 넣으십시오. | Ready |
| Desaturate - 트랜지션 또는 영구 효과로 모든 요소를 ​​회색 음영으로 페이드인 합니다. | Ready |
| HalfTone - 설정 가능한 하프 톤 패턴을 모든 UI 요소에 적용합니다. | Ready |
| Glass - 모든 UI 요소의 배경에 실시간 blur 및 색상 등급을 적용하여 유리 모양을 나타냅니다. | Planned |
| ColorCorrect - 모든 UI 요소에 색상 보정을 실시간으로 적용하십시오. | Planned |
| Custom Shader Effects - Fuse 이펙트 시스템은 일반화되어 있으며, 우리는 Uno 코드로 커스텀 효과를 쉽게 만들 수 있습니다. 기존 효과들 중 하나를 포크하고 [Effect](https://www.fusetools.com/docs/fuse/effects/effect) 클래스를 확장하여 GPU 셰이더 효과들을 만듭니다. | Ready |

> GPU 효과에는 성능 비용이 있으며, 모든 조합들과 매개 변수들이 모든 디바이스들에 적합하지 않을 수 있습니다. 그러나 신중하게 잘 적용된 모든 효과들은 저가형 디바이스에서도 60 FPS로 처리 될 수 ​​있습니다. 덜 강력한 디바이스에서는 앱이 중지될 수 있으므로, 앱의 필수 기능들에는 효과들을 사용하지 않는 것이 좋습니다.

## Fuse 도구들

|  | 상태 |
| --- | --- |
| Fuse CLI Tools - Fuse 프로젝트들을 생성하고 빌드하고 미리보기하기 위한 `fuse` 커맨드 | Ready |
| Fuse Desktop Preview - 부드럽게 60 FPS의 성능으로 macOS 및 Windows에서 응용 프로그램을 미리 봅니다. iOS 또는 Android 에뮬레이터/시뮬레이터가 필요하지 않습니다. | Ready |
| Fuse On-Device Preview - 실제 iOS 및 Android 디바이스에서 앱을 미리 봅니다. | Ready |
| Fuse Monitor - 데스크톱, 디바이스 또는 에뮬레이터에서 미리보기로 실행되는 앱의 모든 출력 및 진단 정보를 수집합니다. | Ready |
| Sketch Importer - `fuse import` 명령은 .sketch 파일에서 UX 클래스 및 이미지 자원을 자동으로 만들고 업데이트합니다. | Ready |
| Fuse Package System - Fuse 패키지들, 컴포넌트들, 프로젝트 파일들을 더 쉽게 공유. | In progress |
| Fuse Profiler - UX 마크업 노드들의 실시간 성능 데이터를 시각화하여, UI 성능 병목 현상을 쉽게 식별할 수 있습니다. | Planned |

## 뭔가 놓친것이 있습니까?

우리의 기능 로드맵에 없지만 당신이 있어야 한다고 느끼는 뭔가가 있다면, 부끄러워하지말고 [기능 요청](https://www.fusetools.com/community/forums/feature_requests) 을 통해 우리에게 알려주십시오. 우리는 여러분 의견을 더 듣는것을 기쁘게 생각합니다.
