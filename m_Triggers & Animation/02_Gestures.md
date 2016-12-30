원본: https://www.fusetools.com/docs/fuse/gestures/transformgesture

제스처 TransformGesture 클래스

 고급 기능 표시
이동 :
목차
TransformGesture는 포인터 제스처를 해석하고 이에 대한 응답으로 InteractiveTransform을 수정합니다.

TransformGesture 자체는 시각적 인 영향을 미치지 않으며 실제 시각적 변환을 제공하는 InteractiveTransform 만 수정합니다. 예를 들어 다음은 간단한 이미지보기 설정입니다.

<Panel HitTestMode = "LocalBounds">
    <Image File = "my_image.jpg">
        <InteractiveTransform ux : Name = "ImageTrans"/>
    </ Image>
    <ZoomGesture Target = "ImageTrans"/>
    <PanGesture Target = "ImageTrans"/>
    <RotateGesture Target = "ImageTrans"/>
</ Panel>
하나의 InteractiveTransform이 여러 제스처의 대상이 될 수 있습니다. 그들은 서로 통일 된 경험을 제공하기 위해 서로 올바르게 협력 할 것입니다. InteractiveTransform에는 총 변환을 나타내는 값이 포함됩니다.

탭, LongPress 등과 같은 단일 손가락 제스처의 전체 목록을 보려면 트리거를 참조하십시오.

사용 가능한 제스처

PanGesture UX
패닝 (2D 변환)을 제공하는 TransformGesture입니다.
RotateGesture UX
회전을 제공하는 TransformGesture입니다.
ZoomGesture UX
확대 / 축소를 제공하는 TransformGesture입니다.
TransformGesture의 인터페이스

