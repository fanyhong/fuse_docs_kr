원본: https://www.fusetools.com/docs/fuse/controls/stackpanel

StackPanel 클래스

  고급 기능 표시
이동 :
목차
자식을 세로로 (기본값) 또는 가로로 스택합니다.

기본 레이아웃은 세로 스택이지만 Orientation 속성을 사용하여 스택을 가로로 배치해야한다고 지정할 수 있습니다.

<StackPanel Orientation = "Horizontal">
     ... 요소 ...
</ StackPanel>
ItemSpacing 속성을 사용하여 요소 사이에 간격을 만들 수 있습니다. 각 요소의 여백을 설정하는 것과 다릅니다. 각 요소 주위의 공간이 아니라 요소 사이의 공간 만 조정합니다.

예

다음 예제는 ItemPacing 속성을 사용하여 간격을 둔 StackPanel의 세 패널을 보여줍니다.

<StackPanel ItemSpacing = "20">
     <패널 높이 = "100"배경 = "빨간색"/>
     <패널 높이 = "100"배경 = "녹색"/>
     <패널 높이 = "100"배경 = "파랑"/>
</ StackPanel>
StackPanel의 인터페이스