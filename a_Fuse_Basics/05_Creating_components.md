원본: https://www.fusetools.com/docs/basics/creating-components

# 컴포넌트 생성 #

UX 마크업은 기존의 컴포넌트들을 결합하여, 보다 새로운 복잡한 컴포넌트들을 만들기 위한 구성이 가능한 선언적 언어입니다. UX 마크업 요소들의 모든 트리는 `ux:Class` 속성을 사용함으로써, 쉽게 컴포넌트로 변환 될 수 있습니다.

Fuse는 몇 가지 이유로 앱을 컴포넌트 (클래스)로 분리하도록 권장합니다:

- 좋은 훈련이 됩니다. 컴포넌트 지향은 여러분 코드베이스를 깨끗하고, 테스트 가능하며, 확장가능하고, 유지보수하기 쉽게 유지하도록 합니다.
- 재사용이 가능합니다. 다양한 곳에서 UI 및 로직을 재사용 할 수 있도록 컴포넌트를 만드는 것이 유용합니다.
- 스타일링. 원시적인 것들에 기반한 새로운 클래스들을 만드는 것은, 프로젝트 전체에서 일관된 look and feel (모양과 느낌) 을 만들기 위한 방법으로 추천됩니다.

## 서브클래스화(상속) ##

`ux:Class` 속성 자체적으로 해당 요소의 타입이 *기본클래스(base class)* 인 새로운 Uno 클래스를 만듭니다. 이는 새로운 클래스가 서브클래스로부터 모든 공용 속성들, 이벤트들 및 동작을 상속 받는 것을 의미합니다.

