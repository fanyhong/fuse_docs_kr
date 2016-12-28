
원본: https://www.fusetools.com/docs/tutorial/multiple-hikes

## 소개 ##
지난 챕터에서 우리는 하이킹 화면을 수정할 수 있는 첫 화면을 대강 만들어 보았습니다. 이건 우리가 하이킹을 수정하는 것 만큼 단순한 하이킹 하나를 나타내는 간단한 뷰였습니다. 그러나 우리가 개인적으로 여러 하이킹들 중 하나를 선택하고 편집하도록 하기위해 필요한 좋은 시작이었습니다.
이 챕터에서 우리가 할것이 바로 그겁니다. 관리를 쉽게하기 위해서 우리는 여전히 `Mainview.ux` 파일 전부를 유지하고 있을 겁니다. 우리가 수정을 원하는 하이킹을 선택하는 것을 허락하기 위한 뷰의 상단에 "selector" 를 추가할 것이고, 그런 뒤에 마지막 챕터의 하이킹 뷰 수정은 이 데이터로 옮겨질 것입니다. 변경사항들은 아직 보여지지 않을 겁니다 - 우리는 간단히 우리의 모델에서 뷰모델로 데이터를 불러올 것입니다만, 다른 방법은 아닐겁니다. 우리는 우리 백엔드를 가짜로 만드는 것에 대한 지난 챕터에서 해결을 할 겁니다.
여기 이 챕터의 최종 코드가 있습니다.

# 하이킹 목록 생성 #
하이킹 목록으로 부터 선택하기 위해서는 먼저 하이킹 목록이 필요합니다. 이건 우리 앱의 모델 처럼 보이는 무언가를 빌딩하기 시작하는 곳입니다. 먼저, 그냥 간단한 하이킹의 배열을 사용합니다. `MainView.ux` 파일의 자바스크립트 위치 상단 부근에 아래의 배열을 위치 시킵니다:
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
이 배열 안의 각각의 아이템이 우리의 하이킹 뷰가 `id` 필드를 추가하는 것을 예외로 했고, 그냥 아이템들 각각에 대한 유니크한 구분자였던 것과 같은 필드들을 가지고 있을 나타낼 것입니다.

## 하이킹 목록 보여주기 ##
이제 하이킹 목록을 얻었으니 그걸 보여줄 간단한 뷰를 만들어 봅시다.
먼저 UX 로 부터 `hikes` 변수를 export 합니다.
```
module.exports = {
    hikes: hikes,

    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments
};
```
이 후에 우리는 보여지고 있는 하이킹들 중 하나를 선택할 것입니다. 각각의 하이킹을 버튼으로 나타낼 것이고, 누르면 특정 하이킹을 선택하여 하이킹 수정화면으로 이동할 것입니다.
먼저, 버튼으로 하이킹의 배열을 보여줄 겁니다. 그러나 UX 안에 어떻게 해야 할까요? 이런 시나리오에서, UX 에서는 Each 라고 불리는 아주 유용한 메커니즘을 제공합니다:
```
<ScrollView>
    <StackPanel>
        <Each Items="{hikes}">
        </Each>

        <Text Value="{name}" />
```
Each 는 아주 강력한 UX 기능 입니다. Each 가 하는 것은 그것의 `Items`프로퍼티에 의해 특정 컬랙션을 취하고, Each 태그 안쪽의 비주얼 하위트리의 복사본으로 각각 아이팀들을 계획합니다. 우리는 `Items`안의 각각의 아이템에 대해 Each 안쪽의 코드를 복사하고 붙여넣기한 것 같은 종류의 것으로 생각할 수 있습니다.   
이 경우에는 하이킹의 `name`을 지정할 Text 하이킹 각각에 대해 Button 을 생성 하기 위해 Each 를 사용할겁니다. 그건 이와 같을 겁니다:
```
<Each Items="{hikes}">
    <Button Text="{name}" />
</Each>
```
이렇게 저장하면, 우리는 이제 Each 가 우리가 예상했던 것 처럼 `hikes` 안 각각의 아이템에 대해 Button을 만들었음을 볼수 있습니다. 멋집니다! Each 안쪽에 더 많은 코드를 넣어 작업할 수 있지만 이 경우에는 각각의 하이킹에 우리가 필요한 Button 하나만 넣었습니다.
우리가 전혀 `name`이라 불리는 어떤 변수를 노출하지 않고도, 각각의 Button 의 Text 프로퍼티에 `name`을 어떻게 데이터바인딩 한 것인지 알려드립니다. Each에 대한 멋진 것들 중 하나는 각각의 `Items`에 관한 것입니다. Each는 "narrow down" 우리가 현재 아이템에 바인딩 되고 있는 데이터 컨텍스트 일겁니다. 글서 우리가 `name`을 바인딩할때, 우리는 JS로부터 노출되는 `name`이라 불리는 변수를 바인딩하지 않을 것입니다: 우리는 이 경우에는 현재 하이킹에 해당하는 현재 아이템의 `name` 프로퍼티에 바인딩 할 겁니다. 쉽습니다!

