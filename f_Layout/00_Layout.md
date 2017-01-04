원본: https://www.fusetools.com/docs/layout/layout

레이아웃

퓨즈는 다양한 화면 크기와 종횡비에 맞는 반응적인 방식으로 UI를 만들 수있는 매우 강력한 레이아웃 시스템과 함께 제공됩니다. 또한 광범위한 입력 유형 및 크기에서 작동하는 구성 요소를 만들 수 있습니다.

UI 레이아웃 정의하기

퓨즈의 UI는 패널의 계층 구조와 직사각형 및 이미지와 같은 다른 기본 요소에 의해 생성됩니다. StackPanel, Grid 및 DockPanel과 같은 다양한 Panel 유형은 사용 가능한 화면 크기에 상대적으로 여러 요소를 서로 상대적으로 배치하고 크기를 지정하는 데 사용됩니다.

다음 예제는 두 개의 행과 세 개의 열이 다양한 색상의 사각형으로 채워진 격자를 만드는 방법을 보여줍니다.

<App>
    <Grid RowCount = "2"ColumnCount = "3">
        <사각형 색상 = "# ff4500"/>
        <사각형 색상 = "# ee7942"/>
        <사각형 색상 = "# ee6363"/>
        <사각형 색상 = "# ffb90f"/>
        <사각형 색상 = "# eeb422"/>
        <사각형 색상 = "# eedc82"/>
    </ Grid>
</ App>
보다 복잡한 구조를 만들기 위해 여러 개의 패널을 중첩 할 수 있습니다.

<App>
    <StackPanel>
        <DockPanel Height = "50">
            <텍스트 값 = "Hello"Dock = "Left"/>
            <사각형 색상 = "# f00"/>
        </ DockPanel>
        <DockPanel Height = "50">
            <텍스트 값 = "세계"도킹 = "왼쪽"/>
            <사각형 색상 = "# 00f"/>
        </ DockPanel>
    </ StackPanel>
</ App>
요소 레이아웃

Visual 요소를 나타내는 각 UX 태그에 대해 Width, Height, Opacity 등의 요소에 영향을주는 속성을 설정할 수 있습니다. 또한 Alignment, Padding 및 Margin을 사용하여 부모 노드와 자식 노드와의 관계를 제어 할 수 있습니다.

여백 / 여백

여백 및 여백은 가장 많이 사용되는 요소 속성 중 두 가지입니다. 여백은 요소 가장자리에서 해당 컨테이너 가장자리까지의 거리를 제어합니다. Padding은 Margin과 비슷하게 작동하지만 대신 안쪽으로 작동합니다. 가장자리에서 자식 요소의 해당 가장자리까지의 거리를 설정합니다.

Margin과 Padding은 모두 float4 유형이며, 이는 4 개의 부동 소수점 값 구성 요소를 가짐을 의미합니다. UX에서는 쉼표로 구분 된 목록으로 지정됩니다.

<패널 여백 = "10,20,30,40"/>
<패널 패딩 = "10,20,30,40"/>
각 값은 네 개의 가장자리 "10 (왼쪽), 20 (위쪽), 30 (오른쪽), 40 (아래)"요소 중 하나를 나타냅니다.

우리는 또한 단축 형식으로 작성할 수 있습니다 :

<패널 여백 = "10"/> <! - "10,10,10,10"->
<패널 패딩 = "10,20"/> <! - "10,20,10,20"->
조정

요소가 부모에 의해 할당 된 사용 가능한 공간보다 작은 경우 Alignment 속성을 사용하여 사용 가능한 공간의 위치를 ​​제어 할 수 있습니다.

다음 예제는 화면의 네 구석에 직사각형을 배치하는 방법을 보여줍니다.

<App>
    <패널>
        <사각형 정렬 = "TopLeft"Width = "40"Height = "40"Color = "Red"/>
        <직사각형 맞춤 = "TopRight"너비 = "40"높이 = "40"색상 = "파랑"/>
        <직사각형 맞춤 = "아래쪽 왼쪽"너비 = "40"높이 = "40"색상 = "녹색"/>
        <직사각형 맞춤 = "BottomRight"Width = "40"Height = "40"Color = "Yellow"/>
    </ Panel>
</ App>
너비 / 높이

