원본: https://www.fusetools.com/docs/fuse/controls/switch

스위치 클래스

  고급 기능 표시
이동 :
목차
스위치를 표시합니다.

ToggleControl로 구현 된 스위치입니다. NativeViewHost에서 사용하는 경우 플랫폼 기본 스위치가 표시됩니다.

예

<StackPanel>
     <Switch ux : Name = "_ sw">
         <While True 값 = "{ReadProperty _sw.Value}">
             <DebugAction Message = "Switch.Value = true"/>
         </ WhileTrue>
     </ Switch>
     <NativeViewHost>
         <스위치 />
     </ NativeViewHost>
</ StackPanel>
스위치 인터페이스
