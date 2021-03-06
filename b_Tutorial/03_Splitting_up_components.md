원본: https://www.fusetools.com/docs/tutorial/splitting-up-components

## 소개 ##

[지난챕터](https://www.fusetools.com/docs/tutorial/multiple-hikes) 에서, 수정하려는 하이킹을 선택 할 수 있는, 두번째 뷰를 추가함으로써 EditHike 뷰를 확장했습니다. 이건 크게 한걸음 나간것입니다, 하지만 점점 프로젝트가 커짐에 따라 관리가 쉽도록 재사용 가능한 코드로 작게 분리 할 필요가 있습니다. 이것은 우리가 현실 세계의 복잡한 앱 프로젝트들을 관리하기 위해, Fuse가 제공하는 중요한 도구들을 어떻게 사용하는지 알려 줄 것입니다.

보다 구체적으로 이야기 하자면, 이 챕터에서 우리가 할 것은 두 개의 뷰를 분리하는 것입니다. - 그것은 우리의 *홈페이지가 될 하이킹을 선택하는 뷰*, 그리고 *하이킹을 수정하게 될 EditHike 뷰* 가 될 것입니다. 이것은 또한 우리의 단순한 데이터모델(마지막 챕터에서의 `hikes` 배열)도 분리되어야 함을 의미하며, 그러기 위해서는 두개의 뷰들과 독립적으로, 분리되어 사용될 수 있어야 합니다.

이 부분은 다루어야 할 것이 많기 때문에, 튜토리얼의 이 부분에서는 우리의 뷰를 분리된 컴포넌트로 나누는 방법에 관한 것만 다룰 것이며, 우리가 그것들을 다시 끼워 맞출 방법과 앱 흐름을 개발 하는 것은 다루지 않을 것입니다. 이것은 특정 하이킹을 수정하기 위한 뷰가 더 이상 데이터로 채워지지 않을 것이라는 것을 의미하지만, 괜찮습니다. 한번에 한걸음씩 걷길 바라니까요. 뷰들을 다시 모으고 그들 사이에 데이터를 전달하는 것은, 다음 챕터에서 다룰 것이므로 걱정하지 마십시오!

이 챕터 최종 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-3) 에서 받을 수 있습니다.

## hikes 컬렉션 분리하기 ##

먼저 우리는 `hikes` 배열을 자체 컴포넌트로 분리하여, 모든 뷰들과 독립적으로 존재하게 할 것입니다. 이는 일부 자바스크립트 데이터이기 때문에, 자체 *모듈* 안에 배치할 것입니다. *모듈* 이란 기본적으로, JavaScript 가 자체적으로-포함되어 있는, 재사용 가능한 덩어리 입니다. 우리는 사실 이미 모듈들을 만들어 놓고 있는 것입니다. - 우리가 UX에 `<JavaScript>` 태그를 넣어 코딩할 때마다, 하나의 모듈을 생성하고 있습니다! 

> 팁: UX에서 `<JavaScript>` 태그를 사용하면 `<JavaScript File="myScript.js"/>` 와 같은 다른 JavaScript 파일을 참조 할 수도 있습니다. 이는 `<JavaScript>` 태그 안에 JavaScript 코드를 삽입하는 것과 같은 것이지만, UX 코드를 컴포넌트의 로직과 분리 할 수 ​​있기 때문에 더 깔끔해질 수 있습니다! 이 챕터의 뒷부분에서 좀 더 구체적인 예제를 보도록 하겠습니다.

그러나, `hikes` 배열의 경우, 특정 UX 파일로 묶지 않을 것입니다. 대신, 데이터를 별도의 .js 파일에 저장하고, Fuse에게 이 모듈을 우리의 앱과 번들로 묶도록 지시 할 겁니다. 그런 다음, 우리는 이 모듈을 뷰의 각 모듈들에서 *import* 할 것입니다.

