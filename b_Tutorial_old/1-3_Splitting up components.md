
## 소개 ##

지난챕터에서, 우리는 우리가 수정할 하이킹을 선택할 두번째 뷰를 추가함으로써 Edit Hike 뷰를 확장했습니다. 이건 크게 한걸음 나간것입니다, 이제 우리 프로젝트가 커지는 만큼 관리가 쉽도록 하기 위해 재사용 가능한 코드로 작게 분리하기 시작할 때입니다. 이것은 우리가 현실 세계의 복잡한 앱 프로젝트들을 관리하기 위해  Fuse가 제공하는 임포트 툴들을 어떻게 사용하는지 알려줄겁니다.
특별하게 더하자면, 이 챕터에서 우리가 할 것은 우리의 두개의 뷰들을 분리하는 것입니다. - 우리의 홈페이지가 될 하이킹을 선택하는 뷰,그리고 하이킹을 수정하게 될 하이킹 수정 뷰가 될 것입니다. 이것은 또한 우리의 단순한 데이터 모델(마지막 챕터에서의 `hikes` 배열)이 마찬가지로 분리되어야 함을 의미하며, 그러기 위해서는 두개의 뷰에서 독립적으로 이고 분리되어 사용될 수 있어야 합니다.
이제, 이런 광범위한 것을 때문에 튜토리얼의 이 부분은 단지 우리가 그것들을 뒤에서 함께 끼워맞춰서 우리의 앱의 흐름을 개발하는 방법이 아닌, 분리된 컴포넌트들안에 우리의 뷰를 어떻게 분리하는지에 대한 것을 넘어설 것입니다.
이것은 우리의 특정 하이킹을 수정하기 위한 뷰가 더이상 조금도 어떤 데이터로 이동되지 않을것임을 의미합니다. 그러나 괜찮습니다. 우리는 한번에 한걸음씩 취하길 바랍니다. 우리 뷰들 두에사 함께 가로채기하고 그들 사이에 데이터를 전달ㄹ하는 것은 다음 챕터에서 다룰 것입니다. 그러니 걱정마세요!
이 챕터 최종 코드는 여기서 받을 수 있습니다.

## hikes 컬렉션 분리하기 ##
먼저 우리는 `hikes` 배열을 그것 자신의 컴포넌트로 분리할 것입니다. 그러려면 그것은 우리의 뷰들 중 하나로 독립적으로 존재할 것입니다. 이것이 어떤 자바스크립트 데이터이기 때문에, 우리는 그것을 그것 스스로의 모듈에 배치할 것입니다. 하나의 모듈은 기본적으로 자체적으로-포함된, 재사용 가능한 자바스크립트 덩어리 입니다. 우리는 사실 이미 모듈을을 만들어 놨습니다. - 우리가 UX 안에 `<JavaScript>` 태그를 인라인코드로 사용할때마다 하나의 모듈을 생성할 것입니다.

> 팁: UX 안에서 `<JavaScript>` 태그를 사용하는것은, 또한 이와 같이 또 다른 자바스크립트 파일을 참조할 수 있습니다: `<JavaScript File="myScript.js" />`. 이 것은  `<JavaScript>` 태그 안에  자바스크립트 코드 인라인을 위치하는 것과 같은 것입니다. 그러나 우리가 우리의 컴포넌트들의 로직으로부터 UX코드를 분리할수 있는 것 같은 클리너가 될 수 있습니다! 우리는 이 챕텅안에서 나중에 구체적인 예제들을 더 볼 수 있습니다.

그러나, `hikes` 배열의 경우에, 특정 UX 파일에 모듈이 묶여 있지 않을 것입니다. 대신 우리는 그 데이터를 그 자신의 .js 파일로 분리하여 배치할 것이고, Fuse에게 이 모듈이 우리 앱과 번들이라고 말해 줄 것입니다. 그러면, 우리는 우리 뷰들의 모듈들 각각에 이 모듈을 임포트 할 것입니다.

