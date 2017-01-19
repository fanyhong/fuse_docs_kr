원본: https://www.fusetools.com/docs/fuse/controls/multilayoutpanel

MultiLayoutPanel 클래스

 고급 기능 표시
이동 :
목차
자리 표시 자 클래스를 사용하여 다른 레이아웃간에 요소를 이동할 수 있습니다.

이를 통해 시각적 트리에서 다른 위치로 요소를 이동하고 특정 레이아웃을 즉시 전환 할 수 있습니다.

참고 : MultiLayoutPanel은 특정 데이터의 값에 따라 다른 레이아웃을 사용하려는 경우에 적합한 옵션입니다. 애니메이션을 만드는 수단으로 다른 레이아웃을 주로 사용하는 경우에는 Element.LayoutMaster 속성을 사용하는 것이 더 좋습니다.

예

이 예제는 LayoutAnimation과 함께 MultiLayoutPanel을 사용하여 선택한 옵션에 대한 표시기에 애니메이션을 적용하는 간단한 3- 선택 Selection을 보여줍니다.

<Panel Alignment = "Center"Width = "200"Height = "50">
    <MultiLayoutPanel ux : Name = "multiLayout">
        <Grid ColumnCount = "3">
            <Panel ux : Name = "offPanel">
                <자리 표시 자>
                    <Panel ux : Name = "pointer"Color = "# 2196F3"Width = "50"Height = "2">
                        <LayoutAnimation>
                            <이동 X = "1"Y = "1"RelativeTo = "LayoutChange"지속 시간 = ". 4"Easing = "QuadraticInOut"/>
                        </ LayoutAnimation>
                    </ Panel>
                </ 자리 표시 자>
                <Text TextAlignment = "Center"> 끄기 </ Text>
                <클릭>
                <set multiLayout.LayoutElement = "offPanel"/>
                </ Clicked>
            </ Panel>
            <패널 ux : Name = "standbyPanel">
                <자리 표시 자 Target = "포인터"/>
                <Text TextAlignment = "Center"> 대기 </ Text>
                <클릭>
                    <set multiLayout.LayoutElement = "standbyPanel"/>
                </ Clicked>
            </ Panel>
            <Panel ux : Name = "onPanel">
                <자리 표시 자 Target = "포인터"/>
                <Text TextAlignment = "Center"> 설정 </ Text>
                <클릭>
                    <set multiLayout.LayoutElement = "onPanel"/>
                </ Clicked>
            </ Panel>
        </ Grid>
    </ MultiLayoutPanel>
</ Panel>
MultiLayoutPanel의 인터페이스
