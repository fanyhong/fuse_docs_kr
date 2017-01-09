원본: https://www.fusetools.com/docs/tutorial/navigation-and-routing

## 소개 ##

[지난 챕터](https://www.fusetools.com/docs/tutorial/splitting-up-components) 에서, 우리는 앱의 뷰들을 분리된 컴포넌트들로 나눴습니다. 이것은 확장 가능한 아키텍쳐로 크게 한걸음 나간것입니다. 또한 우리는 [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 을 사용하는 네비게이션을 잠시 다뤄보았고, 이는 의미있는 방식으로 페이지들을 연결하기 위한 첫 걸음으로써 잘 동작했습니다.

"나란히" 놓여 스와이프가 가능하도록 하길 원하는 몇 개의 뷰들에는 [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 이 훌륭하게 동작하지만, 우리가 사용했던 사례에선 최선이 아닙니다. 예를 들어, `EditHikePage` 는 하이킹을 편집이 필요할때 까지는 그다지 유용하진 않습니다. 그래서 단순히 해당 뷰로 스와이프하는 것은 의미가 없습니다. 대신, `HomePage` 에서 편집할 수 있는 하이킹 중 하나를 선택하면 해당 뷰로 이동하고, 필요할 때까진 `EditHikePage` 를 불러 올 필요가 없다면 더 좋을 겁니다. 

그를 위해 Fuse는 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 및 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 클래스를 사용하여 이 모든 것을 처리 할 수있는 도구를 제공하고, 몇 가지 간단한 컨셉을 제공합니다. 이번 챕터에서는 이러한 것들을 앱에 단계별로 적용 할 때 어떻게 하는지 살펴 보겠습니다. 그럼 시작합시다!

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-4)에서 볼 수 있습니다.

## [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 로 마이그레이션 ##

[PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 과 마찬가지로 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 *네비게이션 컨테이너* 입니다. 이것은 우리가 탐색 할 수 있는 컴포넌트 (일반적으로 [Page](https://www.fusetools.com/docs/fuse/controls/page))를 포함 할 수 있는 컨트롤이라고 할 수 있습니다. 그러나 [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 과 달리 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 템플릿을 사용하여 필요에 따라 하위 컴포넌트를 인스턴스화 합니다. 이를 통해 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 필요한 경우 페이지를 인스턴스화하고 재활용하여 귀중한 시스템 자원을 보존 할 수 있습니다. 또한 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 자식 요소들 사이의 풍부한 관계를 표현할 수 있는 반면,[PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 처럼 페이지 사이를 스와이프하는 것을 지원하진 않습니다.

그렇다면 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 를 사용할 때 탐색 할 컴포넌트를 어떻게 요청합니까? 우리는 다음 섹션에서 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에 대해 다룰 것입니다. 본질적으로, 이러한 개념을 설명하는 가장 좋은 방법은 실제로 작동하는 것을 보는 것이고, 실행을 위해서는 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 필요합니다. [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 을 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 로 바꾸는 것으로 시작합시다.

먼저, `MainView.ux` 를 열어 [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 을 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 로 간단히 바꿉니다!:

```
<App>
    <ClientPanel>
        <Navigator>
            <HomePage />
            <EditHikePage />
        </Navigator>
    </ClientPanel>
</App>
```

이제 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 자식요소 대신 인스턴스의 템플릿을 기대하기 때문에 이를 업데이트해야 합니다. 다행스럽게도 Fuse를 사용하면 다음과 같이 쉽게 만들 수 있습니다. - 우리가해야 할 일은 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 의 각 자식 요소에 `ux:Template` 속성을 추가하는 것입니다.:

```
<App>
    <ClientPanel>
        <Navigator>
            <HomePage ux:Template="home" />
            <EditHikePage ux:Template="editHike" />
        </Navigator>
    </ClientPanel>
</App>
```

기본적으로, 이러한 각 속성은 지정된 키 (이 경우 `home` 및 `editHike` )에 대해 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 관련 클래스를 인스턴스화하게 하려는 것입니다. 따라서 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 `home`으로 이동하라는 요청을 받으면 `HomePage` 인스턴스를 인스턴스화하고 (이미 없는 경우) 거기에서 탐색합니다. 마찬가지로 `editHike` 로 이동하라는 메시지가 표시될때 `EditHikePage` 인스턴스가 인스턴스화되지 않은 경우 해당 인스턴스를 인스턴스화하고 대신 탐색합니다. 각 키가 고유하다는 점을 고려하여 원하는 템플릿을 추가 할 수 있습니다 (그렇지 않은 경우 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 주어진 키에 사용할 템플릿을 알 수 없을 것입니다). 

이제 저장하면 미리보기가 업데이트 됩니다만, 페이지가 사라집니다! 이것은 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 자식 컴포넌트를 생성하도록 하기 위한 템플릿을 지정하기만 하고 실제로 아무것도 인스턴스화하지 않았기 때문입니다. 다음 섹션에서 살펴 보겠지만, 일반적으로 특정한 *route* 로 이동하여 이 작업을 수행합니다. 그러나 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 아직 자식 항목 중 하나를 탐색하지 않은 경우 기본 자식 항목을 만드는 데 사용할 *기본 경로* 를 지정하는 기능도 지원합니다. 이는 우리의 사용 사례에 가장 적합한 일반적인 경우로, 먼저 우리의 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 가 홈페이지를 표시하고 편집을 위해 하이킹을 선택하면 `EditHikePage` 로 이동하도록 하기를 원합니다. 

[Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 의 기본 경로를 지정하려면 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 에서 처음 인스턴스화 할 때 사용할 템플릿의 키를 지정하는 `DefaultPath` 속성을 추가해야 합니다.:

```
<App>
    <ClientPanel>
        <Navigator DefaultPath="home">
            <HomePage ux:Template="home" />
            <EditHikePage ux:Template="editHike" />
        </Navigator>
    </ClientPanel>
</App>
```

이제 저장을 하면 `HomePage` 가 우리가 기대하는 것처럼 표시 될 것입니다. 멋지죠! 우리가 원한다면 `EditHikePage` 를 보여주기 위해 `DefaultPath` 를 `editHike` 로 변경할 수도 있습니다. 한번 해보세요!

## [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 통한 라우팅 ##

이제 우리는 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 와 템플릿을 사용하고 있으므로, [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 에게 어느 [Page](https://www.fusetools.com/docs/fuse/controls/page) 를 탐색 할 것인지 알려줄 차례입니다. 이제 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 가 등장 합니다!

[Router](https://www.fusetools.com/docs/fuse/navigation/router) 는 *라우팅* 을 관리합니다. 이 라우팅은 앱에서 어디로 이동할지 지정하고 실제 도착하게 하는 것을 포함합니다. 보다 구체적으로, [Router](https://www.fusetools.com/docs/fuse/navigation/router) 는 우리가 탐색하고자 하는 일종의 "타겟"을 결정하는 *route* 를 사용하여, 가능하면 추가 데이터까지 포함하는 해당 응용 프로그램을 탐색합니다. 라우팅에 참여할 컨트롤인 *router outlets* 를 찾기 위한 시각적 트리를 검색하는 것으로써, 실제 네비게이션을 수행합니다. 예를 들어, [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 과 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 는 모두 route outlets 입니다. [Router](https://www.fusetools.com/docs/fuse/navigation/router) 는 이전에 있었던 경로의 내역을 추적 할 수 있으며 원하는 경우 다시 탐색 할 수 있습니다.

이제 많은 것을 할 수 있게 되었습니다. 실제로 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 는 꽤 많은 일을 할 수 있고 매우 강력합니다! 사용하기 쉽고 직관적이기 때문에 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에 앱을 추가하고 두 페이지를 탐색하는데 어떻게 사용할 수 있는지 보도록 하겠습니다.

우리가 할 첫 번째 일은 실제로 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 생성하는 것입니다. 여기서는 전체 앱에 대해 하나의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 만 필요합니다. 이것은 아주 일반적인 것입니다. 그래서 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 `App` 클래스의 최상위에 추가 할 것입니다 :

```
<App>
    <Router />

    <ClientPanel>
        <Navigator DefaultTemplate="home">

        ...
```

이제 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 얻었으므로 라우터에 `EditHikePage` 로 이동하도록 알릴 수 있게 `HomePage` 에 이 인스턴스를 *삽입* 해야 합니다. 이를 수행하는 데는 몇 가지 방법이 있지만 우리의 경우에는 *dependency* 을 사용하는 것이 최선의 방법입니다. 디펜던시를 통해 컴포넌트가 제대로 작동하기 위해 필요한 추가 항목을 지정할 수 있습니다. 컴포넌트 중 하나에 디펜던시를 추가하면 Fuse는 이 디펜던시를 충족시킬 수있는 무언가를 지정해야 합니다. 그렇지 않으면 컴파일 오류가 발생합니다. 이렇게 하면 컴포넌트의 디펜던시가 항상 *충족* 되고 올바른 코드를 확인하는데 매우 유용합니다.

자, 우리의 `HomePage` 에 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 디펜던시를 추가합시다. 이는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 추가하는 것과 매우 유사하므로 먼저 [Page](https://www.fusetools.com/docs/fuse/controls/page) 상단에서 이를 수행합니다.

```
<Page ux:Class="HomePage">
    <Router />

    ...
```

디펜던시를 만들기 위해 해야 할 일은 `ux:Dependency` 속성을 추가하여 컴포넌트의 컨텍스트에서 이 디펜던시의 이름을 지정하는 것입니다. 예를 들어, 이 속성을 추가하고 우리의 디펜던시 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 호출합니다.

```
<Page ux:Class="HomePage">
    <Router ux:Dependency="router" />

    ...
```

이제 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 생성하는 대신 Fuse는 컴포넌트 인스턴스를 생성 할 때 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 디펜던시에 대한 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 지정해야 합니다. 이것은 디펜던시를 지정하는 것 외에도 `MainView` 의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 이 디펜던시에 연결해야 함을 의미합니다.

이를 위해 `MainView.ux` 로 돌아갑니다. 먼저 `ux:Name` 속성을 사용하여 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에 이름을 지정합니다.:

```
<App>
    <Router ux:Name="router" />

    ...
```

이름을 지정함으로써 이 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 쉽게 참조 할 수 있습니다. 다음으로, 우리는 이 인스턴스가 `HomePage` 인스턴스에 제공되는지 확인합니다.:

```
<App>
    <Router ux:Name="router" />

    <ClientPanel>
        <Navigator DefaultPath="home">
            <HomePage ux:Template="home" router="router" />

            ...
```

`HomePage` 인스턴스에 `router = "router"` 를 추가함으로써 `HomePage` 의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 디펜던시 ( `router=` 로 표시) 가 우리의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 오브젝트 ( `"router"` 로 표시됨)에 만족함을 알렸습니다. 디펜던시를 위해 소문자 이름을 사용하는 것을 제외하고는 속성 값을 지정하는 것과 비슷하게 보입니다. 또한 컴포넌트의 디펜던시는 잘 동작하는 컴포넌트에 충족 되어야 하기 때문에, 이 값을 지정하지 않으면 Fuse는 오류를 발생합니다.

이제 `MainView.ux` 와 `HomePage.ux` 를 모두 저장할 수 있습니다. 미리보기를 실행하는 경우 Fuse는 파일을 저장하는 중 오류가 발생할 수 있습니다. 파일을 저장하는 순서에 따라서 어떤 오류가 발생할 수 있는데, 이는 Fuse가 우리의 디펜던시나 이를 만족시키기 위해 지정한 값을 알지 못한다는 사실에서 비롯됩니다. 그러나 두 파일을 모두 저장 한 후에는, 모든 것이 정상적으로 돌아가고 `HomePage` 는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에 액세스 할 수 있게 됩니다.

우리의 `HomePage` 는 작업 할 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스를 가지므로, 우리는 이 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 사용하여 `EditHikePage` 를 탐색하기를 원합니다. `HomePage.js` 파일을 살펴보면 `chooseHike` 라고 하는 selector 뷰에 연결한 함수가 이미 있습니다.

```
function chooseHike(arg) {
    // TODO
}
```

이 함수를 작성하기 전에, `chooseHike` 에서 `goToHike` 로 이름을 바꾸고 싶습니다. 이는 세부 사항이지만 기능의 의도를 더 잘 전달하고 작업을 조금 더 깨끗하게 유지하도록 합니다. 이름을 바꿉시다.:

```
function goToHike(arg) {
    // TODO
}
```

아래 `module.exports` 에서 이름을 업데이트 합니다.

```
module.exports = {
    hikes: hikes,

    goToHike: goToHike
};
```

마지막으로 `HomePage.ux` 에서 참조를 업데이트합니다.:

```
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{goToHike}" />
            </Each>
```

좀 나아졌네요! 이제 파일을 저장하면 `HomePage.js` 로 돌아가서 `goToHike` 함수를 채울 준비가 된 것입니다!

우선 우리가 해야 할 일은 함수의 인수에서 hike 오브젝트를 잡는 것입니다. 우리는 [챕터2](https://www.fusetools.com/docs/tutorial/multiple-hikes) 에서 했던 것처럼 정확하게 다음과 같이 할 것입니다.:

```
function goToHike(arg) {
    var hike = arg.data;
}
```

이제 hike 오브젝트를 얻었고, `EditHikePage` 로 이동해야 하므로 UX의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 디펜던시가 필요합니다. 다행스럽게도, Fuse에서 `ux:Dependency` 의 값은 JavaScript의 이름을 참조함으로써 간단히 JavaScript로부터 검색 될 수 있습니다. 따라서 `HomePage.js` 의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 참조한다고 하면, 실제로는 `HomePage.ux` 의 `ux:Dependency` 를 통해 `MainView.ux` 로 부터 전달되어진 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 참조하고 있는 것입니다.:

```
function goToHike(arg) {
    var hike = arg.data;
    router // We have our Router, but what will we do with it?
}
```

쉽습니다! 이제 우리가 해야 할 일은 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에게 `EditHikePage` 로 이동하라고 하는 것입니다. 이를 위해 우리는 `push` 라는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에서 사용할 수있는 JavaScript 함수를 사용합니다.:

```
function goToHike(arg) {
    var hike = arg.data;
    router.push("editHike");
}
```

`push` 기능은 지정된 *경로* 로 이동하는 것입니다. 이 경우에는 단순히 `"editHike"` 로 지정됩니다. 우리의 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 와 `MainView.ux` 의 템플릿을 기억한다면, 이것은 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 가 우리가 원했던 것처럼 `EditHikePage` 의 인스턴스로 지정 할 `editHike` 로 이동한다는 것을 의미합니다! `HomePage.js` 를 저장하고 하이킹 중 하나를 선택하면 우리 앱이 예상했던 것처럼 `EditHike` 페이지로 이동합니다. 물론, 우리가 아직 실제로 hike 오브젝트를을 이 페이지로 보내지는 않았지만, 곧 그렇게 할 것입니다.

우리가 `HomePage` 에서 `EditHikePage` 에 이르기까지 네비게이션을 얻은 것은 분명 좋았지만, 계속 진행하기 전에 `HomePage`로 돌아갈 수 있는 방법이 필요합니다. 원래 디자인을 보면 `Save` 버튼과 `Cancel` 버튼이 있음을 알 수 있습니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/9ad834ec45583c3d17817e1973f50449__media/hikr/tutorial/app-flow.webp)

그러나 아직 모델을 변경하지 않으므로 `Back` 버튼을 만드는 것으로 시작합니다. 나중에 `Cancel` 버튼으로 쉽게 변경할 수 있습니다.

먼저 `HomePage` 에서와 마찬가지로 `EditHikePage` 에 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 디펜던시를 추가해야 합니다. `EditHikePage.ux` 에서 디펜던시를 추가합니다.

```
<Page ux:Class="EditHikePage">
    <Router ux:Dependency="router" />

    ...
```

그리고 `MainView.ux` 에서 우리는 그 디펜던시를 충족 시킬 것입니다 :

```
        <Navigator DefaultPath="home">
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
```

충분히 쉽죠! 이 파일을 저장 한 다음 `EditHikePage.ux` 로 돌아가서 하단에 간단한 back 버튼을 만듭니다.:

```
            ...

            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Button Text="Back" />
        </StackPanel>
    </ScrollView>
</Page>
```

우리가 아직 참조 할 함수를 만들지는 않았지만, `Clicked` 핸들러를 추가해 봅시다. (바로 뒤에 하겠습니다):

```
            ...

            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Button Text="Back" Clicked="{goBack}" />
        </StackPanel>
    </ScrollView>
</Page>
```

우리의 핸들러는 `goBack` 이라고 할 겁니다. `EditHikePage.js` 로 이동하여 이 함수를 만들고 exports 에 추가하십시오.:

```
...

function goBack() {
    // TODO
}

module.exports = {
    ...

    goBack: goBack
};
```

이제 이 함수를 채울 준비가 되었습니다. 이것은 정말 간단합니다.:

```
function goBack() {
    router.goBack();
}
```

여기서는 우리의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 오브젝트가 `goBack` 함수를 호출하여 이전에 있었던 경로로 돌아가도록 말하고 있습니다. 이는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 가 앱을 통해 이동하는 동안 히스토리를 추적 할 수 있기 때문에 작동하는 것이며, `push` 기능을 사용하여 이 페이지로 이동했기 때문에 보장되는 것입니다. 흥미로운건 우리가 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 의 `goto` 함수를 사용했다면, 우리는 여전히 `EditHikePage` 에 접근 할 수 있었겠지만, 그렇게 하면 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 가 히스토리를 추적하지는 못했을 겁니다.

> 참고 : `goBack`은 자동으로 디바이스의 기본 제공되는 back 버튼에 연결됩니다 (적용 가능한 경우). Cmd+B (macOS) 또는 Control+B (Windows)를 사용하여 PC 미리보기에서 이 버튼을 눌러 시뮬레이션 할 수 있습니다.

마지막으로, 이 모든 것을 저장하고 back 버튼이 실제로 작동하는지 확인하십시오! 이제 우리는 이 조각들이 어떻게 함께 작동하는지 보기 시작했습니다.

## 우리 페이지들 간에 데이터 전송 ##

우리가 해야 할 마지막 일은 하이킹을 네비게이션과 함께 `EditHikePage` 에 보내는 것입니다. 이 부분에는 실제로 두 부분이 있습니다; `HomePage` 로 부터 하이킹을 보내는 부분, `EditHikePage` 에서 하이킹을 받는 부분입니다. 하이킹을 보내는 첫 번째 부분부터 시작하겠습니다.

이 부분은 꽤 쉬울 것입니다. `HomePage.js` 로 가면, `goToHike` 함수에 이미 대부분 작성 되어 있습니다.:

```
function goToHike(arg) {
    var hike = arg.data;
    router.push("editHike");
}
```

우리가 해야 할 일은 `push` 를 호출 할 때 hike 오브젝트를 route 에 추가하여, [Router](https://www.fusetools.com/docs/fuse/navigation/router) 가 이 데이터를 우리에게 보내 주도록 하는 것 뿐 입니다. 이것은 hike 오브젝트가 *plain old data* (함수 또는 그와 같은 어떤 화려한 것을 포함하지 않는 단순한 문자열 데이터) 이기 때문에 동작이 가능합니다. 이것이 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 통해 전송되는 오브젝트들에 대해 필요한 요구사항 입니다.

다음과 같이 하이킹을 보내기 위해, `push` 호출시 route 와 함께 `hike` 오브젝트를 전달합니다.

```
function goToHike(arg) {
    var hike = arg.data;
    router.push("editHike", hike);
}
```

이 방법으로 우리는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 에게 `editHike` route 로 이동하고 `hike` 오브젝트를 거기에 있는 타겟 (여기서는 `EditHikePage` 인스턴스) 에 전달하도록 지시합니다.

> 참고: 이 튜토리얼에서는 간단한 네비게이션만 사용하지만, 멀티 네비게이션도 수행 할 수 있습니다. 자세한 정보는 [Navigation](https://www.fusetools.com/docs/navigation/navigation#multi-level-navigation) 문서의 [Multi-level navigation](https://www.fusetools.com/docs/navigation/navigation#multi-level-navigation) 섹션을 참조하십시오!

이제 하이킹 데이터를 `EditHikePage` 에 보냈으므로, `EditHikePage` 에서 해당 데이터를 받기 위한 코드를 추가해야 합니다. Fuse는 이런 목적을 위한 특별한 도구를 제공합니다. JavaScript, JS 모듈에서 사용할 수 있는 `Parameter` 라 불리는 특수한 [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 액세스 할 수 있습니다. 이 `Parameter` [Observable](https://www.fusetools.com/docs/fusejs/observable) 은 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 를 통해 컴포넌트로 전달되는 모든 수신 값을 나타냅니다. 이것을 사용하기 위해 우리가 이미 `EditHikePage.js` 에 가지고 있는 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 살펴 보겠습니다.:

```
var Observable = require("FuseJS/Observable");

var hike = Observable();

...
```

이상적으로, 우리가 하고자 하는 것은 이 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 `Parameter` [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 통해 들어오는 모든 값을 채우는 것입니다. 이렇게 할 수 있는 몇 가지 방법이 있지만 가장 간단한 방법은 다음과 같이하는 것입니다.:

```
var hike = this.Parameter;

...
```

사실, [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 새로 만들 필요는 없습니다; 우리는 그냥 hike 오브젝트만 받을 것이므로 `Parameter` 를 사용해도 됩니다. `var Observable = require("FuseJS/Observable");` 라인은 더 이상 참조 할 필요가 없습니다, 이를 어떻게 제거했는지 보십시오. 정말 간단하게 만들 수 있죠!

> 참고 : `Parameter` 는 단순한 `Parameter` 가 아니라, `this.Parameter` 를 사용하여 참조됩니다. 이는 `Parameter` 가 모듈 정의의 일부이고 `EditHikePage` 의 이 특정 인스턴스를 참조하므로, `this` 를 사용하여야만 합니다. 또한 `this` 는 JavaScript에서 사용되는 위치에 따라 다른 의미를 가질 수 있으므로, 모듈의 루트에서 올바른 인스턴스를 사용하는 것이 중요합니다.

이제 이 모든 것을 저장하면 `EditHikeView` 에 최종적으로 데이터가 다시 채워지는 것을 볼 수 있습니다! 우리는 우리 페이지 사이를 왔다 갔다 할 수 있고, `EditHikePage` 가 매번 정확한 하이킹의 데이터로 채워지는 것을 볼 수 있습니다. 멋집니다!

## 지금까지의 진행 ##  

이제 우리는 네비게이션으로 설정한 두 컴포넌트를 얻었고, 그들 사이에 데이터를 전달할 수 있습니다! 우리의 응용 프로그램은 현재 다음과 같습니다.:

[https://res.cloudinary.com/fusetools/image/upload/documentation_v2/7afd5d6af587ad4f2030099b91818d95__media/hikr/chapter-4/chapter-4.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/7afd5d6af587ad4f2030099b91818d95__media/hikr/chapter-4/chapter-4.mp4)

이 장에서 수정 한 다양한 파일의 코드는 다음과 같습니다.:

`MainView.ux` :

```
<App>
    <Router ux:Name="router" />

    <ClientPanel>
        <Navigator DefaultPath="home">
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
    </ClientPanel>
</App>
```

`Pages/HomePage.ux` :

```
<Page ux:Class="HomePage">
    <Router ux:Dependency="router" />

    <JavaScript File="HomePage.js" />

    <ScrollView>
        <StackPanel>
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{goToHike}" />
            </Each>
        </StackPanel>
    </ScrollView>
</Page>
```

`Pages/HomePage.js` :

```
var hikes = require("hikes");

function goToHike(arg) {
    var hike = arg.data;
    router.push("editHike", hike);
}

module.exports = {
    hikes: hikes,

    goToHike: goToHike
};
```

`Pages/EditHikePage.ux` :

```
<Page ux:Class="EditHikePage">
    <Router ux:Dependency="router" />

    <JavaScript File="EditHikePage.js" />

    <ScrollView>
        <StackPanel>
            <Text Value="{name}" />

            <Text>Name:</Text>
            <TextBox Value="{name}" />

            <Text>Location:</Text>
            <TextBox Value="{location}" />

            <Text>Distance (km):</Text>
            <TextBox Value="{distance}" InputHint="Decimal" />

            <Text>Rating:</Text>
            <TextBox Value="{rating}" InputHint="Integer" />

            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Button Text="Back" Clicked="{goBack}" />
        </StackPanel>
    </ScrollView>
</Page>
```

`Pages/EditHikePage.js` :

```
var hike = this.Parameter;

var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });

function goBack() {
    router.goBack();
}

module.exports = {
    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments,

    goBack: goBack
};
```

## 다음은 뭔가요? ##

우리 컴포넌트는 멋지게 동작합니다. 그러나 모델에서 실제 데이터를 변경할 수는 없습니다; 우리는 뷰 모델에서 일부 데이터만 변경할 수 있습니다. [다음 챕터](https://www.fusetools.com/docs/tutorial/mock-backend) 에서 우리는 이것을 정렬하기 위한 백엔드 임시모형(mock) 을 만드는 작업을 할 것입니다. 이렇게 하면 향후 어느 시점에 실제 백엔드를 쉽게 추가 할 수 있도록 앱 아키텍처를 마무리 할 수 있습니다. 그럼 [파헤쳐 봅시다](https://www.fusetools.com/docs/tutorial/mock-backend) !

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-4) 에서 볼 수 있습니다.
