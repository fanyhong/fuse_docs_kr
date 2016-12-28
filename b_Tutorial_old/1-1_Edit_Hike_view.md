
원본: https://www.fusetools.com/docs/tutorial/edit-hike-view

## 소개 ##
튜토리얼의 첫번째 파트에서 우리는 우리앱의 첫번째 뷰인 Edit Hike 화면에 살을 붙여나가기 시작할 겁니다. 이 뷰는 우리가 하나의 하이킹 데이터를 수정하고 보여줄 수 있도록 할겁니다. 이 것은 우리가 상호작용할 비주얼 컴포넌트들을 나태내면서, 뷰에 의해 데이터가 보여지고 수정가능한 뷰 모델을 보여주는 뷰들로 빌딩되어질 것임을 의미합니다.
이 챕터의 최종코드는 여기에 있습니다.

# 프로젝트 생성하기 #
먼저 'hikr' 라고 불리게 될 우리 프로젝트를 생성할 것입니다. 퀵스타트에서 처럼 Fuse 대시보드나 Fuse 커맨드라인 같은 도구들을 사용함이로써 가능합니다. 예를 들어:
```
fuse create app hikr [optional path]
```
아래 디렉토리 구조를 생성해야 합니다.
```
$ tree
.
|- MainView.ux
|- hikr.unoproj
```
두 파일들이 Fuse를 빌드하기위한 전부입니다. `MainView.ux`는 앱의 최상위 뷰를 위한 UX코드를 포함할 것입니다, 그리고 `hikr.uniproj`는 다양한 프로젝트 세팅들을 포함할 것입니다. 우리가 이 챕터에서 다루는 모든 것을은 `MainView.ux`안에서 일어날 겁니다.
프로젝트를 생성했다면 프로젝트 디렉토리에 들어가 미리보기를 시작할겁니다. 다시 말씀드리지만, 대시보드나 커맨드라인툴에서 가능합니다.
```
cd hikr
fuse preview
```

# 첫번째 하이킹 보여주기 #
프로젝트와 작동중 인 프리뷰를 얻었습니다. 이제 보이는 것들과 데이터 모델에살을 붙일 시간입니다. 아직 앱안의 어떤 다른 페이지들을 설정하는 것에는 걱정하지 않을 겁니다. - 나중에 다룹니다. 간단하게 약간의 텍스트를 보여주는 것을 시작할겁니다.
`MainView.ux`를 열었다면, 이와 같은게 보여야 합니다:
```
<App>
</App>
```
볼 수 있듯이 우리가 동작시키기위한 깨끗한 상태를 나타내는 빈 App 태그 입니다. 또한 UX 안에서 태그들은 대소문자를 구별함을 알립니다. App 은 예를 들어 `app`과 다릅니다.
기본 Text 엘리먼트를 추가할 것입니다.
```
<App>
    <Text>Tricky Trails</Text>
</App>
```
이 쯤에서 `MainView.ux`를 저장할겁니다. 그러면 바로 우리의 변경들을 보여주고 있는 다양한 미리보기들에 업데이트 할것입니다.Fuse가 동작하고 있을때 에러와 오타를 쌓았다가 처리하는 것 대신 즉시 발견하는 용도로 종종 저장하는 것을 권장합니다.
보았듯이 Text 는 텍스트의 읽기전용 블록을 나타내는데 사용됩니다. 이건 훌륭하지만 디바이스에서 미리보기를 했다면 여러분은 아마 이 텍스트가 부분적으로 상태바 에 덮여있다는 것을 알아챘을겁니다. 그래서 우리는 정말 빠르게 수정할 것입니다. 수정을 위해서는 우리가 했던 모든것을 이렇게 ClientPanel 태그안쪽으로 Text 엘리먼드로 감쌉니다:
```
<App>
    <ClientPanel>
        <Text>Tricky Trails</Text>
    </ClientPanel>
</App>
```
ClientPanel 은 다양한 OS 별 바쥬얼 화면의 상단과 하단에 공간을 예정하는 하나의 컨테이너입니다. 이 경우 완벽히 적용되었습니다.
우리가 이동하기 전에, StackPanel 안으로 Text 엘리먼트 또한 배치합시다. StackPanel 은 자식 엘리먼트들 각각을 세로 혹은 가로로 쌓은 다양한 엘리먼트들을 컨테이너에 정렬합니다. 이 경우에는, 하나의 엘리먼트만 가지고 있습니다, 그래서 어떤것도 실제로 일어나진 않지만, 그것은 나중 걱정 할 필요없이 하나로 만드는 좋은 아이디어 일 것입니다. 이 코드는 꽤 단순합니다:
```
<App>
    <ClientPanel>
        <StackPanel>
            <Text>Tricky Trails</Text>
        </StackPanel>
    </ClientPanel>
</App>
```

