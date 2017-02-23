원본: https://www.fusetools.com/docs/full-| UX |-class-reference

## 전체 UX 마크업 클래스 레퍼런스 ##

이것은 Fuse 와 함께 제공되는 UX 마크업에서 사용가능한 클래스들의 전체 목록입니다. 이 각 클래스들은 마크업 안에서 태그의 이름으로 인스턴스화 될 수 있습니다.

> Fuse는 고급 사용사례 에서 Uno 프로그래머가 사용할 수 있는 더 많은 클래스들을 제공합니다. Native Interop 섹션을 참조하십시오.

## 응용프로그램 (Application) ##

`App` 클래스는 여러분 응용 프로그램의 루트를 표시합니다.

## 노드들 (Nodes) ##

| | | |
| --- | --- | --- |
| Activated | UX | page 가 활성화 될때마다 활성화됩니다. |
| ActivatingAnimation | UX | 활성화될 요소의 애니메이션을 지정합니다. |
| AddingAnimation | UX | 요소가 시각적 트리에 추가되면 트리거 합니다. |
| AlternateRoot | UX | 트리에서 이 동작의 데이터 컨텍스트를 유지하면서 노드를 이 동작의 위치와 다른 곳에 배치할 수 있습니다. |
| Android | UX | Android 디바이스에서 실행시 트리거 됩니다. |
| Attractor<T> | UX | 속성에 물리적으로 끌려가는 듯한 애니메이션을 적용하여 목표값으로 애니메이션 합니다. |
| BackButton | UX | 뒤로가기 버튼을 표시합니다. |
| Blur | UX | 요소에 가우시안 불러를 적용합니다. |
| BottomBarBackground | UX | 화면 하단의 키보드 및 기타 OS 별 요소들이 차지하는 공간을 보완합니다. |
| BottomFrameBackground | UX ||
| Busy | UX | UX 노드를 사용 중으로 표시합니다. |
| Button | UX | 버튼을 표시합니다. |
| ButtonBase | UX | 버튼들의 기본 클래스 |
| Circle | UX | 원을 표시합니다. |
| CircularRangeBehavior | UX | 원형 RangeControls 를 구현하기 위한 helper 클래스 |
| Clicked | UX | 시각적인 요소가 클릭되면 트리거됩니다. |
| ClientPanel | UX | `ClientPanel`은 온스크린키보드, 상태표시줄 및 화면 상하단 가장자리에 있는 다른 OS별 관련 요소들로 차지되는 공간을 보완합니다. |
| Closure | UX | 해당 스코프에서 이름이 있는 UX 오브젝트들과 디펜던시들을 캡쳐했다가 준비가 되었을때 스크립트 이벤트로 전송합니다. |
| Completed | UX | 노드의 사용중 상태가 해제 될때의 pulse 입니다. |
| Container | UX | 여러분이 사용자 컨테이너를 만들수 있도록 하는, 비주얼 `Subtree` 요소들을 위해 자식 요소들을 배치하는 Panel 입니다. |
| ContainingText | UX | 제거됨: WhileContainsText 를 대신 사용하십시오. |
| ContentControl | UX | 하나의 비주얼 자식 요소를 표시합니다. |
| Deactivated | UX | page 가 비활성화 될때마다 활성화 됩니다. |
| DeactivatingAnimation | UX | 비활성 상태가 되는 요소의 애니메이션을 지정합니다. |
| Deferred | UX | 노드들의 생성을 지연하여 초기화 시간을 향상시킵니다. |
| Desaturate | UX | [요소](https://www.fusetools.com/docs/fuse/elements/element) 의 채도를 저하시킵니다. |
| Deselected | UX | 선택을 해제하면 발생됩니다. |
| DirectNavigation | UX | Navigation 요청 |
| DockPanel | UX | 자식요소를 다른 쪽으로 순서대로로 도킹하여 배치합니다. |
| DoubleClicked | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 요소를 더블클릭하면 트리거 됩니다. |
| DoubleTapped | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 요소를 더블탭하면 트리거 됩니다. |
| Draggable | UX ||
| DropShadow | UX | DropShadow 는 [요소](https://www.fusetools.com/docs/fuse/elements/element) 에 그림자를 적용합니다. |
| Each | UX | 각 아이템에 대해 지정된 템플릿을 사용하여 오브젝트들의 모음을 표시합니다. |
| EdgeNavigation | UX ||
| EdgeNavigator | UX ||
| EdgeSwipeAnimation | UX | 제거됨: `EdgeNavigator` 와 함께 `SwipeGesture` 를 대신 사용하십시오. |
| Ellipse | UX | 타원을 표시합니다. |
| EnteredForceField | UX | 드래그가 가능한 요소인 트리거들이 force 필드에 들어갑니다. |
| EnteringAnimation | UX | 활성 page의 앞에 있는 페이지의 애니메이션을 지정합니다. |
| ExitedForceField | UX | 드래그 가능한 요소인 트리거들이 force 필드를 나갑니다. |
| ExitingAnimation | UX | 활성 페이지 뒤에 있는 페이지의 애니메이션을 지정합니다. |
| GraphicsView | UX | 그래픽 렌더링된 UI 컨트롤을 담는 네이티브 뷰입니다. |
| Grid | UX | 자식요소들을 격자형태로 배치합니다. |
| Halftone | UX | 요소에 클래식 하프톤 효과를 적용합니다. |
| HierarchicalNavigation | UX ||
| Image | UX | 이미지를 표시합니다. |
| InForceFieldAnimation | UX | 포인트 어트랙터에 얼마나 가까운지에 따라서 드래그 가능한 요소를 애니메이션 합니다. |
| Instance | UX | 주어진 템플릿들의 인스턴스를 생성하고 삽입합니다. |
| InteractionCompleted | UX | 상호작용이 완료될때 트리거됩니다. |
| iOS | UX | iOS 디바이스에서 실행될때 트리거됩니다. |
| JavaScript | UX | `JavaScript` 태그는 JavaScript 를 실행하는데 사용되며, 부모 비주얼 요소에 대한 데이터 컨텍스트로써 `module.export` 를 할당합니다. |
| KeepFocusInView | UX ||
| KeepInView | UX ||
| LayoutAnimation | UX | 요소의 레이아웃이 변경될때 트리거합니다. |
| LayoutControl | UX | 레이아웃 컨트롤들은 자식요소들의 레이아웃을 수행합니다. |
| LinearNavigation | UX | Navigation 요청 |
| LinearRangeBehavior | UX | RangeControl 을 구현하기 위해 사용되는 일반적인 선형 슬라이딩 동작입니다. |
| LongPressed | UX | 포인터가 일정시간 눌려질때 트리거 합니다. |
| MapMarker | UX | [MapView](https://www.fusetools.com/docs/fuse/controls/mapview) 에 맵 마커를 추가합니다. |
| MapView | UX | 네이티브 맵 뷰를 표시합니다. |
| Mask | UX | 이미지를 [요소](https://www.fusetools.com/docs/fuse/elements/element) 에 마스킹합니다. |
| Match | UX | 일련의 상수들과 값을 비교하고 해당 상수들과 관련된 비주얼 트리를 활성/비활성화 합니다. |
| MultiLayout | UX ||
| MultiLayoutPanel | UX | `Placeholder` 클래스를 사용하여 여러분이 다른 레이아웃간에 [요소](https://www.fusetools.com/docs/fuse/elements/element) 들을 이동할 수 있도록 합니다. |
| NativeViewHost | UX | GraphicsView 위에 네이티브 컨트롤 레이어를 작성합니다. 하위트리의 컨트롤은 OS가 제공하는 네이티브 컨트롤에 매핑됩니다. |
| NavigationBar | UX ||
| Navigator | UX | 요청 받을때(on-demand) 인스턴스 생성 및 page 들을 재활용 할 수 있는 범용 네비게이션 컨테이너 입니다. |
| NavigatorSwipe | UX | 스와이프 제스처로 타겟 page 이동하는 네비게이션을 추가합니다. |
| NodeGroup | UX | 마치 직접 포함되어 있는 것처럼, 부모 요소에 직접 추가되는 여러 노드들과 리소스들을 포함하는 클래스를 만들 수 있도록 합니다. |
| Number | UX | 이전 버전과의 호환성을 위해 더 이상 사용되지 않습니다. |
| OnBackButton | UX | 뒤로가기 버튼을 누르면 트리거 됩니다. |
| OnKeyPress | UX | 특정키를 누를때 트리거 됩니다. |
| OnUserEvent | UX | [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 가 발생하면 트리거 합니다. |
| Page | UX | 네비게이션에 참여하는 page를 나타냅니다. |
| PageBeginLoading | UX | [WebView](https://www.fusetools.com/docs/fuse/controls/webview) 에서 페이지가 시작될때 트리거합니다. |
| PageControl | UX | 표준 트랜지션들, 사용자 상호작용, 그리고 기본 선형 네비게이션을 위한 page 처리 기능을 제공합니다. |
| PageIndicator | UX ||
| PageIndicatorDot | UX ||
| PageLoaded | UX | [WebView](https://www.fusetools.com/docs/fuse/controls/webview) 에서 페이지로드가 완료될때 트리거합니다. |
| PageResourceBinding<T> | UX ||
| PageView | UX | 표준 트랜지션들을 가지지 않는 특수한 [네비게이터](https://www.fusetools.com/docs/fuse/controls/navigator) 입니다. |
| Panel | UX | 모든 자식 요소들이 사용 가능한 공간 전체에 디폴트 레이아웃을 수행합니다. |
| PanGesture | UX | 패닝 (2D translation) 을 제공하는 [TransformGesture](https://www.fusetools.com/docs/fuse/gestures/transformgesture) 입니다. |
| Path | UX ||
| Placeholder | UX ||
| PointAttractor | UX ||
| Pressed | UX | 비주얼 요소들을 포인터로 누르면 트리거 됩니다. [클릭](https://www.fusetools.com/docs/fuse/gestures/clicked) 또는 [탭](https://www.fusetools.com/docs/fuse/gestures/tapped) 과는 대조적으로 이 트리거는 포인터를 시각적으로 누르면 트리거 됩니다. 포인터 해제 또는 최소 누름시간을 기다리지 않습니다. |
| ProgressAnimation | UX | [Slider](https://www.fusetools.com/docs/fuse/controls/slider) 또는 다른 호환 컨트롤이 값을 변경하면 트리거 됩니다. |
| PullToReload | UX | `ScrollView` 를 사용하여 "당겨서 다시 불러오기" 를 만들도록 돕습니다. |
| RangeAdapter<T> | UX | 애니매이션의 범위를 변경합니다. |
| RangeAnimation | UX | 프로그레스로 최소값과 최대값 사이에 고정된 값을 사용하여 애니메이션합니다. |
| RangeControl | UX | 범위값을 포함하는 컨트롤의 기본 클래스 |
| RangeControl2D | UX ||
| Rectangle | UX | 직사각형을 표시합니다. |
| RegularPolygon | UX ||
| Released | UX | 비주얼 요소에 포인터가 해제될때 트리거 합니다. |
| RemovingAnimation | UX | 부모 요소가 제거될때의 애니메이션입니다. |
| ResourceBool | UX ||
| ResourceFloat | UX ||
| ResourceFloat2 | UX ||
| ResourceFloat3 | UX ||
| ResourceFloat4 | UX ||
| ResourceObject | UX ||
| ResourceString | UX ||
| RightFrameBackground | UX ||
| RootViewport | UX ||
| RotateGesture | UX | 회전을 제공하는 [TransformGesture](https://www.fusetools.com/docs/fuse/gestures/transformgesture) 입니다. |
| Rotation | UX | 요소를 지정된 각도만큼 회전합니다. |
| Router | UX | Fuse 앱 전체 혹은 일부에 대한 라우팅 및 내비게이션 히스토리를 관리합니다. |
| Scaling | UX | 지정된 요소로 요소를 확대 또는 축소 합니다. |
| Scrolled | UX | ScrollView 가 특정한 영역으로 스크롤 될때 트리거 합니다. |
| Scroller | UX ||
| ScrollingAnimation | UX | 주어진 스크롤 범위에 애니메이션을 적용합니다. |
| ScrollView | UX | 사용 가능한 크기보다 큰 내용을 탐색하는데 사용됩니다. |
| ScrollViewBase | UX | `ScrollView` 는 내용을 스크롤 할 수 있는 컨트롤 입니다. 스크롤 할 수 있는 영역의 크기가 계산되는 하나의 자식 요소만 허용됩니다. 해당 자식 요소는 하나의 단일 요소 이거나 컨트롤들의 UX 트리가 될 수 있습니다. |
| Select | UX | 제거됨. |
| Selectable | UX | `Selectable` 은 비주얼 요소를 선택이 가능하도록 만듭니다. 선택가능한 비주얼 요소들은 [Selection](https://www.fusetools.com/docs/fuse/selection/selection) 컨트롤에서 선택될 수 있는 비주얼 요소들 입니다. |
| Selected | UX | [선택가능한 요소 (Selectable)](https://www.fusetools.com/docs/fuse/selection/selectable) 들이 [선택(Selection)](https://www.fusetools.com/docs/fuse/selection/selection) 되었을때 발생합니다. |
| Selection | UX | [Selection](https://www.fusetools.com/docs/fuse/selection/selection) 은 item list, radio buttons, 혹은 picker 같은 선택 컨트롤을 만드는데 사용됩니다. Selection 자체는 선택을 정의하며, 고수준의 동작을 관리하고 현재 값을 추적합니다. 다영한 선택 가능한 오브젝트들은 [선택 가능한 요소 (Selectable)](https://www.fusetools.com/docs/fuse/selection/selectable) 항목을 정의합니다. |
| Shadow | UX | 요소뒤에 그림자를 그립니다. |
| Shear | UX | 비주얼 요소에 Shear 를 적용합니다(왜곡합니다). shear 를 애니메이션으로 만들려면 [Skew](https://www.fusetools.com/docs/fuse/animations/skew) 애니메이터를 대신 사용하십시요. |
| Slider | UX | 슬라이더를 표시합니다. |
| Spring | UX ||
| StackPanel | UX | 자식요소들을 세로(기본값) 또는 가로로 쌓습니다. |
| Star | UX | 별표를 표시합니다. |
| State | UX | 일반 [Trigger](https://www.fusetools.com/docs/fuse/triggers/trigger) 로 작동하지만, 포함하는 [StateGroup](https://www.fusetools.com/docs/fuse/triggers/stategroup) 에 의해 활성화 됩니다. |
| StateGroup | UX | [State](https://www.fusetools.com/docs/fuse/triggers/state) 들을 함께 그룹화하고 그것들 사이를 전환하는데 사용됩니다. |
| StatusBarBackground | UX | 상태표시줄이 차지하는 공간을 보완합니다. |
| StatusBarConfig | UX | Android 에서 상태표시줄의 모양을 구성합니다.. |
| StatusBarConfig | UX | iOS 에서 상태표시줄의 모양을 구성합니다. |
| Swiped | UX | 스와이프가 발생하면 활성화되는 pulse 트리거입니다. |
| SwipeGesture | UX | 스와이프(특정방향의 포인터 이동)를 인식합니다. |
| SwipeNavigate | UX ||
| SwipingAnimation | UX | SwipeGesture 의 진행상황을 일련의 애니메이션에 매핑하는 트리거 입니다. |
| Switch | UX | 스위치를 표시합니다. |
| Tapped | UX | [비주얼 요소](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 에서 포인터를 두르리면 트리거됩니다. |
| Text | UX | 텍스트 블록을 표시합니다. |
| TextBlock | UX | 이전 버전과의 호환성을 이유로 더이상 사용되지 않습니다. |
| TextBox | UX | 기본 look & feel 을 가지는 [TextInput](https://www.fusetools.com/docs/fuse/controls/textinput) 입니다. |
| TextInput | UX | 싱글라인 텍스트를 입력 컨트롤입니다. |
| TextInputActionTriggered | UX | 입력 동작을 위한 트리거 입니다. |
| TextView | UX | 멀티라인 텍스트 에디터 입니다. |
| Timeline | UX | 여러 애니매이션들을 함께 그룹화합니다. |
| ToggleControl | UX | 토글가능한 값이 포함된 Panel 입니다. |
| TopFrameBackground | UX ||
| Transition | UX | Navigator 에서 page 간 전환에 대한 애니메이션을 제어합니다. |
| Translation | UX | 공간의 선형 오프셋을 나타냅니다. 애니메이션되는 translation 의 경우 이 클래스 속성들을 애니메이션하는 대신 Move 애니메이터 사용을 고려하십시오. |
| UserEvent | UX | 컴포넌트에서 발생시키고 해당 컴포넌트의 사용자가 응답할 수 있는 사용자 지정 이벤트를 정의합니다. |
| Video | UX | 비디오를 표시합니다. |
| Viewbox | UX | 사용 가능 공간에 맞춰 컨텐츠(스케일링으로)를 조정합니다. |
| WebView | UX | Android 및 iOS 에 기본적으로 웹 컨텐츠를 표시합니다. |
| WhileActive | UX | 페이지가 활성화되어 있을때 애니메이션을 적용합니다. |
| WhileBusy | UX | 형제 또는 부모가 사용 중으로 표시될때마다 활성화되는 트리거 입니다. |
| WhileCanGoBack | UX | 뒤로가기 이동이 가능할때마다 활성화됩니다. |
| WhileCanGoForward | UX | 앞으로가기 이동이 가능할때마다 활성화됩니다. |
| WhileCompleted | UX | [비디오](https://www.fusetools.com/docs/fuse/controls/video) 가 완료되면 활성화됩니다. |
| WhileContainsText | UX | 주변 컨텍스트에 맞는 텍스트를 포함하는 동안 활성됩니다. |
| WhileCount | UX | 컬렉션(collection)의 아이템 수가 몇가지 기준을 충족하면 활성화됩니다. |
| WhileDisabled | UX | 포함 요소의 `IsEnabled` 속성이 `False` 인 동안 활성화됩니다. |
| WhileDragging | UX | 요소가 드래그되는 동안 활성화됩니다. |
| WhileEmpty | UX | 컬렉션(collection)의 아이템 수가 0 일때 활성화 됩니다. |
| WhileEnabled | UX | 포함 요소의 `IsEnabled` 속성이 `True` 인 동안 활성화됩니다. |
| WhileFailed | UX | 해당 컨텍스트가 fail 인 동안 활성화됩니다. |
| WhileFalse | UX | `Value` 속성이 `false` 인 동안 활성화되는 트리거입니다. |
| WhileFloat | UX | `float` `Value` 가 몇가지 조건을 충족하면 활성화됩니다. |
| WhileFocused | UX | 포함요소에 포커스가 있을때마다 활성화됩니다. |
| WhileFocusWithin | UX | 해당 요소의 하위요소에 포커스가 있을때마다 활성화됩니다. |
| WhileHovering | UX | 포인터가 포함요소의 경계 내에 있는 동안 활성화됩니다. |
| WhileInactive | UX | 페이지가 비활성 상태일때 애니메이션을 적용합니다. |
| WhileInEnterState | UX | WhileInactive 의 방향 버전입니다. |
| WhileInExitState | UX | WhileInactive 의 방향 버전입니다. |
| WhileInteracting | UX | 사용자가 주변 요소와 상호작용하는 동안 활성화됩니다. |
| WhileKeyboardVisible | UX | on-screen 키보드가 보일때마다 활성화됩니다. |
| WhileLoading | UX | 주변 컨텍스트의 리소스가 로드되는 동안 활성됩니다. |
| WhileNavigating | UX | 사용자가 현재 두 페이지 사이를 탐색하는 동안 활성화됩니다. |
| WhileNotEmpty | UX | 컬렉션(collection)의 아이템 수가 0 보다 큰 경우 활성화 됩니다. |
| WhileNotFocused | UX | 포함 요소에 포커스가 없을때 마다 활성화됩니다. |
| WhilePageActive | UX | 선택적으로 지정된 기준과 일치하는 page 가 네비게이션에서 활성화되어 있는 동안 활성화됩니다. |
| WhilePageLoading | UX | 상위 [WebView](https://www.fusetools.com/docs/fuse/controls/webview) 가 로드되는 동안 활성화되는 트리거입니다. |
| WhilePaused | UX | [비디오](https://www.fusetools.com/docs/fuse/controls/video) 가 일시 중지된 동안 활성됩니다. |
| WhilePlaying | UX | [비디오](https://www.fusetools.com/docs/fuse/controls/video) 가 일시 플레이 되는 동안 활성됩니다. |
| WhilePressed | UX | 비주얼 요소에 의해 적어도 하나 이상의 포인터가 눌려 있는 동안 활성화됩니다. |
| WhileScrollable | UX | ScrollView 를 스크롤 할 수 있을때 활성화됩니다. |
| WhileScrolled | UX | ScrollView 가 지정된 영역 내에서 스크롤되는 동안 활성화됩니다. |
| WhileSelected | UX | [선택(Selection) 요소](https://www.fusetools.com/docs/fuse/selection/selection) 에서 [선택가능한(Selectable) 요소](https://www.fusetools.com/docs/fuse/selection/selectable) 가 현재 선택되어 있는 동안 활성화되는 트리거입니다.  |
| WhileString | UX | 문자열 값에 대한 조건이 true 일때 활성화합니다. |
| WhileSwipeActive | UX | [활성](https://www.fusetools.com/docs/fuse/gestures/swipegesture#swipetype-active-overview) 타입 [SwipeGesture](https://www.fusetools.com/docs/fuse/gestures/swipegesture) 가 활성 상태로 스와이프 될때마다 활성화됩니다. |
| WhileSwiping | UX | 스와이프 제스처가 진행되는 동안 활성화됩니다. |
| WhileTrue | UX | Value 속성이 true 인 동안 활성화된 트리거 입니다. |
| WhileVisible | UX | 부모요소가 표시되면 활성화됩니다. |
| WhileVisibleInScrollView | UX | 요소가 ScrollView 의 표시 영역 내에 배치되는 동안 활성화됩니다. |
| WhileWindowLandscape | UX | 앱의 viewport 너비가 높이보다 큰 경우 활성화됩니다. |
| WhileWindowPortrait | UX | 앱의 viewport 높이가 너비보다 큰 경우 활성화됩니다. |
| WhileWindowSize | UX | 앱의 viewport 크기가 일정 제약조건을 충족하는 경우 활성화됩니다. |
| With | UX | 현재 데이터 컨텍스트가 범위가 좁혀지는 범위를 나타냅니다. |
| WrapPanel | UX | 주어진 방향으로 자식요소들을 차례로 배치하고 끝에 도달 할때마다 감쌉니다. |
| ZoomGesture | UX | 줌(zoom) 을 제공하는 [TransformGesture](https://www.fusetools.com/docs/fuse/gestures/transformgesture) 입니다. |

## 변형 (Transforms) ##

| | | |
| --- | --- | --- |
| Rotation | UX | 요소를 지정된 각도만큼 회전합니다. |
| Scaling | UX | 지정된 요건으로 요소를 확대하거나 축소합니다. |
| Shear | UX | 비주얼 요소에 Shear 를 적용합니다(왜곡합니다). shear 를 애니메이션으로 만들려면 [Skew](https://www.fusetools.com/docs/fuse/animations/skew) 애니메이터를 대신 사용하십시요. |
| Translation | UX | 공간의 선형 오프셋을 나타냅니다. 애니메이션되는 translation 의 경우 이 클래스 속성들을 애니메이션하는 대신 Move 애니메이터 사용을 고려하십시오. |

## 효과 클래스들 (Effect classes) ##

## 특별한 리소스들 (Special Resources) ##

- [Font](https://www.fusetools.com/docs/fuse/font)

| | | |
| --- | --- | --- |
| FileImageSource | UX | [Image](https://www.fusetools.com/docs/fuse/controls/image) 요소가 표시할 데이터 원본으로 이미지 파일을 지정합니다. |
| HttpImageSource | UX | [Image](https://www.fusetools.com/docs/fuse/controls/image) 컨트롤로 표시할 수 있는 HTTP 를 통해 가져온 이미지를 제공합니다. |
| MultiDensityImageSource | UX | [Image](https://www.fusetools.com/docs/fuse/controls/image) 요소가 다른 픽셀 밀도들로 표시할 수 있는 여러 이미지 소스들을 지정하는데 사용됩니다. |
| TextureImageSource | UX | [Image](https://www.fusetools.com/docs/fuse/controls/image) 요소가 표시 되는 [texture2D](https://www.fusetools.com/docs/uno/graphics/texture2d) 오브젝트를 지정합니다. |
