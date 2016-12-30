원본: https://www.fusetools.com/docs/fuse/controls/dockpanel

DockPanel 클래스

  고급 기능 표시
이동 :
목차
자녀를 다른면에 차례로 도킹하여 자녀를 배치합니다.

다음과 같이 Dock 속성을 사용하여 요소 당 어느 쪽을 지정할 수 있습니다.

<DockPanel>
     <사각형 도크 = "왼쪽"/>
</ DockPanel>
Dock 속성은 Left, Right, Top, Bottom 또는 Fill (기본값)으로 지정 될 수 있습니다.

<DockPanel>
     <사각형 ux : Class = "MyRectangle"MinWidth = "100"MinHeight = "200"/>
     <MyRectangle Color = "Red"Dock = "Left"/>
     <MyRectangle Color = "Green"Dock = "Top"/>
     <MyRectangle Color = "Blue"Dock = "Right"/>
     <MyRectangle Color = "Yellow"Dock = "Bottom"/>
     <MyRectangle Color = "청록"/>
</ DockPanel>
DockPanel의 인터페이스