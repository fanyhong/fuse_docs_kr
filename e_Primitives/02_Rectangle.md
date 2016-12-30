원본: https://www.fusetools.com/docs/fuse/controls/rectangle

Rectangle 클래스

 고급 기능 표시
이동 :
목차
직사각형을 표시합니다.

사각형의 Color 속성을 설정하면 단색 채우기가됩니다.

<사각형 색상 = "파랑"너비 = "100"높이 = "100"/>
직사각형에는 임의 개수의 채우기 및 획을 지정할 수 있습니다. 채우기는 Brush 유형이며 사각형 내부의 태그로 지정할 수 있습니다.

기본적으로 사각형에는 채우기 나 획이 없으므로 일부를 지정하거나 지정하지 않으면 보이지 않게됩니다.
예

<Grid Alignment = "Center"Rows = "100,100,100"Columns = "100">
    <직사각형 여백 = "10"CornerRadius = "4">
        <SolidColor Color = "# a542db"/>
    </ Rectangle>
    <직사각형 여백 = "10"CornerRadius = "4">
        <선형 그래디언트>
            <GradientStop Offset = "0"Color = "# a542db"/>
            <GradientStop Offset = "1"Color = "# 3579e6"/>
        </ LinearGradient>
    </ Rectangle>
    <직사각형 여백 = "10"CornerRadius = "4">
        <스트로크 간격 띄우기 = "4"Width = "1"Color = "# 3579e6"/>
        <SolidColor Color = "# 3579e6"/>
    </ Rectangle>
</ Grid>

## Interface of Rectangle ##