이를 위해 우선 `hikes.js` 라는 프로젝트에 새 파일을 만듭니다. 이것을 프로젝트 디렉토리의 루트에 위치시킬 것이므로, 디렉토리는 다음과 같이 보일 것입니다 :

```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
```

여기에 컨텐츠를 넣기 전에, 먼저 Fuse가 앱과 함께 그것을 번들로 묶어 주는지 확인해 봅시다. 일반적으로, `JavaScript` 태그를 사용하여 UX에 인라인 JavaScript 코드를 작성하거나, `<JavaScript File="myFile.js"/>` 와 같은 특정 파일을 참조하는 경우, Fuse는 자동으로 JavaScript 코드가 우리앱의 번들로 제공되는지 확인합니다. 그러나, 우리가 프로젝트에서 `JavaScript` 태그로 직접 참조되지 않는 개별 JavaScript 모듈을 가지고 있다면, 우리는 이 코드가 우리 프로젝트의 일부임을 Fuse 에게 알려야 합니다.

그러기 위해 우리는 프로젝트 파일에 정보를 추가하여, 퓨즈에게 `hikes.js` 파일이 우리의 앱과 번들되어야 한다고 알려줍니다. `hikr.unoproj` 파일을 열면 다음과 같은 내용이 표시됩니다:

```
{
  "RootNamespace":"",
  "Packages": [
    "Fuse",
    "FuseJS"
  ],
  "Includes": [
    "*"
  ]
}
```

보시다시피, 많은 것들이 있지는 않습니다. 기본적으로 우리 프로젝트에 대한 간단한 정보들 입니다. 우리가 특별히 관심을 가졌던 섹션은 `"Includes"` 섹션으로, 현재 하나의 항목을 가지고 있습니다:

```
"Includes": [
    "*"
  ]
```

이것은 기본적으로 Fuse 프로젝트가 공통적으로 Fuse 프로젝트들과 관련된 많은 파일을 포함해야 한다는 것을 의미합니다. 여기에는 .ux 파일들, .uno 파일들 및 기타 몇 가지 파일들이 포함됩니다. 그러나 *JavaScript 파일들은 포함되어 있지 않으므로* , `hikes.js` 파일을 명시 적으로 추가해야합니다. 이렇게 하기 위해서, 이 리스트에 또 다른 항목을 추가할 것입니다.:

```
{
  "RootNamespace":"",
  "Packages": [
    "Fuse",
    "FuseJS"
  ],
  "Includes": [
    "*",
    "hikes.js:Bundle"
  ]
}
```

여기서 한 모든 것은 `"*"` 항목 다음에 쉼표가 붙고, 그 뒤에 `"hikes.js:Bundle"` 항목이 추가된 것입니다. 이것은 단지 "이봐, Fuse, 그 hikes.js 을 알지? 내 앱과 함께 묶어줘." 라고 말하는 것과 같습니다. 멋지고 간단하죠! 이제 이 파일을 저장하면 모든 설정 완료됩니다.

이제 `MainView.ux` 에서 `hikes` 배열을 `hikes.js` 파일로 옮겨야합니다. 먼저 `MainView.ux` 파일에서 복사 한 다음, 제거합니다:

```
        <JavaScript>
            var Observable = require("FuseJS/Observable");

            var hike = Observable();

            var name = hike.map(function(x) { return x.name; });
            var location = hike.map(function(x) { return x.location; });
            var distance = hike.map(function(x) { return x.distance; });
            var rating = hike.map(function(x) { return x.rating; });
            var comments = hike.map(function(x) { return x.comments; });

            function chooseHike(arg) {
                hike.value = arg.data;
            }

            module.exports = {
                hikes: hikes,

                name: name,
                location: location,
                distance: distance,
                rating: rating,
                comments: comments,

                chooseHike: chooseHike
            };
        </JavaScript>
```

`module.exports` 에서 `hikes` 에 대한 참조를 어떻게 남겼는지 주목하십시오. 파일을 저장하면 오류가 발생하지만, 걱정하지 않아도 됩니다. - 잠시 후에 해결 할 겁니다.

