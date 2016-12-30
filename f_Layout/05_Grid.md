원본: https://www.fusetools.com/docs/fuse/controls/grid

그리드 클래스

 고급 기능 표시
이동 :
목차
아이들을 격자 모양으로 배치합니다.

RowCount 및 ColumnCount 속성

필요한만큼의 동일한 크기의 행 및 / 또는 열이 필요한 경우 RowCount 및 ColumnCount 속성을 사용할 수 있습니다.

<Grid RowCount = "4"ColumnCount = "2"/>
기본적으로 그리드의 요소는 UX에 나타나는 순서대로 왼쪽에서 오른쪽으로, 위에서 아래로 배치됩니다. 그러나 행 및 열 특성을 사용하여 배치 할 격자 셀을 명시 적으로 지정할 수 있습니다.

<Grid RowCount = "1"ColumnCount = "2">
    <Rectangle Row = "0"Column = "1"Color = "Red"/>
    <Rectangle Row = "0"Column = "0"Color = "Blue"/>
</ Grid>
요소가 여러 행이나 열을 차지하도록하려면 RowSpan 및 ColumnSpan 속성을 사용할 수 있습니다.

<Grid RowCount = "2"ColumnCount = "2">
    <Rectangle ColumnSpan = "2"RowSpan = "2"Color = "Red"/>
</ Grid>
행 및 열 속성

행 및 열 크기를 계산하는 방법에 대한보다 세밀한 제어는 행 및 열 속성을 사용하여 수행 할 수 있습니다. 이러한 속성은 쉼표로 구분 된 표 크기의 목록을 받아들이며 몇 가지 형식을 취할 수 있습니다. 값은 절대, 상대 또는 자동이 될 수 있습니다.

10, 10, 50 포인트의 3 개의 행과 3 개의 열이있는 그리드의 예는 첫 번째가 사용 가능한 공간의 20 %를 차지하고 마지막 행이 60 %를 차지합니다.

<Grid Rows = "10,10,50"Columns = "1 *, 1 *, 3 *"/>
여기서 비례 열 크기는 모든 값 (1 + 1 + 3 = 5)을 먼저 합산하여 계산됩니다. 그런 다음 우리의 가치를 합계 (1/5 = 20 %, 1/5 = 20 %, 3/5 = 60 %)로 나눕니다.

비례 크기는 그리드가 부모 패널을 채우도록 확장되거나 고정 된 크기 인 경우에만 의미가 있습니다. 내용에 맞게 축소하는 경우 비례 행 / 열의 크기는 0입니다.

다음 Grid에는 첫 번째 행이 해당 행에서 가장 높은 요소의 높이를 가져 오는 두 행이 있고 두 번째 행은 나머지 공간을 차지합니다.

<Grid Rows = "auto, 1 *"/>
그리드 인터페이스