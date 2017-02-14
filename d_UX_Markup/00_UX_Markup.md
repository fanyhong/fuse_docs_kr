원본: https://www.fusetools.com/docs/ux-markup/ux-markup

# UX 마크 업 참조 #

UX 마크업은 유저 인터페이스, 레이아웃, 효과(effects) 및 모션(motion) 들을 표현하기 위한 XML 기반 언어입니다. UX 마크업은 fuse의 필수적인 부분입니다.

이 장에서는 UX 마크업 언어의 내장 구문(built-in syntaxes) 및 속성(attributes)의 의미에 대해 설명합니다.

> 이 장에서 다른 topcis를 탐색하려면 왼쪽 메뉴를 사용하십시오.

## 개요 ##

### 오브젝트 생성 ###

UX 마크업 요소의 기본 기능은 특정 *클래스* 의 오브젝트를 생성(인스턴스화) 하는 것입니다. 예를 들면, 다음 코드는 App 및 Rectangle 클래스의 인스턴스를 만들고 Rectangle을 App의 자식으로 만듭니다.

```
<App>
    <Rectangle Color="Blue" />
</App>
```

요소 태그의 이름은 프로젝트에서 액세스 할 수 있는 *Uno 클래스* 의 이름이어야 합니다. 위의 예에서 [App](https://www.fusetools.com/docs/fuse/app) 과 [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) 은 Fuse 라이브러리 내에 미리 정의된 클래스입니다.

> 프로젝트에서 Fuse 라이브러리들을 액세스 할 수 있게 하려면, `.unoproj` 의 `Packages` 섹션에 `"fuse"` 가 추가되어 있어야 합니다.

## 계층 구조 만들기 ##

하나의 요소(자식요소)가 UX 마크업에서 다른 요소(부모요소) 안에 배치되면, 바깥쪽 요소를 *부모 요소* 라고 부르며, 안쪽 요소를 *자식 요소* 라고 합니다.

```
<Grid Rows="1*,1*">
    <Rectangle Color="Blue" Margin="10" />
    <Rectangle Color="Blue" Margin="10" />
</Grid>
```

기본적으로 UX 마크업은 부모 요소에서 자식 요소를 바인딩 할 수 있는 적합한 속성을 찾습니다.

주어진 자식 요소에 대해 이 동작을 사용하지 않으려면 `ux:AutoBind="false"` 를 지정하면 됩니다. 이렇게 하면 나중에 로직으로 앱에 첨부 할 수 있는 느슨한 오브젝트가 앱 내에 생성됩니다.

부모의 특정 속성에 바인딩하려면 `ux:Binding="TheProperty"` 를 지정할 수 있습니다. 이렇게 하면 요소에 대한 자동 바인딩이 비활성화되고 대신 TheProperty 에 바인딩 합니다.

## 재사용 가능한 클래스들 만들기 (ux:Class) ##

UX 요소 트리는 `ux:Class` 속성을 추가함으로써 재사용 가능한 클래스(컴포넌트)로 변환 될 수 있습니다.

```
<Panel ux:Class="MyNamespace.MyComponent" Color="Yellow" >
    <WhilePressed>
        <Scale Factor="2" Duration="0.3" Easing="BackOut" />
    </WhilePressed>
</Panel>
```

이렇게 하면 `MyNamespace` 네임 스페이스에 `MyComponent` 라는 새 서브클래스 `Panel` 이 만들어집니다. 네임 스페이스는 선택 사항이지만, 다른 프로젝트에서 다시 사용할 수 있도록 컴포넌트를 생성할때, 이름 충돌을 피하기 위해 권장됩니다.

클래스가 한번 만들어지면, 내장 클래스와 마찬가지로 인스턴스화 될 수 있습니다.

```
<MyNamespace.MyComponent />
```

각 `ux:Class` 는 새로운 *루트 범위(root scope)*를 만듭니다. 즉, 클래스 내부의 노드들은 클래스 외부의 이름들( `ux:Name` )에 액세스 할 수 없습니다. 이것은 디펜던시들(dependencies)이 명시적으로 주입되어져야 함을 의미합니다.

## [App](https://www.fusetools.com/docs/fuse/app) 태그 ##

`App` 태그는 여러분 앱 프로젝트의 최상위 루트(root) 입니다.

```
<App>
    <!-- your app goes here -->
</App>
```

모든 [App](https://www.fusetools.com/docs/fuse/app) 태그는 암묵적으로 포함하는 파일의 파일 확장명을 제외한 이름과 같은 `ux:Class` 클래스입니다. `ux:Class` 를 수동으로 지정하여 여러분 앱 클래스에 다른 이름을 지정할 수도 있습니다.

```
<App ux:Class="MyNamespace.MyApp" />
```

`App` 태그를 포함하지 않은 프로젝트들은 실제 앱을 생성하지 않는, 다른 프로젝트들에 의해 참조될 수 있는 컴포넌트들의 패키지들이 됩니다.

## 네임 스페이스 (Namespaces) ##

UX 마크업은 XML 네임 스페이스( `xmlns` )를 지원하지만, 코드를 간결하게 하기 위해, 적절한 기본값들로 설정되어 있습니다. UX 마크업 파일들에서 `xmlns` 가 사용된 것을 거의 볼 수 없는 이유입니다.

기본 XML 네임 스페이스 ( `xmlns` )는 기본적으로 모든 표준 Fuse 네임 스페이스들을 가리키고 있으므로, 클래스 (예: [App](https://www.fusetools.com/docs/fuse/app) 및 [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) )는 full namespace qualifier 없이 사용할 수 있습니다.

다른 클래스들의 경우 태그에 직접적으로 클래스의 full qualified name 을 직접 사용할 수 있습니다.

```
<App>
    <MyNamespace.MyClass />
</App>
```

또는 범위(scope)에 대한 커스텀 `xmlns` 선언을 지정할 수 있습니다.

```
<App xmlns:m="MyNamespace">
    <m:MyClass />
</App>
```

## UX 파일 포함하기 (ux:Include) ##

아래와 같이 `ux:Include` 를 사용하여, 또 다른 UX 파일 하나의 내용을 포함 할 수 있습니다.:

```
<ux:Include File="Resources.ux" />
```

`ux:Include` 는 [글꼴](https://www.fusetools.com/docs/fuse/font) , [이미지](https://www.fusetools.com/docs/fuse/controls/image) , 색상 등과 같은 정적 리소스들을 별도의 파일로 선언하여 사용할때 유용합니다.

그러나 여러분 뷰(view)의 일부를 파일들로 분할하데 사용하는 것은 권장되지 않습니다. 보다 나은 접근법은 [컴포넌트 생성하기](https://www.fusetools.com/docs/basics/creating-components) 기사를 참조하십시오.