## 하이킹들 선택하기 ##
이제 우리는 우리의 모델에서 하이킹들 각각의 버튼을 얻었습니다. 우리는 이 버튼들 중 하나를 클릭함으로써 하나를 선택하기를 원합니다.
현재 우리의 뷰 모델은 우리가 수정할 수 있는 다양한 필드들에 대해서 많은 Observable로 분리되어 있습니다. 우리가 현재 수정하고 잇는 하이킹을 대표하는 `value` Observable 을 또 하나 추가해 봅시다. 초기에, 이것은 아직 우리가 수정할 하이킹 선택을 하지 않았으니 비어있을 겁니다.
```
var hike = Observable();
```
이제 다른 Observable의 `hike`로부터 데이터를 얻을 필요가 있습니다. 그리고 이것은 흥미있는 곳이 있습니다. 한 편에서는 `hike`가 이동되어질때면 부득이하게 우리의 Observable 모든 필드를 옮겨야 할 수 있습니다, 그러나 이것은 Observable을 구독해야만 함을 의미하고 나의 취향에 대해 솔직히 좀 너무 강제합니다. 우리는 대신 더 나은 것을 할 수 있는 선언적인 어떤것을 하는것을 훨씬 좋아하는 것 같습니다.
오히려 우리가 할 것은 transform, 혹은 map, 그것의 다양한 프로퍼티들의 값 안의 `hike` Observable 의 값입니다. 예를 들면, `name`과 같은 것은 아마도 이런것일 겁니다:
```
var name = hike.map(function(x) { return x.name; });
```
이것은 여러분이 함수형 리액티브 프로그래밍을 아직 사용해 보지 않았다면, 아마 처음엔 좀 주눅들게 하는 것일지 모르겠습니다. 그렇지만 걱정하지 마세요 - 그것에 대한 한번 느껴본다면 꽤 감이 올 수 있을 것입니다.
우리가 이 코드륿 부분들로 쪼개어 본다면, 우리는 여러가지 것들을 알 수 잇습니다. 먼저, `hike`에서 `map`함수라고 불리는 것입니다. `map`은 스스로 다른 어떤 Observable 같은 것들의 동작인 새로운 Observable 에 전달될 `hike` Observable 속에 들어가는 각각의 값에 해당하는 혜택이 추가된, 새로운 Observable 을 하나 반환할 것입니다. 다시말하면, `name` 안의 모든 값들은 `hike` 안의 값 으로부터 기초하게 되거나 매핑되어 질 것입니다.

여러분은 또한 우리가 `map`안으로 하나의 함수를 전달하고 있음을 알 것입니다. 매핑함수A 라 불리는 이 함수는 어떤 다른 함수들가 다르지 않은 매핑함수 입니다. - 그것은 인자와 그 인자에 기반해 새로운 값을 리턴을 반환값을 취합니다. 때때로, 이 함수들은 부작용이 있는데, 이 의미는 그들이 그 주변의 다른 세상에 영향을 줄 수 있음을 의미합니다. 그러나 문자그대로, 이 함수들은 그들의 인자에 기반해서 더 특별한 것이 없이 단지 값들을 리턴하는 것을 의미하는 순수한 함수들입니다.

