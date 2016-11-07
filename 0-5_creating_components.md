
원본: https://www.fusetools.com/docs/basics/creating-components

# 컴포넌트 생성하기 #
UX 마크업은 기존 컴포넌트들이 더 복잡한 컴포넌트들과 결합, 새로운 것을 생성할 수 있게 조합될 수 있는 구성가능한 선언적 언어입니다. UX 마크업 엘리먼트들의 어떤 트리도 `ux:Class` 속성을 사용함으로써 하나의 컴포넌트로 쉽게 변환이 가능합니다.

퓨즈는 여러가지 이유로 컴포넌트들(classes)속에 여러분의 앱을 차단하는 것을 권장합니다:
- 좋은 훈련입니다. 컴포넌트-지향은 여러분 코드를 개끗하고, 테스트 가능하고, 확장가능한 쉬운 관리를 유지합니다.
- 재사용. 다양한 장소에서 UI 와 로직의 조각 들을 재사용 하는 것을 허락함으로써 컴포넌트를 생성하는데 유용합니다.
- 스타일링. 원시 기반의 새로운 클래스들을 생성하는 것은 여러분 프로젝트 도처의 보기좋은 구성을 생성하기 위한 방법으로써 권장됩니다.

## 서브클래싱 ##
`ux:class` 속성 그 자체는 엘리먼트 타입이 기반 클래스인인 새로운 Uno 클래스를 생성합니다. 이 것은 새로운 클래스는 모든 공공의 속성들, 하위 클래스들로부터 행동 이벤트들을 상속 받는 것을 의미합니다.
> UX 마크업 내부에 대한 더 많은 정보를 위한 레퍼런스를 보십시오.

## 내부 로직 ##
컴포넌트들은 컴포넌트의 내부 비즈니스로직을 관리하는 자바스크립트 태그들을 포함할 수 있습니다.

## 의존성 (ux:Dependency) ##
컴포넌트들은 종종 작업 환경 안에서 객체들 이나 서비스들을 확실히 하기위해 액세스가 요구 되곤 한다. 예를 들어, 한 컴포넌트는 아마도 앱의 라우터에 액세스하기를 요청할 것입니다.
우리는 아래와 같이 `ux:Dependency` 속성을 사용함으로써 우리의 컴포넌트 안에서 의존성을 정의할 수 있습니다. 
```
<Panel ux:Class="MyBackButton" Clicked="{clicked}">
    <Router ux:Dependency="router" />
    <Panel ux:Dependency="panel" />
    <JavaScript>
        function clicked() {
            router.goBack();
        }
    <JavaScript>
    <WhilePressed>
        <Change panel.Opacity="0.5" Duration="0.3" />
    </WhilePressed>
</Panel>
```
위 예제는 `router`와 `panel` 두가지 의존성을 정의합니다. `router`는 컴포넌트가 클릭될때 `.goBack()`이 사용될 것입니다. `panel` 이라고 하는 패널은 컴포넌트가 눌려져 있는 동안에 반투명처리로 희미해질 것입니다.

디펜던시들은 Uno 안 에서 `readonly` 필드에 저장된 생성자 파라메터와 동등합니다. 이것은 오브젝트는 항상 초기화 때 알려지고, 결코 변화하지 않을 것이며, 그래서 그것의 이름이 부여됨으로써 자바스크립트안 이나 `Change` 같은 애니메이터들 안에서 직접 안전하게 사용할 수 있음을 의미합니다.

디펜던시들로 컴포넌트를 초기화하려할때, 여러분은 각각의 디펜던시를 위한 오브젝트를 제공해야만 합니다, 그렇지 않으면 컴파일 때 에러가 발생할 것입니다.
```
<App>
    <Router ux:Name="router" />
    <Panel ux:name="p1" />
    <MyBackButton router="router" panel="p1" />
</App>
```
컴포넌트는 그것의 디펜던시들을 위한 기본값을 제공하지 않습니다.

### 디펜던시들 상속하기 ###
디펜던시들은 여러분이 하위클래스면 배치되어지지 않습니다. 그러므로, 여러분은 수동으로 여러분이 하위클래스일때 그것들을 베이스클래스의 앞으로 이동시켜야만 합니다.
```
<Page ux:Class="A">
    <Router ux:Dependency="router" />
</Page>
<A ux:Class="B">
    <Router ux:Dependency="router" ux:Binding="router" />
</A>
``` 

