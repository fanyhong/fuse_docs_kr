원본: https://www.fusetools.com/docs/tutorial/multiple-hikes

## 소개 ##

지난 [챕터](https://www.fusetools.com/docs/tutorial/edit-hike-view) 에서 우리는 하이킹 화면을 수정할 수 있는 첫 화면, Edit Hike 뷰를 대강 만들어 보았습니다. 이건 수정 가능한 하나의 하이킹을 나타내는 간단한 뷰였습니다. 좋은 출발이었습니다만, 우리의 앱에서는 많은 하이킹들 중 하나를 골라 개별적으로 편집 할 수 있어야 합니다.

이 챕터에서 할 것이 바로 그겁니다. 관리가 쉽도록 `MainView.ux` 파일에 모든 것을 작성할 것입니다. 수정을 원하는 하이킹을 선택하도록 뷰의 상단에 "selector" 를 추가하고, 마지막 챕터에서 Edit Hike 뷰에 이 데이터가 채워지게 될 것입니다. 변경사항들은 아직 영구 적용되진 않습니다. - 일단 다른 작업없이 단순히 우리 모델의 데이터를 뷰 모델에 로드하는 것만 할 것입니다. 우리는 [백엔드를 임시 모형으로 만드는 것에 대한 이 후 챕터](https://www.fusetools.com/docs/tutorial/mock-backend) 에서 나중에 이를 개선 할 것입니다.

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-2) 에서 볼 수 있습니다.

# 하이킹 목록 생성 #

선택할 하이킹 목록을 표시하려면 먼저 하이킹 목록이 필요합니다. 여기서 우리는 앱용 모델처럼 보이는 것을 만들 것입니다. 처음에는 단순한 배열을 사용합니다. `MainView.ux` 파일의 JavaScript 부분 위 부분에 다음 배열을 배치합니다:

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

이 배열의 각 항목에는 Edit Hike 뷰와 동일한 필드가 있지만 추가 된 `id` 필드는 각 항목의 고유 한 식별자입니다.

## 하이킹 목록 보여주기 ##

이제 하이킹 목록을 얻었으니 그걸 보여줄 간단한 뷰를 만들어 봅시다.
먼저 `hikes` 변수를 UX에서 사용할 수 있도록 export 합니다.

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

표시 할 하이킹 중 하나를 선택할 수 있기를 원하기 때문에 각 하이킹을 버튼으로 표시 할 것입니다. 이 버튼을 누르면 특정 하이킹을 선택하고 Edit Hike 뷰를 채웁니다. 

먼저 하이킹 배열을 버튼으로 표시합니다. 하지만 UX에서 어떻게 할 수 있을까요? 이러한 시나리오에서 UX는 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 이라는 매우 유용한 메커니즘을 제공합니다:

```
<ScrollView>
    <StackPanel>
        <Each Items="{hikes}">
        </Each>

        <Text Value="{name}" />
```

[Each](https://www.fusetools.com/docs/fuse/reactive/each) 는 매우 강력한 UX 기능입니다. [Each](https://www.fusetools.com/docs/fuse/reactive/each) 의 작업은 `Items` 속성에 지정된 컬렉션을 가져 와서 각 항목을 각 태그 내의 시각적 하위 트리 복사본으로 투영합니다. Items의 각 항목에 대해 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 의 코드를 복사하여 붙여 넣는 것과 같은 것으로 생각할 수 있습니다.

이 경우에는 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 를 사용하여 텍스트를 하이킹의 `name` 으로 설정 할 각 하이킹에 대한 [Button](https://www.fusetools.com/docs/fuse/controls/button) 을 만듭니다. 그러면 다음과 같이 보일 것입니다:

```
<Each Items="{hikes}">
    <Button Text="{name}" />
</Each>
```

여기서 저장을 하면, 우리가 예상했던 것처럼 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 로부터 `hikes` 의 각 항목에 대해 각각의 [Button](https://www.fusetools.com/docs/fuse/controls/button) 이 생성 되었음을 볼 수 있습니다. 멋지죠! 이것은 우리가 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 내부에 넣을 수 있는 거의 모든 코드에서 작동합니다만, 우리의 경우는 각 하이킹에 대해 하나의 [Button](https://www.fusetools.com/docs/fuse/controls/button) 만 있으면 됩니다.

우리는 또한 각 [Button](https://www.fusetools.com/docs/fuse/controls/button) 의 [Text](https://www.fusetools.com/docs/fuse/controls/text) 속성을 `name` 으로 데이터 바인딩하는 방법에 주목합니다, 그렇지만 `name` 이라는 변수를 export 하지는 않았습니다. [Each](https://www.fusetools.com/docs/fuse/reactive/each) 에 대한 멋진 점 중 하나는, `Items` 의 각 아이템이 바인딩 중인 데이터 컨텍스트를 현재의 Item 으로 "좁힌다" 라는 것입니다. 그래서 우리가 `name` 에 바인딩 할 때, 우리는 JS에서 export 된 `name` 이라는 변수에 바인딩하지 않습니다; 우리는 현재 Item 의 `name` 속성에 바인딩합니다. 이 경우에는 현재의 하이킹이 바인딩 됩니다. 이렇게 쉽습니다!

## 하이킹 선택 ##

이제 모델에서 각각의 하이킹을 위한 버튼을 얻었으므로 이 버튼 중 하나를 클릭하여 선택할 수 있습니다. 

현재 우리의 뷰 모델은 우리가 편집 할 수있는 다양한 필드에 대해 많은 [Observable](https://www.fusetools.com/docs/fusejs/observable) 로 구성되어 있습니다. 현재 편집중인 하이킹의 `value` 를 나타내는 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 추가합시다. 처음에는 하이킹을 아직 선택하지 않았으므로 이 항목은 비어 있습니다.

```
var hike = Observable();
```

이제 데이터를 다른 [Observable](https://www.fusetools.com/docs/fusejs/observable) 들로 부터 `hike` 로 가져와야 합니다. 이것이 흥미로운 부분입니다. 한편, `hike` 가 채워질 때마다 우리는 부득이하게 [Observable](https://www.fusetools.com/docs/fusejs/observable) 의 모든 필드를 채워야 할 수 있습니다. 이것은 우리가 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 구독해야 한다는 것을 의미 하는 것이므로, 솔직히 스타일을 너무 강요하게 됩니다. 우리는 이런 방법 대신 뭔가 선언적인 방법을 사용 하는 것을 선호합니다!

우리가 할 일은 [Observable](https://www.fusetools.com/docs/fusejs/observable) `hike` 의 값을 다양한 속성의 값으로 *변형* 하거나 *매핑* 하는 것입니다. 예를 들어 `name` 이 다음과 같이 표시 될 수 있습니다:

```
var name = hike.map(function(x) { return x.name; });
```

이것은 여러분이 함수형 리액티브 프로그래밍을 아직 해보지 않았다면, 처음엔 좀 주눅들게 하는 것일지 모르겠습니다. 그렇지만 걱정하지 마세요 - 그것에 대해 한번 느껴본다면 꽤 직관적으로 느껴질 수 있습니다.

이 코드를 부분으로 나누면 몇 가지 사실을 알게 될 것입니다. 우선, 우리는 `hike` 에 `map` 함수를 호출합니다. `map` 자체는 다른 [Observable](https://www.fusetools.com/docs/fusejs/observable) 과 똑같이 작동하는 새로운 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 반환 할 것이며, 추가 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 포함 된 각 값은 새로운 Observable에 *전달* 됩니다. 즉, `name` 의 모든 값은 `hike` 값을 *기준* 으로 하거나 값에서 *매핑* 됩니다. 

또한 함수를 `map` 에 전달한다는 것을 알 수 있습니다. 이 함수는 (당연히) *매핑 함수* 라고 합니다. 매핑 함수는 다른 함수와 마찬가지로 - 인수를 사용하여 해당 인수를 기반으로 새 값을 반환합니다. 때로는 이러한 기능에는 *부작용* 이 있어 주변에 영향을 미칠 수도 있습니다. 그러나 일반적으로 이러한 함수는 별 다른 작업이 추가되지 않은, 인수 만을 기반으로 값을 리턴하는 *순수한 함수* 입니다. 

그럼 이 매핑 함수는 정확히 무엇을 할까요? 좋은 질문입니다! 간단히 말해 `hike` 가 새로운 값이 가질 때마다 매핑 함수가 호출됩니다. 이 새로운 값은 인수로 함수에 전달됩니다. 함수는 이 인수를 기반으로 새로운 값을 반환합니다. 이 값은 우리가 만드는 [Observable](https://www.fusetools.com/docs/fusejs/observable) 의 값을 나타냅니다. 

그래서, 결국 이것은 우리가 `hike` 를 가져 오고, `hike` 에 들어가는 각 값의 `name` 속성을 나타내는 새로운 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 만드는 것을 의미합니다. 꽤 멋지지 않나요?

> 참고 : [Observable 정복 코스](https://www.youtube.com/watch?v=bB9P4mTGtVU) 비디오 및 [Observable](https://www.fusetools.com/docs/fusejs/observable) docs 에서 Observables에 대한 자세한 정보를 확인할 수 있습니다.

자 이제, `name` 에 했던 것처럼 `hike` 를 기반으로 다른 Observable 의 리팩토링을 합시다.

```
var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });
```

마지막으로, 각 버튼을 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 를 채울 함수에 연결해야 합니다 (차례대로 모든 필드의 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 채웁니다).
자바스크립트에서 빈 함수를 만드는 것으로 시작합니다. 잠시 후 작성하겠습니다:

```
function chooseHike() {
}
```

다음으로, UX에서 볼 수 있도록 exports에 추가합니다.

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

그리고 우리는 계속해서 다음과 같이 모든 버튼을 연결합니다:

```
<Button Text="{name}" Clicked="{chooseHike}" />
```

이제 함수를 채울 타이밍 이네요. 기초가 되는 아이디어는 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 의 값을 채울 것이라는 겁니다.

```
function chooseHike() {
    hike.value = ???
}
```

그러나 우리가 설정 할 것이 정확이 무엇일까요? 우리가 [Button](https://www.fusetools.com/docs/fuse/controls/button) 을 `clicked` 하여 함수를 데이터 바인딩 할 때 그 함수는 인수를 받을 수 있습니다. 이 인수는 [Button](https://www.fusetools.com/docs/fuse/controls/button) 의 현재 데이터 컨텍스트를 나타내는 `data` 필드를 포함합니다. 그리고 우리가 [Each](https://www.fusetools.com/docs/fuse/reactive/each) 를 사용한 방식 때문에 `data` 가 실제로 우리가 추구하는 `hike` 가 될 것입니다. 그럼, 인수를 받아들이고 `data` 속성을 우리의 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 넣기 위해 우리 함수를 업데이트 합시다:

```
function chooseHike(arg) {
    hike.value = arg.data;
}
```

그리고 이제 이것을 저장하면 하이킹 selector가 예상대로 작동하는 것을 볼 수 있습니다! 그 중 하나를 클릭하면 Edit Hike 뷰 가 올바르게 채워지고 개별 필드를 편집 할 수 있습니다. 멋지네요!

## 지금까지의 진행 ##

이제 우리는 많은 하이킹들로 구성된 기본 모델, 이러한 하이킹 중 하나를 선택하는 뷰 및 선택한 하이킹을 편집 하기 위한 뷰를 얻기 시작했습니다. 전체적으로 다음과 같이 보입니다:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/8bd3afd968aa2e92726293d5b3393c3c__media/hikr/chapter-2/end-of-chapter.webp)

코드가 아주 많이 커지지는 않았습니다:

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

이제 우리는 Fuse 앱을 통해 데이터가 어떻게 흘러가는지 그리고 다른 뷰들이 어떻게 상호작용을 하는지에 대한 대략적인 느낌을 얻었습니다.

## 다음은 뭔가요? ##

하이킹 목록으로 부터 선택이 가능해지면서 정말 유용해졌습니다만, 이 뷰의 일부를 별도의 뷰로 완전히 분리하는 것이 더 이상적입니다. [다음 챕터](https://www.fusetools.com/docs/tutorial/splitting-up-components) 에서, 우리가 할 것입니다. - 우리는 뷰, 뷰 모델, 그리고 모델들을 조직적이면서 독립적인 컴포넌트들로 분리 할 것입니다. [파](https://www.fusetools.com/docs/tutorial/splitting-up-components) 봅시다!

이 챕터의 최종코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-2) 에 있습니다.