이것을 하기 위해서, 우리는 먼저 `hikes.js`라 불리는 프로젝트의 새로운 빈 파일을 생성합니다. 우리는 이것을 우리 프로젝트 디렉터리 최상단에 배치 할 것입니다, 그래서 우리 디렉터리는 이렇게 보여야 할것입니다.
```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
```
우리가 어떤 컨텐츠를 여기 넣기 전에, Fuse는 그것을 우리앱에 추가로 묶어줄 것임을 분명히 합시다. 일반적으로 우리가 UX 에 `JavaScript` 태그를 사용해서 자바스크립트 코드를 인라인으로 작성하거나, 혹은 `<JavaScript File="myFile.js" />` 와 같이 특별한 파일을 참조하면, Fuse는 분명 자동적으로 자바스크립트 코드를 우리앱과 함께 묶어 줄 것입니다. 그러나, 개인적인 자바스크립트 모듈들을 `JavaScript` 태그를 직접 사용함으로써 참조하지않은 채 우리의 프로젝트에 포함하면, 우리는 이 코드가 Fuse에게 우리 프로젝트의 부분이라고 말해줄 필요가 있습니다.
그러기 위해서 우리는 우리의 `hikes.js` 파일이 우리의 앱에 묶여저야함을 Fuse에 알릴 수 있는 프로젝트 파일에 정보를 좀 추가 할 것입니다. 우리가 `hikr.unoproj` 파일을 열면, 우리는 이것을 볼수 있습니다.:
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
보시다시피, 많은 것이 있지는 않습니다: 기본적으로 우리 프로젝트의 간단한 정보 입니다. 우리가 부분적으로 관심을 가졌던 섹션은 `"Includes"` 섹션 입니다. 현재, 하나의 항목을 가지고 있습니다:
```
"Includes": [
    "*"
  ]
```
이 것은 기본적으로 Fuse 프로젝트는 Fuse 프로젝트들과 공통적으로 관련된 많은 파일들을 포함해야만 함을 의미합니다. 이것은 .ux 파일들, .uno 파일들, 그리고 다른 몇몇의 것들을 커버합니다. 그러나, 자바스크립트들을 포함하지 않습니다, 그래서 우리는 명시적으로 `hikes.js` 파일을 추가해야만 합니다. 이렇게 하기 위해서, 우리는 이 리스트에 또 다른 아이템을 추가할 겁니다:
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
우리가 한 것은 `"*"` 항목 뒤에 콤마를 넣고, `"hikes.js:Bundle"` 항목을 그 뒤에 추가한 것이 전부입니다. 이것은 그냥 말로하자면 "야 Fuse, 너 hikes.js 파일 알지? 내 앱에 같이 포함해서 묶어줘" 멋지고 간단하죠! 이제 이 파일을 저장하면 모든 설정이 된겁니다.
이 쯤에서, 우리는 사실 `hikes` 배열을 `MainView.ux` 에서 `hikes.js` 파일 안으로 옮길 필요가 있습니다. 먼저, `MainView.ux` 파일로 부터 그것을 복사하고 난뒤 그것을 제거합니다:
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
우리가 `module.exports` 안에 `hikes` 참조를 남겨둔 것을 . 이것은 만약 우리가 그 파일을 저장한다면 에러의 원인이 될겁니다, 그러나 걱정하지 마세요 - 우리는 잠시뒤에 이걸 수정할겁니다.
그러나 먼저, 우리는 `hikes.js` 안쪽에 배열을 배치할겁니다.
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
그것이 처리되면, 우리는 이 새로운 모듈로부터 이 배열을 export 하는 것이 필요합니다. 그래서 우리는 다시 우리 뷰들로부터 데이터를 볼수 있습니다. 이것은 `module.exports`를 사용하여 처리합니다. 우리의 UX 코드 주변에 자바스크립트 모듈들을 인라인으로로 데이터를 노출시켰을때와 같습니다. 그렇지만 이경우에는, 약간 더 단순합니다. 왜냐하면 우리는 이 파일에 단지 데이터의 한조각만을 가지고 있을 것이기 때문입니다. 우리는 아래왜 같이 간단하게 작성할 수 있습니다:
```
module.exports = hikes;
``` 
그럼 끝입니다! 이 파일을 저장하면 이제 우리 모듈은 준비가 된겁니다.
이제 우리 뷰코드로 돌아가서, 우리가 이 파일의 밖으로 `hikes` 배열을 옮긴 후에, 우리는 새 모듈을 임포트할 필요가 있습니다. 이것은 사실 굉장히 쉽습니다. `MainView.ux` 에서의 `var Observable = require("FuseJS/Observable");` 라인을 기억하시나요? 이것은 Fuse가 FuseJS 의 Observable 모듈을 임포트하고, `Observable` 이란 이름 에 바인딩 하는 것임을 말해주고 있습니다. 우리의 새로운 `hikes` 모듈에 대해서 우리는 거의 정확히 같은 것을 할 것입니다:
```
            var Observable = require("FuseJS/Observable");
            var hikes = require("hikes");
```
이 경우에 Fuse는 `hikes`라 불리는 하나의 모듈을 임포트 할 겁니다. 우리의 `hikes.js` 파일과 일치하는 것 말이죠. 그것은 또한 `module.exports` 객체를 `hikes` 라는 이름에 바인딩 할 것입니다. `hikes.js`는 우리의 `hikes` 배열이죠. 멋져요! 저건 사실 우리가 우리가 새로 생성한, 재사용기능한 자바스크립트 모듈을 사용하는데 필요한 모든 것입니다. 만약 우리가 `MainView.ux` 를 저장한다면, 우리는 라이브 미리보기를 재시작 할 것입니다. 그것은 외부에선 아마 다른 어떤것도 볼수 없을 것입니다만, 내부에선 우리 프로젝트 구조가 관리 가능한 아주 깨끗한 하나의 방법을 잘 구현한것으로 보일것입니다.