## 뷰 모델 과 데이터바인딩 ##
멋집니다! 이렇게 화면에 텍스트를 좀 얻었습니다만, UX로 부터 하드코딩된 문자열을 보여주었습니다. 이 뷰가 끝내 하이킹들 중 하나를 수정하기위해 사용되어질 때부터 우리는 대신 동적인 텍스트가 필요합니다. 이건 우리가 자바스크립트로 구현할 뷰 모델이 나오는 곳에 있습니다. 그러고 나서, 그것을 모두 함께 묶기위해 데이터바인딩을 사용할 것입니다.
```
<App>
    <ClientPanel>
        <JavaScript>
            var name = "Tricky Trails";

            module.exports = {
                name: name
            };
        </JavaScript>

        <StackPanel>
            <Text>Tricky Trails</Text>
        </StackPanel>
    </ClientPanel>
</App>
``` 
이제 노출된 JS 값을 UX 에서 얻습니다, 그러나 여전히 UX로부터 하드코딩된 문자열을 나타내고 있습니다. 대신 JS 변수 값을 나타내기위한 Text 엘리먼트를 변경합니다:
```
<Text Value="{name}" />
```
좋습니다! 이제 하이킹들 중 하나의 이름이 나타납니다. 이 것은 자바스크립트로부터 UX로 데이터가 흐르는 단방향 데이터바인딩의 한 예 입니다. 이 것은 데이터를 보여주기에 훌륭합니다, 그러나 마찬가지로 수정하기를 원할 겁니다. 그래서 다음을 설정해 봅시다.


## Observable 과 양방향 데이터바인딩 ##
우리는 UI 에 TextBox 를 추가할 예정입니다. TextBox는 한 라인의 기본 스타일 입력 필드에 단순한 텍스트를 나타냅니다. 이 경우에는 `name`을 수정하기 위해 알맞습니다. 그러나 먼저, TextBox의 `name`의 값을 수정할때 분명하게 할 필요가 있어 이 변경이 UI의 여분에 알려질 필요가 있습니다. 다시말해서, 우리는 관찰되어질 수 있는 이 변수를 확실하게 할 필요가 있고, 이것을 위한 것이 FuseJS 의 Observable 입니다. 
이 것이 어떻게 동작하는지 보려면 먼저 FuseJS의 Observable 모듈을 임포트 합니다. 그러기위해서 아래를 자바스크립트 상단에 추가합니다.
```
var Observable = require("FuseJS/Observable");
```
이제 우리가 할 것은 이렇게 `name` 변수의 정의를 변경하는 것입니다:
```
var name = Observable("Tricky Trails");
```
이제 `name` 은 일반 변수가 아닌 하나의 Observable 입니다. 이건 UI 나 다른 Observable 구독자들에 의한 어떤 변경도 그것이 관찰되어 질 수 있음을 의미합니다. 그래서 데이터베이스가 Observable 이면, 이 것은 정확하게 일어난 것입니다. - Fuse는 업데이트 될 UI 에 따라서 변경되어진 모든 Observable 의 값을 확실히 보장하기위해 필요한 모든 배관들을 관리합니다. 멋집니다!
물론, 우리는 사실 이 값을 변경할 어떤 것이 필요합니다. 우리가 이 전에 말했었던 TextBox 를 추가합니다.
```
<App>
    <ClientPanel>
        <JavaScript>
            var Observable = require("FuseJS/Observable");

            var name = Observable("Tricky Trails");

            module.exports = {
                name: name
            };
        </JavaScript>

        <StackPanel>
            <Text Value="{name}" />

            <TextBox Value="{name}" />
        </StackPanel>
    </ClientPanel>
</App>
```
이제 이렇게 저장하게되면, Text 와 TextBox 엘리먼트 모두 `name` Observable 의 값이 보여지게 됩니다. 그리고 우리가 TextBox의 내용들을 수정할때, 우리는 변경된 Text의 값을 볼 수 있습니다! 들어오는 값들 을 나타내는 UI 업데이트 뿐 아니라 마찬가지로  이 값들을 변경 할 수도 있는 것을 의미하는, 양방향 데이터바인딩을 제공하는 TextBox 의 `value` 프로퍼티에 데이터바인딩 하기 때문입니다.
그리고 이 상태에서, 간단하게 또다른 Text 엘리먼트를 그 위에 추가 함으로써 캡션을 추가합시다.
```
<Text>Name:</Text>
<TextBox Value="{name}" />
```

