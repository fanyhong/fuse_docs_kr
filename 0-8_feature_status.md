
원본: https://www.fusetools.com/docs/features

# 특성 #
현재 그리고 곧 다가오는 저희 베타 버전을 통해 진화하는 퓨즈의 특징들을 기록합니다.
> 이 리스트는 완벽한 것을 의미하는 것은 아니고 현재 약간 뒤쳐져 있음을 밝힙니다. 저희는 현재 저희가 앞으로 가야할 로드맵에 대한 피드백을 모으고 소통하기 위한 새로운 전략을 결정 중애 있습니다. 
###### 최종업데이트: 2016년 8월 6일 ######

## 범례 ##
| | |
|-|-|-|
| Ready	| 앱 제작을 위해 준비가 된 기능입니다. 여러분에게 Ready 로 마크된 기능의 문제가 발생해한다면, 퓨즈팀은 버그 리포트를 빠르게 처리할 것입니다.  |
| In progress | 활발히 작업 상태에 있는 기능입니다. |
| Planned | 곧 시작할 작업으로 계획된 기능입니다. |
| Backlog | 기능을 위한 저희의 할일목록이지만, 구현은 아직 계획되어 있지 않습니다. |
| N/A | 타겟 플랫폼 혹은 설정에 있어서 구현할 수 없거나 가능하지 않은 기능입니다. |

## 크로스플랫폼 UX 컴포넌트들 ##
이 UX 컴포넌트들은 크로스플랫폼 앱 화면을 위한 기본 빌딩 블락들 입니다.이 컴포넌트들은 `Native` 모드와 `Graphics` 모드에서 모든 테마들을 거쳐 동작 합니다. 버튼 같은 명확한 컨트롤러들의 룩앤필은 아마 다른 테마들에서 다양할 것입니다.

| | 그래픽스 모드 | 네이티브 iOS | 네이티브 안드로이드 |
|-|-|-|-|-|
| Layout - 유연성 및 앱화면에 대한 반응형 레이아웃 정의 | Ready | Ready | Ready |
| Image - 어떤 화면 크기와 밀도에도 적용하되는 것을 지원하기 위한 다중-밀도로 이미지들과 아이콘들을 출력 | Ready | Ready | Ready |
| Video - 여러분 UI 디자인의 부분으로써나 미디어플레이어로써 비디오 플레이 | Ready | Planned | Planned |
| Navigation - 여러분 앱의 어떤 일부분에 커스텀 제스쳐-드리븐 애니메이션을 추가하도록 하게 하는 일반적인 목적의 크로스플랫폼 네이게이션 시스템 | Ready | Ready | Ready |
| TextEdit - 테마나 스타일링이 없는 입력 텍스트 필드 | Ready | Ready | Ready |
| TextInput - 테마로부터의 룩앤필로 꾸며진 입력 텍스트 필드 | Ready | Ready | Ready |
| Button - 클릭이 가능한 기본 컨트롤 | Ready | Ready | Ready |
| Slider - 트랙과 손잡이로 숫자 값을 지정할 수 있는 컨트롤 | Ready | Ready | Ready |
| Switch - 온/오프 불리언 값을 지정할 수 있는 컨트롤 | Ready | Ready | Ready |
| CameraFeed - UI 엘리먼트로써 디바이스카메라에서 라이브 피드를 출력 | Planned | N/A | N/A |


## 네이티브 UX 컴포넌트들 ##
이 UX 컴포넌트들은 퓨즈에서`Native` 테마와 `NativeViewHost`를 통해 가능합니다. 그들은 `Graphics` 테마들에서는 가능하지 않고, 퓨즈 이펙티브 시시템으로 상호작용 할 수 없습니다.
그것들의 특성들은, 아마 플랫폼들 사이에서 다양하게 기능하고 나타날 것입니다.

| | iOS | 안드로이드 |
|-|-|-|-|
| WebView - 앱 내 브라우져를 보여주거나, HTML5응 사용하고 있는 앱의 어떤 부분들을 생성 | Ready | Ready |
| MapView - OS 네이티브 앱을 출력 | Ready | Ready |
| Native Pickers - 네이티브 픽커들, 날짜 픽커, 값 픽커 등등을 표현 | Planned | Planned |
| Native UI Controls - 다른 네이티브 컨트롤들과 위젯들. 모달 다이얼로그들, 탭 바 같은 것들을 포함. 근원적인 기술이 준비 되었을때 자세한 기능-목록이 출력됨  | In progress | In progress |
| Audio - UX 마크업 트리거들의 부분으로써 소리 재생 | Planned | Planned |
| iOS Navigation - iOS 내장 네비게이터 컨트롤들 과 네이티브 트랜지션들에 기반한 네이티브 네비게이션 시스템을 생성 | Planned | N/A |
| Android Material Design Navigation - 구글에 의해 제공되는 머트리얼 디자인 컴포넌트 팩에 기반한 네이티브 네비게이션 시스템을 생성  | N/A | Planned |

## FuseJS 기능들 ###
| | 네이티브 iOS | 네이티브 안드로이드 |
|-|-|-|-|
| Observable - 반응형 데이터 바인딩 | Ready | Ready |
| XMLHttpRequest | Ready | Ready |
| fetch() | Ready | Ready |
| Push Notifications | Ready | Ready |
| Local Notifications | Ready | Ready |
| Take photo with device camera | Ready | Ready |
| Geolocation | Ready | Ready |
| Local Storage | Ready | Ready |
| WebSocket | In progress | In progress |