## 하나의 컴포넌트로 하이킹 수정 화면 변경하기 ##
이제 우리는 `hikes` 배열을 분리해냈습니다 다음은 우리 뷰코드의 밖으로 새 컴포넌트를 생성하길 원합니다. 우리는 마찬가지로 작은 단계들로 이것을 할 것입니다. 먼저, 우리는 `Pages` 라 불리는 우리 프로젝트의 새 폴더를 하나 생성하고 싶습니다.
```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
|- Pages
```
새 프로젝트의 안쪽에 우리는 `EditHikePage.ux` 라고 불리는 새로운 파일을 하나 생성할 겁니다. 그 안쪽에 아래 코드를 넣을 것입니다:
```
<Page ux:Class="EditHikePage"></Page>
```
우리의 디레토리 트리가 이렇게 보여야 합니다:
```
$ tree
.
|- MainView.ux
|- hikes.js
|- hikr.unoproj
|- Pages
   |- EditHikePage.ux
```
옮기기전에, 몇가지 설명이 필요합니다. 또다른 새로운 `EditHikePage.ux` 파일의 내용들을 살펴봅시다:
```
<Page ux:Class="EditHikePage"></Page>
```
먼저 살짝 훑어 보면, 우리는 그냥 `ux:Class` 프로퍼티를 가진, 그리고 그 안에 아무것도 없는 하나의 페이지를 생성하는 것처럼 보입니다. 그러나 하나의 페이지는 정확히 뭘까요? 그리고 이 `ux:Class` 라는 건 또 뭘까요?
첫번째 질문에 대답하기 위해서, 하나의 페이지는 기본적으로 네비게이션에 포함될 특별한 종류의 UI 엘리먼트입니다. 우리는 사실 여기서 Page 를 사용할 필요가 없습니다: 우리는 그냥 `Panel` 이나 `Button` 혹은 완전히 다른 어떤것을 사용해도 됩니다. 그러나 일반적으로 페이지를 사용하는 것은 가장 나은 연습이라 생각됩니다. 여러분이 특별히 이 챕터 뒷부분에서 나올 것들 중 하나인 네이게이션과 함께 컴포넌트를 사용할 것이라면 말이죠.  
두번째 질문에 대한 것은, `ux:Class` 는 Page 의 인스턴스를 생성하는 것 대신 Page 클래스를 확장한 class 를 생성하는 것을 의미합니다. 만약 이전에 여러분들이 어떤 객체지향 프로그래밍을 했던 적이 있다면, 이 용어가 매우 친숙하게 들릴 것입니다. 만약 그렇지 않다면, 약간 이상하게 들리 수 도 있습니다, 그러나 사실 이해하기에 꽤 단순한 컨셉입니다. 기본적으로 우리는 엘리먼트의 한 종류로써 class 를 생각할수 있고 하나의 인스턴스는 그런 엘리먼트의 하나 라고 볼 수 있습니다. Button 은 하나의 class 이고, 우리가 UX 에서 `<Button />` 을 사용하면 우리는 Button 클래스의 인스턴스를 만드는 것입니다. Button 은 하나의 버튼 엘리먼트가 어떻게 보여져야 할지에 대해 묘사하고 있고, `<Button />` 은 우리 뷰의 실제 Button 엘리먼트의 존재를 묘사합니다.
그러나 여기서 '확장'이 의미하는 것이 무엇일까요? 그것은 간단하게 우리가 우리가 우리 자신의 클래스를 만드는것이 기본적으로 주어진 클래스와 같지만, 우리 자신의 것에는 뭔가 약간 더 추가되어있을 것임을 의미합니다. 이 경우에선, 우리는 여전히 하나의 Page 를 결국 원하지만, 마찬가지로 어떤 추가적인 뭔가를 원하기도 합니다. (그 것은 우리의 뷰에 지정하는 것입니다) 그래서 우리가 `<Page ux:Class="EditHikePage" />` 로 작성하면, 이것은 우리가 Page 클래스를 확장한 `EditHikePage` 라 불리는 우리 자신의 클래스를 생성하려고 하는 것을 의미합니다. 그러면, 우리가 이 클래스(인스턴스를 생성하는)라는 걸 사용하여 원한 것이 무엇이든지 간에, `<Page />` 를 사용해서 그 안에 사용자화된 모든 것들을 넣는 것 대신에, 간단히 `<EditHikePage />` 로 사용할 수 있습니다. 
> 알림: 우리는 컴포넌트들 생성하기 문서에서 클래스들에 대한 더 자세한것을 발견 할 수 있습니다.
이제 우리는 이파일의 기본적인 내용들에 대해 이해했습니다. `MainView.ux` 로 부터 우리의 뷰로 넘어온 우리가 원하는 코드를 통합할때 입니다. 우리가 그 파일을 살펴보면, 이와 같은 것들을 볼수 있을 겁니다:
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
이 파일의 최상단을 보면, 우리는 먼저 App 읇 볼 수 있고 그다음 ClientPanel 이 보입니다. 모든 다른 컨텐츠들이 (부분적으로는 `JavaScript` 와 ScrollView 태그들) 분명 우리의 뷰를 구성하는 것들입니다. 그래서 이 것들은 Page 안의 `EditHikePage.ux` 파일에 옮기길 원하는 부분들 입니다.

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
우리가 이 파일들을 저장한다면, 우리의 미리보기는 업데이트 될 것입니다만, 우리의 컨텐츠들은 사라질 것입니다! 그런 이유는 간단합니다 - 우린 방금 `EditHikePage` 클래스를 생성했습니다만 우리는 그걸 어디서도 사용하지 않았습니다! 이것 수정하기 위해 ClientPanel 안의 `EditHikePage` 클래스의 인스턴스를 곧 추가할겁니다.
```
<App>
    <ClientPanel>
        <EditHikePage />
    </ClientPanel>
</App>
```
이것은 어떻게 우리가 Fuse에서 표준으로 나오는 어떤 다른 클래스를 사용했던 것 처럼 보이는 것인지를 알려줍니다. - 그러나 그것은 지금 우리가 우리 스스로 생성했던 클래스입니다. 만약 `MainView.ux` 로 저장하면,  우리는 우리의 뷰를 미리보기에서 다시 볼 수 있을 겁니다. 멋지죠!
이제 옮기기 전에 우리는 새로운 컴포넌트에 하나를 더하길 원합니다. 그건 그 자신의 파일로 자바스크립트 코드를 분리하는 것입니다. 이것은 아주 쉽습니다. 먼저, `EditHikePage.js` 라고 불리는 새로운 파일을 우리 프로젝트 폴더의 `Pages` 폴더안에 있는 `EditHikePage.ux` 파일 바로 다음에 하나 생성할 것입니다:
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
그런 다음 우리는 `EditHikePage.ux` 안에 `JavaScript` 태그로 부터 그 코드 전부를 가져와서 그 것을 `EditHikePage.js` 파일로 옮길 것입니다:

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
이 파일을 저장하십시오. 마침내 우리는 `EditHikePage.ux` 파일 안의 `JavaScript` 태그들을 새로 생성한 자바스크립트 파일을 참조하는 싱글 태그로 변경할 것입니다:
```
<Page ux:Class="EditHikePage">
    <JavaScript File="EditHikePage.js" />

    <ScrollView>

    ...
``` 
끝났습니다! 우리가 `EditHikePage.ux` 파일을 저장하면, 다시 미리보기가 재시작 될것이고, 그것은 이 전과 같은 것을 보여줄 것입니다. 이것은 모든 것이 동작되고, 우리가 아직 환상적인 Fuse 프로젝트 구조에 한걸음 더 가까이 다가갔음을 의미합니다. 또한, 우리가 어떻게 우리 프로젝트파일 에서 `EditHikePage.js` 파일을 명시적으로 포함할 필요가 없었는지를 알립니다. 단순히 그것이 우리의 앱 안 `JavaScript` 태그들 중 하나로 부터 참조되었기 때문이었습니다. 멋지죠!


