원본: https://www.fusetools.com/docs/fuse/elements/element

요소 요소 클래스

 고급 기능 표시
이동 :
목차
요소는 직사각형 2D 영역을 다루는 비주얼입니다.

공통 속성

HitTestMode

요소와 상호 작용할 때 상호 작용할 수있는 요소를 구별 할 수있는 것이 바람직 할 때도 있습니다. 이를 일반적으로 "적중 테스트"라고합니다. 퓨즈에서 요소가 사용자 입력과 어떻게 상호 작용하는지 HitTestMode를 사용하여 설정할 수 있습니다.

예

이 예제에서는 두 개의 Rectangle을 레이아웃하고 Clicked-triggers를 둘 다에 추가합니다. 그러나 적중 한 테스트가 오른쪽 직사각형에서 명시 적으로 비활성화되어 있으므로 왼쪽 클릭 만 클릭하면 아무 것도 출력되지 않습니다.

<Grid ColumnCount = "2">
    <직사각형 너비 = "100"높이 = "100"채우기 = "# 808">
        <클릭>
            <DebugAction Message = "왼쪽 클릭"/>
        </ Clicked>
    </ Rectangle>
    <Rectangle Width = "100"Height = "100"Fill = "# 808"HitTestMode = "None">
        <클릭>
            <DebugAction Message = "Clicked right"/>
        </ Clicked>
    </ Rectangle>
</ Grid>
이것은 시각적 요소가 하위 요소를 모호하게하는 경우 매우 유용 할 수 있습니다. 하위 요소가 입력에 응답하도록하려면이 요소를 사용하십시오.

ClipToBounds

일반적으로 요소를 다른 요소 내부에 배치 할 때 내부 요소는 부모 요소 외부에서 자유롭게 살 수 있습니다.

<Panel Width = "100"Height = "100">
        <이미지 여백 = "- 100"File = "Pictures / Picture1.jpg"
            StretchMode = "UniformToFill"/>
</ Panel>
이 이미지는 300pt 너비이고 키가 크지 않습니다. 패널이 경계선까지 자식을 클립하지 않기 때문입니다.

이미지 클립을 부모 크기로 만들려면 Panel에서 ClipToBounds = "true"로 설정하면됩니다.

    <Panel Width = "100"Height = "100"ClipToBounds = "true">
        <이미지 여백 = "- 100"File = "Pictures / Picture1.jpg"
            StretchMode = "UniformToFill"/>
    </ Panel>
이제 이미지는 패널 경계를 넘지 않습니다.

불투명

불투명도 속성을 사용하여 객체의 투명도를 설정할 수 있습니다. 불투명도 = "0"이면 요소가 완전히 투명합니다.

예

이 예제에서 패널의 불투명도는 0.5로 설정됩니다.

<패널>
    <불투명 값 = "0.5"/>
</ Panel>
레이어

기존 컨트롤의 모양을 재정의하는 것이 도움이되는 경우가 많습니다. 컨테이너에 추가되는 요소는 다른 레이어에 할당 될 수 있습니다. 빨간색 배경으로 버튼을 표시하려면 배경 레이어를 다시 정의 할 수 있습니다.

<Button Text = "Hello!">
    <직사각형 채우기 = "# 931"Layer = "배경"/>
</ Button>
이 버튼의 레이아웃이나 동작은 변경되지 않지만 모양이 변경됩니다.
