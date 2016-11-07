# 특징 개요 #
이 문서는 퓨즈 안에서 고수준의 컨셉들과 패턴들의 개요를 제공합니다. 아래는 각각의 토픽 세션들에 더 깊이 있는 자료를 링크합니다.

## Uno 프로젝트들 (.unoproj) ##
우노 프로젝트들 문서는 어떻게 여러분 앱 프로젝트를 설정하고, 레퍼런스들을 관리하며, 인클루드 하고 빌딩하는 것을 다룹니다.
이 문서는 여러분의 프로젝트가 App 태그를 포함한 최소한 하나의 UX 마크업 파일을 포함하는 설정이 되어 있다고 가정합니다. 


## App 태그 ##
App 태그는 여러분 어플리케이션 트리의 루트입니다. 어러분 프로젝트의 UX마크업 파일들 중 하나에 App 태그가 하나 존재하는 것은 그 프로젝트가 하나의 앱이며, 단순한 컴포넌트들의 라이브러리가 아님을 나타냅니다.
퓨즈는 자동적으로 여러분 프로젝트에서 App 태그를 찾아 루트 컴포넌트로서 사용합니다. 우리는 우리 프로젝트에서 하나의 App 태그만을 사용할 수 있습니다.
```
<App>
    <Text>Hello, world!</Text>
</App>
```


## 컴포넌트들 ##
퓨즈에서 하나의 앱은 단순히 UX마크업 컴포넌트들의 하나의 트리입니다.
초기 빌딩 블록은 Text, Rectangle, Video, Slider, MapView 같은 기초 요소입니다. 이들은 StackPanel 과 Grid 같은 계층 레이아웃을 위한 패널들(Panels) 를 사용함으로써 구성될 수 있습니다.
```
<App>
    <StackPanel>
        <Text>Hello, World!</Text>
        <Text>Hello again!</Text>
    </StackPanel>
</App>
```

여러분 앱의 루트레벨에서, 각각의 엘리먼트는 앱 전체 라이프사이클동안 단지 한번만 인스턴스화(생성)됩니다. 그러나, Each 같은 특별한 UX 노드들과 적절한 지연-인스턴스화 와 재활용 컴포넌트들 멀티 인스턴스들을 생성할 수 있는 `ux:Template` 같은 속성들이 있습니다. 
> 이런 토픽들에 대한 더 많은 정보는 기초요소들, 레이아웃들 과 컨트롤 챔터들을 보십시오.


## 스크립팅과 데이터 컨텍스트들 ##
퓨즈에서 비즈니스로직은 자바스크립트 클래스와 같은 스크립트 컴포넌트들을 사용하여 처리됩니다.
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
스크립트들은 노드를 포함하고 있는 각각의 인스턴스를 위해 한번 실행 될 것입니다. 위 예제의 첫번째 자바스크립트에서는 전체앱 동안 단 한번임을 의미랍니다. 그러나 Each 안쪽의 자바스크립트는 Panel 각각의 인스턴스 통안 한번 실행할 것입니다. 위 예제에서 hello! 는 콘솔에 3회 출력됩니다.

각각의 스크립트로 부터 module.exports 결과는 하위트리 동안 데이터 컨텍스트가 될 것입니다. 우리는 데이터로 된 뷰를 채우기 위한 데이터 컨텍스트들에 대해 중괄호 {binding} 문법으로 데이터바인딩을 할 수 있습니다. 

데이터 컨텍스트들은 캐스케이드됩니다. 이 것은 어떤 노드에서, 여러분이 외부 트리에 있는 모든 데이터 컨텍스트들에 접근하는 것을 뜻합니다. 만약 이름 충돌이 나면, 트리의 상단에서 가장 가까운 것을 우선적으로 취합니다.

> 이 토픽에 대해 더 자제한 정보는 스크립팅과 데이터 챕터를 보십시오.


## 새 컴포넌트들 생성 (ux:Class) ##
UX마크업은  
UX 마크업은 기존 컴포넌트들이 더 복잡한 컴포넌트들과 결합, 새로운 것을 생성할 수 있게 조합될 수 있는 구성가능한 언어입니다. UX 마크업 엘리먼트들의 어떤 트리도 `ux:Class` 속성을 사용함으로써 하나의 컴포넌트로 쉽게 변환이 가능합니다. 이 것이 다양한 쓰임새를 갖습니다.

### 스타일링 ###
여러분 앱 전반에 하나의 일관된 룩 앤 필을 생성하기 위해서, 퓨즈는 기본 프로퍼티들과 행동들을 할당하는 기초요소들의 하위클래스들을 생성하는 것에 의존합니다.
예를 들어, 여기 고정된 텍스트 스타일을 제공하는 간단한 클래스(컴포넌트) 가 있습니다.
```
<Text ux:Class="HeaderText" FontSize="36" Color="#88f">
    <DropShadow Size="5" Angle="120" />
</Text>
```
이렇게 사용되어 질 수 있습니다:
```
<HeaderText>This is a header</HeaderText>
```
어떻게 퓨즈가 어떤 CSS 와 같은 것을 가지지 않는 것인지, 구조로부터 스타일을 분리하기 위한 어떤 시도를 만들지 않는 것인지에 주목합니다. 하지만, 퓨즈가 (자바스크립트로 정의 된) 비즈니스 로직으로 부터 (UX 마크업으로 정의 된) 비쥬얼 사용자 경험을 분리하기 위해서는 장황해집니다.