## 부가적인 필드 추가 ##
동작하는 하나의 에디터를 가진 지금, 우리의 뷰와 뷰 모델에 우리가 관심있는 다른 필드들을 포함해서 약간 더 살을 붙여 봅시다. 특히 하이킹은 아래의 필드들을 포함하길 원할겁니다.
- name (이미 우리가 다뤘던 것)
- location
- distance (km)
- rating (1-5)
- comments
우리 뷰모델을 간단하게 하기위해, 우리는 다른 Observable 로 이 데이터를 유지하고 그걸 수정하기 위한 TextBox 를 사용할 겁니다. 그럼, 이것들을 먼저 다른 Observable 로 만들어 봅시다.
```
var name = Observable("Tricky Trails");
var location = Observable("Lakebed, Utah");
var distance = Observable(10.4);
var rating = Observable(4);
var comments = Observable("This hike was nice and hike-like. Glad I didn't bring a bike.");
``` 
그리고 우리는 마찬가지로 JS 모듈로 부터 그것들을 노출시키는지 확인할 겁니다.
```
name: name,
location: location,
distance: distance,
rating: rating,
comments: comments
```
그리고나서, 이 Observable을 바인딩 하기 할 TextBox 한 묶음을 추가합니다.
```
<Text>Location:</Text>
<TextBox Value="{location}" />

<Text>Distance (km):</Text>
<TextBox Value="{distance}" InputHint="Decimal" />

<Text>Rating:</Text>
<TextBox Value="{rating}" InputHint="Integer" />

<Text>Comments:</Text>
<TextBox Value="{comments}" />
```
또한 숫자 데이터를 수정할 TextBox에 `InputHint`를 어떻게 추가하는지를 보여줍니다. 이것은 iOS 나 안드로이드에서 이 필드들을 수정할때 나올 더 적절한 숫자 키보드를 나오게 할 작은 개선입니다.

## 몇몇 터치들 완료 ##
이젠 우리의 모든 값들이 모두 다 동작하는 에디터를 가져야 합니다. 좋죠! 좀 더 추가적인 향상을 시켜봅시다.
특히, `comments`에 대한 입력 필드를 살펴봅시다.

![comments-capture](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/150155e18098cdb3c3aaab1f29f87d6e__media/hikr/chapter-1/textbox-overflow.webp)

보시다시피, 이것은 한 줄에 적합하다고 하기엔 너무 많은 텍스트가 있습니다. 이 필드에 대해 여러줄로 입력할 수 있도록 하여 모든 텍스트를 보여줄 수 있는 에디터라면 더 나을 것 같습니다.
아주 간단하게 고칩니다. - TextBox 사용 대신에 TextView 를 사용할 것이고, 그것의 `TextWrapping` 프로퍼티가 `Wrap`으로 설정되도록 할 것입니다:
```
<Text>Comments:</Text>
<TextView Value="{comments}" TextWrapping="Wrap" />
```
이건 우리에게 다음에 할 것이 정확히 뭔지 알려줍니다. - 그 필드에 있는 텍스트를 전부를 텍스트를 감싸는 것으로 표현하는 멀티라인 에디터입니다. 물론 좀 어렵게 보일 수 있습니다만, 스타일링에 대한 나중 챕터에서 고칠 것입니다. 지금은 그냥 우리앱의 주요 부분들을 대강 스케치하는 것에 포커스를 두고 계속 진행합시다.
우리가 할 마지막 것은 그것들이 많은 공간을 차지했을때에도 (예를들어, `comments` 값이 꽤 길어졌을때), 우리의 값 에디터들 전부를 접근 가능하게 하는 것일 겁니다. 이렇게 하기 위해서, 다음 같이 간단히 하나의 ScrollView 안에 우리의 StackPanel 을 배치합니다:
```
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
```   
이것은 우리가 원하는 필드를 수정하기 위해 필요한만큼 위아래 스크롤을 할 수 있도록 합니다. 멋집니다!

## 지금까지 우리의 진행 ##
이제 우리는 아래와 같이 특정 하이킹에 대해 데이터를 보여주고 수정할 수 있는 뷰를 얻었습니다:
![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/acc2e02b176d72c27f9c3a4fb62d860c__media/hikr/chapter-1/end-of-chapter.webp)

그리고 여기 최종코드가 있습니다.
```
<App>
    <ClientPanel>
        <JavaScript>
            var Observable = require("FuseJS/Observable");

            var name = Observable("Tricky Trails");
            var location = Observable("Lakebed, Utah");
            var distance = Observable(10.4);
            var rating = Observable(4);
            var comments = Observable("This hike was nice and hike-like. Glad I didn't bring a bike.");

            module.exports = {
                name: name,
                location: location,
                distance: distance,
                rating: rating,
                comments: comments
            };
        </JavaScript>

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
    </ClientPanel>
</App>
```
우리가 볼수 있었던 것 처럼, 이와 같이 위처럼 실행하는 하나의 뷰를 얻는 것은 많이 걸리지 않습니다. 그래서 우리는 이미 이런 것들을 포함하는 광범위한 것들을 다루었습니다:
- 프로젝트 생성/미리보기/수정하기
- Observable 과 데이터바인딩 사용하기
- 하나의 뷰와 뷰모델 빌딩하기

## 다음은 뭔가요 ##
물론, 이 뷰는 단지 하나의 하이킹만 보여줍니다. 그래서 우리가 할 다음은 우리가 보여주고 수정할 수 있는 하이킹들의 리스트입니다.다음 챕터에서, 우리는 여러 하이킹들을 소개하고 하나를 선택해 보여주고 수정할 수 있게 함으로써 지금까지 만든 것들을 확장해 갈 겁니다. 그럼 시작할 준비가 되면 빠져봅시다!
여기에 이 챕터의 최종 코드가 잇습니다.