## 속성들 (ux:Property) ##
프로퍼티들은 디펜던시들과 유사합니다, 그러나 그들은 변경가능합니다(chageable, animateable), 컴포넌트는 기본값이 제공 되어 질 수 있고, 프로퍼티는 프로퍼티 바인딩을 사용함으로써 property-bound 가 될 수 있습니다.
```
<Panel ux:Class="MyButton" BackgroundColor="#f00">
    <float4 ux:Property="BackgroundColor" />
    <Text>SUBMIT</Text>
    <Rectangle Layer="Background" Color="{Property this.BackgroundColor}" CornerRadius="10" />
</Panel>
```
위의 예제에서, 컨트롤은 `Background`라 불리는 새로운 프로퍼티를 기본값(루트 노드에 설정) `#f00`으로 정의합니다. 이 색은 그러고나면 코너가 라운딩된 배경 사각형의 `Color` 속성 property-bound 입니다.
컴포넌트가 인스턴스화 되면, 사용자는 기본 `BackgroundColor` 를 남겨두거나, 새로운 것을 설정할 수 있습니다.
```
<MyButton />  <!-- this one will keep the default color -->
<MyButton BackgroundColor="#0f0" /> <!-- this one will use green instead -->
```  
프로퍼티들은 또한 `<Change>` 애니메이터를 사용함으로써 애니메이션화 될 수 있습니다:
```
<MyButton ux:Name="mb1" />
<WhilePressed>
    <Change mb1.BackgroundColor="#00f" Duration="1" />
</WhilePressed>
```
디펜던시들 대신에 프로퍼티들을 사용하는 것의 문제점은 그 사이에 변경가능한 그들의 특성입니다, 프로젝트들로서 안에 들어온 오브젝트들은, 예를 들면 `<Change>` 애니메이터들 내, UX 마크업 안에서 이름에 의해 참조되어질 수 없습니다.
프로퍼티들은 자바스크립트 안에서 이름지어진 오브젝트들을 직접적으로 부르는 것이 불가능하지만, 오히려 모듈의 루트 스코프 내 `this` 오브젝트의 `Observable` 같은 프로퍼티들로써가 낫습니다.

여기 자바스크립트 안에서 프로퍼티 변화에 대한 응답을 어떻게 하는지에 대한 예제가 있습니다:
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
    module.exports = {rgbString};
    </JavaScript>
    <Text Value="{rgbString}" />
</Panel>
```

## 프로퍼티들을 통한 observable 전달하기 ##
Observables 은 또한 프로퍼티들을 사용해 자체제작한 컴포넌트들을 속으로 전달되어질 수 있습니디. 우리는 `object`프로퍼티를 만드는것으로써  Observables 를 수용하는 프로퍼티를 추가할 수 있습니다.
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
속성을 통해 전달된 observable 을 가져오려할때 여러분은 거의 항상 `inner()` 사용에 관심을 가집니다. 이것은 자바스크립트 값 `this.Propertyname` 가 `Properyname`이 포함하는 것들이 무엇이든지 간에 함께 하나의 observable 이기 때문입니다. 만약 우리가 하나의 Observable 을 전달한다면, `this.Propertyname`은 우리가 생각했던 그 observable 로 observable 하나를 포함할 것입입니다.  


## 템플릿들 (ux:Template) ##
템플릿들은 그것들의 외형을 위해 사용되어지는 사용자 엘리먼트들을 사용하게 함으로써 하나의 컴포넌트에 사용자의 특별함을 증가시키는데 사용되어 질수 있습니다. 이것의 하나의 예는 하나의 `PageControl` 내 각각의 페이지를 위한 템플릿으로부터 양신되는 엘리먼트 PageIndicator 엘리먼트입니다. 한 엘리먼트는 `ux:Template` 프로퍼티를 당신이 원하는 템플릿에 의한 정의를 키로 설정함으로써 하나의 템플릿으로 만들어집니다. 이 키는 템플릿이 적절한 템플릿들을 그리기 위한 탐색시 접근하는 엘리먼트들을 접근하는 것으로 사용되어집니다.  

템플릿들은 `Each`를 사용하여 그려집니다. `Each`는 `Each`가 그것의 템플릿으로부터 얻는 것으로부터 엘리먼트를 명시하는 `TemplateSource`라 불리는 프로퍼티를 가지고 있습니다. 전에 언급했듯이, 템플릿들은 그들 스스로를 하나의 키를 사용하여 그들스스로를 확인합니다. 템플릿을 탐색할 키 `Each`는 `TemplateKey`프로퍼티를 사용하여 지정되어집니다.

공통의 `TemplateSource`는 하나의 `ux:Class` 입니다. `Each`는 하나의 자식입니다:
```
<StackPanel ux:Class="CoolRepeater" Background="#FAD">
    <Each TemplateSource="this" TemplateKey="Item" Count="20" />
        <Text>Default template</Text>
    </Each>