그럼, 먼저 배열을 `hikes.js` 에 배치합니다.:

```
var hikes = [
    {
        id: 0,
        name: "Tricky Trails",
        location: "Lakebed, Utah",
        distance: 10.4,
        rating: 4,
        comments: "This hike was nice and hike-like. Glad I didn't bring a bike."
    },
    {
        id: 1,
        name: "Mondo Mountains",
        location: "Black Hills, South Dakota",
        distance: 20.86,
        rating: 3,
        comments: "Not the best, but would probably do again. Note to self: don't forget the sandwiches next time."
    },
    {
        id: 2,
        name: "Pesky Peaks",
        location: "Bergenhagen, Norway",
        distance: 8.2,
        rating: 5,
        comments: "Short but SO sweet!!"
    },
    {
        id: 3,
        name: "Rad Rivers",
        location: "Moriyama, Japan",
        distance: 12.3,
        rating: 4,
        comments: "Took my time with this one. Great view!"
    },
    {
        id: 4,
        name: "Dangerous Dirt",
        location: "Cactus, Arizona",
        distance: 19.34,
        rating: 2,
        comments: "Too long, too hot. Also that snakebite wasn't very fun."
    }
];
```  

다 되었으면, 이 뷰에서 데이터를 다시 볼 수 있도록, 이 새 모듈로 부터 이 배열을 *export* 해야 합니다. 이것은 인라인 JavaScript 모듈에서 주변 UX 코드로 데이터를 노출 할 때와 마찬가지로, `module.exports` 를 사용하여 수행됩니다. 그러나 이 경우 좀 더 간단해집니다. 이 파일에서 이 데이터 하나만 가질 것이므로. 맨 아래에 다음과 같이 간단하게 작성할 수 있습니다:

```
module.exports = hikes;
``` 

이제 다 된겁니다! 이 파일을 저장하면 우리 모듈은 준비가 끝났습니다.

이제, 우리 뷰 코드로 돌아가서, 우리가 이 파일의 밖으로 `hikes` 배열을 옮겼으므로, 새 모듈을 *import* 할 필요가 있습니다. 이것은 사실 굉장히 쉽습니다. `MainView.ux` 에서의 `var Observable = require("FuseJS/Observable");` 라인을 기억하시나요? 이것은 Fuse가 FuseJS 의 Observable 모듈을 import 하고, `Observable` 이란 이름으로 바인딩하도록 합니다. 새로운 `hikes` 모듈의 경우도 거의 똑같은 작업을 수행합니다.:

```
            var Observable = require("FuseJS/Observable");
            var hikes = require("hikes");
```

이 경우 Fuse는 `hikes.js` 파일에 해당하는, `hikes` 라는 모듈을 import 합니다. 또한 해당 모듈의 `module.exports` 오브젝트를, `hikes` 라는 이름에 바인딩합니다. 이것은 `hikes.js` 모듈의 `hikes` 배열이었습니다. 좋습니다! 이것이 새롭게 생성 된 재사용 가능 JavaScript 모듈을 사용하기 위해 필요한 전부입니다. `MainView.ux` 를 저장하면 실시간 미리보기가 다시 로드됩니다. 외부는 다르게 보이지 않지만, 내부는 매우 깨끗하고 관리하기 쉬운 프로젝트 구조로 나아가는 중입니다!

## EditHike 뷰를 컴포넌트로 변환하기##

`hikes` 배열을 분리 했으므로, 다음으로 할 일은 뷰 코드에서 새로운 컴포넌트를 만드는 것입니다. 작은 단계부터 합니다. 먼저 프로젝트에서 `Pages` 라는 새 폴더를 만듭니다.:

```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
|- Pages
```

이 새 폴더 안에 `EditHikePage.ux` 라는 새 파일이 생성됩니다. 내부에 다음 코드를 삽입합니다.:

