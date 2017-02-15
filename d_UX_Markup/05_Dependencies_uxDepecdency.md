원본: https://www.fusetools.com/docs/ux-markup/dependencies

# 디펜던시 (ux:Dependency) #

UX 마크업의 `ux:Dependency` 속성은 이를 포함하는 `ux:Class` 가 생성 가능하도록 하기위해 필요로 하는 새 디펜던시(Uno 생성자 인수)를 정의합니다.

## 구문 (Syntax) ##

```
<type ux:Dependency="dependency_name" />
```

여기서 `type` 은 UX 마크 업에서 액세스 할 수 있는 모든 타입이며, `depdendency_name` 은 유효한 Uno 식별자입니다.

## 비고 (Remarks) ##

컴포넌트( `ux:Class` 로 정의 됨)들은 종종 작업 환경에서 특정 오브젝트들 이나 서비스들에 대한 액세스를 필요로 합니다. 예를 들어, 컴포넌트가 [App](https://www.fusetools.com/docs/fuse/app) 의 [라우터](https://www.fusetools.com/docs/fuse/navigation/router) 에 액세스 해야 할 수 있습니다.

다음과 같이 `ux:Dependency` 속성을 사용하여 컴포넌트에 디펜던시를 선언 할 수 있습니다.

```
<Panel ux:Class="MyBackButton">
    <Router ux:Dependency="router" />
    <Panel ux:Dependency="panel" />

    <JavaScript>
        function clicked() {
            router.goBack();
        }

        module.exports = { clicked: clicked };
    <JavaScript>

    <Clicked>
        <Callback Handler="{clicked}">
    </Clicked>

    <WhilePressed>
        <Change panel.Opacity="0.5" Duration="0.3" />
    </WhilePressed>
</Panel>
```

위는 `router` 와 `panel`, 두 디펜던시들을 선언하는 예제 입니다. 컴포넌트가 클릭되면 `router` 는 `.goBack()` 을 위해 사용될 것입니다. 패널을 더빙한 `panel` 은 컴포넌트를 누른 상태에서 반투명으로 페이드(fade) 될 것입니다.

디펜던시들은 Uno의 생성자 인수와 동일하며, `readonly` 필드들에 저장됩니다. 이것은 오브젝트가 초기화시에 항상 알려지며 절대로 변경되지 않기 때문에, 우리는 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 나 애니메이터들에서 `Change` 같이 주어진 이름으로 직접 안전하게 사용할 수 있음을 의미합니다.

디펜던시들이 있는 컴포넌트를 인스턴스화 할 때 각 디펜던시에 대해 객체를 제공(즉, 디펜던시 주입) 해야 합니다. 그렇지 않으면 컴파일 타임에 오류가 생성됩니다.

```
<App>
    <Router ux:Name="router" />
    <Panel ux:Name="p1" />
    <MyBackButton router="router" panel="p1" />
</App>
```

컴포넌트는 디펜던시들에 대한 기본값(default value)을 제공 할 수 없습니다.

## 디펜던시 상속 ##

서브클래스(subclass)를 만들때 디펜던시는 전달되지 않습니다. 따라서 서브클래싱(subclassing) 하고 있는 기본 클래스(baseclass)에 수동으로 전달해야 합니다.

```
<Page ux:Class="A">
    <Router ux:Dependency="router" />
</Page>
<A ux:Class="B">
    <Router ux:Dependency="router" ux:Binding="router" />
</A>
```

## ux:Dependency 는 ux:Property 와 어떻게 다릅니까? ##

디펜던시들은 속성들과 비슷하게 작동하지만, 몇 가지 주요 차이점들이 있습니다.

- 디펜던시들은 변경할 수 없으므로, 시간이 지남에 따라 값이 변하지 않습니다.
- 디펜던시들은 인스턴스화 할 때 값이 제공되어야 합니다.
- 디펜던시들은 기본값을 가질 수 없습니다.
- 디펜던시들은 `<JavaScript>` 태그들에서 로컬 명명된 오브젝트들로써(Observables 아님) 직접적으로 사용할 수 있습니다.
- 디펜던시들은 해당 클래스 범위(scope)에서 로컬 이름들로 사용할 수 있습니다. 즉, `{Property foo}` 와 같은 바인딩을 사용하여 액세스할 필요없이, 그냥 `foo` 를 사용하면 됩니다.
