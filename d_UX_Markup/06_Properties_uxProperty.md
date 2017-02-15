원본: https://www.fusetools.com/docs/ux-markup/properties

# 속성 (ux:Property) #

UX 마크업의 `ux:Property` 속성은 이를 포함하는 `ux:Class` 에 새 속성을 정의합니다.

사용자 정의된 속성들은 커스텀 컴포넌트들에 대해 우리가 구성 가능한 것들 혹은 애니매이션이 가능한 매개변수들을 노출(expose) 할 수 있도록 합니다.

속성들은 `{Property PropertyName}` 또는 `{Property objectName.PropertyName}` 형식으로 *속성 바인딩* 을 사용하여 UX 마크업 내에서 참조 될 수 있습니다.

> UX 마크업에서 클래스(컴포넌트)를 만들 때, 기본 클래스(base class)의 모든 속성을 자동으로 상속합니다.

## 구문 (Syntax) ##

```
<type ux:Property="property_name" />
```

여기서 `type` 은 Uno 타입 ( `Panel` 또는 `string` 같은)에서의 이름이며, `property_name은` 유효한 Uno 식별자(identifier) 입니다.

## 기본값 (Default Values) ##

속성의 기본값은 이를 포함하는 `ux:Class` 의 일반 속성으로 지정될 수 있습니다.

예:

```
<Panel ux:Class="MyComponent" NumberOfThings="13">
    <int ux:Property="NumberOfThings" />
    ...
</Panel>
```

## 비고 (Remarks) ##

### 속성 바인딩 (Property bindings) ###

속성들은 `{Property PropertyName}` 또는 `{Property objectName.PropertyName}` 형식으로 *속성 바인딩* 을 사용하여 UX 마크업 내에서 참조 될 수 있습니다.