Width 및 Height 속성을 사용하여 지정된 Element가 화면에서 특정 치수를 갖도록 할 수 있습니다. 기본적으로 레이아웃은 자동으로 설정됩니다. 즉, 요소의 크기가 레이아웃 엔진에 의해 자동으로 결정됩니다. 명시 적으로 구체적인 값으로 설정하면 요소의 크기를 세밀하게 제어 할 수 있습니다.

이러한 속성은 서로 독립적으로 설정할 수 있습니다. 너비 = "30px"및 높이 = "자동".

또한 MinWidth / MaxWidth 및 MinHeight / MaxHeight와 같은 속성을 사용하여 요소의 크기를 각각 지정된 최소 또는 최대 값으로 제한 할 수 있습니다.

단위

너비, 높이, X 및 Y와 같은 많은 레이아웃 속성을 다른 단위로 지정할 수 있습니다. 대부분의 경우 다음 단위를 사용할 수 있습니다.

퍼센트 - 너비 = "50 %"는 요소 너비를 부모 너비의 50 %로 만듭니다.
Points (기본값) - Width = "50"은 요소의 너비를 50 포인트로 만듭니다. 즉, 너비가 모든 화면 밀도에서 동일하게 나타납니다.
픽셀 - 너비 = "50 픽셀"은 너비를 정확히 50 픽셀로 만듭니다. 즉, 픽셀 밀도가 높은 화면에서는 너비가 더 작게 표시됩니다.
레이아웃 규칙

패널은 서로 관련하여 여러 요소를 배열하는 데 사용됩니다. 모든 패널 유형에는 연결된 Layout 객체가 있습니다. 일반 패널 유형은 DefaultLayout을 사용하여 자식을 배치합니다. 그것들을 Z 축상에 놓는 것 외에는 아무것도하지 않습니다. StackPanel은 연관된 StackLayout을 가지며 자식을 세로 또는 가로 스택에 배치합니다. StackPanel 및 기타 패널 유형에 대한 자세한 내용은 다음 하위 절을 참조하십시오.

Grid 및 DockPanel과 같은 일부 패널 유형에는 레이아웃을 위해 하위 요소별로 정보가 필요합니다. 이러한 경우 패널 유형과 관련된 첨부 된 속성을 사용할 수 있습니다. 연결된 속성의 형식은 ClassName.PropertyName = "Value"이며 모든 요소에서 사용할 수 있습니다. 모호하지 않은 모든 경우에 "클래스"부분을 생략하고 PropertyName = "Value"를 수행 할 수 있습니다. 이 경우 첨부 된 속성은 일반적인 속성과 구문 상 동일합니다.

몇 가지 예가 있습니다.

Dock - Dock 연결된 속성을 사용하여 특정 요소를 도킹해야하는면 (위쪽, 아래쪽, 왼쪽, 오른쪽, 채우기)을 지정합니다.
그리드 - 행 및 열 첨부 속성을 사용하여 요소를 배치 할 행 및 열을 지정합니다.
다음은 UX에서 어떻게 보이는지에 대한 예입니다.

<Grid ColumnCount = "3"RowCount = "1">
    <Panel Row = "0"Column = "0"Color = "# f00"/>
    <Panel Row = "0"Column = "2"Color = "# 00f"/>
</ Grid>
순서 및 ZOffset 속성 그리기

일반적으로 요소가 그려지는 순서는 자식 목록의 위치에 따라 다릅니다. 문서에서 더 이상 정의 된 요소는 아래쪽에 정의 된 요소 위에 그려집니다. 다음과 같은 경우 빨간색 사각형이 녹색 원 위에 그려집니다.

<패널>
    <사각형 색상 = "빨강"여백 = "100"/>
    <원색 = "녹색"/>
</ Panel>
이 동작을 제어하려면 ZOffset 속성을 사용할 수 있습니다. 이 속성은 기본값이 0.0 인 부동 소수점 값입니다. 위쪽이 높을수록 요소가 그려집니다.

다음 예제에서는 1.0의 ZOffset이 0.0 (기본값) 인 ZOffset보다 높기 때문에 Circle은 Rectangle 위에 그려집니다.

<패널>
    <사각형 색상 = "빨강"ZOffset = "0.0"여백 = "100"/>
    <원색 = "녹색"ZOffset = "1.0"/>
</ Panel>