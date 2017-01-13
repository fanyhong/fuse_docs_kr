원본: https://www.fusetools.com/docs/tutorial/edit-hike-view

## 소개 ##

이 튜토리얼의 첫 부분에서 우리는 앱의 첫번째 뷰, EditHike 뷰에 살을 붙여 나갈 겁니다. 이 뷰는 단일 하이킹에 대한 데이터를 표시하고 편집 할 수 있습니다. 즉, 우리가 상호 작용할 시각적 컴포넌트들을 나타내는 *뷰* 와 뷰에 의해 표현된 편집 가능한 데이터를 나타내는 *뷰 모델* 을 모두 만들 것입니다.

이 챕터의 최종코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-1) 에 있습니다.

# 프로젝트 생성하기 #

우리가 할 첫 번째 일은 우리 프로젝트를 만드는 것입니다. 우리는 그것을 "hikr"이라고 부를 것입니다. [quickstart](https://www.fusetools.com/docs/basics/quickstart) 와 마찬가지로 Fuse대시보드 또는 Fuse 명령을 사용하여 수행 할 수 있습니다.

예:

```
fuse create app hikr [optional path]
```

이렇게하면 다음과 같은 디렉토리 구조가 생성됩니다.

```
$ tree
.
|- MainView.ux
|- hikr.unoproj
```

이 두 파일이 Fuse 앱을 만드는데 필요한 전부입니다! `MainView.ux` 는 앱의 최상위 뷰에 대한 UX 코드가 포함 될 것이며, `hikr.unoproj` 에는 다양한 프로젝트 설정이 포함될 것입니다. 이 챕터에서 다룰 내용은 모두 `MainView.ux` 에서 발생됩니다.

이제 프로젝트가 생겼으므로 프로젝트 디렉토리로 가서, 미리보기를 시작합니다. 대시보드나 커맨드라인 도구를 사용하여 수행할 수 있습니다:

```
cd hikr
fuse preview
```

디바이스에서도 미리보기를 시작할 수 있음을 기억하시고, 지금 한번 해보십시오. 어떻게 작동시키는지에 대한 정보는 [미리보기 및 내보내기](https://www.fusetools.com/docs/basics/preview-and-export) 가이드를 참조하십시오.

# 첫번째 하이킹 보여주기 #

이제 프로젝트가 완료되고 미리보기가 실행되었으므로, 뭔가 디스플레이를 시작하고 데이터 모델을 구현해야 할 때입니다. 우리는 앱의 다른 페이지들을 설정하는 것에 대해서는 아직 걱정할 필요가 없습니다. - 나중에 나옵니다. 지금은, 단순히 텍스트만 표시하는 것으로 시작하겠습니다.

`MainView.ux` 를 열면 다음과 같이 보입니다:

```
<App>
</App>
```

보시다시피, 이 태그는 그냥 비어있는 [App](https://www.fusetools.com/docs/fuse/app) 태그일 뿐이며, 우리가 작업을 시작할 수 있는 깨끗한 공간을 나타냅니다. UX에서 태그는 대소문자를 구분한다는 것 또한 알고 계십시오. 예를 들어, [App](https://www.fusetools.com/docs/fuse/app) 은 `app` 과 다릅니다.

이제 우리는 기본 [Text](https://www.fusetools.com/docs/fuse/controls/text) 요소를 추가합니다:

```
<App>
    <Text>Tricky Trails</Text>
</App>
```

여기서, 우리가 `MainView.ux` 를 저장하면, 즉시 다양한 미리보기들에서 변경 사항을 보여주는 업데이트를 할 것입니다! Fuse로 작업 할 때 자주 저장하는 것이 좋습니다. 그러면 오류 및 오타를 나중에 모았다가 처리할 필요 없이, 바로 처리 할 수 있습니다.

보시다시피, 텍스트는 읽기 전용 텍스트 블록을 표시하는 데 사용됩니다. 이 기능은 유용하지만, 만약 사용자가 디바이스에서 미리보기를 한다면, 이 텍스트가 상태표시줄에 부분적으로 겹쳐서 표시된다는걸 눈치 챌 수 있습니다. 상태표시줄과 겹치지 않도록 하려면, `ClientPanel` 태그 안에 Text 요소를 래핑하면 (감싸면) 됩니다:

```
<App>
    <ClientPanel>
        <Text>Tricky Trails</Text>
    </ClientPanel>
</App>
```

ClientPanel 은 사실 특정-OS 의 비주얼들(이 경우 완벽하게 적용된) 을 위해 화면 상단과 하단에 공간을 확보하는 하나의 컨테이너일 뿐입니다. 

계속하기 전에 [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 내부에 [Text](https://www.fusetools.com/docs/fuse/controls/text) 요소를 배치 해 봅시다. [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 은 자식요소 각각을 세로 또는 가로로 쌓는 여러 요소에 대한 *컨테이너* 의 *일종* 입니다. 우리의 경우, 요소가 하나뿐이므로 아무것도 하지 않겠지만, 나중에 걱정할 필요 없게 지금 만들어 두는게 좋을 것 같습니다. 코드는 꽤 간단합니다:

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

멋지죠! 이제 화면에 텍스트가 있지만, UX에서 하드코딩된 문자열을 표시합니다. 이 뷰는 결국 하이킹 중 하나를 수정하는데 사용되므로, 하드코딩 대신 이 텍스트를 동적으로 만들 필요가 있습니다. 이것은 자바스크립트로 구현된 우리의 *뷰 모델* 이 나오는 곳에 위치합니다. 그런 다음, 데이터 바인딩을 사용하여 모두 연결합니다.

하이킹의 name 을 exports (내보내기) 하게 될 우리 앱에, 인라인 자바스크립트를 추가합시다.:

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

이제 UX에 노출 된 JS 값을 얻었지만, 여전히 UX에서 하드코딩된 문자열을 표시합니다. 이 대신 JS 변수의 값을 표시하도록 [Text](https://www.fusetools.com/docs/fuse/controls/text) 요소를 변경해 보겠습니다:

```
<Text Value="{name}" />
```

좋습니다! 이제 하이킹들 중 하나의 이름이 나타납니다. 이것은 데이터가 JavaScript(뷰 모델)에서 UX(뷰)로 이동하는 *단방향* 데이터 바인딩의 한 예입니다. 이는 데이터를 *표시하는데* 적합하지만, 우리는 *수정* 도 하길 원합니다. 설정해 보겠습니다.

## Observable 과 양방향 데이터바인딩 ##

우리가 할 일은 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 UI에 추가하는 겁니다. [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 는 기본 스타일을 가진 간단한 단일행 텍스트 입력필드를 나타내며, 우리의 경우엔 `name` 편집으로 사용되기에 적합합니다. 하지만,ㅂ 먼저 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 가 `name` 값을 편집할 때 나머지 UI에 이 변경 사항을 알릴 수 있어야 합니다. 즉, 우리는 이 변수가 관찰 되어질 수 있는지 확인해야 합니다. 이것이 FuseJS의 [Observable](https://www.fusetools.com/docs/fusejs/observable) 이 무엇인지에 대한 것입니다.
이것이 어떻게 작동하는지 보기 위해서, 먼저 FuseJS [Observable](https://www.fusetools.com/docs/fusejs/observable) 모듈을 가져와 봅시다. JavaScript 코드 상단에 다음을 추가합니다.

```
var Observable = require("FuseJS/Observable");
```

이제 우리는 `name` 변수의 선언을 다음과 같이 변경해야 합니다:

```
var name = Observable("Tricky Trails");
```

이제 `name` 은 일반 변수가 아닌 [Observable](https://www.fusetools.com/docs/fusejs/observable) 입니다. 즉, UI 또는 Observable을 가진 다른 *구독자* 는 이를 변경할 수 있음을 의미합니다. 이것은 정확히 [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 데이터 바인딩할 때 일어납니다. Fuse가 [Observable](https://www.fusetools.com/docs/fusejs/observable) 의 값이 변경되면, 그에 따라 UI가 업데이트 되도록 하는데 필요한 모든 내부 연결 작업을 처리해 줍니다. 멋집니다!

물론, 이제 우리는 이 값를 변화시키도록 할 무언가가 필요합니다. [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 추가해 보겠습니다.

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

이제 저장하면 `name` [Observable](https://www.fusetools.com/docs/fusejs/observable) 값을 표시하는 [Text](https://www.fusetools.com/docs/fuse/controls/text) 및 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 요소를 모두 갖게됩니다. 그리고 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 의 내용을 편집하면 [Text](https://www.fusetools.com/docs/fuse/controls/text) 값도 변경된다는 것을 알 수 있습니다! 이는 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 의 `value` 속성에 대한 데이터 바인딩이 양방향 데이터 바인딩을 제공하기 때문에, UI 업데이트로 들어오는 값을 표시 할뿐만 아니라 이러한 값을 변경할 수도 있음을 의미합니다.

그럼 그 위에 다른 텍스트 요소로 캡션을 추가해 보겠습니다.

```
<Text>Name:</Text>
<TextBox Value="{name}" />
```

괜찮아 보입니다!

## 부가적인 필드 추가 ##

이제 동작되는 에디터가 하나 생겼으므로, 우리가 관심을 가지고 있는 다른 필드들도 포함하기 위한 뷰와 뷰모델을 조금 더추가해 보겠습니다. 특히 하이킹은 다음의 필드들이 필요합니다.

- name (이미 우리가 다뤘던)
- location
- distance (km)
- rating (1-5)
- comments

우리 뷰모델을 단순하게 유지하기 위해, 우리는 [Observable](https://www.fusetools.com/docs/fusejs/observable) (`name` 같은) 들 안에 이 데이터들을 저장 할 것이고, 그것들을 수정하기 위해 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 사용 할 것입니다. 그럼 먼저 이것들을 [Observable](https://www.fusetools.com/docs/fusejs/observable) 들로 만들어 봅시다:

```
var name = Observable("Tricky Trails");
var location = Observable("Lakebed, Utah");
var distance = Observable(10.4);
var rating = Observable(4);
var comments = Observable("This hike was nice and hike-like. Glad I didn't bring a bike.");
``` 

그리고 JS 모듈에서 이들을 exports (내보내기) 할 것입니다.

```
name: name,
location: location,
distance: distance,
rating: rating,
comments: comments
```

그런 다음 [Observable](https://www.fusetools.com/docs/fusejs/observable) 에 바인딩 할 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 추가합니다 (도움말 캡션과 함께):

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

우리가 숫자 데이터를 수정할 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 의 `InputHints` 를 어떻게 추가했는지 주목하십시오. 이것은 iOS 및 Android에서 이 필드를 편집할 때 더 적합한 숫자 키보드가 표시되도록 하는 작은 개선 사항입니다.

## 약간의 마무리 작업 ##

이제 우리는 우리의 모든 값에 대해 완벽하게 작동하는 에디터를 가지고 있습니다. 좋습니다! 그럼 몇 가지 추가적인 개선을 시도해 봅시다.

특히, `comments` 입력 필드를 살펴 보겠습니다:

![comments-capture](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/150155e18098cdb3c3aaab1f29f87d6e__media/hikr/chapter-1/textbox-overflow.webp)

보시다시피 한 줄에 너무 많은 텍스트가 들어갑니다. 이 에디터가 모든 텍스트를 표시하고, 이 필드에 여러 줄을 삽입 할 수있게 하면 훨씬 더 좋을 것입니다. 

이 문제를 해결하려면 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 사용하는 대신 [TextView](https://www.fusetools.com/docs/fuse/controls/textview) 를 사용하고, `TextWrapping` 속성이 `Wrap` 으로 설정되어 있는지 확인합니다:

```
<Text>Comments:</Text>
<TextView Value="{comments}" TextWrapping="Wrap" />
```

이것으로써, 필드의 모든 텍스트를 표시하는 텍스트 줄바꿈이 포함된 멀티라인 편집기가 완성되었습니다. 물론 좀 보기 힘들지만, 스타일링에 대한 챕터에서 나중에 수정 할 것입니다. 지금은 앱의 주요 부분에 계속 집중하겠습니다. 

우리가 할 마지막 작업은, 내용이 길어 에디터의 많은 공간을 차지하더라도 (예를 들어 `comments` 값이 상당히 긴 경우) 액세스 할 수 있도록 하는 것입니다. 이렇게 하려면, 다음과 같이 [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 을 [ScrollView](https://www.fusetools.com/docs/fuse/controls/stackpanel) 로 감싸서 배치하면 됩니다.

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

이렇게하면 우리가 원하는 필드를 편집할때, 필요에 따라 위/아래로 스크롤 할 수 있습니다.

## 지금까지 우리의 진행 ##

이제 특정 하이킹 데이터를 표시/편집 할 수있는 뷰를 볼 수 있습니다. 다음과 같이 보일 것입니다.:

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

우리가 볼수 있었듯이, 위와 같이 실행되는 하나의 뷰를 얻는데 그다지 많은 시간이 걸리지 않았습니다. 그리고 우리는 이미 다음과 같은 광범위한 것들을 다루었습니다:

- 프로젝트 생성/미리보기/수정하기
- Observable 과 데이터바인딩 사용하기
- 하나의 뷰 및 뷰모델 만들기

## 다음은 뭔가요? ##

물론, 이 뷰는 하나의 하이킹만 보여주지만, 이 뒤에는 디스플레이/수정 가능한 하이킹 리스트가 있습니다. [다음 챕터](https://www.fusetools.com/docs/tutorial/multiple-hikes) 에서는, 다양한 하이킹들 소개 후 이 중 하나를 선택해 보여주고 수정할 수 있게 하는 뷰를 만들어 보면서, 지금까지 한 것들을 확장해 갈 겁니다. 그럼 시작할 준비가 되면 [빠져봅시다](https://www.fusetools.com/docs/tutorial/multiple-hikes) !

[여기](https://github.com/fusetools/hikr/tree/chapter-1) 에 이 챕터의 최종 코드가 있습니다.
