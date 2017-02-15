원본: https://www.fusetools.com/docs/ux-markup/expressions

# UX 반응형 표현식 (Reactive Expressions) #

UX 마크업은 모든 오브젝트의 속성에서 사용할 수 있는 간단하고 강력하며 확장 가능한 *reactive expression (반응형 표현식)* 구문을 제공합니다. 이 문서는 어떻게 작동되는지 설명합니다.

> UX 표현식(expressions) 는 시험적인 기능으로 변경될 수 있습니다. UX 마크 업에서 이것을 사용한다면, 업데이트 할 때 changelog 를 잘 살펴보십시오. Uno 에서 이 기능을 사용할 예정이라면, 몇 가지 큰 API 변경이 계획 되어 있다는 것을 알아 두십시오.

## 빠른 요약 ##

UX Markup 의 *Reactive Expressions(반응형 표현식들)* 은 여러분이 *리터럴(literals)* , *자원(resources)* 및 지원되는 모든 *바인딩들(bindings)* 을 표현식(expressions)에 결합할 수 있게 합니다. 이런 표현식(expressions)들은 *바인딩(bindings)* 들이 새 값을 생성하는 경우 푸시 기반으로 자동 업데이트됩니다.

다음 예제에서는 슬라이더를 사용하여 사각형의 위치를 ​​제어합니다.

```
<Rectangle X="spring({Property slider.Value})*30% + 50%" Alignment="TopLeft" Width="32" Height="32" />
<Slider ux:Name="slider" Minimum="-1" Maximum="1" />
```

특히 사각형의 `X` 오프셋에 대한 표현식에 유의하십시오.

```
spring({Property slider.Value})*30% + 50%
```

즉, 사각형이 `50%` 오프셋되고, `spring` 물리 함수를 통해 `-30%` 에서 `30%` 사이의 값이 오프셋됩니다.

## 바인딩 (Bindings) ##

바인딩은 환경의 변화를 감지하고, UX 표현식(expression)에 영향을주는 새로운 값을 생성하는 객체입니다. 모든 바인딩 타입들은 표현식들의 일부로 사용될 수 있습니다.

- 데이터 바인딩 ( `{path.to.data}` )
- 속성 바인딩 ( `{Property someObject.SomeProperty}` 또는 `{Property SomeProperty}` )
- 리소스 바인딩 ( `{Resource resourceKey}` )

인라인 표현식( `{= expression}` )들도 중괄호 구문을 사용하지만, 이것들은 아직 기술적으로는 바인딩이 아닙니다.

## 연산 (Arithmetic) ##

UX 표현식은 단위( `%` 및 `px` 같은)들 존재여부와 관계없이, 스칼라 혹은 벡터 값을 더하기 ( `+` ), 빼기 ( `-` ), 곱하기 ( `*` ) 및 나누기 ( `/` ) 할 수 있습니다.

```
<Panel Width="{foo} * 100% + 40%" />
<Slider Margin="{foo}" />
```

연산은 동일한 단위의 값 사이에서, 또는 단위가 있는 하나의 값과 단위가 없는 하나의 값 사이에서 수행될 수 있습니다. 예를 들어 `10% + 10px` 은 연산할 수 **없습니다.** 이렇게 하면 런타임 오류가 발생합니다.

## 벡터 (Vectors) ##

벡터(최대 4개 컴포넌트들)들은 콤마 `,` 연산자를 사용하여 구성될 수 있습니다.

```
<Panel Margin="10, {spacing}, 10, {spacing} / 2" />
```

벡터는 `X` -> `XXXX` , `XY -> XYXY` 및 `XYZ` -> `XYZ1` 규칙에 따라, 더 짧은 벡터에서 적절한 크기로 자동 확장될 수도 있습니다.

```
<Panel Margin="{spacing}" />
```

## 문자열(strings) 과 텍스트(text) ##

`string` 속성( `<Text Value = ""` 같은)들은 다른 속성들과 다르게 파싱(구문 분석) 됩니다. 문자열 속성은 중괄호 내 피연산자들을 제외하고, 리터럴 문자열(literal string)로 처리됩니다.

```
<Text>Hello, {username}!</Text>
```

또는, 동등하게:

```
<Text Value="Hello, {username}" />
```

계산된 표현식을 string 값에 포함 시키려면, `{= expression}` 구문을 사용할 수 있습니다. 예를 들면 다음과 같습니다.:

```
<Text>Hello, {= toLower({username})}!</Text>
```

## 함수들 (functions) ##

> 참고: 현재 함수들 세트는 실험적이며 변경 될 것입니다. 앞으로의 changelogs 변화에 ​​주목하십시오.

현재 사용할 수 있는 함수들은 다음과 같습니다.

- `max(a, b)` - 최대 두 개의 숫자 값을 반환합니다 (벡터 인 경우 구성 요소 별 구성 요소).
- `min(a, b)` - 최소 두 개의 숫자 값을 반환합니다 (벡터 인 경우 구성 요소별로 구성 요소).
- `toLower(s)` - 주어진 문자열의 소문자 버전을 반환합니다.
- `toUpper(s)` - 주어진 문자열의 대문자 버전을 반환합니다.
- `width(element)` - 다른 UX 요소의 레이아웃 결과로 계산 된 너비를 반환합니다.
- `height(element)` - 다른 UX 요소의 레이아웃으로 계산 된 높이를 포인트 단위로 반환합니다.
- `spring(value)` - 주어진 값을 추적하는 스프링

**참고**: 레이아웃 함수( `width(element)` , `height(element)` )들에 대한 입력으로, 레이아웃 속성( `Width` , `Height` , `Margin` , `Padding` 같은)들을 사용하는 것을 피하십시오. 이렇게 하면 원치않은 이상한 동작이 발생할 수 있습니다.:

```
<Panel Width="10" Height="width(this)" />
```