##  홈페이지 분리하기 ##
우리의 `EditHikePage` 클래스는 사실 하나로된 두개의 컴포넌트 (하이킹 선택 부분 과 하이킹 수정 부분) 였습니다. 일반적으로, 컴포넌트들을 만들때, 그 것들을 가능하게 단순하게 만드는 것은 좋은 아이디어입니다. 그리고 그들은 하나의 단순한 뷰 처럼 단지 하나의 것을 나타냅니다. 우리가 다음에 할 것은 `Homepage` 라 불리는 하이킹 선택 부분을 그 자신의 클래스로 분리하는 것입니다.
우리는 이전에 이것의 기본적인걸 했었으니, 두 번째인 지금 더 쉬워야 할겁니다. 먼저, `Pages` 디렉터리 안 `HomePage.ux` 라 불리는 파일안에 우리의 홈페이지가 될 빈 페이지 클래스를 생성합니다:
```
<Page ux:Class="HomePage"></Page>
``` 
다음은, `HomePage.ux` 파일로 `EditHikePage`의 홈 페이지 관련 UX code를 통합할 것입니다. 정확하게는 `hikes` 컬렉션을 보여주기 위한 Each 태그 뒤에 있습니다.
```
<Each Items="{hikes}">
    <Button Text="{name}" Clicked="{chooseHike}" />
</Each>
```
그럼 이동시켜 봅시다:
```
<Page ux:Class="HomePage">
    <Each Items="{hikes}">
        <Button Text="{name}" Clicked="{chooseHike}" />
    </Each>
</Page>
```
또한, 우리가 이 하이킹들을 하나로 묶어서 보여줄 것이므로, StackPanel 안에 그것들을 배치하길 원합니다. ScrollView 안에 으로 가져가는 것 또한 좋은 생각입니다. 그것 역시 설정 할 겁니다:
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
그러나 만약 우리가 이 모든것을 저장하면, 우리는 여러가지 것을들 알 수 있습니다. 부분적으로는 우리의 새로운 `HomePage.ux`가 아직 `hikes` 컬렉션에 대해 알지 못한다는 것입니다. 우리는 이 뷰에 대해 약간의 자바스크립트를 추가함으로써 수정할 겁니다.
이전처럼, 우리는 `HomePage.ux` 바로 다음에 우리 `HomePage.js` 파일을 생성할 것입니다. 그리고 거기에 아래와 같은 코드를 배치할 것입니다.
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
이것은 기본적으로 우리가 주석처리했던 `chooseHike` 함수의 컨텐츠들을 제외하면, 현재 `EditHikePage.js` 안의 것들과 같은 코드입니다. 우리는 다음 챕터에서 그것을 얻을 겁니다. 그러나 우리는 `EditHikePage` 가 모든 하이킹을 보여줄 필요가 없을 것이므로, 더이상 양쪽 모두에서 이 코드를 필요로 하지 않습니다. 그래서 우리는 `require`, `chooseHike` 함수, 그리고 `EditHikePage.js` 로 부터의 그들의 exports 를 제거할 것입니다:
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
우리의 자바스크립트가 모든 준비가 된 지금, 이렇게 `HomePage.ux` 에서 그 파일을 링크할 것입니다.
```
<Page ux:Class="HomePage">
    <JavaScript File="HomePage.js" />
```
이쯤에서 우리 페이지는 보여질 준비가 되어야 합니다!

