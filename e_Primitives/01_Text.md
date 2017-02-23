원본: https://www.fusetools.com/docs/fuse/controls/text

# Text Class #

> 점프: [UX](https://www.fusetools.com/docs/fuse/controls/text#section-ux) [목차](https://www.fusetools.com/docs/fuse/controls/text#section-table-of-contents) [비고(Remarks)](https://www.fusetools.com/docs/fuse/controls/text#section-remarks) [예제들](https://www.fusetools.com/docs/fuse/controls/text#section-examples)

텍스트 블록을 표시합니다.

`Text` UI 컨트롤은 읽기 전용 텍스트를 렌더링합니다.

트루 타입 글꼴을 포함하는 ttf 파일에서 [Font](https://www.fusetools.com/docs/fuse/font) 를 가져올 수 있습니다. 일반적으로 font 는 응용 프로그램 전체에서 참조되므로 `ux:Global` 을 사용하여 글로벌 리소스로 간단하게 만드는 것이 가장 좋습니다. 이 font 가져 오기 방법을 통해 font를 전체 프로젝트에서 사용할 수 있으며 한번만 로드할 수 있습니다.

> 알림 <br /><br />
데스크톱 미리보기를 실행할 때, 대체 글꼴(fallback fonts), 색칠된 글리프(colored glyphs), 기본 다국어 외의 유니 코드 문자들도 지원되지 않습니다. <br /><br />
이런 이유로, 로컬 미리보기를 실행할 때 특정 텍스트 기능(예:그림 이모티콘)이 지원되지 않습니다. 데스크톱 렌더링이 디바이스 렌더링과 100% 일치하지 않더라도 놀라지 마십시오. 이 문제는 해결 중입니다.

이 예제에서는 두 개의 font를 로드하고 사용 가능한 `Text` 속성들 중 일부를 결합한 세 가지 다른 의미를 가진 `Light` , `Medium` 및 `Warning` 클래스들을 만듭니다. 이 예제에서 font 들은 관련 UX 파일과 동일한 디렉토리에 있습니다.

## UX ##

```
<App>
    <Font File="Roboto-Medium.ttf" ux:Global="Medium" />
    <Font File="Roboto-Light.ttf" ux:Global="Light" />

    <Text ux:Class="Light" Font="Light" />
    <Text ux:Class="Medium" Font="Medium" TextWrapping="Wrap" />
    <Text ux:Class="Warning" 
        Font="Medium" 
        FontSize="42"
        TextAlignment="Center"
        Color="#f00" />

    <StackPanel>
        <Light>Just some text</Light>
        <Warning>The robots are coming!</Warning>
        <Medium>This is just some medium text, but it will happily wrap when the edges of the screen is reached.</Medium>
    </StackPanel>
</App>
```

## 위치 (Location) ##

**Namespace**
[Fuse.Controls](https://www.fusetools.com/docs/fuse/controls)

**Package**
Fuse.Controls.Primitives 0.45.5

## Text 의 인터페이스 (Interface of Text) ##

| | | |
|---|---|---|
| LoadAsync : bool | UX | 앱에 많은 텍스트가 있는 경우 이런 앱의 텍스트 요소들이 너무 많아 초기로드 시간이 오래걸려 눈에 띄는 딜레이 없이 작동되기 어려울 수 있는데, 이런 경우 유용할 수 있습니다. [더보기](https://www.fusetools.com/docs/fuse/controls/text/loadasync) |
| Text Constructor | UNO | Creates a new Text |


| Inherited from [TextControl](https://www.fusetools.com/docs/fuse/controls/textcontrol) | | |
|---|---|---|
| Color : float4 | UX | 텍스트의 색 (TextColor 의 별칭) 입니다 |
| Font : Font | UX ||
| FontSize : float | UX ||
| GetITextView : ITextView | UNO ||
| InternalLoadAsync : bool | UNO ||
| InvalidateRenderer | UNO ||
| LineSpacing : float | UX | 각 텍스트 줄 사이의 간격을 지정합니다. |
| MaxLength : int | UX | 값이 가질 수있는 최대 문자 수를 지정합니다. |
| OnColorChanged(IPropertyListener) | UNO ||
| OnFontChanged | UNO ||
| OnFontSizeChanged | UNO ||
| OnLineSpacingChanged | UNO ||
| OnLoadAsyncChanged | UNO ||
| OnMaxLengthChanged | UNO ||
| OnTextAlignmentChanged | UNO ||
| OnTextTruncationChanged | UNO ||
| OnTextWrappingChanged | UNO ||
| OnValueChanged(IPropertyListener) | UNO ||
| SetColor(float4, IPropertyListener) | UNO ||
| SetValue(string, IPropertyListener) | UNO ||
| SetValueInternal(string) | UNO | 네이티브 뷰에 통지하지 않고 값을 설정합니다. TextEdit의 ITextEditHost 구현함으로써 사용됩니다. |
| TextAlignment : TextAlignment | UX ||
| TextColor : float4 | UX | 텍스트의 색입니다. [더보기](https://www.fusetools.com/docs/fuse/controls/textcontrol/textcolor)] |
| TextTruncation : TextTruncation | UX ||
| TextWrapping : TextWrapping | UX | TextControl에서 텍스트를 래핑하는 방법을 지정합니다. |
| Value : string | UX ||
| ValueChanged : ValueChangedHandler<string> (object, ValueChangedArgs<string>) | UX ||


| Inherited from [LayoutControl](https://www.fusetools.com/docs/fuse/controls/layoutcontrol) | | |
|---|---|---|
| Layout : Layout | UX | 자식 요소들에게 적용되는 레이아웃입니다. [더보기](https://www.fusetools.com/docs/fuse/controls/layoutcontrol/layout) |


| Inherited from [Control](https://www.fusetools.com/docs/fuse/controls/control) | | |
|---|---|---|
| Background : Brush | UX | 이 컨트롤의 배경을 그리는데 사용될 브러시입니다. [더보기](https://www.fusetools.com/docs/fuse/controls/control/background) |
| BootstrapNativeViewGroup : bool | UNO ||
| CompensateForScrollView(float4x4) | UNO ||
| CreateGraphicsVisual : Visual | UNO ||
| CreateNativeView : IView | UNO ||
| CreateNativeViewGroup : IViewGroup | UNO ||
| DrawBackground(DrawContext, float) | UNO ||
| DrawVisual(DrawContext) | UNO ||
| EjectTemplate | UNO ||
| GraphicsVisual : Visual | UNO ||
| InjectTemplate | UNO ||
| NativeParent : IViewGroup | UNO ||
| NativeTransform : float4x4 | UNO ||
| NativeView : IView | UNO ||
| NativeViewGroup : IViewGroup | UNO ||
| PushPropertiesToNativeView | UNO ||
| RootOnNativeParent | UNO ||
| UpdateNativeSize(LayoutParams) | UNO ||


| Inherited from [Element](https://www.fusetools.com/docs/fuse/elements/element) | | |
|---|---|---|
| ActualAnchor : float2 | UNO | 요소의 앵커(anchor) 입니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/actualanchor) |
| ActualPosition : float2 | UNO | 요소의 위치, 부모의 왼쪽 상단 모서리에 대한 왼쪽 상단 모서리의 위치입니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/actualposition) |
| ActualSize : float2 | UNO | 요소의 크기입니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/actualsize) |
| Alignment : Alignment | UX | `요소(Element)` 의 [정렬(Alignment)](https://www.fusetools.com/docs/fuse/elements/element/alignment) |
| Anchor : Size2 | UX | 엘리먼트 내의 "진원지"로 취급할 지점입니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/anchor) |
| ArrangePaddingBox(LayoutParams) | UNO | 제공된 LayoutParams는 padding box를 레이아웃 하는데 사용되야 하는 정의된 크기를 갖도록 합니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/arrangepaddingbox_28e7c99e) |
| Aspect : float | UX | 레이아웃에서 요소가 충족해야하는 종횡비입니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/aspect) |
| AspectConstraint : AspectConstraint | UX | 종횡비가 최소 또는 최대 크기 제한을 위반하는 상황에서 유지되는 방법을 결정합니다. |
| BoxSizing : BoxSizingMode | UX | 요소의 크기와 위치가 계산되는 방식. |
| CachingMode : CachingMode | UX | 그리는 동안 요소의 비주얼을 캐시하는 방법. [더보기](https://www.fusetools.com/docs/fuse/elements/element/cachingmode) |
| CalcRenderBounds : VisualBounds | UNO ||
| CalcRenderBoundsWithEffects : VisualBounds | UNO ||
| CaptureRegion(DrawContext, Rect, float2) : framebuffer | UNO ||
| ClipToBounds : bool | UX | 이 요소의 경계에 자식 요소를 시각적으로 클립합니다. |
| DrawNonUnderlayChildren(DrawContext) | UNO ||
| DrawUnderlayChildren(DrawContext) | UNO ||
| DrawWithChildren(DrawContext) | UNO ||
| ExplicitTransformOrigin : Size2 | UX ||
| FastTrackDrawWithOpacity(DrawContext) : bool | UNO ||
| GetContentSize(LayoutParams) : float2 | UNO ||
| GetVisibleViewportInvertPixelRect(DrawContext, VisualBounds) : Recti | UNO ||
| Height : Size | UX | `요소(Element)` 의 높이. [더보기](https://www.fusetools.com/docs/fuse/elements/element/height)  |
| HitTestLocalVisualBounds : VisualBounds | UNO ||
| HitTestMode : HitTestMode | UX | 이 요소에서 hit 테스트를 수행하는 방법을 지정합니다. |
| InvalidateRenderBoundsWithEffects | UNO ||
| IsPointInside(float2) : bool | UNO ||
| IsSelectionParentOf(Element) : bool | UNO ||
| LimitHeight : Size | UX | `BoxSizing="Limit"`를 사용하는 요소의 높이 제한입니다. |
| LimitWidth : Size | UX | `BoxSizing="Limit"`를 사용하는 요소의 너비 제한입니다. |
| Margin : float4 | UX | 요소의 여백 [더보기](https://www.fusetools.com/docs/fuse/elements/element/margin) |
| MaxHeight : Size | UX | 요소의 최대 높이 [더보기](https://www.fusetools.com/docs/fuse/elements/element/maxheight)  |
| MaxWidth : Size | UX | 요소의 최대 너비 [더보기](https://www.fusetools.com/docs/fuse/elements/element/maxwidth)  |
| MinHeight : Size | UX | 요소의 최소 너비 [더보기](https://www.fusetools.com/docs/fuse/elements/element/minheight)  |
| MinWidth : Size | UX | 요소의 최소 높이 [더보기](https://www.fusetools.com/docs/fuse/elements/element/minheight)  |
| Offset : Size2 | UX | 다른 모든 레이아웃이 적용된 후에 요소의 위치를 ​​오프셋합니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/offset) |
| OnDraw(DrawContext) | UNO ||
| OnHitTestLocalVisual(HitTestContext) | UNO ||
| Opacity : float | UX | 요소의 불투명도 [더보기](https://www.fusetools.com/docs/fuse/elements/element/opacity)  |
| Padding : float4 | UX | 요소의 여백(padding) [더보기](https://www.fusetools.com/docs/fuse/elements/element/padding) |
| Placed : PlacedHandler (object, PlacedArgs) | UX | 요소가 레이아웃 엔진에 의해 새로운 위치와 크기를 받으면 발생합니다. [더보기](https://www.fusetools.com/docs/fuse/elements/element/placed) |
| RenderBoundsWithEffects : VisualBounds | UNO ||
| RenderBoundsWithoutEffects : VisualBounds | UNO ||
| SetExplicitTransformOrigin(Size2, IPropertyListener) | UNO ||
| SetHitTestMode(HitTestMode, IPropertyListener) | UNO ||
| SetOpacity(float, IPropertyListener) | UNO ||
| SetVisibility(Visibility, IPropertyListener) | UNO ||
| TransformOrigin : ITransformOrigin | UX | 변형(transformation) 동작들 및 이동( [Move](https://www.fusetools.com/docs/fuse/animations/move) ), 크기 조절 ( [Scale](https://www.fusetools.com/docs/fuse/animations) , 회전, 크기 조절 ( [Scaling](https://www.fusetools.com/docs/fuse/scaling) ) 등과 같은 애니메이터들 에서 사용되는 변형(transformation)의 원형을 지정합니다. |
| Visibility : Visibility | UX | 요소의 가시성( [Visibility](https://www.fusetools.com/docs/fuse/elements/element/visibility) ) [더보기](https://www.fusetools.com/docs/fuse/elements/element/visibility) |
| Width : Size | UX | 요소의 너비 [더보기](https://www.fusetools.com/docs/fuse/elements/element/width) |
| X : Size | UX | 요소의 X 위치 [더보기](https://www.fusetools.com/docs/fuse/elements/element/x) |
| Y : Size | UX | 요소의 Y 위치 [더보기](https://www.fusetools.com/docs/fuse/elements/element/y) |


| Inherited from Visual | | |
|---|---|---|
| _firstNonUnderlay : int | UNO ||
| AbsoluteViewportOrigin : float2 | UNO | 뷰포트 (world) 공간에서 이 Visual의 원점을 반환합니다. [더보기](https://www.fusetools.com/docs/fuse/visual/absoluteviewportorigin) |
| AbsoluteZoom : float | UNO ||
| Add(Node) | UNO ||
| AddDrawCost(double) | UNO ||
| ArrangeMarginBox(float2, LayoutParams) : float2 | UNO ||
| BeginInteraction(object, Action) | UNO ||
| BeginRemoveChild(Node, Action) | UNO | 실제로 제거하기 전에, 모든 RemovedAnimations를 재생하면서 지정된 노드를 제거하기 시작합니다. |
| BeginRemoveVisual(Visual, Action) | UNO | 지정된 비주얼을 제거하고 실제 제거 전에 모든 RemovedAnimations를 재생합니다. |
| BringIntoView | UNO ||
| bringIntoView() | JS | 이 비주얼 요소가 화면에 표시되도록 요청합니다. 일반적으로 포함하는 `ScrollView` 는 표시되는지 확인하기 위해 스크롤 합니다. |
| BringToFront(Visual) | UNO | 주어진 자식을 Z-order 의 맨 앞으로 가져옵니다. UX 마크업에서는 [BringToFront](https://www.fusetools.com/docs/fuse/triggers/actions/bringtofront) 트리거 작업을 대신 사용하십시오. |
| CancelInteractions(CancelInteractionsType) | UNO ||
| Children : IList of Node | UX | 비주얼들의 자식요소들입니다. UX에서 비주얼 마크업 안에 있는 모든 노드가 이 목록에 지정됩니다. 이 목록의 순서에 따라 [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 들의 레이아웃 순서가 결정됩니다. 자식 요소의 Z-order 는 기본적으로 이 순서와 동일하지만 [BringToFront](https://www.fusetools.com/docs/fuse/triggers/actions/bringtofront)  및 [SendToBack](https://www.fusetools.com/docs/fuse/triggers/actions/sendtoback) 과 같은 메서드를 사용하여 개별적으로 조작할 수 있습니다. |
| Draw(DrawContext) | UNO ||
| DrawCost : double | UNO ||
| DrawLocalSelectionRect(DrawContext, Rect) | UNO ||
| DrawSelection(DrawContext) | UNO ||
| EndInteraction(object) | UNO ||
| FindByType<T> : T | UNO ||
| FindTemplate(string) : Template | UNO ||
| FindViewport : IViewport | UNO ||
| FirstChild<T> : T | UNO ||
| FirstVisualChild : Visual | UNO ||
| GetHitWindowPoint(float2) : Visual | UNO ||
| GetMarginSize(LayoutParams) : float2 | UNO ||
| GetNearestAncestorOfType<T> : T | UNO ||
| GetTransformTo(Visual) : float4x4 | UNO ||
| GetVisualChild(int) : Visual | UNO ||
| GetZOrderChild(int) : Visual | UNO ||
| HasChildren : bool | UNO ||
| HasPendingRemove : bool | UNO ||
| HasVisualChildren : bool | UNO ||
| HitTest(HitTestContext) | UNO ||
| HitTestBounds : VisualBounds | UNO ||
| HitTestChildrenBounds : VisualBounds | UNO ||
| HitTestLocalBounds : VisualBounds | UNO ||
| IfSnap(float2) : float2 | UNO ||
| IfSnapDown(float2) : float2 | UNO ||
| IfSnapUp(float2) : float2 | UNO ||
| Insert(int, Node) | UNO ||
| InvalidateHitTestBounds | UNO ||
| InvalidateLayout(InvalidateLayoutReason) | UNO | 일부 레이아웃 매개 변수 또는 내용이 변경되면 이 요소에 새 레이아웃이 필요함을 나타냅니다. [더보기](https://www.fusetools.com/docs/fuse/visual/invalidatelayout_f1613bd4) |
| InvalidateLocalTransform | UNO ||
| InvalidateRenderBounds | UNO | `RenderBounds` 가 변경되어 재계산될 필요가 있는 것을 나타냅니다. [더보기](https://www.fusetools.com/docs/fuse/visual/invalidaterenderbounds) |
| InvalidateVisual | UNO | 이 노드의 비주얼이 변경된 것을 나타냅니다. 이를 통해 루트 수준 노드는 그려야 한다는 것을 알 수 있고 이 노드의 캐시를 무효화 해야하는 모든 캐싱을 허용합니다. |
| InvalidateVisualComposition | UNO | 비주얼 구성이 변경되었지만 비주얼 도면 자체가 여전히 유효함을 나타냅니다 (예: 위치 변경). |
| IsContextEnabled : bool | UNO | 이 노드가 사용 가능한 컨텍스트에 있는지 여부입니다. 상위 노드들 중 하나에서 [IsEnabled](https://www.fusetools.com/docs/fuse/visual/isenabled) 가 `false` 로 설정된 경우 컨텍스트가 비활성화됩니다. |
| IsEnabled : bool | UX | 이 노드가 현재 상호 작용할 수 있는지 여부입니다. 비활성화된 비주얼은 입력 포커스를 받지 못합니다. 그러나, 그들은 여전히 ​​보이게될 수 있고 하위레이어 object에 대한 hit 테스트를 차단할 수 있습니다. |
| IsInteracting : bool | UNO ||
| IsLocalVisible : bool | UNO | 상위 비주얼이 숨겨져 있는지 또는 닫혀 있는지에 관계없이 이 비주얼을 볼 수 있는지 여부를 반환합니다. |
| IsMarginBoxDependent(Visual) : LayoutDependent | UNO ||
| IsVisible : bool | UNO | 이 비주얼이 현재 표시되는지 여부를 반환합니다. 상위 비주얼들이 숨겨 지거나 없어지게 되면 false를 반환합니다. 비주얼이 다른 비주얼에 의해 가려 지거나 뷰 밖에 있지만 보이지 않는 이유로 이 속성을 사용하여 비주얼을 숨길지 여부를 확인하는데 이 속성이 사용되지는 않습니다 |
| LastVisualChild : Visual | UNO ||
| Layer : Layer | UX | 이 비주얼 요소의 레이어는 [부모(Parent)](https://www.fusetools.com/docs/fuse/node/parent) 컨테이너에 속합니다. |
| LayoutRole : LayoutRole | UX | 이 비주얼이 레이아웃에 어떻게 참여 하는지를 설명합니다. |
| LocalBounds : Box | UNO ||
| LocalRenderBounds : VisualBounds | UNO ||
| LocalToParent(float2) : float2 | UNO | 좌표를 로컬 공간(local space)에서 부모 공간(parent space)으로 변환합니다. |
| LocalTransform : float4x4 | UNO ||
| LocalTransformInternal : FastMatrix | UNO ||
| LocalTransformInverse : float4x4 | UNO ||
| OnArrangeMarginBox(float2, LayoutParams) : float2 | UNO ||
| OnBeginRemoveVisual(PendingRemoveVisual) | UNO ||
| OnChildAdded(Node) | UNO ||
| OnChildRemoved(Node) | UNO ||
| OnHitTest(HitTestContext) | UNO ||
| OnInvalidateLayout | UNO ||
| OnInvalidateRenderBounds : bool | UNO ||
| OnInvalidateVisual | UNO ||
| OnInvalidateVisualComposition | UNO ||
| OnInvalidateWorldTransform | UNO ||
| OnIsContextEnabledChanged | UNO ||
| OnIsSelectedChanged(bool) | UNO ||
| OnIsVisibleChanged | UNO ||
| OnLocalVisibleChanged | UNO ||
| onParameterChanged(callback) | JS | 라우팅 매개 변수가 변경 될 때마다 호출 할 함수를 등록합니다. [더보기](https://www.fusetools.com/docs/fuse/visual/onparameterchanged_84d6a67b) |
| OnPropertyChanged(PropertyObject, Selector) | UNO ||
| OnZOrderInvalidated | UNO ||
| Parameter : string | UX | 이 비주얼에 대한 JSON 인코딩된 매개 변수 데이터입니다. 이 비주얼이 네비게이션 페이지를 나타내는 경우 라우터에 의해 제공됩니다. [더보기](https://www.fusetools.com/docs/fuse/visual/parameter) |
| PerformLayout | UNO ||
| PerformLayout(float2) | UNO ||
| PrependImplicitTransform(FastMatrix) | UNO ||
| PrependInverseTransformOrigin(FastMatrix) | UNO ||
| PrependTransformOrigin(FastMatrix) | UNO ||
| Remove(Node) : bool | UNO ||
| RemoveAllChildren<T> | UNO ||
| RemoveDrawCost(double) | UNO ||
| Resources : IList of Resource | UX | 이 노드에 정의된 리소스들의 목록 |
| SendToBack(Visual) | UNO | 주어진 자식을 Z-order 뒤쪽으로 보냅니다. UX 마크업에서는 대신 [SendToBack](https://www.fusetools.com/docs/fuse/triggers/actions/sendtoback) 트리거 작업을 사용하십시오. |
| SetResource(string, object) | UNO ||
| Snap(float2) : float2 | UNO ||
| SnapDown(float2) : float2 | UNO ||
| SnapToPixels : bool | UX | 이 비주얼의 레이아웃 결과를 물리적 디바이스 픽셀에 스냅할지 여부. |
| SnapUp(float2) : float2 | UNO ||
| Templates : IList of Template | UX | 이 비주얼을 채우는데 사용할 템플릿 목록입니다.  |
| TryParentToLocal(float2, float2) : bool | UNO | 좌표를 부모 공간에서 로컬 공간으로 변환합니다. |
| ValidFrameCount : int | UNO ||
| Viewport : IViewport | UNO ||
| VisualContext : VisualContext | UNO ||
| WindowToLocal(float2) : float2 | UNO ||
| WorldPosition : float3 | UNO ||
| WorldTransform : float4x4 | UNO ||
| WorldTransformInverse : float4x4 | UNO ||
| ZOffset : float | UX | ZOffset을 지정하면 더 높은 값이 다른 노드 앞에 표시됩니다. `Panel` 과 같은 특정 노드에서만 사용됩니다. ZLayer는 우선 순위가 있고 ZOffset, ZOffsetNatural 순으로 정렬됩니다. |
| ZOrderChildCount : int | UNO ||


| Inherited from [Node](https://www.fusetools.com/docs/fuse/node) | | |
|---|---|---|
| findData(symbol) | JS | 노드의 데이터 컨텍스트에서 주어진 심볼에 대한 데이터의 observable 을 반환합니다. [더보기](https://www.fusetools.com/docs/fuse/node/_finddata_b2ef1cd4) |
| Add(Binding) | UNO ||
| Bindings : IList of Binding | UX | 이 노드에 속하는 바인딩 목록입니다. |
| ContextParent : Node | UNO | content parent는 이 노드의 semantic parent 입니다. DataContext, 네비게이션 또는 다른 semantic 항목을 찾는 것처럼 non-UI 구조가 해결되어야 하는 곳입니다. |
| FindNodeByName(Selector, Predicate<Node> (Node)) : Node | UNO | 주어진 accepter 에 만족되는 특정 이름을 가지는 최초의 노드를 찾습니다. serach 알고리즘은 다음과 같이 작동합니다: 하위 트리의 노드가 먼저 일치하면 상위 노드들의 하위 트리들에 있는 노드들을 루트까지 끝까지 일치시킵니다. 일치하는 노드가 없으면 이 함수는 null을 반환합니다. |
| GetFirstData : object | UNO ||
| Insert(int, Binding) | UNO ||
| IsRootingCompleted : bool | UNO | 이 노드의 Rooting 이 완료되었는지 여부입니다. unrooting 이 시작되면 false를 반환합니다. |
| IsRootingStarted : bool | UNO | 이 노드의 Rooting 이 시작되었는지 여부. 이 property가 true를 반환하는 경우에서도, 노드의 rooting 은 아직 완료되지 않았을 수 있습니다. IsRootingCompleted를 참조하십시오. |
| Name : Selector | UX | 노드의 런타임 이름. 이 등록 정보는 ux:Name 속성을 사용하여 자동으로 설정됩니다. |
| OnDataContextChanged | UNO | 이 노드에서 DataContextChanged 이벤트를 발생시킵니다. |
| OnRooted | UNO | `OnRooted` 를 재정의하려면 파생 클래스에서 먼저 `base.OnRooted()` 를 호출해야 합니다. 다른 처리가 먼저 일어나지 않아야 합니다. 그렇지 않으면 정의되지 않은 상태가 될 수 있습니다. |
| OnSubtreeDataContextChanged | UNO | 파생 클래스에서 재정의 될 때, 이 노드의 바로 다음 자식 요소에서 OnDataContextChanged를 호출합니다. |
| OnUnrooted | UNO ||
| Parent : Visual | UNO | 이 노드의 부모 비주얼 입니다. 해당 노드가 rooting 되지 않은 경우는 null를 반환합니다.  |
| Properties : Properties | UNO | 외부 속성들에 대한 데이터가 있는 연결된 목록(liked list holding data) 입니다. |
| Remove(Binding) : bool | UNO ||
| SoftDispose | UNO ||
| SubtreeToString : string | UNO ||
| SubtreeToString(StringBuilder, int) | UNO ||
| TryGetResource(string, Predicate<object> (object), object) : bool | UNO ||
| VisitSubtree(Action<Node> (Node)) | UNO ||


| Inherited from [PropertyObject](https://www.fusetools.com/docs/uno/ux/propertyobject) | | |
|---|---|---|
| AddPropertyListener(IPropertyListener) | UNO ||
| OnPropertyChanged(Selector, IPropertyListener) | UNO ||
| OnPropertyChanged(Selector) | UNO ||
| RemovePropertyListener(IPropertyListener) | UNO ||


| Inherited from [object](https://www.fusetools.com/docs/uno/object) | | |
|---|---|---|
| Equals(object) : bool | UNO ||
| GetHashCode : int | UNO ||
| GetType : Type | UNO ||
| ToString : string | UNO ||


| Attached UX Attributes | | |
|---|---|---|
| ColorScheme (attached by Resources) : ColorScheme | UX ||
| Dock (attached by DockPanel) : Dock | UX | [DockPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) 내부에서 요소가 도킹되는 방법을 지정합니다. |
| Items (attached by Each) : object | UX | 이 비주얼을 채우는 데 사용될 아이템(item) 컬렉션입니다. [더보기](https://www.fusetools.com/docs/fuse/reactive/each/setitems_8527ec78) |
| MatchKey (attached by Each) : string | UX | `Items` attached 속성을 사용할때 생성된 암시적(implicit) `Each` 의 MatchKey 속성을 설정하기 위한 속기(shorthand). |
| Edge (attached by EdgeNavigation) : NavigationEdge | UX ||
| LayoutMaster (attached by LayoutControl) : Element | UX | 요소가 다른 요소의 레이아웃을 상속 받는다. [더보기](https://www.fusetools.com/docs/fuse/controls/layoutcontrol/setlayoutmaster_084fadc8) |
| Delegate (attached by Focus) : Visual | UX ||
| Gained (attached by AttachedFocusMembers) : FocusGainedHandler (object, FocusGainedArgs)  | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 에서 focus를 받을때 호출됩니다. |
| IsFocusable (attached by Focus) : bool | UX ||
| Lost (attached by AttachedFocusMembers) : FocusLostHandler (object, FocusLostArgs) | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 에서 focus를 잃을때 호출됩니다. |
| Clicked (attached by Clicked) : ClickedHandler (object, ClickedArgs) | UX ||
| Tapped (attached by Tapped) : TappedHandler (object, TappedArgs) | UX ||
| Column (attached by Grid) : int | UX | Grid에서 요소가 차지하는 열의 인덱스입니다. 설정되지 않은 경우 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 는 요소를 자식 목록의 위치에 따라 cell 에 배치합니다. |
| ColumnSpan (attached by Grid) : int | UX | 이 요소가 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 에서 차지하는 열 수입니다. 기본값은 1입니다. |
| Row (attached by Grid) : int | UX | Grid에서 요소가 차지하는 행의 인덱스입니다. 설정되지 않은 경우 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 는 요소를 자식 목록의 위치에 따라 cell 에 배치합니다. |
| RowSpan (attached by Grid) : int | UX | 이 요소가 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 에서 차지하는 행 수입니다. 기본값은 1입니다. |
| KeyPressed (attached by AttachedKeyboardMembers) : KeyPressedHandler (object, KeyPressedArgs) | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 에서 입력 포커스가 있는 동안 키 누르기(key press) 이벤트를 받으면 호출됩니다. 모바일 장치에서 키보드 입력은 소프트 키보드가 아닌 실제 단추 (예: BackButton)에만 적용됩니다. |
| KeyReleased (attached by AttachedKeyboardMembers) : KeyReleasedHandler (object, KeyReleasedArgs) | UX | [비주얼](https://www.fusetools.com/docs/fuse/controls/graphics/visual) 입력 포커스가 있는 동안 키 릴리스(key release) 이벤트를 받으면 호출됩니다. 모바일 장치에서 키보드 입력은 소프트 키보드가 아닌 실제 단추 (예: BackButton)에만 적용됩니다. |
| FillPadding (attached by Layout) : bool | UX ||
| LayoutMasterMode (attached by LayoutMasterAttr) : LayoutMasterMode | UX | [마스터 요소](https://www.fusetools.com/docs/fuse/controls/layoutcontrol/setlayoutmaster_084fadc8) 의 레이아웃을 사용하여 이 요소의 크기를 제어하는 ​​방법을 결정합니다. |
| Activated (attached by Activated) : PulseHandler (object, EventArgs) | UX | 페이지가 활성화 될 때 handler 를 추가합니다. |
| Deactivated (attached by Deactivated) : PulseHandler (object, EventArgs) | UX | 페이지가 비활성화 될 때 handler 를 추가합니다. |
| Navigation (attached by Navigation) : IBaseNavigation | UX ||
| Page (attached by NavigationPageProperty) : Visual | UX ||
| Transition (attached by NavigationControl) : NavigationControlTransition | UX ||
| IsReusable (attached by Navigator) : bool | UX ||
| Remove (attached by Navigator) : RemoveType | UX ||
| Reuse (attached by Navigator) : ReuseType | UX ||
| SwipeBack (attached by Navigator) : NavigatorSwipeDirection | UX ||
| Friction (attached by BodyAttr) : float | UX ||
| IsPhysicsWorld (attached by World) : bool | UX ||
| Entered (attached by AttachedPointerMembers) : PointerEnteredHandler (object, PointerEnteredArgs) | UX | 포인터가 비주얼로 들어올때 호출됩니다. |
| Left (attached by AttachedPointerMembers) : PointerLeftHandler (object, PointerLeftArgs)  | UX | 포인터가 비주얼을 벗어날 때 호출됩니다. |
| Moved (attached by AttachedPointerMembers) : PointerMovedHandler (object, PointerMovedArgs) | UX | 포인터가 비주얼 위에서 움직일때 호출됩니다. |
| Pressed (attached by AttachedPointerMembers) : PointerPressedHandler (object, PointerPressedArgs) | UX | 비주얼에서 포인터가 눌려지면 호출됩니다. |
| Released (attached by AttachedPointerMembers) : PointerReleasedHandler (object, PointerReleasedArgs) | UX | 비주얼에서 포인터가 해제되면 호출됩니다. |
| WheelMoved (attached by AttachedPointerMembers) : PointerWheelMovedHandler (object, PointerWheelMovedArgs) | UX | 비주얼에서 포인터 휠이 움직일 때 호출됩니다. |
| GlobalKey (attached by Resource) : string | UX | `ux:Global` 속성은 UX 마크업 모든 곳에서 액세스 할 수있는 글로벌 리소스를 만듭니다. [더보기](https://www.fusetools.com/docs/uno/ux/resource/setglobalkey_4c3ac72d) |


| Implemented Interfaces | | |
|---|---|---|
| IValue<string> | UNO ||
| IShow | UNO ||
| IHide | UNO ||
| ICollapse | UNO ||
| IActualPlacement | UNO ||
| IResize | UNO ||
| IList<Node> | UNO ||
| IPropertyListener | UNO ||
| ICollection<Node> | UNO ||
| IEnumerable<Node> | UNO ||
| IList<Binding> | UNO ||
| IScriptObject | UNO | 스크립트 엔진 표현을 가질 수있는 오브젝트들을 위한 인터페이스 |
| ICollection<Binding> | UNO ||
| IEnumerable<Binding> | UNO ||


## Remarks (비고) ##

`Text` 앱에 읽기 전용 텍스트를 표시하기 위한 기본 컨트롤입니다.

## Examples ##

### Text 속성(properties) ###

```
<Text Color="#999">Left</Text>
<Text TextAlignment="Center">Center</Text>
<Text FontSize="24" TextAlignment="Right">Right</Text>
<Text LineSpacing="10">
Multiple
Lines
</Text>
```

이 예제에서 첫 번째 텍스트 요소는 기본값이므로 왼쪽 정렬되며 색상은 중간 밝은 회색으로 설정됩니다. 두 번째 텍스트는 가운데 정렬됩니다. 세 번째는 오른쪽 정렬되는 큰 font를 사용합니다. 네번째는 두 줄 사이에 10 줄 간격을 줍니다.



