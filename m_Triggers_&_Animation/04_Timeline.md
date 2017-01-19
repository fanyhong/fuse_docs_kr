원본: https://www.fusetools.com/docs/fuse/triggers/timeline

타임 라인 클래스

 고급 기능 표시
이동 :
목차
여러 애니메이션을 함께 그룹화합니다.

이를 통해 여러 애니메이션을 그룹화하고 상호 작용 논리에서 분리 할 수 ​​있습니다.

타임 라인은 0에서 1 사이의 TargetProgress 속성에 애니메이션을 적용하여 재생할 수 있습니다.

참고 : 타임 라인 자체는 여러 애니메이터를 그룹화하여 키 프레임 애니메이션을 만들려는 의도가 아닙니다. 이를 달성하기 위해 키 프레임을 애니메이터 자체에 추가 할 수 있습니다.

잘못된 것 :

<타임 라인>
    <변경 rect.Opacity = "1"지연 = "0.0"기간 = "0.5"/>
    <변경 rect.Opacity = "0"지연 = "0.5"지속 시간 = "0.5"/>
</ Timeline>
옳은:

```

예

다음은 타임 라인을 사용하여 직사각형의 여러 속성 (폭과 색상)에 애니메이션을 적용한 다음 두 개의 버튼을 클릭하여이 타임 라인의 시작과 끝 사이를 재생하는 방법의 예입니다.

<StackPanel>
    <Rectangle ux : Name = "rect"Height = "40"Width = "100 %">
        <SolidColor ux : Name = "color"Color = "# f00"/>
    </ Rectangle>
    <Grid ColumnCount = "2">
        <Button Text = "Red">
            <클릭>
                <Set timeline.TargetProgress = "0"/>
            </ Clicked>
        </ Button>
        <Button Text = "Green">
            <클릭>
                <Set timeline.TargetProgress = "1"/>
            </ Clicked>
        </ Button>
    </ Grid>

    <Timeline ux : Name = "timeline">
        <Change Target = "rect.Width">
            <Keyframe Value = "10"Time = "0.3"/>
            <Keyframe Value = "100"Time = "0.6"/>
        </ Change>
        <Change color.Color = "# 0f0"Duration = "0.3"Delay = "0.3"/>
    </ Timeline>
</ StackPanel>
타임 라인의 인터페이스