## 홈페이지 보여주기 ##
이제 우리는 여러 페이지들을 얻었고, 그것들을 보여주고 그들 사이에서 네비게이트할 방법이 필요할겁니다. Fuse에서는 Navigator 와 Router 컴포넌트들에 의해 이것이 핸들됩니다. 이 컴포넌트들은 적절한 소개가 필요할 겁니다. 그리고 우리는 그것들이 어떤 강력한 것들이 있는지를 완전히 이해해서 적절하게 사용하기 위한 학습을 할 시간을 가지길 원할 겁니다. 그래서 우리는 다음 챕터에서 이것들에 대해 자세하게 다룰것입니다.
지금은, 간단하게 하기 위해서, PageControl 을 대신 사용할 겁니다. PageControl 은 우리가 swipe 할 나란히 있는 약간의 페이지들을 가지길 원하는 경우에 좋습니다. 우리의 두 페이지들을 초기에 보여주기위한 것으로는 괜찮습니다.
그래서, 우리가 `MainView.ux` 를 살펴보면, 현재 이렇게 보입니다:
```
<App>
    <ClientPanel>
        <EditHikePage />
    </ClientPanel>
</App>
```
이렇게 PageControl 대신 `EditHikePage` 를 배치합시다:
```
<App>
    <ClientPanel>
        <PageControl>
            <EditHikePage />
        </PageControl>
    </ClientPanel>
</App>
```
이제 우리가 해야 하는 것은 PageControl 내의 `HomePage` 인스턴스를 추가하는 것입니다. 그것이 홈페이지 이기 때문에 존재한고 있는 `EditHikePage` 인스턴스의 위에 배치합니다:
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
그럼 이제, 우리는 이 모든 것을 저장하면 새로운 `HomePage` 가 보여질 것입니다. 그리고 우리는 `EditHikePage` 를 나타내기 위해서 오른쪽으로 스와이프 할 수 있습니다.