그러나 이 매핑함수가 정확이 뭘 하는 거나구요? 좋은 질문입니다! 간단하게 말하면, 매핑함수는 `hike` 안의 새로운 값이 있는 무엇이든지간에 호출되어질 수 있을 겁니다. 이 새 값은 그것의 인자로 그 함수에 전달 받은 것입니다. 그 함수는 그런뒤에 우리가 생성한 Observable 안의 값을 대표하는 이 인자에 기반해서 하나의 새로운 값을 반환할 겁니다.

그래서 전적으로, 이것은 우리가 `hike` 를 취하고 `hike` 안에 있는 `name` 프로퍼티 각각의 값 하나의 새로운 Observable을 생성하는 것을 의미합니다. 꽤 멋있지 않습니까?

> 알림: 우리는 Observable Crash Course 비디오에서, 우리 Observable 문서들 만큼, Observables 에 대한 더 자세한 정보를 찾을 수 있습니다.

이제, 우리가 `name`으로 했던것 처럼 또한 `hike` 에 기반해서 다른 Observable 로 리팩토링 합시다.
```
var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });
```
결국, 우리는 (개인적인 필드들에 대한 모든 Observable 을 차례차례 이동시킬) `hike` Observable 을 옮길 하나의 함수를 우리의 버튼들 각각과 연결할 필요가 있습니다. 우리가곧 채울 자바스크립트에서 빈 함수를 만드는 것으로써 시작할 겁니다.:
```
function chooseHike() {
}
```
다음으로, UX가 그것을 볼수 있도록 exports를 추가합시다.
```
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
그리고 차례로 모든 버튼들을 이렇게 할 겁니다.:
```
<Button Text="{name}" Clicked="{chooseHike}" />
```
이제 함수를 채울시간이네요. 기초적인 아이디어는 `hike` Observable 의 값을 채울 것입니다.
```
function chooseHike() {
    hike.value = ???
}
```
그러나 우리가 설정할 것이 정확이 무엇일까요? 보시는 것처럼, 우리가 Button 에의해 클릭된 함수를 데이터바인딩할때, 그 함수는 인자를 하나 받습니다. 이 인자는 Button에 대한 현재 데이터 컨텍스트를 표현하고 있을 `data` 필드를 포함하고 있습니다. 그리고 우리가 Each 를 사용한 방법때문에 `data`는 사실 나중에 있을 `hike`가 될겁니다. 멋집니다! 그래서 인자에 접근해 그것의 `data`프로퍼티를 우리 `hike` Observable 에 놓기 위한 우리 함수를 업데이트 합시다.
```
function chooseHike(arg) {
    hike.value = arg.data;
}
```
이제 이것을 저장하면, 우리는 우리 하이킹 선택자들이 우리가 기대했던대로 작동하는 것을 볼 수 있을겁니다! 우리가 그들중 하나를 클릭하면, 그 하이킹 수정화면으로 적절히 이동될 것이고 우리는 개인적인 필드들을 수정할 수 있습니다. 멋집니다!

## 지금까지 우리의 진행 ##
이제 우리는 이처럼 이 하이킹들 중 하나를 선택하기 위한 뷰, 그리고 선택된 하이킹을 수정하기위한 뷰인, 많은 하이킹들을 만드는 기본적인 모델을 얻기 시작했습니다:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/8bd3afd968aa2e92726293d5b3393c3c__media/hikr/chapter-2/end-of-chapter.webp)

이처럼 코드가 아주 많이 커지지는 않았습니다:
```
<App>
    <ClientPanel>
        <JavaScript>
            var Observable = require("FuseJS/Observable");

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
이제 우리는 어떻게 데이터가 Fuse 앱을 통해서 흘러가는지 그리고 어떻게 다른 뷰들이 상호작용을 시작할지에 대한 대략적인 값을 얻을 수 있을 겁니다.

## 다음은 뭔가요 ##
우리의 하이킹 목록으로 부터 선택이 가능해지면서 정말 유용해졌습니다, 우리는 이상적으로 이뷰의 부분을 완전히 분리된 뷰들로 분리하기를 원할겁니다. 다음 챕터에서, 우리가 할 것입니다. - 우리는 우리의 뷰들, 뷰 모델들, 그리고 모델들을 조작적이면서 독립적인 컴포넌트들로 을 분리할 것입니다. 해봅시다!

이 챕터의 최종코드는 여기에 있습니다.