> UX 마크업에 대한 더 많은 정보는 [레퍼런스](https://www.fusetools.com/docs/ux-markup/ux-markup) 를 보십시오.

## 내부 로직 ##

컴포넌트들은 컴포넌트의 내부 비즈니스로직을 관리하는 [자바스크립트](https://www.fusetools.com/docs/fuse/reactive/javascript) 태그들을 포함하는 것이 가능합니다.

## 디펜던시 (ux:Dependency) ##

컴포넌트들이 작업 환경에서 어떤 오브젝트들이나 서비스들에 액세스를 요청하는 경우가 종종 있습니다. 예를 들어, 컴포넌트가 [App](https://www.fusetools.com/docs/fuse/app) 의 [라우터](https://www.fusetools.com/docs/fuse/navigation/router) 에 액세스를 요청 할 수 있습니다.

다음과 같이 우리 컴포넌트에서 `ux:Dependency` 속성을 사용하여 디펜던시를 선언할 수 있습니다.

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

위의 예는 `router` 와 `panel`, 두 디펜던시들을 선언합니다. 위 컴포넌트가 클릭될 때, `router` 는 `.goBack()` 하기 위해 사용 될 것입니다. `panel` 이 더빙된 패널은 컴포넌트가 눌려진 동안, 반투명으로 페이드 될 것입니다.

디펜던시들은 Uno의 생성자 인수들과 동일하고, `readonly` 필드들에 저장됩니다. 이것은 해당 오브젝트가 초기화시에 항상 알려지고 절대 변경되지 않을 것임을 의미하기 때문에, 우리는 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 혹은 `Change` 같이 이름 붙여진 애니메이터들에서, 직접적으로 해당 오브젝트를 안전하게 사용할 수 있습니다.

디펜던시들을 포함한 컴포넌트를 인스턴스화 할 때, 여러분은 각 디펜던시(즉, 디펜던시 주입) 에 대한 오브젝트들을 제공해야 합니다. 그렇지 않으면 컴파일시 에러가 발생됩니다.

```
<App>
    <Router ux:Name="router" />
    <Panel ux:name="p1" />
    <MyBackButton router="router" panel="p1" />
</App>
```

컴포넌트는 해당 디펜던시들에 대한 초기값 제공이 불가능합니다.

### 디펜던시 상속 ###

여러분이 서브클래스를 만들 때, 디펜던시들은 전달되지 않습니다. 그러므로, 서브클래스화(상속) 하고 있는 해당 기본클래스(baseclass)로 디펜던시들을 여러분이 수동으로 전달해야 합니다.

```
<Page ux:Class="A">
    <Router ux:Dependency="router" />
</Page>
<A ux:Class="B">
    <Router ux:Dependency="router" ux:Binding="router" />
</A>
``` 

## 속성 (ux:Property) ##

속성들은 디펜던시들과 비슷하지만, 변경이 가능(변경 가능, 애니메이션 가능) 하고, 해당 컴포넌트는 초기값 제공이 가능하며, 해당 속성은 속성 바인딩을 사용하여 *property-bound*  될 수 있습니다.

```
<Panel ux:Class="MyButton" BackgroundColor="#f00">
    <float4 ux:Property="BackgroundColor" />
    <Text>SUBMIT</Text>
    <Rectangle Layer="Background" Color="{Property this.BackgroundColor}" CornerRadius="10" />
</Panel>
```

위의 예에서, 해당 컨트롤은 `BackgroundColor` 라는 새 속성을 정의하며 초기값은 (루트 노드에 설정된) `#f00` 을 가집니다. 그런 다음, 이 색은 둥근 모서리를 가진 배경 사각형의 `Color` 속성에 property-bound 됩니다.

컴포넌트가 인스턴스화되면, 사용자는 초기 `BackgroundColor` 를 그대로 두거나, 새로 설정할 수 있습니다.

```
<MyButton />  <!-- 이것은 초기값을 유지합니다. -->
<MyButton BackgroundColor="#0f0" /> <!-- 이것은 속성값으로 green 을 사용합니다. -->
```  

`<Change>` 애니메이터를 사용함으로써, 속성을 애니메이션으로 만들 수도 있습니다.

```
<MyButton ux:Name="mb1" />
<WhilePressed>
    <Change mb1.BackgroundColor="#00f" Duration="1" />
</WhilePressed>
```

디펜던시들 대신 속성들을 사용하는 단점은, 속성들이 변경 가능하다는 점 때문에 속성들로써 전달된 *오브젝트들* 은 UX 마크업에서 이름으로 참조될 수 없다는 것입니다. (예: `<Change>` 애니메이터들)

속성들은 JavaScript 에서 이름 붙여진 오브젝트들로 직접 사용할 수 없으며, 모듈의 루트 스코프(범위)에 있는 `this` 오브젝트의 `Observable` 속성들로는 사용 가능합니다.

다음은 JavaScript 에서 속성 변경들에 반응하기 위한 방법 중 하나의 예입니다:

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

## 속성을 통한 observable 전달 ##

Observable 들은 또한 속성들을 사용하는 커스텀 컴포넌트들로 전달 될 수 있습니다. 우리는 `object` 속성을 만들어, Observable 들을 받아 들이는 속성을 추가할 수 있습니다 :

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

대부분의 경우 속성들을 통해 전달 받은 Observable 을 가져올 때, `inner()` 를 사용하고자 할 것입니다. 이는 자바스크립트 `this.Propertyname` 값이 `Propertyname` 이 뭘 포함하던 상관없이 하나의 observable 이기 때문입니다. 우리가 Observable을 하나 전달하면, `this.Propertyname` 은 우리가 전달했던 그 observable 인 observable 을 하나 포함 할 것입니다.
 
## 속성을 통한 파일 참조 전달 ##

파일에 대한 참조를 컴포넌트로 전달하려는 경우가 있습니다. 예를 들면 이미지 또는 비디오를 우리 컴포넌트에 포함시킬때 입니다. 우리는 `FileSource` 타입의 속성들을 생성 한 뒤 우리 `Image` 또는 `Video` 파일의 `File` 속성을 그것에 property-bind 함으로써, 이 작업을 수행 할 수 있습니다. 그런 다음 클래스를 인스턴스화 할 때, 그것들의 이름으로 로컬 파일을 참조 할 수 있습니다.:

```
<App>
    <Panel ux:Class="TestComponent">

        <FileSource ux:Property="ImageFile" />
        <FileSource ux:Property="VideoFile" />

        <Grid Rows="1*,1*">
            <Image File="{ReadProperty ImageFile}" Margin="10"/>
            <Video File="{ReadProperty VideoFile}" AutoPlay="true" IsLooping="true"/>
        </Grid>
    </Panel>

    <TestComponent ImageFile="test.png" VideoFile="testvideo.mp4"/>
</App>
```

## 템플릿 (ux:Template) ##

템플릿들은 외형에 사용되는 커스텀 요소들을 가져와 컴포넌트의 커스터마이징을 향상시키기 위해 사용됩니다. 예를 들어, [PageIndicator](https://www.fusetools.com/docs/fuse/controls/pageindicator) 요소는 `PageControl` 각 페이지에 대한 템플릿에서 요소를 생성합니다. 요소는 `ux:Template` 속성을 해당 템플릿을 식별할 키로 설정하여 템플릿으로 만듭니다. 이 키는 사용 할 템플릿을 찾을때 템플릿을 자식요소로 허용하는 요소들에 사용됩니다.

템플릿은 `Each` 를 사용하여 그려집니다. `Each` 는 `Each` 가 템플릿을 얻은 것으로부터 요소를 지정하는 `TemplateSource` 라는 속성을 가지고 있습니다. 앞서 언급했듯이, 템플릿은 키를 사용하여 자신을 식별합니다. 해당 `TemplateKey` 속성은 `Each` 가 사용해야 하는 템플릿이 어떤 것인지를 지정합니다.

`Each` 는 부모 클래스 (`CoolRepeater`) 에서 키를 찾아야 하기 때문에, `TemplateSource="this"` 를 설정합니다.:

```
<StackPanel ux:Class="CoolRepeater" Background="#FAD">
    <Each TemplateSource="this" TemplateKey="Item" Count="20" />
        <Text>Default template</Text>
    </Each>
</StackPanel>
```

이 템플릿은 20번 반복 되는 템플릿을 허용하는 커스텀 컴포넌트 입니다. 템플릿이 주어지지 않으면, `Each` 안에 무엇이 있던 간에 `Each`는 기본 템플릿으로 대체될 것입니다. 커스텀 컴포넌트를 다음과 같이 사용할 수 있습니다.

```
<CoolRepeater>
    <Text ux:Template="Item">Hello, world!</Text>
</CoolRepeater>
```

## 이벤트 (UserEvent) ##

우리 컴포넌트가 메시지를 외부 세계로 전달하기를 원하는 경우가 많습니다. 이를 위해 UX 및 JavaScript 에서 우리가 이벤트를 발생시키고 처리 할 수 있도록 하는 [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 를 사용할 수 있습니다.

우리는 특정 이벤트를 발생시킬 수 있음을 나타내기 위해, 컴포넌트 클래스의 루트에 [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 를 두었습니다. 우리가 [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 를 배치하는 위치는 중요합니다, 왜냐하면 그것이 붙어있는 해당 노드와 그 자식요소만 이벤트를 발생시키거나 처리 할 수 ​​있기 때문입니다.

```
<Panel ux:Class="MyComponent">
    <UserEvent ux:Name="myEvent" />
</Panel>
```

이것은 `myEvent` 라는 이벤트를 생성합니다.

> 참고: 앱의 어디에서나 발생되거나 처리 될 수있는 [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 를 만들려면, 다음과 같이 루트 [App](https://www.fusetools.com/docs/fuse/app) 노드에 선언하십시오.:

```
<App>
    <UserEvent ux:Name="myGlobalEvent" />
    <!-- The rest of our app goes here -->
</App>
```

우리는 이제 UX로부터 이벤트를 발생시키기 위해 [RaiseUserEvent](https://www.fusetools.com/docs/fuse/triggers/actions/raiseuserevent) 를 사용할 수 있습니다.

```
<Panel ux:Class="MyComponent">
    <UserEvent ux:Name="myEvent" />

    <Clicked>
        <RaiseUserEvent EventName="myEvent" />
    </Clicked>
</Panel>
```

혹은 자바스크립트에서 이를 발생시킬 수도 있습니다.

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

컴포넌트를 인스턴스화하면, OnUserEvent 트리거를 사용하여 해당 이벤트들에 반응 할 수 있습니다.

```
<MyComponent>
    <OnUserEvent EventName="myEvent">
        <!-- Actions/animators go here -->
    </OnUserEvent>
</MyComponent>
```

[UserEvent](https://www.fusetools.com/docs/fuse/userevent) 는 비록 현재 스코프의 외부에서 선언되었지만, 우리는 이름으로 참조하고 있습니다. `EventName` 이 이벤트의 `Name`을 참조하므로 이 작업을 수행 할 수 있습니다. [UserEvent](https://www.fusetools.com/docs/fuse/userevent) 의 실제 인스턴스는 런타임에 결정 될 것입니다.

JavaScript에서 이벤트들을 처리 할 수도 있습니다.

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

이벤트가 발생할 때 인자값들을 전달 할 수 있습니다.

```
myEvent.raise({
    userName: "james",
    isAdmin: false
});
```

이는 UX로 부터 이벤트가 발생할때도 가능합니다.

```
<RaiseUserEvent EventName="myEvent">
    <UserEventArg Name="userName" StringValue="james" />
    <UserEventArg Name="isAdmin" BoolValue="false" />
</RaiseUserEvent>
```

인수들은 이벤트 핸들러로 전달됩니다.

```
<JavaScript>
    function eventHandler(args) {
        console.log("Username: " + args.userName + ", Is admin: " + args.isAdmin);
    }

    module.exports = { eventHandler: eventHandler };
</JavaScript>

<OnUserEvent EventName="myEvent" Handler="{eventHandler}" />
```