## 지금까지의 진행 ##  
휴, 우리는 여기서 많은 것들을 다뤘습니다! 지금 우리는 컴포넌트들로 분리되어지고 PageControl 안에 나란히 보여지는 두개의 뷰들을 었었습니다. 이렇게 보일겁니다:

`HomePage.ux` :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/1064aed908b2b711dc4949787bd1cbd5__media/hikr/chapter-3/homepage.webp)

`EditHikePage.ux` :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/b5b6dd8b107c662ec37dfcafd73e2b6f__media/hikr/chapter-3/edithikepage.webp)

그리고 여기 우리의 모든 다양한 파일들에 대한 코드가 있습니다. 현재 약간의 파일이 더 있습니다만, 그것들은 지금보다 훨씬 독립적이고 간단합니다. 

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

## 다음은 무엇인가요 ##
다른 컴포넌트들로 우리의 두개의 뷰들을 분리해냈습니다. 다음 단계는 `EditHikePage` 를 수정하고 또한 그것으로 네비게이트 하도록 할 `HomePage` 안의 selector 를 위해서 그것들을 함께 위에서 가로채는 과정이 될 것입니다. Fuse의 Navigator 와 Router 컴포넌트들을 사용하여 우리 앱의 흐름을 개발하기 시작할 겁니다. 이것이 우리가 다음 챕터에서 다룰 것입니다. 준비되면 가봅시다!

이 챕터의 최종 코드는 여기에 있습니다.

