원본: https://www.fusetools.com/docs/ux-markup/binding-and-auto-bind

# ux:Binding #

`ux:Binding` 은 UX 오브젝트가 바인딩해야 하는 속성을 명시적으로 선택하는데 사용됩니다.

## 구문 (Syntax) ##

```
<type ux:Binding="some_property" />
```

## 비고 (Remarks) ##

거의 필요하지는 않지만, 기본값이 아닌 다른 속성에 명시적으로 바인딩해야 하는 경우가 있습니다. `ux:Dependency` 에 대한 챕터 및 `ux:Binding` 을 사용해 이것들이 어떻게 전달되는지에 대한 챕터에서 해당 예제를 찾을 수 있습니다. 다음은 몇 가지 다른 예제들입니다.

### Container 클래스 ###

다음 예제에서는 `ux:Binding` 을 사용하여 `Rectangle` 을 `Container` 의 `Children` 속성에 명시적으로 바인딩해야 합니다. 이는 `Container` 가 `Children` 을 기본 속성으로 가지지 않기 때문입니다. 대신 자식 요소들을 `Subtree` 속성에 의해 지정된 요소로 전달합니다.

```
<Container ux:Class="MyContainer" Subtree="innerPanel">
    <Rectangle ux:Binding="Children" CornerRadius="10" Margin="10">
        <Stroke Color="Red" Width="2" />
        <Panel Margin="10" ux:Name="innerPanel" />
    </Rectangle>
</Container>
```

## CubicBezierEasing 으로 다양한 앞으로(forward) 및 뒤로(backward) easing 지정 ##

```
<Move X="100" Duration="0.3">
    <CubicBezierEasing ux:Binding="Easing" ControlPoints="0.4, 0.0, 1.0, 1.0" />
    <CubicBezierEasing ux:Binding="EasingBack" ControlPoints="0.3, 0.0, 0.3, 1.0" />
</Move>
```

위의 예제에서 두 개의 `CubicBezierEasing` 오브젝트를 다른 속성들에 명시적으로 바인딩하기 위한 목적으로 `ux:Binding` 을 사용해야 합니다. 그렇지 않으면, `CubicBezierEasing` 이 `Easing` 속성이 되도록 하기 위한 `Move` 의 기본 속성에 둘 다 바인딩하려고 할 것입니다.

## ux:AutoBind ##

`ux:AutoBind` 속성은 UX 오브젝트가 부모의 기본 속성(타입과 일치하는)에 자동으로 바인딩되어야 하는지 여부를 지정하는 데 사용됩니다. 이것은 몇 가지 예외를 제외하면, 일반적으로 우리가 별로 생각할 필요가 없는 것입니다.

## 구문 (Syntax) ##

```
<type ux:AutoBind="true/false" />
```

## 비고 (Remarks) ##

```
<StackPanel>
    <Text Value="Hello" />
    <Rectangle Color="Red" />
</StackPanel>
```

위의 예제에서 `Text` 와 `Rectangle` 은 `StackPanel` 의 `Children` 속성에 자동으로 바인딩됩니다. 이것은 일반적으로 우리가 고민할 필요 없는 세부구현 사항입니다만, 원하는 경우에는 `ux:AutoBind` 를 사용하여 해당 동작을 바꿀 수 있습니다.:

```
<StackPanel>
    <Text Value="Hello" ux:AutoBind="false" />
    <Rectangle Color="Red" ux:AutoBind="false"/>
</StackPanel>
```

위의 예제에서 우리는 `ux:Binding` 을 사용하여 속성들에 바인딩 할 수 있는 두 개의 "free floating" 요소를 만들었습니다.
