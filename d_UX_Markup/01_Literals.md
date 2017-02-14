원본: https://www.fusetools.com/docs/ux-markup/literals

# UX 마크업 리터럴 (UX Markup Literals) #

이 문서에서는 numbers, vectors, colors 및 strings 들이 UX 마크업에서 어떻게 해석되는지 자세히 설명합니다.

## Numbers ##

모든 숫자 타입( `float` , `int` , `double` , `Size` 등)은 JavaScript 에서의 numbers 와 같은 표기법으로 표현 되거나 Uno 에서의 `double` 로 표현될 수 있으며, 마침표( `.` )는 선택적인 decimal 구분 기호로 사용될 수 있습니다.

유효한 numbers :

```
1
1.3
13
.4
```

예:

```
<Panel Width="10.5" />
```

## 단위 (Units) ##

일부 속성들은 `%` (percent), `pt` (점) 또는 `px` (픽셀) 단위를 허용합니다. 이것은 단순히 number 리터럴(literal) 뒤에 단위를 넣어 나타냅니다.

예:

```
<Panel Width="50%" Height="200px" />
```

기본 단위는 `unsepecified` (지정하지 않음) 이므로, 지정된 단위의 값으로 연산하지 않는 한 `pt` (points)로 해석됩니다.

## 벡터 (Vectors) ##

벡터 타입들( `float2` , `float3` , `float4` , `Size2` 등)은 아래와 같은 규칙에 따라 해석되어 다양한 방법으로 작성할 수 있습니다.

*single number (하나의 숫자)* 값이 제공되면, 이 값은 각 컴포넌트에 대해 반복됩니다.

예를 들면:

```
<Panel Margin="10" />
```

은 다음과 같습니다.:

```
<Panel Margin="10, 10, 10, 10" />
```

comma( `,` )로 구분 된 *two numbers(두 개의 숫자( `X` 및 `Y` ))* 가 제공되면, 이 두 컴포넌트는 [ `X` , `Y` , `X` , `Y` ] 와 같이 대상 벡터의 크기까지 반복됩니다. 예를 들면:

```
<Panel Margin="4, 8" />
```

은 다음과 같습니다.


```
<Panel Margin="4, 8, 4, 8" />
```

이것은 `margin` 의 경우 두 개의 컴포넌트가 각각 수평(왼쪽/오른쪽) 및 수직(위쪽/아래) 여백을 지정한다는 것을 의미합니다.

comma( `,` )로 구분된 세 개의 숫자 ( `X` , `Y` 및 `Z` )가 제공되는 경우, 이 컴포넌트는 필요한 경우 추가 `1.0` 으로 채워집니다.

```
<Panel Color="1,0,1" />
```

은 다음과 같습니다.:

```
<Panel Color="1,0,1,1" />
```

즉, `Color` 의 경우 알파 채널( `W` )은 기본적으로 `1` 로 설정되고, 달리 지정되지 않으면 불투명한 색상이 지정됩니다.

## 표현식에서의 벡터 (Vectors in exepressions) ##

표현식의 일부로 벡터(vector)를 작성하려면, 괄호 안에 해당 벡터를 캡슐화하여 올바르게 파싱(parse) 하도록 해야합니다.

```
<Panel Color="(1,0,1,1) / {fadeValue}" />
```

## 크기 벡터 (Size Vectors) ##

몇몇 속성들은 `Size2` 타입으로 `X` 및 `Y` 에 대해 별도의 단위들을 지원합니다. 이러한 속성의 예는 `Element.Offset` 입니다. 이 값들은 벡터처럼 표시되며, 각 벡터 컴포넌트는 단위 지정이 가능합니다.

```
<Panel Offset="10%, 20px" />
```

## 색상 (Colors) ##

색상은 `float4` 타입 (또는 알파 채널이 없는 경우 `float3` )으로 표현됩니다. 이것은 벡터(vectors)와 동일한 방법으로 작성할 수 있음을 의미합니다. (위 참조)

색상 작업을 쉽게 하기 위해, `float4` 및 `float3` 벡터는 `#` 기호를 사용하여 16진수(hexadecimal) 표기법으로 표시될 수 있습니다.

```
<Panel Color="#18f" />
```

16진수 색상들(벡터)은 3, 4, 6 또는 8 개의 16진수로 설정될 수 있으며, 다음과 같이 해석됩니다.

- `#RGB` (또는 `#YYZ` )
- `#RGBA` (또는 `#XYZW` )
- `#RRGGBB` (또는 `#XXYYZZ` )
- `#RRGBBABA` (또는 `#XXYYZZWW` )

알파 채널 ( `A` 또는 `W` )가 생략되면, 1.0 ( `f` 또는 `ff` )의 알파 값으로 추정 합니다.

## 문자열 (Strings) ##

문자열(string) 속성들은 다른 속성들과 약간 다르게 파싱(parse) 됩니다. 문자열(String) 속성들은 기본적으로 raw string 리터럴(literal)들로 취급됩니다.

```
<Text Value="Hello, World!" />
```

바인딩 표현식을 삽입하려면, 다음과 같이 문자열 중간에 중괄호 표현식을 삽입하면 됩니다.:

```
<Text Value="Hello, {username}!" />
```

표현식에서 문자열을 연산하려면, *인라인 표현식* 구문을 사용하십시오.:

```
<Text Value="Hello {= toLower({username})}" />
```
