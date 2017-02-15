원본: https://www.fusetools.com/docs/ux-markup/names

# 로컬 이름 (ux:Name) #

UX 마크업의 `ux:Name` 속성은 UX 오브젝트에 로컬 이름을 제공하는데 사용됩니다.

## 구문 (Syntax) ##

```
<type ux:Name="node_name" />
```

여기서 `type` 은 UX 마크업에서 액세스할 수 있는 모든 타입이며, `node_name` 은 유효한 Uno 식별자 입니다.

## 비고 (Remarks) ##

UX 마크업의 모든 오브젝트(XML 요소)는 `ux:Name` 속성을 지정하여 로컬 이름을 지정할 수 있습니다.

```
<Panel ux:Name="panel1" />
```

한번 이름이 주어지면, 동일한 *범위(scope)* 내에서 이름으로 해당 오브젝트를 참조할 수 있습니다. 모든 UX 표현식(expression)은 일반(plain) 이름으로 이 오브젝트를 참조할 수 있습니다. 로컬 이름들은 동일한 이름을 가진 글로벌 리소스들 보다 우선합니다.

```
<Reactangle LayoutMaster="panel1" />
```

이름은 다음과 같은 UX 표현식들에서 직접적으로 사용될 수 있습니다.

```
<Rectangle Width="width(panel1) / 2"
```

> ux:Name 은 컴파일 타임 속성이므로 데이터 바인딩 될 수 없습니다. 데이터 바인딩은 런타임에 발생합니다.

## 유효한 이름들 (Uno identifiers) ##

이름들은 유효한 Uno 식별자들 이어야 합니다. 이는 문자들(A-Z 또는 a-z), 숫자들 및 밑줄들(underscores)만 포함 할 수 있으며 숫자로 시작할 수 없음을 의미합니다.
