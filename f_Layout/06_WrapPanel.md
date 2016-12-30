원본: https://www.fusetools.com/docs/fuse/controls/wrappanel

WrapPanel 클래스

  고급 기능 표시
이동 :
목차
주어진 방향으로 아이들을 차례로 배치하고 끝에 도달 할 때마다 감싼다.

FlowDirection 속성을 할당하여 요소가 배치되는 방향을 지정할 수 있습니다. FlowDirection은 LeftToRight 또는 RightToLeft 일 수 있습니다.

다음 WrapPanel은 자식을 오른쪽에서 왼쪽으로 수평으로 배치합니다.

<WrapPanel FlowDirection = "RightToLeft">
     <각 Count = "10">
         <사각형 여백 = "5"너비 = "100"높이 = "100"색상 = "파랑"/>
     </ 각>
</ WrapPanel>
Orientation 속성을 사용하면 세로 WrapPanel을 다음과 같이 만들 수 있습니다.

<WrapPanel Orientation = "Vertical">
     <각 Count = "10">
         <사각형 여백 = "5"너비 = "100"높이 = "100"색상 = "파랑"/>
     </ 각>
</ WrapPanel>
ItemWidth 및 ItemHeight 속성을 사용하여 WrapPanel에서 요소를 할당 할 최대 영역을 지정할 수도 있습니다.

WrapPanel의 인터페이스