### 컴포넌트들 ###
하위클래스화의 또 다른 중요한 사용 경우는 재사용가능한 컴포넌트들과 (경우에 따라서 내부로직, 공용 프로퍼티들과 이벤트들과) 빌딩하는 것입니다.
또 다른 예제로써, 여기 간단한 사용자 버튼 컴포넌트가 있습니다:
```
<Panel ux:Class="MyButton">
    <string ux:Property="Text" />
    <Text Value="{Property this.Text}" />
    <Rectangle CornerRadius="5" Color="#ccf" />
</Panel>
```
어떤 다른 컴포넌트 같이 프로젝트 내 어느곳에서든지 사용되어 질 수 있습니다. 
```
<MyButton Text="Submit" Clicked="{doSomething}" />
```
> 이 토픽에 대한 더 많은 정보는 컴포넌트 생성을 보십시오.
Uno 코드를 통해 새로운 UX 마크업을 생성하는 것 또한 가능합니다. 더 많은 정보는 네이티브 상호운용 챕터를 보십시오.


## 네비게이션 ##
일반적인 앱은 컨트롤들을 탭하거나 디바이스의 물리적인 뒤로가기 버튼을 사용하는 제스쳐들을 사용하는 사용자 네이게이트를 통한 페이지들의 집합으로 구성되어 있습니다. 
퓨즈앱에서의 네비게이션은 일반적으로 App 태그 안쪽에 바로 위치하는 Router 오브젝트에 의해 제어됩니다. 이 것은 주어진 이름을 사용하는 클래스 스코프 내의 모든 노드들에 라우터를 가능하게 만들고, 자동적으로 디바이스의 물리적 뒤로가기 버튼을 연결합니다. 
```
<App>
    <Router ux:Name="router" />
</App>
```
Router 는 그것 자체로 많은 것을 하진 않습니다. Router 는 실제 페이지들을 포함하는 하위트리 내에 위치한 `PageControl` 과 `Navigator` 같은 라우터 아웃렛들을 통해 동작합니다. Router 는 그 때 자바스크립트로 부터 제어될 수 있습니다.

The Router doesn't do much on its own. The Router works though router outlets such as PageControl and Navigator placed in the subtree, which in turn contains the actual pages. The Router can then be controlled from javascript.
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
> 이 토픽에 대한 더 자세한 것들은 네이게이션 챕터를 보십시오.


## 여러 UX 파일들로 나누기 ##
프로젝트가 커짐으로써, 우리는 보통 앱을 여러개의 UX 마크업 파일들로 분리하기를 원합니다. 퓨즈에서는, UX 마크업 파일을 분리가는 것이 루트 컴포넌트 같은 어떤 컴포넌트로 여러분이 바라는 트리에서 어떤 수준으로 처리 되어 질수 있습니다. 그러나, 자연스럽게 권장되는 시작위치로 추천되는 곳은 페이지로 분리하고 있습니다.

위 예제 에서, 이 것은 하나의 적절한 이름으로 분리된 UX 파일들 내의 `Page` 태크들로 각각 이동하는 것을 의미하는 것 같습니다.우리가 이것을 할 수 있는 두가지 방법이 있습니다:

### ux:Include 사용하기 - 단순하게 코드를 포함하기 ###
이 것이 가장 쉬운 접근 입니다. 이처럼 간단하게 분리된 파일 내 각 페이지를 복사해서 붙여 넣습니다:

`Contacts.ux`:
```
<Page ux:Name="contacts">
    ...
</Page>
```
`NewsFeed.ux` 와 `Settings.ux` 가 유사합니다. 그러면 우리는 우리 앱 UX 를 변경합니다:
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
이 것은 우리가 여러 텍스트 파일들로 작업하는 것을 허용하는 동안 우리에게 동일한 동작을 제공합니다. 
> 정확히 같은 테크닉은 `ux:Name` 대신 `ux:Template` 속성으로 페이지템플릿들을 위해 사용 될 수 있습니다. 


### ux:Class 사용하시 - 컴포넌트로 페이지를 넘기기 ###
UX 마크업 엘리먼트들의 하나의 트리를 뽑아내기 위한 더 강혁한 방법은 그것을 하나의 재사용 가능한 독립적인 컴포넌트로 변경하는 것입니다. 이 것은 그들의 명시적인 디펜던시들을 만드는데 필요한 컴포넌트들만큼 좀 더 많은 코드를 포함합니다. 추가되는 장점은 컴포넌트들이 테스트, 유지보스, 재사용이 더 쉬워진다는 것입니다. 
우리는 분리된 파일로 페이지 코드를 복사하고, 적절한 `ux:Class` 이름으로 `ux:Name` 을 재배치 함으로써 시작합니다. 클래스들의 첫 글자를 대문자로 시작하는 것은 공통입니다.

다음으로, 이 페이지의 디펜던시들에 대한 지시자 와 선언이 필요합니다. 그 것은 아마 라우터에 혹은 다른 오브젝트들이 스코프 바깥쪽에 선언되어집니다. 우리는 아래와 같이 `ux:Dependency` 속성을 사용함으로써 그들을 정의합니다. 
`ContactsPage.ux`
```
<Page ux:Class="ContactsPage">
    <Router ux:Dependency="router" />
    ...
</Page>
```
`NewsFeed.ux` 와 `Settings.ux` 가 유사합니다. 그러면 우리는 우리 앱 UX 를 변경합니다:
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
이 것은 우리가 다른 컨텍스트들에서 재사용되어지는 재사용되어지는 컴포넌트들을 것을 허용하는 동안 동일한 동작을 제공합니다. 컴포넌트를 다른 곳에서 재사용하는 동안 요구되는 것은 단지 우리가 디펜던시들(즉, 라우터)을 제공할 수 있다는 것입니다.
> 이 토픽에 대해 더 자세한 문서는 컴포넌트 생성하기를 보십시오.
