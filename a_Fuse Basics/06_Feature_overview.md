원본: https://www.fusetools.com/docs/basics/feature-overview

# 기능 개요 #
이 문서는 Fuse 안에서 고수준의 컨셉들과 패턴들에 대한 개요를 제공합니다. 각 주제에 대한 자세한 내용을 보려면 링크를 클릭하십시오.


## Uno 프로젝트 (.unoproj) ##
우노 프로젝트들 문서는 어떻게 여러분 앱 프로젝트를 설정하고, 레퍼런스들을 관리하며, 인클루드 하고 빌딩하는 것을 다룹니다.
이 문서는 여러분의 프로젝트가 App 태그를 포함한 최소한 하나의 UX 마크업 파일을 포함하는 설정이 되어 있다고 가정합니다. 

[Uno 프로젝트 문서](https://www.fusetools.com/docs/basics/uno-projects) 에서는 앱 프로젝트 구성, 참조 관리, 포함 및 번들링 하는 방법을 다룹니다.
이 문서에서는 프로젝트 태그가 [App](https://www.fusetools.com/docs/fuse/app) 태그를 포함하는 하나 이상의 UX 마크 업 파일을 포함하도록 구성되어 있다고 가정합니다.

## [App](https://www.fusetools.com/docs/fuse/app) 태그 ##
App 태그는 응용 프로그램 트리의 루트입니다. 프로젝트의 UX 마크 업 파일 중 하나에 [App](https://www.fusetools.com/docs/fuse/app) 태그가 있으면 프로젝트가 구성 요소 라이브러리가 아닌 응용 프로그램임을 나타냅니다.
Fuse는 자동으로 프로젝트의 [App](https://www.fusetools.com/docs/fuse/app) 태그를 찾아 이를 루트 구성 요소로 사용합니다. 프로젝트에는 하나의 앱 태그만 있을 수 있습니다.
```
<App>
    <Text>Hello, world!</Text>
</App>
```


## 컴포넌트 ##
Fuse에서 하나의 앱은 단순히 UX마크업 컴포넌트의 트리입니다. (Uno 클래스의 인스턴스)
기본 빌딩 블록은 [Text](https://www.fusetools.com/docs/fuse/controls/text), [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle), [Video](https://www.fusetools.com/docs/fuse/controls/video), [Slider](https://www.fusetools.com/docs/fuse/controls/slider) 또는 [MapView](https://www.fusetools.com/docs/fuse/controls/mapview) 와 같은 기본 요소입니다. [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 및 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 와 같은 계층 적 레이아웃을위한 [Panel](https://www.fusetools.com/docs/fuse/controls/panel) 을 사용하여 구성 할 수 있습니다.
```
<App>
    <StackPanel>
        <Text>Hello, World!</Text>
        <Text>Hello again!</Text>
    </StackPanel>
</App>
```
[App](https://www.fusetools.com/docs/fuse/app) 의 루트 레벨에서 각 요소는 앱의 전체 수명 동안 한 번만 인스턴스화(생성) 됩니다. 그러나 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 같은 특별한 UX 노드와 `ux : Template` 과 같은 속성이 있어 여러 인스턴스를 생성하고, 적절한 인스턴스를 지연 생성하고, 재활용 할 수 있습니다.
> 이런 토픽들에 대한 더 많은 정보는 [Primitives](https://www.fusetools.com/docs/primitives/primitives) , [Layout](https://www.fusetools.com/docs/layout/layout) 과 [Control](https://www.fusetools.com/docs/fuse/controls/control) 챔터들을 보십시오.


## 스크립팅과 데이터 컨텍스트 ##
Fuse에서 비즈니스로직은 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 클래스와 같은 스크립트 컴포넌트들을 사용하여 수행됩니다. 이것들은 앱 트리의 어느 레벨 에나 배치 할 수 있습니다.
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
스크립트는 포함 노드의 각 인스턴스에 대해 한 번 실행됩니다. 위의 예에서 첫 번째 [Javascript](https://www.fusetools.com/docs/fuse/reactive/javascript) 의 경우 전체 앱에 대해 한 번만 의미합니다. 그러나 Each 안의 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) Panel의 각 인스턴스마다 한번씩 실행됩니다. 위의 예에서,`hello!` 는 3 번 콘솔에 출력됩니다.

각 스크립트의 결과 `module.exports` 는 하위 트리의 데이터 컨텍스트가됩니다. 데이터 컨텍스트에 중괄호 `{binding}` 구문 사용으로 데이터 바인딩을 사용하여 데이터로 뷰를 채울 수 있습니다.

데이터 컨텍스트 캐스케이드. 즉, 모든 노드에서 외부 트리의 모든 데이터 컨텍스트에 액세스 할 수 있습니다. 이름이 충돌하면 트리에서 가장 가까운 것이 우선합니다.

> 이 토픽에 대해 더 자제한 정보는 [스크립팅과 데이터 챕터](https://www.fusetools.com/docs/scripting/scripting) 를 보십시오.


## 새 컴포넌트 생성 (ux:Class) ##
UX 마크 업은 기존 구성 요소를 결합하여 새로운 고급 구성 요소를 만들 수있는 조합형 언어입니다. UX 마크 업 요소의 모든 트리는 `ux:Class` 속성을 사용하여 쉽게 구성 요소로 변환 될 수 있습니다. 여기에는 여러 용도가 있습니다.

### 스타일링 ###
일관된 모양과 느낌을 만들기 위해 Fuse는 기본 속성 및 동작을 할당하기 위해 원시 요소들의 하위 클래스를 만드는 데 의존합니다.
예를 들어 고정 텍스트 스타일을 제공하는 간단한 클래스 (컴포넌트)는 다음과 같습니다.
```
<Text ux:Class="HeaderText" FontSize="36" Color="#88f">
    <DropShadow Size="5" Angle="120" />
</Text>
```
다음과 같이 사용되어 질 수 있습니다:
```
<HeaderText>This is a header</HeaderText>
```
Fuse가 CSS와 동등하지 않으며 구조와 스타일을 분리하려고 시도하지도 않습니다. 그러나 Fuse는 UX 마크 업에서 정의 된 시각적 사용자 경험과 비즈니스로직(예를 들면 자바 스크립트에서 정의 되어진)을 분리하기 위해 많은 노력을 기울이고 있습니다.


### 재사용 가능한 컴포넌트 ###
하위클래스화의 또 다른 중요한 사용 사례는 선택적으로 내부 로직, 공용 속성 및 이벤트를 사용하여 재사용 가능한 구성 요소를 작성하는 것입니다.
또 다른 예로, 간단한 커스텀 버튼 컴포넌트가 있습니다:
```
<Panel ux:Class="MyButton">
    <string ux:Property="Text" />
    <Text Value="{Property this.Text}" />
    <Rectangle CornerRadius="5" Color="#ccf" />
</Panel>
```
그런 다음 다른 구성 요소와 같이 프로젝트의 어느 위치에서나 사용할 수 있습니다.
```
<MyButton Text="Submit" Clicked="{doSomething}" />
```
> 이 항목에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 를 참조하십시오.
Uno 코드를 통해 새로운 UX 컴포넌트를 만들 수도 있습니다. 자세한 내용은 [Native Interop](https://www.fusetools.com/docs/native-interop/native-interop) 장을 참조하십시오.


## 네비게이션 ##
일반적인 앱은 사용자가 제스처를 사용하여 탐색하거나, 컨트롤을 두드 리거나, 기기의 실제 뒤로 버튼을 사용하여 페이지를 구성하는 [페이지](https://www.fusetools.com/docs/fuse/controls/page) 로 구성됩니다.
Fuse 앱의 탐색은 일반적으로 App 태그 내부에 직접 배치되는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 오브젝트를 통해 제어됩니다. 이렇게하면 지정된 이름을 사용하여 클래스 범위의 모든 노드에서 라우터를 사용할 수 있게 되고 라우터가 있는 장치의 실제 뒤로가기 버튼이 자동으로 연결됩니다.
```
<App>
    <Router ux:Name="router" />
</App>
```
[Router](https://www.fusetools.com/docs/fuse/navigation/router) 그 자체로 많은 것을하지 않습니다. [Router](https://www.fusetools.com/docs/fuse/navigation/router) 는 하위 트리에 있는 `PageControl` 및 `Navigator` 와 같은 *router outlets* 를 사용하지만 실제로는 실제 페이지를 포함합니다. 그러면 라우터는 자바 스크립트로 제어 할 수 있습니다.
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
프로젝트가 성장함에 따라, 우리는 대개 앱을 여러 UX 마크 업 파일로 분할하려고 합니다. Fuse에서 UX 마크 업은 원하는 컴포넌트를 루트 노드로 사용하여 원하는 모든 트리에서 수행 할 수 있습니다. 그러나 자연스럽고 추천할만한 것은 *page* 별로 나누는 것입니다.
위의 예에서 각 `Page` 태그를 적절한 이름의 개별 UX 파일로 옮기는 것을 의미합니다. 우리가 이것을 할 수있는 두 가지 방법이 있습니다 :

### ux:Include 사용 - 단순하게 코드를 포함 ###
이것이 가장 쉬운 방법입니다. 다음과 같이 각 페이지를 별도의 파일에 붙여 넣기 만하면됩니다.
`Contacts.ux` :
```
<Page ux:Name="contacts">
    ...
</Page>
```
`NewsFeed.ux` 와 `Settings.ux` 가 비슷합니다. 그런 다음 앱 UX를 다음과 같이 변경합니다:
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
이것은 우리에게 여러 가지 텍스트 파일로 작업 할 수 있게 하면서 우리에게 동일한 행동을 제공합니다.
> 정확히 동일한 기술은 `ux:Name` 대신 `ux:Template` 속성이 있는 페이지 템플릿에 사용할 수 있습니다.


### ux:Class 사용 - 페이지를 컴포넌트로 전환 ###
UX 마크 업 요소 트리를 추출하는 보다 강력한 방법은 격리된(재사용할 수있는) 컴포넌트로 변환하는 것입니다. 여기에는 컴포넌트가 종속성을 명시적으로 작성해야 하므로 코드가 조금 더 필요합니다. 또한 컴포넌트의 테스트, 유지 보수 및 재사용이 더 쉽다는 이점이 있습니다.
먼저 페이지 코드를 별도의 파일에 복사하고 `ux:Name` 을 적절한 `ux:Class` 이름으로 대체합니다. 클래스의 첫 글자에는 대문자를 사용하는 것이 일반적입니다.
그런 다음이 페이지의 종속성을 확인하고 선언해야 합니다. 라우터 또는 외부 범위에서 선언 된 다른 오브젝트에서 발생할 수 있습니다. 아래와 같이 `ux:Dependency` 속성을 사용하여 선언합니다.
`ContactsPage.ux`
```
<Page ux:Class="ContactsPage">
    <Router ux:Dependency="router" />
    ...
</Page>
```
그리고 `NewsFeedPage.ux` 와 `SettingsPage.ux` 도 비슷합니다. 그런 다음 App UX를 다음과 같이 변경합니다.
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
이렇게 하면 페이지 컴포넌트를 다른 컨텍스트에서 재사용 할 수있는 동시에 동일한 동작을 얻을 수 있습니다. 컴포넌트를 다른 곳에서 재사용 할 수있는 유일한 요구 사항은 종속성(예: `Router` )을 제공 할 수 있다는 것입니다.
> 이 항목에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 문서를 참조하십시오.