</StackPanel>
```
이 것은 20번 반복으로 하나의 템플릿에 접근하는 사용자 컴포넌트 입니다. 만약 템플릿이 주어지지 않았다면, `Each`는 `Each`안에 무엇이 있던지간에 그것의 기본템플릿으로 대비할 것입니다.
```
<CoolRepeater>
    <Text ux:Template="Item">Hello, world!</Text>
</CoolRepeater>
```


## 이벤트들 (사용자이벤트) ##
세상밖에는 우리가 원하는 우리의 컴포넌트가 메시지를 전달할 많은 경우가 있습니다. 우리는 이 것을 위해 우리들이 UX 와 자바스크립트 안에서 이벤트들을 일으키고 핸들링 하기위해 허용되는 UserEvent 를 사용할 수 있습니다.
우리의 컴포넌트 클래스가 그것이 일으킬 수 있는 하나의 특정한 이벤트를 가리키는 것을 위한 우리 컴포넌트 클래스의 루트에 UserEvent 를 배치합니다. 우리의 UserEvent 가 배치된 곳은 중요하고, 이 후의 노드들은 단지 그것에 붙어서 그것의 자식이 그것을 일으키거나 핸들링합니다.
```
<Panel ux:Class="MyComponent">
    <UserEvent ux:Name="myEvent" />
</Panel>
```
이 것은 `myEvent`라 이름붙여진 하나의 이벤트를 생성합니다.
> 알림: 앱안 어느곳에서나 일어나고 핸들링 할 수 있는 UserEvent를 만들기 위해서 그 것을 다음과 같이 루트 App 노드에 정의합니다.:
```
<App>
    <UserEvent ux:Name="myGlobalEvent" />
    <!-- The rest of our app goes here -->
</App>
```
우리는 UX로 부터 이벤트를 발생시키기 위해 RaiseUserEvent를 사용할 수 있습니다.
```
<Panel ux:Class="MyComponent">
    <UserEvent ux:Name="myEvent" />

    <Clicked>
        <RaiseUserEvent EventName="myEvent" />
    </Clicked>
</Panel>
```
혹은 우리는 자바스크립트로 부터 그것을 발생시킬 수도 있습니다.
```
<Panel ux:Class="MyComponent">
    <UserEvent ux:Name="myEvent" />

    <JavaScript>
        setTimeout(function() {
            myEvent.raise();
        }, 5000);
    </JavaScript>
</Panel>
```
우리가 우리의 컴포넌트를 슨스턴스화 할 때, 우리는 OnUserEvent 트리거를 사용해 그것의 이벤트들에 응답할 수 있습니다.
```
<MyComponent>
    <OnUserEvent EventName="myEvent">
        <!-- Actions/animators go here -->
    </OnUserEvent>
</MyComponent>
```
우리가 우리의 현재 스코프의 밖에 선언되어졌음에도 불구하고 우리의 UserEvent를 이름에 의해 참조하고 있음을 알립니다. 우리는 `EventName` 이 이벤트의 `Name`을 참조하기때문에 이렇게 사용할 수 있습니다. 사실 UserEvent 인스턴스는 실행시간에 해석될 것입니다.
우리는 또한 자바스크립트에서 이벤트들을 핸들링할 수 있습니다.
```
<JavaScript>
    function eventHandler() {
        //do something
    }

    module.exports = { eventHandler: eventHandler };
</JavaScript>

<MyComponent>
    <OnUserEvent EventName="myEvent" Handler="{eventHandler}"/>
</MyComponent>
```
우리는 이벤트가 발생할 때 인자값들을 전달 할 수 있습니다.
```
myEvent.raise({
    userName: "james",
    isAdmin: false
});
```
이것은 또한 UX로 부터 이벤트가 일어날때 가능합니다.
```
<RaiseUserEvent EventName="myEvent">
    <UserEventArg Name="userName" StringValue="james" />
    <UserEventArg Name="isAdmin" BoolValue="false" />
</RaiseUserEvent>
```
The arguments are then passed to the event handler.
그리고나서 인자값들은 이벤트 핸들러에 전달되어 집니다. 
```
<JavaScript>
    function eventHandler(args) {
        console.log("Username: " + args.userName + ", Is admin: " + args.isAdmin);
    }

    module.exports = { eventHandler: eventHandler };
</JavaScript>

<OnUserEvent EventName="myEvent" Handler="{eventHandler}" />
```