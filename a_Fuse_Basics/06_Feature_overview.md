원본: https://www.fusetools.com/docs/basics/feature-overview

# 기능 개요 #

이 문서는 Fuse 의 고수준 컨셉들과 패턴들에 대한 개요를 제공합니다. 각 주제에 대한 자세한 내용을 보려면 링크를 클릭하십시오.

## Uno 프로젝트 (.unoproj) ##

[Uno 프로젝트 문서](https://www.fusetools.com/docs/basics/uno-projects) 는 여러분 앱 프로젝트를 설정하는 방법, 참조 관리, 인클루드 및 번들링을 다룹니다.

이 문서는 여러분의 프로젝트가 여러분의 [App](https://www.fusetools.com/docs/fuse/app) 태그를 포함하는 최소 하나 이상의 UX 마크업 파일을 포함하도록 설정되어 있다고 가정합니다.

## [App](https://www.fusetools.com/docs/fuse/app) 태그 ##

App 태그는 응용 프로그램 트리의 루트입니다. 프로젝트의 UX 마크 업 파일 중 하나에 [App](https://www.fusetools.com/docs/fuse/app) 태그가 있으면, 프로젝트가 컴포넌트 라이브러리가 아닌 하나의 앱임을 나타냅니다.

Fuse는 자동으로 프로젝트의 [App](https://www.fusetools.com/docs/fuse/app) 태그를 찾아, 이를 루트 컴포넌트로 사용합니다. 프로젝트에는 하나의 [App](https://www.fusetools.com/docs/fuse/app) 태그만 있을 수 있습니다.

```
<App>
    <Text>Hello, world!</Text>
</App>
```

## 컴포넌트 ##

Fuse 에서 하나의 앱은 단순히 UX 마크업 컴포넌트의 트리입니다. (Uno 클래스의 인스턴스)

기본 빌딩 블록들은 [Text](https://www.fusetools.com/docs/fuse/controls/text), [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle), [Video](https://www.fusetools.com/docs/fuse/controls/video), [Slider](https://www.fusetools.com/docs/fuse/controls/slider) 또는 [MapView](https://www.fusetools.com/docs/fuse/controls/mapview) 와 같은 기본 요소들 입니다. [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 및 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 와 같은 계층적 레이아웃을 위해서는, [Panel](https://www.fusetools.com/docs/fuse/controls/panel) 을 사용하여 구성 할 수 있습니다.

```
<App>
    <StackPanel>
        <Text>Hello, World!</Text>
        <Text>Hello again!</Text>
    </StackPanel>
</App>
```

[App](https://www.fusetools.com/docs/fuse/app) 의 루트 레벨에서, 각 요소는 앱의 전체 라이프사이클 동안 한 번만 인스턴스화(생성) 됩니다. 그러나 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 같은 특별한 UX 노드들과 `ux:Template` 같은 속성들은 여러 인스턴스들을 생성 하거나, 지연-인스턴스화 하거나, 적절하게 컴포넌트들을 재사용 하도록 할 수 있습니다.

> 이런 토픽들에 대한 더 많은 정보는 [Primitives](https://www.fusetools.com/docs/primitives/primitives) , [Layout](https://www.fusetools.com/docs/layout/layout) 그리고 [Control](https://www.fusetools.com/docs/fuse/controls/control) 챕터들을 보십시오.

## 스크립팅과 데이터 컨텍스트 ##

Fuse 앱에서 비즈니스로직은 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 클래스 같은 스크립트 컴포넌트들을 사용하여 수행됩니다. 이것들은 앱 트리의 어느 레벨에나 배치 할 수 있습니다.

```
<App>
    <JavaScript>
        console.log("Hello, World!");

        module.exports = {
            items: [
                { color: "#f00" },
                { color: "#0f0" },
                { color: "#00f" }
            ]
        }
    </JavaScript>
    <StackPanel>
        <Each Items="{items}">
            <Panel Color="{color}" Height="40" Margin="10">
                <JavaScript>
                    console.log("hello!");
                </JavaScript>
            </Panel>
        </Each>
    </StackPanel>
</App>
```

스크립트들은 그것을 포함하고 있는 노드의 각 인스턴스에 대해 한 번씩만 실행됩니다. 위의 예에서 첫 번째 [Javascript](https://www.fusetools.com/docs/fuse/reactive/javascript) 의 경우, 전체 앱에 대해 한 번만 실행 됨을 의미합니다. 그러나 Each 안의 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 는 해당 Panel 의 각 인스턴스마다 한번씩 실행됩니다. 위의 예 에서,`hello!` 는 콘솔에 3번 출력됩니다.

각 스크립트의 `module.exports` 결과는, 하위 트리의 *데이터 컨텍스트* 가 됩니다. 데이터로 뷰를 채우기 위한 데이터 컨텍스트에 대해 중괄호 `{binding}` 구문으로 *데이터 바인딩* 을 사용 할 수 있습니다. 

데이터 컨텍스트들은 casecade 됩니다. 이는 모든 노드에서 외부 트리의 모든 데이터 컨텍스트들에 액세스 한다는 것을 의미합니다. 이름이 충돌하면, 해당 트리에서 가장 가까운 것이 우선합니다.

> 이 토픽에 대해 더 자제한 정보는 [스크립팅과 데이터 챕터](https://www.fusetools.com/docs/scripting/scripting) 를 보십시오.

## 새 컴포넌트 생성 (ux:Class) ##

UX 마크업은 기존 컴포넌트들을 결합하여 새로운 고급 컴포넌트를 만들 수 있는 조합 가능한 언어입니다. UX 마크업 요소들의 모든 트리는 `ux:Class` 속성을 사용함으로써 쉽게 컴포넌트로 변환 될 수 있습니다. 이것은 다양한 용도를 가집니다.

### 스타일링 ###

여러분 앱 전체에 일관된 look and feel (모양과 느낌)을 만들기 위해서, Fuse 는 기본 속성들 및 동작들을 할당하기 위한 원시 요소들의 하위 클래스들을 만드는 것을 필요로 합니다.

예를 들면, 고정된 텍스트 스타일을 제공하는 간단한 클래스(컴포넌트) 는 다음과 같습니다.

```
<Text ux:Class="HeaderText" FontSize="36" Color="#88f">
    <DropShadow Size="5" Angle="120" />
</Text>
```

다음과 같이 사용될 수 있습니다:

```
<HeaderText>This is a header</HeaderText>
```

Fuse 는 어떤 CSS 와 비슷한 걸 가지고 있지 *않고*, 구조로부터 스타일을 분리 하려는 어떤 시도도 하지 않습니다. 그러나 Fuse 는 비즈니스로직(예를 들면 자바스크립트에서 정의 되어진) 에서 시각적 user experience (UX 마크업에 정의된) 를 분리하기 위한 많은 노력을 기울이고 있습니다.

### 재사용 가능 컴포넌트 ###

하위 클래스화의 또 다른 중요한 사용 예는 선택적으로 내부 로직, 공용 속성들 및 이벤트들을 사용해 재사용 가능 컴포넌트들을 만드는 것입니다.

또 다른 예로, 여기 간단한 커스텀 버튼 컴포넌트가 있습니다:

```
<Panel ux:Class="MyButton">
    <string ux:Property="Text" />
    <Text Value="{Property this.Text}" />
    <Rectangle CornerRadius="5" Color="#ccf" />
</Panel>
```

여느 다른 컴포넌트 처럼 프로젝트의 어느 위치에서도 사용할 수 있습니다.

```
<MyButton Text="Submit" Clicked="{doSomething}" />
```

> 이 항목에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 를 참조하십시오. <br /> <br /> Uno 코드를 통해 새로운 UX 컴포넌트를 만들 수도 있습니다. 자세한 내용은 [Native Interop](https://www.fusetools.com/docs/native-interop/native-interop) 챕터를 참조하십시오.

## 네비게이션 ##

일반적인 앱은 사용자가 제스처들을 사용해 탐색하거나, 컨트롤을 탭하거나, 디바이스의 물리적인 뒤로가기 버튼 사용을 통해, [페이지](https://www.fusetools.com/docs/fuse/controls/page) 들의 한 세트를 구성합니다.

Fuse 앱의 네비게이션은 일반적으로 [App](https://www.fusetools.com/docs/fuse/app) 태그 내부에 직접 배치되는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 오브젝트를 통해 제어됩니다. 이렇게 하면 지정된 이름을 사용하여 해당 클래스 범위 내 모든 노드들에서 라우터를 사용할 수 있게 되고, 디바이스들의 물리적인 뒤로가기 버튼에 자동으로 연결합니다.

```
<App>
    <Router ux:Name="router" />
</App>
```

[Router](https://www.fusetools.com/docs/fuse/navigation/router) 그 자체로는 많은 것을 하지 않습니다. [Router](https://www.fusetools.com/docs/fuse/navigation/router) 는  하위 트리에 있는 실제 페이지들을 포함하는 `PageControl` 및 `Navigator` 와 같은 *router outlets* 를 통해 동작합니다. 그러면 해당 라우터를 자바스크립트로 제어 할 수 있습니다.

```
<App>
    <Router ux:Name="router">
    <PageControl>
        <Page ux:Name="contacts">
            ...
        </Page>
        <Page ux:Name="newsfeed">
            ...
        </Page>
        <Page ux:Name="settings">
            ...
        </Page>
    </PageControl>
</App>
```

> 이 주제에 대한 자세한 내용은 [Navigation chapter](https://www.fusetools.com/docs/navigation/navigation) 를 참조하십시오.

## 여러 UX 파일들로 분리 ##

프로젝트가 성장함에 따라, 우리는 보통 앱을 여러 UX 마크업 파일로 분할하길 원합니다. Fuse에서 UX 마크업을 분리하는 것은 모든 컴포넌트를 루트 노드로 여러분이 원하는 트리의 모든 레벨에서 수행 될 수 있습니다. 그러나 자연스럽고 추천할만한 것은 *page* 별로 나누는 것입니다.

위의 예는, 각 `Page` 태그들을 적절한 이름의 개별 UX 파일로 옮기는 것을 의미합니다. 우리가 이렇게 할 수 있는 두 가지 방법이 있습니다 :

### ux:Include 사용 - 단순하게 코드를 포함 ###

이것이 가장 쉬운 방법입니다. 다음과 같이 각 페이지를 별도의 파일에 붙여넣기만 하면됩니다.

`Contacts.ux` :

```
<Page ux:Name="contacts">
    ...
</Page>
```

`NewsFeed.ux` 와 `Settings.ux` 가 비슷합니다. 그런 다음 앱 UX를 다음과 같이 변경합니다.:

```
<App>
    <Router ux:Name="router" />
    <PageControl>
        <ux:Include File="Contacts.ux" />
        <ux:Include File="NewsFeed.ux" />
        <ux:Include File="Settings.ux" />
    </PageControl>
</App>
```

이것은 우리에게 여러 가지 텍스트 파일들로 작업 할 수 있도록 하면서, 동일한 행동을 제공합니다.

> 정확히 동일한 기술은 `ux:Name` 대신 `ux:Template` 속성이 있는 페이지 템플릿에 사용할 수 있습니다.

### ux:Class 사용 - 페이지를 하나의 컴포넌트로 전환 ###

UX 마크업 요소들의 트리를 추출하는 보다 강력한 방법은, 독립된(재사용 가능) 컴포넌트로 변환하는 것입니다. 이것은 컴포넌트들이 디펜던시들을 명시적으로 할 필요가 있으므로 약간의 코드를 더 포함합니다. 또한 컴포넌트의 테스트, 유지보수 및 재사용이 더 쉽다는 이점이 있습니다.

우리는 먼저 페이지 코드를 별도의 파일에 복사하고, `ux:Name` 을 적절한 `ux:Class` 이름으로 대체합니다. 클래스들의 첫 글자에는 대문자를 사용하는 것이 일반적입니다.

그런 다음, 이 페이지의 *디펜던시* 들을 확인하고 선언해야 합니다. 그것은 아마도 `router` 또는 외부 범위에서 선언 된 다른 오브젝트들에 있을 것입니다. 아래와 같이 `ux:Dependency` 속성을 사용하여 선언합니다.

`ContactsPage.ux`

```
<Page ux:Class="ContactsPage">
    <Router ux:Dependency="router" />
    ...
</Page>
```

`NewsFeedPage.ux` 와 `SettingsPage.ux` 도 비슷합니다. 그런 다음 App UX를 다음과 같이 변경합니다.

```
<App>
    <Router ux:Name="router" />
    <PageControl>
        <ContactsPage ux:Name="contacts" router="router" />
        <NewsFeedPage ux:Name="newsfeed" router="router" />
        <Settings ux:Name="settings" router="router" />
    </PageControl>
</App>
```

이렇게 하면 다른 컨텍스트들에서 재사용 될 수 있는 페이지 컴포넌트들을 허용하는 동시에, 동일한 동작을 얻을 수 있습니다. 컴포넌트를 다른 곳에서 재사용 할 수 있도록 하기 위한 유일한 요구사항은 우리가 디펜던시들을 제공하는 것입니다. (예: `Router` )

> 이 토픽에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 문서를 참조하십시오.
