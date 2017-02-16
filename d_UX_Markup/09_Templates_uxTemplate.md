원본: https://www.fusetools.com/docs/ux-markup/templates

# 템플릿 (ux:Template) #

UX 마크업의 `ux:Template` 속성은 XML 요소에 놓일 때 해당 XML 요소와 전체 하위 트리가 템플릿( `Uno.UX.Template` ) 임을 지정합니다. 템플릿들은 런타임에 UX 마크업 트리의 복사본을 임의 수로 생성할 수 있습니다.

## 구문 (Syntax) ##

```
<type ux:Template="template_name" />
```

여기서 `type` 은 UX 마크업에서 사용할 수 있는 모든 타입이 가능하며 `template_name` 은 유효한 Uno 식별자입니다.

## 비고 (Remarks) ##

템플릿들은 다음과 같은 몇 가지 상황에서 주로 사용됩니다.

- `<Each>` 반복자(repeater) 내부에서 컨텐츠 노드들은 암시적으로 템플릿입니다.
- page 템플릿들이 일반 노드들 대신 제공될 수 있는 `<Navigator>` 내에서 해당 `Navigator` 가 필요할 때마다 각 page 템플릿의 새 복사본들을 생성할 수 있도록 합니다.

## 템플릿 인스턴스의 ux:Name ##

`ux:Template` 로 표시된 노드 내부에서, 현재 템플릿의 현재 복사본(인스턴스)들은 `ux:Name` 을 사용하여 같은 방법으로 명명되었던 것과 같은 방식으로 `template_name` 에 의해 참조될 수 있습니다.

예를 들면, 포함한 `<JavaScript>` 태그에서 우리는 이름으로 템플릿 인스턴스를 참조할 수 있습니다.

```
<Page ux:Template="homePage">
    <JavaScript>
        homePage.Parameter.onValueChanged(module, function() { ...
```

또한, 일반적인 UX 마크업에서 표현식들(expressions)에 page 이름을 사용할 수 있습니다.

```
<Page ux:Template="homePage">
    <Rectangle LayoutMaster="homePage" ... />
```

## 커스텀화 컴포넌트를 위한 템플릿 사용 (Using templates for component customizability) ##

템플릿들은 외형을 위해 사용되는 커스텀 요소들을 가져오도록 하여 컴포넌트의 커스텀화를 향상 시키는데 사용될 수 있습니다. 이런 예를 들면 `PageControl` 내 각 페이지에 대한 템플릿에서 요소를 생성하는 [PageIndicator](https://www.fusetools.com/docs/fuse/controls/pageindicator) 요소가 있습니다. 요소는 `ux:Template` 속성을 여러분이 원하는 해당 템플릿 식별 키로 설정함으로써 하나의 템플릿으로 만들어집니다. 이 키는 그리기에 적절한 템플릿들을 찾을 때 요소들을 받아들이는 템플릿에 의해 사용됩니다.

템플릿은 `Each` 를 사용하여 그려집니다. `Each` 는 `Each` 가 해당 템플릿을 가져온 것으로부터 요소를 지정하는 `TemplateSource` 라는 속성을 가지고 있습니다. 앞서 언급했듯이 템플릿들은 키를 사용하여 자신들을 식별합니다. `Each` 키는 `TemplateKey` 속성을 사용하여 지정된 템플릿을 찾습니다.

일반적으로 `TemplateSource` 는 `Each` 를 자식요소로 가지는 `ux:Class` 입니다.:

```
<StackPanel ux:Class="CoolRepeater" Background="#FAD">
    <Each TemplateSource="this" TemplateKey="Item" Count="20" />
        <Text>Default template</Text>
    </Each>
</StackPanel>
```

이것은 20번 반복하는 템플릿을 허용하는 커스텀 컴포넌트입니다. 템플릿이 주어지지 않으면, `Each` 안에 무엇이 있던간에 `Each` 템플릿은 기본 템플릿으로 대체(fallback) 됩니다. 커스텀 컴포넌트는 다음과 같이 사용될 수 있습니다.

```
<CoolRepeater>
    <Text ux:Template="Item">Hello, world!</Text>
</CoolRepeater>
```