## 네이티브 상호운용 ##
퓨즈는 네이티브 API들, 존재하는 네이티브 코드와 서드파티 SDK들과 상호운용될 수 있는 여러가지 방법을 가지고 있습니다.
| | 네이티브 iOS | 네이티브 안드로이드 |
|-|-|-|
| Foreign Code - 네이티브 자바, 오브젝티브-C, C++ 코드를 여러분의 퓨즈 프로젝트에 바로 넣고 FuseJS 에 새로운 기능들을 노출시킴 | Ready | Ready |
| Uno Inline Foreign Code - Uno 메서드들을 자바, 오브젝티브-C, C++ 인 네이티브 플랫폼 언어로 직접 구현 | Ready | Ready |
| Uno Native Foreign Bindings - Uno 원래 문법 으로부터 바로 어떤 안드로이드와 iOS API를 호출 | Ready (삭제 예정) | Ready (삭제 예정) |
| Uno Extension Layer (UXL) - Uno와 네이티브 타겟 언어 사이에 고급 상호운용을 위한 낮은-수준의 다양한 메서드들 | Ready (삭제 예정) | Ready (삭제 예정) |

## 써드파티 라이브러리 래퍼들 ##
퓨즈는 대부분의 유명한 써드파티 SDK들을 UX 및 JS API들을 사용하여 쉽게 감쌀 것입니다. 많은 써드-파티 자바스크립트 SDK들은 그 박스 밖에서 동작합니다. 
퓨즈에 의해 공식적으로 래핑되지 않은 SDK들 과 API들은 여전히 네이티브 상호운용을 통해 사용되어 질 수 있습니다.
| | iOS | 안드로이드 |
|-|-|-|
| Parse (JavaScript SDK) - 클라이언트측 JS를 통한 앱을 위한 데이터, 푸시 와 분석들 | 그대로 동작함 | 그대로 동작함 |
| Facebook SDK Wrapper - 페이스북 SDK들로 부터 FuseJS API들과 UX마크업 컴포넌트들로 기본 기능들을 노출 | Planned | Planned |
| SQLite Wrapper - 디바이스 내 로컬 데이터베이스를 래핑. 현재 구현은 서트파티에 의해 제공된것이며, 전체 통합을 계획 중임 | Ready | Ready |

## 실시간 그래픽스 효과들 ##
실시간 그래픽스 효과들은 실시간 UI 엘리먼트들의 GPU 효과들을 적용하기위한 퓨즈의 강력한 그래픽스 모드의 장점을 가집니다. 이 것은 디자이너들에게 애니메이션들과 트랜지션들 같은 비주얼 표현들들을 만드는데 새로운 가능성의 영역을 줍니다.
| | 그래픽스 모드 |
|-|-|
| Blur - 어떤 UI 엘리먼트에 실시간 blur 를 적용 | Ready |
| DropShadow - 실시간 부드러운 그림자를 어떤 UI 요소에 배치 | Ready |
| Desaturate - 어떤 엘리먼트에 전환 혹은 영속적인 효과로써 그레이스케일로 페이드 | Ready |
| HalfTone - 어떤 UI 엘리먼트에 하프톤 설정을 적용 | Ready |
| Glass - 유리로 나타낼 UI 엘리먼트의 배경에 실시간 blur 와 색등급을 적용 | Planned |
| ColorCorrect - 실시간으로 어떤 UI 엘리먼트에 색보정을 적용 | Planned |
| Custom Shader Effects - 퓨즈 이펙트 시스템은 일반적이고 우리는 사용자 효과를 Uno 코드로 쉽게 만들 수 있습니다. 존재하는 효과들 중 하나를 고르거나 GPU 쉐이더 효과들을 생성하기위한 Effect 클래스를 확장합니다. | Ready |

> 성능에 대한 비용과 모든 조합들과 파라메터들을 가지고 있는 GPU 이펙티브들은 아마도 모든 디바이스들에서는 적합하지 않을 수 있습니다. 그러나, 모든 적용되어 관리되는 이펙티브들은 최저사양 디바이스들 에서도 초당 60 프레임으로 처리될 수 있습니다. 여러분 앱의 중요한 기능에 이펙티브를 사용하는 것이 추천되어지지느 않습니다, 여러분은 아마 비교적 성능이 좋지 않은 디바이스들에서는 사용하지 않을 필요가 있을 것입니다.  

## 퓨즈 툴스 ##
| | 상태 |
|-|-|
| Fuse CLI Tools - 퓨즈 프로젝트들을 생성하고 빌드하고 미리보기하기 위한 `fuse` 커맨드 | Ready |
| Fuse Desktop Preview - OSX 와 윈도우즈에서 부드럽운 초당 60프레임 성능의 앱 미리보기. iOS 나 안드로이드 에뮬레이터나 시뮬레이터가 필요하지 않음 | Ready |
| Fuse On-Device Preview - iOS 와 안드로이드 물리적인 디바이스에서 실시간 미리보기 | Ready |
| Fuse Monitor - 데스크탑, 디바이스, 에뮬레이터들에서 미리보기 실행 중인 앱들의 모든 출력과 진단 정보 | Ready |
| Sketch Importer - `fuse import` 커맨드로 자동적으로 UX 클래스들과 이미지 리소스들을 .sketch 파일로 부터 생성 및 업데이트 | Ready |
| Fuse Package System - 퓨즈 패키지들, 컴포넌트들, 프로젝트 파일들을 더 쉽게 공유 | In progress |
| Fuse Profiler - UX 마크업 노드들에 대한 실시간 성능 데이터를 시각화하여 UI 성능 병목현상을 쉽게 확인 | Planned |

## 뭔가 놓친것이 있습니까? ##
우리의 기능 로드맵에 없지만 당신이 있어야 한다고 느끼는 뭔가가 있다면, 부끄러워하지말고 기능 요청 을 통해 우리에게 알려주세요. 우리는 여러분 의견을 더 듣는것을 기쁘게 생각합니다. 

---
