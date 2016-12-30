원본: https://www.fusetools.com/docs/fuse/controls/graphicsview

GraphicsView 클래스

  고급 기능 표시
이동 :
목차
그래픽으로 렌더링 된 UI 컨트롤을 호스트하는 네이티브 뷰입니다.

GraphicsView는 NativeViewHost에 상응하는 도구이며 Fusion 뷰를 NativeViewHost 범위에 추가 할 수있게합니다.

<App>
     <NativeViewHost>
         <StackPanel>
             <Button Text = "나는 네이티브 버튼입니다!" />
             <GraphicsView>
                 <Button Text = "그래픽 버튼입니다!" />
             </ GraphicsView>
         </ StackPanel>
     </ NativeViewHost>
</ App>
NativeViewHost 노트에서와 같이 깊이 순서는 네이티브 뷰와 퓨즈 뷰를 혼합 할 때 다르게 동작합니다.

GraphicsView의 인터페이스