속성 바인딩은 *양방향* (기본값), *읽기 전용* ( `{ReadProperty ...}` ) 또는 *쓰기 전용* ( `{WriteProperty ...}` ) 일 수 있습니다. 자세한 내용은 [`PropertyBinding`](https://www.fusetools.com/docs/fuse/controls/propertybinding_1) 문서를 참조하십시오.

### 예제 ###

다음 예제에서, 컨트롤은 `BackgroundColor` 라는 새 속성을 정의하며, 기본값은 (루트 노드에 설정된) `#f00` 입니다. 이 색은 둥근 모서리가 있는 배경 사각형의 `Color` 속성에 속성 바인딩 됩니다.

```
<Panel ux:Class="MyButton" BackgroundColor="#f00">
    <float4 ux:Property="BackgroundColor" />
    <Text>SUBMIT</Text>
    <Rectangle Layer="Background" Color="{Property BackgroundColor}" CornerRadius="10" />
</Panel>
```

컴포넌트가 인스턴스화되면 사용자는 기본 `BackgroundColor` 를 그대로 두거나 새로 설정할 수 있습니다.

```
<MyButton />  <!-- 기본 색상을 사용합니다. -->
<MyButton BackgroundColor="#0f0" /> <!-- 기본 색상 대신 #0f0 을 사용합니다. -->
```

속성들은 `<Change>` 애니메이터를 사용하여 애니메이션 될 수도 있습니다.:

```
<MyButton ux:Name="mb1" />
<WhilePressed>
    <Change mb1.BackgroundColor="#00f" Duration="1" />
</WhilePressed>
```

### 속성에 적합한 타입 선택하기 ###

속성들은 *강한 형식* 이며, Uno 타입 이름으로 qualified 되어야 합니다. 모든 일반 UX 태그 이름들을 포함하는 모든 Uno 타입이 사용될 수 있습니다.

종종 프로퍼티들은 숫자(numbers), 텍스트(text), 색상(colors), 크기(sizes)(잠재적으로 `%` 또는 `px` 같은 단위) 혹은 백터(vectors)들과 같은 값 타입(value types)들을 가집니다. 이런 속성들에 대한 유효 값들의 유효 범위를 가장 잘 포착하는 속성 타입을 선택하는 것이 중요합니다.

대부분의 경우, 우리는 속성을 기존 속성에 직접 *속성 바인딩* 합니다. 이 경우 기존 속성에 대한 문서를 참조하고 동일한 타입을 사용하는 것이 좋습니다. 위의 예에서는 *float4* 를 타입으로 사용했는데, 이는 [Panel](https://www.fusetools.com/docs/fuse/controls/panel) 의 [Color](https://www.fusetools.com/docs/uno/color) 속성 타입이기 때문입니다.

다음은 타입 사용에 대한 몇 가지 지침입니다.:

- 전체 숫자들 (Whole Numbers)- `int` 사용
- 십진수 (Decimal Numbers) - `double` (혹은 `float` ) 사용
- `10%` 같은 단위를 가진 숫자들 - `size` 사용
- 2D 벡터들 (2D-Vectors) - 값이 hs 이면 `float2` 사용

### ux:Property 와 ux:Dependency 의 차이는 무엇입니까? ###

속성들은 디펜던시들(ux:Dependency)과 비슷하지만, 다음과 같은 차이점들이 있습니다.:

- 속성은 변경할 수 있습니다. 즉, 언제든지 변경하고 애니메이션으로 만들 수 있습니다.
- 컴포넌트는 기본값을 제공할 수 있지만, 디펜던시들은 항상 사용자가 주입해야합니다.
- 속성은 속성 바인딩을 사용하여 *속성 바인딩* 될 수 있습니다.

디펜던시들 대신 속성들을 사용하는 경우 한 가지 단점은 속성은 변경이 가능하다는 특성으로 인해 속성들로 전달된 *오브젝트* 들은 UX 마크업에서 이름으로 참조 될 수 없다는 것입니다 (예: `<change>` 애니메이터들에서).

디펜던시들과 달리 속성들은 JavaScript 에서 명명된 오브젝트들로 직접 사용할 수는 없지만, 해당 모듈의 루트 범위(scope)에서 `this` 오브젝트에 대한 `Observable` 속성들로 사용할 수 있습니다.

## JavaScript에서 속성 사용. ##

모든 사용자 정의 속성( `ux:Property` 를 통해 정의된)들은 해당 속성 범위(scope) 내에서 `JavaScript` 모듈들의 `Observable` 오브젝트들로 자동 반영됩니다.

다음은 JavaScript 에서 속성 변경들에 응답하는 방법의 예입니다.:

```
<Panel ux:Class="RgbDisplayer" RGB="#A00">
    <float4 ux:Property="RGB" />
    <JavaScript>
        var Observable = require("FuseJS/Observable");
        var rgbString = this.RGB.map(function(val) {
            return  "R: " + (val[0]*255).toFixed(1) + 
                " G: " + (val[1]*255).toFixed(1) + 
                " B: " + (val[2]*255).toFixed(1);
        });
        module.exports = { rgbString: rgbString };
    </JavaScript>
    <Text Value="{rgbString}" />
</Panel>
```

### JavaScript 에서 속성(properties)들에 대한 이해에 대한 중요성 ###

즉, JavaScript 코드가 백그라운드에서 실행되는 동안, 모든 UX 오브젝트들과 속성들은 UI 쓰레드에 존재합니다. 즉, **속성의 값은 여러분 JavaScript 코드가 실행중 일때 반드시 최신 상태는 아님을 의미합니다.** 이것은 혼란을 주는 일반적인 원인입니다.

observable 의 `.value` 속성을 읽는 대신, `.map()` 또는 `.onValueChanged()` 와 같은 reactive 연산자를 사용하십시오. 이것들도 역시 속성 값이 변경되거나 애니메이션이 적용될때 컴포넌트가 업데이트 되도록 합니다.

여러분이 모듈 실행시 바로 사용할 수 있는 속성 값이 필요하면, `ux:Dependency` 를 대신 사용해보십시오.

### 속성들을 통해 observables 전달하기 ###

Observables 역시 속성들을 사용하여 커스텀 컴포넌트들로 전달될 수 있습니다. 우리는 `object` 속성을 하나 만듦으로써 Observables 를 받아들이는 속성을 하나 추가할 수 있습니다.

```
<Panel ux:Class="CoolPanel">
    <object ux:Property="ObservableProperty" />
    <JavaScript>
        var passedInObservable = this.ObservableProperty.inner();
    </JavaScript>
</Panel>
<JavaScript>
    var Observable = require("FuseJS/Observable");
    module.exports = {
        valueToPass: Observable("123")
    };
</JavaScript>
<CoolPanel ObservableProperty="{valueToPass}" />
```

여러분은 거의 항상 속성들을 통해 전달된 observables 를 가져올때 `inner()` 를 사용하는 것에 관심이 있습니다. 이것은 javascript 값 `this.PropertyName` 이 `PropertyName` 이 포함한 것이 무엇이든 간에 하나의 observable 때문입니다. 우리가 하나의 Observable을 전달하면, `this.PropertyName` 은 우리가 전달했던 observable 인 observables 하나를 포함하게 됩니다.
