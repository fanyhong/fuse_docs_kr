원본: https://www.fusetools.com/docs/fuse/controls/scrollview

ScrollView 클래스

 고급 기능 표시
이동 :
목차
사용 가능한 크기보다 큰 내용을 탐색하는 데 사용됩니다.

예

이 예제는 일반적으로 볼 수 없을만큼 큰 Panel을 포함하여 ScrollView를 사용하는 방법을 보여줍니다.

<ScrollView>
    <Panel Width = "2000"Height = "2000"/>
</ ScrollView>
AllowedScrollDirections 속성을 사용하여 ScrollView에서 스크롤 할 수있는 방향을 제한 할 수도 있습니다.

<ScrollView AllowedScrollDirections = "Horizontal">
    <! - 내용 ->
</ ScrollView>
기본적으로 ScrollView는 스크롤 할 수있는 방향으로 내용과 동일한 양의 공간을 차지하려고합니다. 그러나 Panel (또는 DockPanel, Grid 등)에 배치하면 ScrollView 자체의 크기는 부모 크기로 제한됩니다.

노트

StackPanel은 자손의 크기를 제한하지 않고 받아 들이기를 원하는 크기로 확장 할 수 있습니다. ScrollView는 기본적으로 콘텐츠의 크기를 상속하므로 ScrollView의 문제입니다. StackPanel 안에 ScrollView를 배치하면 ScrollView의 크기가 화면 경계를 넘어 확장됩니다. 우리가 원하는 것은 ScrollView의 내용이 필요한 크기로 확장되어야한다는 것입니다. 반면 ScrollView 자체는 부모의 경계로 제한됩니다.

즉, StackPanel 내의 ScrollView는 예상대로 동작하지 않습니다. 다른 대안으로는 다른 유형의 Panel (예 : DockPanel)을 ScrollView의 부모로 사용하거나 크기를 명시 적으로 지정하는 것입니다.
하위 내용의 정렬은 시작 ScrollPosition과 MinScroll 및 MaxScroll 값에 영향을줍니다. 예를 들어 아래쪽 정렬 요소는 내용의 아래쪽에서부터 시작하여 (ScrollView의 아래쪽에 정렬 됨) 시작되며 MinScroll은 음수가됩니다. 이는 오버플로가 ScrollView의 맨 위에 있기 때문입니다.

레이아웃 모드

기본적으로 레이아웃이 변경되면 ScrollView는 일관된 ScrollPosition을 유지합니다. 이로 인해 내용이 추가 / 제거 될 때 점프가 발생할 수 있습니다.

대신 대체 모드 인 LayoutMode = "PreserveVisual"은 하위 또는 상위 레이아웃이 변경 될 때 시각적 일관성을 유지하려고 시도합니다. 그것은 즉각적인 내용을 컨테이너라고 가정하고 그 컨테이너의 자식을 봅니다. 예를 들어, 다음과 같은 레이아웃이 있습니다.

<ScrollView>
    <StackPanel>
        <패널 />
        <패널 />
    <StackPanel>
</ ScrollView>
시각적 일관성을 유지하면 LayoutRole = Standard가없는 시각은 고려되지 않습니다. LayoutMode 속성을 사용하여이 동작을 조정할 수 있습니다.

ScrollView의 인터페이스
