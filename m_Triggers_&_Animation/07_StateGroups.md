원본: https://www.fusetools.com/docs/fuse/triggers/stategroup

상태 그룹 클래스

 고급 기능 표시
이동 :
목차
상태 세트를 함께 그룹화하고 이들을 전환하는 데 사용됩니다.

StateGroup에는 현재 해당 그룹에서 활성화 된 상태를 할당하는 데 사용되는 Active 속성이 있습니다.

또한 Exclusive 또는 Parallel 중 하나 일 수있는 Transition을 지정할 수 있습니다. Exclusive는 다음 상태가 활성화되기 전에 각 상태를 완전히 비활성화해야 함을 의미합니다. 병렬이란 하나의 상태가 비활성화 될 때 다음 상태가 활성화되고 애니메이션 속성이 둘 사이에 삽입됨을 의미합니다.

예

다음은 StateGroup을 사용하여 세 가지 상태 사이에서 Rectangle의 색상을 전환하는 방법의 예입니다.

<StackPanel>
    <Panel Width = "100"Height = "100">
        <SolidColor ux : Name = "someColor"/>
    </ Panel>
    <StateGroup ux : Name = "stateGroup">
        <상태 ux : 이름 = "redState">
            <change someColor.Color = "# f00"Duration = "0.2"/>
        </ State>
        <상태 ux : Name = "blueState">
            <Change someColor.Color = "# 00f"Duration = "0.2"/>
        </ State>
        <상태 ux : Name = "greenState">
            <change someColor.Color = "# 0f0"Duration = "0.2"/>
        </ State>
    </ StateGroup>
    <Grid ColumnCount = "3">
        <Button Text = "Red">
            <클릭>
                <Set stateGroup.Active = "redState"/>
            </ Clicked>
        </ Button>
        <Button Text = "Blue">
            <클릭>
                <Set stateGroup.Active = "blueState"/>
            </ Clicked>
        </ Button>
        <Button Text = "Green">
            <클릭>
                <Set stateGroup.Active = "greenState"/>
            </ Clicked>
        </ Button>
    </ Grid>
</ StackPanel>
StateGroup의 인터페이스