```
<Page ux:Class="EditHikePage"></Page>
```

디렉토리 트리는 이제 다음과 같이 보일 것입니다 :

```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
|- Pages
   |- EditHikePage.ux
```

계속 진행하기 전에 몇 가지 사항을 설명해야 합니다. 새로운 `EditHikePage.ux` 파일의 내용을 다시 한번 살펴 보겠습니다.:

```
<Page ux:Class="EditHikePage"></Page>
```

먼저 살짝 훑어 보면, 그냥 `ux:Class` 프로퍼티를 가진, 그리고 그 안에 아무것도 없는 하나의 [Page](https://www.fusetools.com/docs/fuse/controls/page) 를 생성하는 것처럼 보입니다. 그러나 그 하나의 [Page](https://www.fusetools.com/docs/fuse/controls/page) 는 정확히 뭘까요? 그리고 이 `ux:Class` 라는 건 또 뭘까요?

첫번째 질문에 대답하자면, [Page](https://www.fusetools.com/docs/fuse/controls/page) 는 기본적으로 네비게이션에 포함되는 특별한 종류의 UI 요소입니다. 여기에 사실 [Page](https://www.fusetools.com/docs/fuse/controls/page) 를 사용 할 필요는 없습니다. 우리는 `Panel` 이나 `Button` 또는 그 밖의 다른 것을 사용해도 됩니다. 그러나, 잠시 후 이 챕터 뒷부분에서 설명하게 될, 네비게이션을 사용하여 컴포넌트를 사용하려는 경우, 일반적으로 [Page](https://www.fusetools.com/docs/fuse/controls/page) 를 사용하는 것이 가장 좋습니다.

두 번째 질문에서 `ux:Class` 는 [Page](https://www.fusetools.com/docs/fuse/controls/page) 의 인스턴스를 만드는 대신, [Page](https://www.fusetools.com/docs/fuse/controls/page) 클래스를 *확장* 하는 *class* 를 작성하는 걸 의미합니다. 이전에 객체 지향 프로그래밍을 해본 적이 있다면 이 용어가 매우 익숙 할 겁니다. 그렇지 않다면 약간 이상하게 들릴 수도 있습니다만, 사실 이해하기 쉬운 개념입니다. 기본적으로 *요소의 한 종류* 처럼 *class* 를 생각할 수 있고, *인스턴스* 는 *그런 종류의 한 요소* 라고 볼 수 있습니다. 예를 들어, [Button](https://www.fusetools.com/docs/fuse/controls/button) 은 클래스이며, UX에서 `<Button />` 을 말할 때 [Button](https://www.fusetools.com/docs/fuse/controls/button) 클래스의 인스턴스를 만드는 것입니다. [Button](https://www.fusetools.com/docs/fuse/controls/button) 은 버튼 요소의 룩앤필(모양과 느낌) 을 설명하고, `<Button />` 은 우리 뷰에  실제 [Button](https://www.fusetools.com/docs/fuse/controls/button) 요소가 존재하고 있음을 나타냅니다. 

그러나 이 맥락에서 *확장* 은 무엇을 의미할까요? 그것은 기본으로 주어진 클래스와 동일한 우리만의 클래스를 만들면서, 우리만의 것들을 몇가지 추가 할 것임을 의미합니다. 마찬가지로 이 경우, 우리는 여전히 [Page](https://www.fusetools.com/docs/fuse/controls/page) 하나를 원하지만, 거기에 어떤 추가적인 것들(우리의 뷰에 해당하는 것) 을 필요로 합니다. 그래서 우리는 `<Page ux:Class = "EditHikePage"/>` 라고 하는 것이고, 이는 [Page](https://www.fusetools.com/docs/fuse/controls/page) 클래스를 확장하는 `EditHikePage` 라는 자체 클래스를 생성한다는 것을 의미합니다. 그러고 나면, 이 클래스를 사용(인스턴스 생성) 을 원할 때마다, `<Page />` 대신, 그 안에 우리의 커스텀 사항들이 전부 적용된 `<EditHikePage />` 를 간단하게 사용 할 수 있습니다.

> 참고: 클래스 만들기에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 문서를 참조하십시오.

이제 이 파일의 기본 내용을 이해 했으므로, 우리가 바라는 코드를 `MainView.ux` 에서 마이그레이션 할 차례입니다. 이 파일을 살펴보면 다음과 같이 표시됩니다.:

```
<App>
    <ClientPanel>
        <JavaScript>
            var Observable = require("FuseJS/Observable");
            var hikes = require("hikes");

            var hike = Observable();

            var name = hike.map(function(x) { return x.name; });
            var location = hike.map(function(x) { return x.location; });
            var distance = hike.map(function(x) { return x.distance; });
            var rating = hike.map(function(x) { return x.rating; });
            var comments = hike.map(function(x) { return x.comments; });

            function chooseHike(arg) {
                hike.value = arg.data;
            }

            module.exports = {
                hikes: hikes,

                name: name,
                location: location,
                distance: distance,
                rating: rating,
                comments: comments,

                chooseHike: chooseHike
            };
        </JavaScript>

        <ScrollView>
            <StackPanel>
                <Each Items="{hikes}">
                    <Button Text="{name}" Clicked="{chooseHike}" />
                </Each>

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
            </StackPanel>
        </ScrollView>
    </ClientPanel>
</App>
```

이 파일의 최상위 부분을 살펴보면, 먼저 [App](https://www.fusetools.com/docs/fuse/app) 과 ClientPanel을 볼 수 있습니다. 그 외 다른  내용 전부가 (특히 `JavaScript` 및 [ScrollView](https://www.fusetools.com/docs/fuse/controls/scrollview) 태그) 우리의 뷰를 구체적으로 구성하는 것들입니다. 그리고 이것들이 우리의 `EditHikePage.ux` 파일 [Page](https://www.fusetools.com/docs/fuse/controls/page) 내부로 옮기고자 하는 부분에 해당합니다. 옮기고 나면, 다음과 같이 보일 것입니다.:

MainView.ux:

```
<App>
    <ClientPanel>
    </ClientPanel>
</App>
```

EditHikePage.ux:

```
<Page ux:Class="EditHikePage">
    <JavaScript>
        var Observable = require("FuseJS/Observable");
        var hikes = require("hikes");

        var hike = Observable();

        var name = hike.map(function(x) { return x.name; });
        var location = hike.map(function(x) { return x.location; });
        var distance = hike.map(function(x) { return x.distance; });
        var rating = hike.map(function(x) { return x.rating; });
        var comments = hike.map(function(x) { return x.comments; });

        function chooseHike(arg) {
            hike.value = arg.data;
        }

        module.exports = {
            hikes: hikes,

            name: name,
            location: location,
            distance: distance,
            rating: rating,
            comments: comments,

            chooseHike: chooseHike
        };
    </JavaScript>

    <ScrollView>
        <StackPanel>
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{chooseHike}" />
            </Each>

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
        </StackPanel>
    </ScrollView>
</Page>
```

이 파일들을 저장하면, 미리보기는 업데이트 되지만, 컨텐츠들은 사라질 것입니다! 그 이유는 간단합니다 - 우린 방금 `EditHikePage` 클래스를 생성했지만 그걸 어디서도 사용하지 않았습니다! 이 문제를 해결하기 위해, 다음과 같이 `MainView.ux` 의 ClientPanel 안에 `EditHikePage` 클래스의 인스턴스를 추가합니다.:

```
<App>
    <ClientPanel>
        <EditHikePage />
    </ClientPanel>
</App>
```

퓨즈에서 표준으로 제공되는 클래스를 사용한 것처럼 보이지만, 이것은 우리가 직접 만든 커스텀 클래스입니다. `MainView.ux` 를 저장하면 뷰가 다시 미리보기로 표시됩니다. 훌륭합니다!

이제 새로운 컴포넌트로 한번 더 해보겠습니다. 자바스크립트 코드를 자체파일로 분리하는 것입니다. 이것은 매우 쉽습니다. 먼저 프로젝트의 `Pages` 폴더에있는 `EditHikePage.ux` 파일 옆에, `EditHikePage.js` 라는 새 파일을 만듭니다.:

```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
|- Pages
   |- EditHikePage.js
   |- EditHikePage.ux
```

그런 다음, `EditHikePage.ux` 의 `JavaScript` 태그 속 코드 전부를 가져와서, 그것을 `EditHikePage.js` 파일로 옮깁니다:

EditHikePage.ux :

```
<Page ux:Class="EditHikePage">
    <JavaScript>
    </JavaScript>

    <ScrollView>
        <StackPanel>
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{chooseHike}" />
            </Each>

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
        </StackPanel>
    </ScrollView>
</Page>
```

EditHikePage.js :

```
var Observable = require("FuseJS/Observable");
var hikes = require("hikes");

var hike = Observable();

var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });

function chooseHike(arg) {
    hike.value = arg.data;
}

module.exports = {
    hikes: hikes,

    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments,

    chooseHike: chooseHike
};
```

이 파일을 저장합니다. 마지막으로 `EditHikePage.ux` 파일의 `JavaScript` 태그를, 새로 생성한 자바스크립트 파일을 참조하는 태그로 변경합니다.:

```
<Page ux:Class="EditHikePage">
    <JavaScript File="EditHikePage.js" />

    <ScrollView>

    ...
``` 

끝났습니다! 우리가 `EditHikePage.ux` 파일을 저장하면, 다시 미리보기가 재시작 되어, 이 전과 동일하게 보일 것입니다. 이것은 모든 것이 작동됨을 의미하고, 환상적인 Fuse 프로젝트 구조를 향해 한걸음 가까이 다가간 것입니다. 또한 우리는 프로젝트 파일의 `EditHikePage.js` 파일을, 앱의 `JavaScript` 태그들 중 하나로부터 참조 했기 때문에, 따로 명시적으로 include 할 필요는 없었던 점을 알립니다.

##  홈페이지 분리 ##

우리의 `EditHikePage` 클래스는 사실 하나로된 두개의 컴포넌트 (하이킹 선택 부분과 하이킹 수정 부분) 였습니다. - 일반적으로 컴포넌트를 만들때 가능한 한 간단하게 단일 뷰 같이 하나만 나타내는 것이 좋습니다. 우리가 다음에 할 것은, `Homepage` 라 불리는 하이킹 선택 부분을 자체 클래스로 분리하는 것입니다.

우리는 기본적으로 이 전에 이 작업을 수행 했으므로, 지금은 두 번째로 좀 더 쉬워야합니다. 먼저, `Pages` 디렉토리에 있는 `HomePage.ux` 라는 파일에서 홈페이지가 될 빈 페이지 클래스를 만듭니다.:

```
<Page ux:Class="HomePage"></Page>
``` 

다음으로, 홈페이지에 해당하는 UX 코드를 `EditHikePage.ux` 에서 `HomePage.ux` 로 이동시킬 것입니다. `hikes` 컬렉션을 표시 한 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 태그가 있습니다.:

```
<Each Items="{hikes}">
    <Button Text="{name}" Clicked="{chooseHike}" />
</Each>
```

이동시켜 봅시다:

```
<Page ux:Class="HomePage">
    <Each Items="{hikes}">
        <Button Text="{name}" Clicked="{chooseHike}" />
    </Each>
</Page>
```

또한, 이 `hikes` 를 여러번 표시 할 예정이므로 [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 안쪽으로 배치합니다. 그리고 [ScrollView](https://www.fusetools.com/docs/fuse/controls/stackpanel) 내부에 넣는 것이 좋을 것 같으므로, 그렇게 설정합니다.

```
<Page ux:Class="HomePage">
    <ScrollView>
        <StackPanel>
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{chooseHike}" />
            </Each>
        </StackPanel>
    </ScrollView>
</Page>
```

이것을 저장하면, 우리는 몇 가지 사실들을 알게 될 겁니다. 특히, 새 `HomePage.ux` 는 아직 `hikes` 컬렉션에 대해 모릅니다. 우리는 이 뷰에 JavaScript를 약간 추가하여 이를 해결 할 것입니다.

이 전과 마찬가지로 `HomePage.ux` 파일 바로 옆에 `HomePage.js` 파일을 만들고, 거기에 다음 코드를 추가합니다.

```
var hikes = require("hikes");

function chooseHike(arg) {
    // TODO
}

module.exports = {
    hikes: hikes,

    chooseHike: chooseHike
};
```

이것은 우리가 `chooseHike` 함수의 내용을 주석 처리 한 것을 제외하고는, 현재 `EditHikePage.js` 에 있는 코드와 기본적으로 같습니다. 우리는 다음 챕터에서 그 부분을 완성하게 될 것입니다. 그런데, `EditHikePage` 가 모든 하이킹을 표시 할 필요가 없으므로, 더 이상 양쪽 모두 이 코드를 필요로 하지 않습니다. 따라서 우리는 `EditHikePage.js` 에서 `require`, `chooseHike` 함수, 그리고 해당 exports 를 제거 할 것입니다.:

```
var Observable = require("FuseJS/Observable");

var hike = Observable();

var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });

module.exports = {
    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments
};
```

JavaScript가 모두 준비되었으므로, 다음과 같이 `HomePage.ux` 에서 해당 파일을 링크 할 것입니다.

```
<Page ux:Class="HomePage">
    <JavaScript File="HomePage.js" />
```

여기서, 디스플레이를 위한 우리의 페이지가 준비되야 합니다!

## 홈페이지 보여주기 ##

지금까지 우리는 여러 페이지들을 얻었습니다. 그리고 우리는 그것들을 보여주고 그들 사이를 이동할 방법이 필요합니다. Fuse에서는 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 와 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 컴포넌트들에 의해 이것들이 처리됩니다. 이 컴포넌트들은 적절한 소개가 필요합니다. 우리는 이것들이 얼마나 파워풀한 것인지 완전히 이해하고, 적절하게 사용 할 방법을 배울 시간을 가지기를 원합니다. 그래서 우리는 다음 챕터에서 그것들을 자세하게 살펴 볼 것입니다.

지금은 일단 간단하게, [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 을 대신 사용합니다. [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 은 우리가 스와이프 할 페이지들을 나란히 놓고 싶을 때 유용합니다.

우리가 `MainView.ux` 를 살펴보면, 현재 이렇게 보입니다:

```
<App>
    <ClientPanel>
        <EditHikePage />
    </ClientPanel>
</App>
```

이렇게 [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 안에 `EditHikePage` 인스턴스를 배치합시다:

```
<App>
    <ClientPanel>
        <PageControl>
            <EditHikePage />
        </PageControl>
    </ClientPanel>
</App>
```

이제 우리 PageControl 안에 `HomePage` 인스턴스를 추가하면 됩니다. 홈페이지이기 때문에 기존의 `EditHikePage` 인스턴스 위에 놓습니다.:

```
<App>
    <ClientPanel>
        <PageControl>
            <HomePage />
            <EditHikePage />
        </PageControl>
    </ClientPanel>
</App>
```

이제 저장하면, 새로운  `HomePage` 가 표시되고, 오른쪽으로 스와이프하여 `EditHikePage` 를 표시 할 수 있습니다. 멋집니다!

## 지금까지의 진행 ##  

휴, 우리는 여기서 많은 것들을 다뤘습니다! 이제 두 개의 뷰가 컴포넌트들로 분리되어, [PageControl](https://www.fusetools.com/docs/fuse/controls/pagecontrol) 안에 나란히 표시됩니다. 이렇게 보일겁니다:

`HomePage.ux` :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/1064aed908b2b711dc4949787bd1cbd5__media/hikr/chapter-3/homepage.webp)

`EditHikePage.ux` :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/b5b6dd8b107c662ec37dfcafd73e2b6f__media/hikr/chapter-3/edithikepage.webp)

그리고 우리의 다양한 파일들에 대한 코드는 다음과 같습니다. 파일이 몇개 더 많아졌지만, 그것들은 지금 훨씬 독립적이고 간단합니다. 

`hikr.unoproj` :

```
{
  "RootNamespace":"",
  "Packages": [
    "Fuse",
    "FuseJS"
  ],
  "Includes": [
    "*",
    "hikes.js:Bundle"
  ]
}
```

`MainView.ux` :

```
<App>
    <ClientPanel>
        <PageControl>
            <HomePage />
            <EditHikePage />
        </PageControl>
    </ClientPanel>
</App>
```

`hikes.js` :

```
var hikes = [
    {
        id: 0,
        name: "Tricky Trails",
        location: "Lakebed, Utah",
        distance: 10.4,
        rating: 4,
        comments: "This hike was nice and hike-like. Glad I didn't bring a bike."
    },
    {
        id: 1,
        name: "Mondo Mountains",
        location: "Black Hills, South Dakota",
        distance: 20.86,
        rating: 3,
        comments: "Not the best, but would probably do again. Note to self: don't forget the sandwiches next time."
    },
    {
        id: 2,
        name: "Pesky Peaks",
        location: "Bergenhagen, Norway",
        distance: 8.2,
        rating: 5,
        comments: "Short but SO sweet!!"
    },
    {
        id: 3,
        name: "Rad Rivers",
        location: "Moriyama, Japan",
        distance: 12.3,
        rating: 4,
        comments: "Took my time with this one. Great view!"
    },
    {
        id: 4,
        name: "Dangerous Dirt",
        location: "Cactus, Arizona",
        distance: 19.34,
        rating: 2,
        comments: "Too long, too hot. Also that snakebite wasn't very fun."
    }
];

module.exports = hikes;
```

`Pages/EditHikePage.ux` :

```
<Page ux:Class="EditHikePage">
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
        </StackPanel>
    </ScrollView>
</Page>
```

`Pages/EditHikePage.js` :

```
var Observable = require("FuseJS/Observable");

var hike = Observable();

var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });

module.exports = {
    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments
};
```

`Pages/HomePage.ux` :

```
<Page ux:Class="HomePage">
    <JavaScript File="HomePage.js" />

    <ScrollView>
        <StackPanel>
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{chooseHike}" />
            </Each>
        </StackPanel>
    </ScrollView>
</Page>
```

`Pages/HomePage.js` :

```
var hikes = require("hikes");

function chooseHike(arg) {
    // TODO
}

module.exports = {
    hikes: hikes,

    chooseHike: chooseHike
};
```

## 다음은 뭔가요? ##

두 개의 뷰를 서로 다른 컴포넌트로 분리 했으므로, 우리 여정의 다음 단계는 `HomePage` 의 selector 가 `EditHikePage` 에디터를 채우고 해당 페이지로 이동할 수 있도록, 그것들을 다시 연결하는 과정이 될 것입니다. 여기서 우리는 Fuse의 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 와 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 컴포넌트를 사용함으로써, 우리 앱의 *흐름* 을 개발하기 시작 할 겁니다. 이것은 [다음 챕터](https://www.fusetools.com/docs/tutorial/navigation-and-routing) 에서 다룰 내용이므로, 준비가 되었으면 계속 [진행해 봅시다](https://www.fusetools.com/docs/tutorial/navigation-and-routing)!

이 챕터의 최종 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-3) 에 있습니다.
