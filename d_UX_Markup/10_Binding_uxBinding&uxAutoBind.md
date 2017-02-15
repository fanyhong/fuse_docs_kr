원본: https://www.fusetools.com/docs/ux-markup/binding-and-auto-bind

ux : 바인딩

ux : 바인딩은 UX 객체가 바인딩해야하는 속성을 명시 적으로 선택하는 데 사용됩니다.

통사론

<유형 ux : 바인딩 = "some_property"/>
비고

거의 필요하지는 않지만, 기본값이 아닌 다른 속성에 명시 적으로 바인딩해야하는 경우가 있습니다. 예제는 ux : Dependency에 대한 장 및 ux : Binding을 사용하여 종속성을 전달하는 방법을 참조하십시오. 다음은 몇 가지 다른 예입니다.

Container 클래스

다음 예제에서는 ux : Binding을 사용하여 Rectangle을 Container의 Children 속성에 명시 적으로 바인딩해야합니다. 이는 Container가 Children을 기본 속성으로 가지지 않기 때문입니다. 대신 하위 요소를 Subtree 속성으로 지정된 요소로 전달합니다.

<컨테이너 ux : Class = "MyContainer"Subtree = "innerPanel">
    <Rectangle ux : Binding = "Children"CornerRadius = "10"Margin = "10">
        <스트로크 색상 = "빨간색"너비 = "2"/>
        <패널 여백 = "10"ux : Name = "innerPanel"/>
    </ Rectangle>
</ Container>
CubicBezierEasing으로 다양한 전진 및 후진 완화 지정

<이동 X = "100"지속 시간 = "0.3">
    <CubicBezierEasing ux : Binding = "Easing"ControlPoints = "0.4, 0.0, 1.0, 1.0"/>
    <CubicBezierEasing ux : Binding = "EasingBack"ControlPoints = "0.3, 0.0, 0.3, 1.0"/>
</ Move>
위의 예제에서 두 개의 CubicBezierEasing 객체를 다른 속성에 명시 적으로 바인딩하기 위해 ux : Binding을 사용해야합니다. 그렇지 않으면 둘 다 CubicBezierEasing이 Easing 속성이 될 Move의 기본 속성에 바인딩하려고합니다.

ux : 자동 바인딩

ux : AutoBind 특성은 UX 객체가 부모의 기본 속성 (유형과 일치하는)에 자동으로 바인딩되어야하는지 여부를 지정하는 데 사용됩니다. 이것은 몇 가지 예외를 제외하고 우리가 일반적으로 생각할 필요가없는 것입니다.

통사론

<유형 ux : AutoBind = "true / false"/>
비고

<StackPanel>
    <텍스트 값 = "Hello"/>
    <사각형 색상 = "빨강"/>
</ StackPanel>
위의 예제에서 Text와 Rectangle은 StackPanel의 Children 속성에 자동으로 바인딩됩니다. 이것은 일반적으로 걱정할 필요가없는 구현 세부 사항입니다.하지만 우리가하는 경우에는 ux : AutoBind를 사용하여 해당 동작을 바꿀 수 있습니다 :

<StackPanel>
    <텍스트 값 = "Hello"ux : AutoBind = "false"/>
    <Rectangle Color = "Red"ux : AutoBind = "false"/>
</ StackPanel>
위의 예제에서 우리는 ux : Binding을 사용하여 속성에 바인딩 할 수있는 두 개의 "free floating"요소를 만들었습니다.
