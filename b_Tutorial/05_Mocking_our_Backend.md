원문: https://www.fusetools.com/docs/tutorial/mock-backend

# 소개 #

[이 전 챕터](https://www.fusetools.com/docs/tutorial/navigation-and-routing) 에서는 Fuse의 [네비게이터](https://www.fusetools.com/docs/fuse/controls/navigator) 와 [라우터](https://www.fusetools.com/docs/fuse/navigation/router) 클래스를 살펴본 후 그것들을 사용하여 앱의 뷰 를 하나로 묶어 그들 사이에 데이터를 전송했습니다. 그러나 지금까지 `EditHikePage` 는 실제 데이터 모델을 영구적으로 변경할 수 없었습니다. 예를 들어, 하이킹을 선택하고 변경 한 다음 다른 하이킹으로 이동 한 후 다시 탐색하면 변경 한 내용이 손실되어 있습니다. 이 것은 변경 사항들이 `EditHikePage` 의 뷰 모델에서만 로컬로 만들어질 뿐, 우리의 모델에는 전혀 반영 되지 않기 때문입니다.

그러나 이제는 아키텍처에 대해 알게 되었으므로 이 문제를 해결할 시점입니다. 실제 백엔드처럼 작동하는 모듈인 모의(mock) 백엔드 를 구현하면됩니다. 그러나 일부 데이터는 디바이스의 저장소나 데이터베이스 어딘가에 보관하는 대신, 실행중인 응용 프로그램에서 로컬로 저장될 것입니다.

Fuse를 사용하여 앱을 제작할 때 이와 같은 모의 백엔드를 만드는 것은 꼭 필요한 것은 아닙니다. 예를 들어, 기존 백엔드 솔루션을 선택할 수도 있고, 이를 직접 구현할 수도 있습니다. 그러나 이 튜토리얼은 가능한 한 일반적인 상황을 위한 것이기 때문에, 이런 일반적인 개념들을, 특정 백엔드의 세부 사항보다는 핵심 개념에 집중 할 수 있는 독립적-백엔드 방식으로 제공하려고 합니다. 이는 백엔드 솔루션이 실제로 사용되는지 여부에 관계없이, 향후 앱을 실제 백엔드에 연결할 때 기대할 수 있는 것을 이해할 수 있도록, 최소 한 번 이상 수행하는 것이 좋습니다.

> 참고: 이 튜토리얼 시리즈를 완성한 후에는 모의(mock) 백엔드를 특정 백엔드 솔루션 및 기타 멋진 기능과 통합하여 다양한 "트랙" 으로 확장 할 예정입니다!

또한 모의(mock) 백엔드를 직접 사용하는 대신 뷰 모델이 상호 작용할 모의 백엔드 위에 씬 추상화를 만듭니다. 이렇게하면 모의 백엔드 작동 방법을 변경하거나 실제 백엔드로 교체하는 경우, 뷰 모델 중 어떤 것도 변경할 필요가 없게 됩니다. 새로운 백엔드와 상호 작용하도록 추상화를 업데이트 할 수 있으며 이전과 동일한 인터페이스를 계속 제공 할 수 있습니다. 객체 캐싱과 같은 [모의] 백엔드가 가지고 있지 않은 기능을 채워서 이러한 추상화를 활용할 수도 있습니다.

그러나 우리가 뭔가 만들기 시작하기 전에, 일반적인 백엔드에 대한 *인터페이스* 가 어떻게 보이는지를 고려해야 합니다. 살펴봅시다!

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-5) 에서 볼 수 있습니다.

## 일반적인 백엔드 인터페이스 ##

백엔드는 매우 복잡 할 수 있으며, 어떻게 보이고/작동하는지 는 매우 다양 할 수 있지만 아주 기본적인 인터페이스는 전반적으로 비슷합니다. 특히 몇 가지 핵심 기능만 수행하면 더욱 그렇습니다. 예를 들어, 초기화, 가입, 인증 등과 같은 것들은 무시할 수 있습니다. 왜냐하면 그 부분들은 고도로 백엔드에 종속적이고, 우리가 기본 앱에서 관심을 두지 않는 기능이기 때문입니다. 우리의 간단한 앱의 경우, 간단하게 단순한 데이터 저장/검색 그리고 해당 데이터를 업데이트하는 방법만 필요합니다. 이러한 기능을 염두에두고, 간단한 백엔드 인터페이스는 다음과 같이 보일 수 있습니다.

```
// item 오브젝트들의 배열을 반환합니다
function getItems() { ... }
// item을 업데이트 합니다
function updateItem(...) { ... }
```

그럼, 우리 앱은 이 인터페이스를 매우 직접적으로 사용합니다.:

```
// 백엔드로부터 item 오브젝트를 얻습니다
var someItems = getItems();
// 백엔드에서 item 중 하나를 업데이트 합니다
updateItem(...);
```

이것은 충분히 간단합니다. 그러나 우리는 대부분의(전부는 아닐지라도) 백엔드가 처리해야하는 *distribution* 에 대한 매우 중요한 세부 사항을 무시했습니다. 우리의 단순한 인터페이스는 이미 데이터를 로컬에 가지고 있다면 잘 작동 하겠지만, 데이터가 다른 곳에 있는 서버에 저장된다면 어떻게 될까요? 백엔드 서버 (또는 디스크 등)에 요청한 데이터가 응답하기를 기다리면서 코드를 그냥 멈춰 있게 할 수는 없습니다. 우리의 요청이 백엔드에 도착하고, 백엔드가 데이터를 다시 전송하도록 요청하는 데는 불명확한 시간이 걸릴 수 있습니다. 따라서 데이터 검색 및 업데이트는 *비동기* 적으로 이루어져야 합니다. 그리고 자바스크립트에서 어떤 비동기식 계산이 포함될 때, 여러분은 그 근처에서 `Promise` 를 몇 개 발견 할 가능성이 매우 큽니다.

`Promise` 에 대한 [MDN의 문서](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) 를 의역하자면, "`Promise` 는 현재 또는 미래에 사용할 수 있는 값을 나타냅니다." 이것은 `Promise` 가 할 수 있는 일에 대한 아주 기본적인 설명이지만, 이미 이 설명에서 우리는, 백엔드와 비동기적으로 통신하려는 우리의 사용 사례에 부합한다는 것을 알 수 있습니다. `Promise` 를 사용하면 일반적인 백엔드 인터페이스가 다음과 같이 보입니다.:

```
// item 오브젝트들의 배열을 나타내는 Promise를 반환 합니다
function getItems() { ... }
// 백엔드에서 item 이 업데이트 될때, 수행 될 Promise를 반환 합니다.
function updateItem(...) { ... }
```

이것으론 우리 인터페이스가 전혀 바뀌지 않은 것처럼 보입니다! 물론, 이러한 함수를 (그리고 우리 모의 백엔드의 경우에 그것들을 구현하는 것) 실제로 사용하는 것은 약간 다를 수 있지만, 많이 다르진 않습니다. 예를 들면 이 인터페이스를 사용하는 코드는 다음과 같습니다.

```
// 비동기로 백엔드로부터 item 오브젝트를 얻습니다
var someItems = [];
getItems()
    .then(function(items) {
        someItems = items;
    })
    .catch(function(error) {
        console.log("Couldn't get items: " + error);
    });

// 비동기로 백엔드에서 item들 중 하나를 업데이트 합니다
updateItem(...)
    .catch(function(error) {
        console.log("Couldn't update item: " + error);
    })
```

`Promise` 를 제대로 사용하려면, 비동기 코드를 일부 도입해야 합니다. `Promise` 와 상호 작용하는 가장 간단한 방법은 그것이 제공하는 두 가지 함수, 즉, `then` 과 `catch` 를 호출하는 것입니다.

`then` 함수를 호출함으로써, `Promise` 가 수행 될 때 발생 될 작업을 기술 할 수 있습니다. 우리는 `Promise` 가 수행되는 값을 선택적으로 받아들이는 다른 함수를 전달함으로써 이 작업을 수행합니다; 이 경우엔, 백엔드의 items 입니다. 그러나 이 인수는 optional(선택 사항) 입니다. 예를 들면 우리의 `updateItem` 함수의 경우, 업데이트가 완료될때 그것이 반환 하는 `Promise` 는 값을 산출하기 위한 것이 아닙니다. 우리는 `Promise` 가 실제로 완료되었는지 아닌지 여부에만 관심이 있으므로, 이 경우는 인수가 사용되지 않습니다.

`catch` 함수를 호출하여, `Promise` 를 수행하는 동안 오류가 발생하면 어떻게 되는지를 기술 할 수 있습니다. 이 경우 실패의 원인에 대한 설명을 수용하는 함수를 전달할 수 있습니다. 예를 들어 실제 백엔드가 백엔드 서버에 연결할 수 없거나 인증에 실패하면, 오류를 보고 할 수 있습니다. 오류 감지/처리 는 복잡한 주제이지만, 간단한 작업을 위해 이런 많은 세부 사항들을 대충 훑어 볼 것입니다. 우리는 여전히 몇 개의 간단한 에러 핸들러를 `Promise` 에 붙여, 나중에 실제 백엔드에 연결하는 경우에도 사용하기를 원합니다.

`Promises` 는 많은 기능을 제공하며, 매우 유용합니다. 그러나 이 짧은 설명으로, 나중에 실제 백엔드의 인터페이스를 이해 할 정도의, 모의 백엔드를 만들고 사용하기 위해 알아야 할 모든 것을 명확히 해야 합니다.

이제 여러분 중 일부는 흥미로운 것을 발견했을 겁니다. 그것은 우리가 이미 사용하고있는 Fuse의 [Observable](https://www.fusetools.com/docs/fusejs/observable) 들과 비슷한 `Promise` 입니다. `Observable` 은 변경 및 관찰 할 수있는 값을 나타내며, 비동기 인터페이스에도 적합합니다. 실제로 우리가 원한다면 우리의 백엔드 인터페이스를 모의(mock) 하기 위해 `Promise` 들 대신 `Observable` 들을 사용할 수 있습니다. 그 의미는 매우 유사합니다. 그리고 특히 Fuse를  목표로 하는 백엔드를 제작한다면, 백엔드와의 통합이 쉽기 때문에 이것이 권장됩니다! 그러나 Fuse 보다 더 많은 플랫폼을 지원하기 위한 많은 백엔드 솔루션이 구축 되어있기 때문에, 우리는 `Promise` 를 제공하는 인터페이스와 상호 작용 할 가능성이 높습니다. 따라서 그런 경우라고 생각하고, 우리의 mock을 디자인 할 것입니다. 다행스럽게도 `Promise` 들과 `Observable` 들 사이의 유사점 때문에, 이 두 가지 사이의 틈을 메우는 것은 나중에 꽤 쉽게 할 수 있습니다.

> 참고: [MDN Promise 가이드](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) 에서 `Promises` 에 대한 자세한 내용을 배울 수 있고, Fuse 가 구체적으로 준수하는 Promise의 진수를 설명하는 [A+ Promise 표준](https://promisesaplus.com/) 을 볼 수 있습니다.

대체로 `Promise` 들을 사용하는 것이, 일반적인 JS 기반 백엔드 인터페이스가 보여주는 것들과 꽤 근접한 결과를 도출하므로, 이를 모의 백엔드를 모델링하는데 사용 할 겁니다.

## 모의(mock) 백엔드 구현 ##

> 참고: 우리 프로젝트에서 잠시 떠나 있을 것이므로, 한동안 컴파일이나 미리보기를 할 수는 없겠지만, 챕터가 끝나기 전까지는 백업하고 실행 할 것임을 알고 계십시오.

모의 백엔드 구현을 시작하기 전에 JavaScript를 어떻게 구성 할 것인지에 대해 이야기하고자 합니다. 현재 프로젝트 (물론 뷰 모델을 제외하고)에는 독립형 JS 모듈 하나만 있습니다. `hikes.js` 파일입니다. 더 많은 모듈이 추가 될 것이기 때문에, 우리는 프로젝트가 훌륭하고 체계적으로 유지되도록 할 것입니다. 우리는 우리 앱의 서로 다른 페이지들과 관련된 모든 파일들을 배치했던 `Pages` 폴더와 유사하게, 우리 프로젝트의 루트에 `Modules` 폴더를 만들어서 우리의 모든 독립형 JS 모듈을 배치 해 보겠습니다.:

```
.
|- MainView.ux
|- Modules
|- Pages
|  |- EditHikePage.js

...
```

다음으로 할 일은 Fuse가 이 디렉토리에 있는 모듈에 대해 알고 있는지 확인하는 것입니다. 이전에는 `hikes.js` 파일을 프로젝트에 추가 할 때, 프로젝트 파일 ( `hikr.unoproj` )에 이 파일에 대한 참조를 추가하여 응용 프로그램과 함께 번들로 제공했습니다. `Modules` 폴더를 만들었으므로, 이제는 해당 항목을 `Module` 디렉터리의 모든 JavaScript 파일을 번들로 포함하는 항목으로 바꿉니다.

```
...

  "Includes": [
    "*",
    "Modules/*.js:Bundle"
  ]
}
```

이제 모의 백엔드 구현을 시작할 준비가되었습니다. 이전 `hikes.js` 파일에는 이미 제시해야 할 모든 데이터가 포함되어 있으므로 이를 시작점으로 사용할 수 있습니다. 먼저 파일을 `Modules` 디렉토리로 옮기고 그 이름을 `Backend.js` 로 바꿉니다.

새로운 `Backend.js` 파일을 살펴보면, 단순히 하이킹 배열을 그대로 내보내는 것입니다. 그러나 이것이 우리의 모의 백엔드가 될 것이므로, 이전에 논의한 인터페이스와 유사한 인터페이스를 노출시켜야 합니다. 이것은 다음과 같이 보일 것입니다.:

```
// hike 오브젝트들의 배열을 나타내는 Promise를 반환합니다.
function getHikes() { ... }
// 백엔드에서 하이킹이 업데이트되면 수행 될 Promise를 반환합니다.
function updateHike(...) { ... }
```

먼저 `getHikes` 함수를 만듭니다. `hikes` 배열 밑에, 빈 함수부터 시작하겠습니다.:

```
function getHikes() {
}
```

이 함수가 하이킹 배열을 반환하기를 원한다면 다음과 같이 작성할 수 있습니다.:

```
function getHikes() {
    return hikes;
}
```

대신 우리 함수가 `Promise` 를 반환하기를 원합니다. 이 `Promise` 는 `hikes` 가 백엔드에서 그것들을 가져오는 것을 시뮬레이션 할 준비가 되면 수행됩니다. 그래서 우리의 실제 `getHikes` 함수는 다음과 같이 보일 것입니다.:

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        resolve(hikes);
    });
}
```

이제 `hikes` 배열을 반환하는 대신 `new Promise` 를 사용하여 `Promise` 를 생성 합니다. `Promise` 생성자는 promise를 수행 하거나 거부 하기 위한 목적으로 호출 될 함수에서 사용합니다. 이 함수는 `resolve` 와 `reject` 의 두 인수를 취합니다. 이러한 인수는 실제로는 함수 자체입니다. `resolve` 는 우리가 만들고 있는 `Promise` 를 수행하는 함수이며, `then` 함수를 사용하여 `Promise` 에 핸들러를 연결하면 해당 핸들러가 호출됩니다. 우리 코드에는, 이것이 `Promise` 를 `hikes` 컬렉션으로 resolve 하기 위해 호출 해야 하는 함수 전부 입니다. 에러가 발생하면 `reject` 함수가 대신 호출 될 수 있고, `catch` 를 통해 `Promise` 에 첨부 된 핸들러는 이 후에 호출 됩니다.

JS의 내장 `setTimeout` 함수를 사용하여, 실제로 밀리 초 단위로 처리가 완료 될 때 연기 할 수 있습니다. `setTimeout` 도 두 개의 인수를 취합니다. 첫 번째 함수는 미래에 언젠가 호출 될 함수이고, 두 번째 함수는 해당 함수를 호출하기 전에 지연되는 시간 (밀리 초)입니다. 예를 들어, 이 코드는 0.5 초 후 `Promise` 를 resolve 합니다.:

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(hikes);
        }, 500);
    });
}
```

이 코드는 우리의 앱이 백엔드에서 오는 데이터를 기다릴 필요가 있을 때 어떻게 처리되는지를 테스트해 보고자 할 때 매우 유용합니다. 그러나, 테스트하는 동안 자체적으로 단순하게 유지하려면 딜레이 없이 `0` 을 사용하십시오.

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(hikes);
        }, 0);
    });
}
```

완벽한 인터페이스와 적절한 딜레이 시뮬레이션을 가진 멋진 `getHikes` 함수를 얻었습니다!

이제 `updateHike` 함수 차례입니다. 이것은 모의 백엔드에서 업데이트 할 특정 하이킹에 대한 정보를 취하고, 업데이트가 완료되면 완료 될 `Promise` 를 반환하는 함수입니다. 빈 함수에서 시작합니다.:

```
function updateHike() {
}
```

그런 다음 특정 하이킹을 식별하고 업데이트하기위한 몇 가지 인수를 추가합니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
}
```

이 경우 `id` 인수를 사용하여 업데이트 할 하이킹을 식별하고, 나머지 인수는 해당 하이킹의 해당 필드를 모두 덮어 쓰게 됩니다.

다음으로 `Promise` 생성자와 `setTimeout` 을 `getHikes` 에 했던 것처럼 사용하여 선택적인 시간 딜레이로 `Promise` 를 반환합니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
        }, 0);
    });
}
```

좋아 보입니다! 이제 `id` 로 하이킹을 실제로 식별하고, 멤버를 업데이트하는 코드를 추가합니다. 작업을 간단하게 하기 위해, 우리는 우리가 찾고있는 하이킹을 찾기 위한 간단한 선형 검색을 할 것입니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            for (var i = 0; i < hikes.length; i++) {
                var hike = hikes[i];
                if (hike.id == id) {
                }
            }
        }, 0);
    });
}
```

하이킹이 확인되면, 함수의 인수로 데이터를 업데이트하고 검색 루프를 빠져 나옵니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            for (var i = 0; i < hikes.length; i++) {
                var hike = hikes[i];
                if (hike.id == id) {
                    hike.name = name;
                    hike.location = location;
                    hike.distance = distance;
                    hike.rating = rating;
                    hike.comments = comments;
                    break;
                }
            }
        }, 0);
    });
}
```

거의 다 되었습니다. 마지막으로, hike 오브젝트가 업데이트 된 후에 `Promise` 를 resolve 할 것입니다. 우리가 만드는 `Promise` 는 어떤 데이터도 반환하지 않으므로, 매개 변수없이 `resolve` 만 호출하면 됩니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            for (var i = 0; i < hikes.length; i++) {
                var hike = hikes[i];
                if (hike.id == id) {
                    hike.name = name;
                    hike.location = location;
                    hike.distance = distance;
                    hike.rating = rating;
                    hike.comments = comments;
                    break;
                }
            }

            resolve();
        }, 0);
    });
}
```

그리고 그건 우리 모의 백엔드 인터페이스를 위한 겁니다. 마지막으로 해야 할 일은, 다음과 같이 `hikes` 배열 대신 이 함수를 exports 하는 겁니다.:

```
module.exports = {
    getHikes: getHikes,
    updateHike: updateHike
};
```

이것으로 모의 백엔드가 완성되었습니다!

## 우리의 컨텍스트 추상화 ##

이미 언급했듯이, 우리 뷰 모델은 우리 모의 백엔드와 직접적으로 상호 작용 *할 수 있습니다*. 기술적으로는 아무 문제가 없지만, 우리 뷰 모델들이 대신 상호 작용하는, 백엔드 위 작은 추상화 레이어를 만드는 것이 더 유리한 측면이 있습니다. 이렇게 하면 모델에 보다 일관된 인터페이스를 제공 할 수 있으며, 캐싱과 같은 기능을 구현함으로써 앱에서 사용하는 대역폭과 배터리 양을 줄일 수 있게됩니다. 따라서 우리가 할 다음 작업은 `Context` 라고 하는 추상화를 만드는 것입니다.

`Context` 모듈을 만들려면 `Modules` 디렉토리에서 `Context.js` 라는 새 파일을 만듭니다.:

```
.
|- MainView.ux
|- Modules
|  |- Backend.js
|  |- Context.js
|- Pages
|  |- EditHikePage.js

...
```

이 파일에서, FuseJS `Observable` 모듈을 가져옵니다.

```
var Observable = require("FuseJS/Observable");
```

또한 `Backend` 모듈을 다음과 같이 가져옵니다.

```
var Observable = require("FuseJS/Observable");
var Backend = require("./Backend");
```

`Backend` 모듈을 `./Backend` 로 가져온 방법을 잘 보십시오. 일반적으로 앱에 번들로 제공되는 JS 모듈을 가져올 때, `require` 표현식을 사용하고, 그 모듈을 resolve 하기 위해 프로젝트의 루트 디렉토리에 상대적인 경로를 지정합니다. 그러나 마찬가지로 어떤 모듈을 resolve 하기 위한 상대 경로를 사용 할 수도 있습니다. 이 경우 그게 정확히 우리가 하는 일입니다. - 우리는 `Backend.js` 가 `Context.js` 파일과 같은 디렉토리인, `Modules` 디렉토리에 있다는 것을 알고 있습니다. 따라서 `./Backend` 를 사용하면 됩니다.

이제 `Context` 모듈의 핵심에 대한 것입니다. `Context` 는 우리의 뷰 모델에 간단한 인터페이스를 제공하여, 앱의 데이터를 쉽게 소비하고 수정할 수 있도록 해야합니다. 우리 뷰는 결국 데이터 바인딩을 통해 뷰 모델의 데이터를 표시 할 것이므로, `Context` 는 하나 이상의 [Observable](https://www.fusetools.com/docs/fusejs/observable) 을 통해 데이터를 노출하는 것이 이상적입니다. 그래서, 우리는 간단한 `hikes` `Observable` 로 시작할 것입니다  :

```
var hikes = Observable();
```

이 `Observable` 은 뷰 모델에서 사용 가능한 모든 하이킹을 표시하는데 사용됩니다. 앱이 시작되면 우리는 백엔드의 데이터를 사용하여 `Observable` 을 채 웁니다. 앱이 실행될 때, 이 컬렉션은 본질적으로 백엔드 내 같은 컬렉션의 로컬 미러 가 됩니다. 변경 사항을 적용하면 뷰 모델이 즉시 업데이트되도록 이 컬렉션의 내용을 업데이트합니다. 또한 백엔드와 통신하여 정보를 비동기 적으로 업데이트 할 수 있습니다.

우리의 응용 프로그램이 시작되면, 우리는 `Backend` 모듈의 `getHikes` 함수를 호출하고, 초기 데이터로 우리의 `hike` `Observable` 을 채워서 반환하는 `Promise` 를 사용할 것입니다.:

```
var hikes = Observable();

Backend.getHikes()
    .then(function(newHikes) {
        hikes.replaceAll(newHikes);
    })
    .catch(function(error) {
        console.log("Couldn't get hikes: " + error);
    });
```

`Promise` 가 수행될때, 우리가 얻는 배열의 내용으로 `hike` `Observable` 의 내용을 덮어쓰기 위한 `replaceAll` 함수를 사용한 방법을 주목해서 보십시오. 작은 에러 핸들러도 추가로 붙였습니다. 이제 시작시 백엔드로부터의 초기 데이터로 `hikes` `Observable` 을 채웁니다.

다음으로 뷰 모델이 데이터를 업데이트하는 메커니즘을 제공해야 합니다. 모의 백엔드와 마찬가지로, 업데이트 할 하이킹을 식별하는 `id` 와 업데이트 할 데이터를 취하는 `updateHike` 함수를 제공 할 수 있습니다. 이 기능은 로컬 `hike` `Observable` (일을 간단하고 일관성있게 유지하는 데 사용 된 백엔드와 유사한 검색을 사용하여) 을 업데이트하고, 백엔드로 하여금 데이터를 업데이트하도록 알립니다.

```
function updateHike(id, name, location, distance, rating, comments) {
    for (var i = 0; i < hikes.length; i++) {
        var hike = hikes.getAt(i);
        if (hike.id == id) {
            hike.name = name;
            hike.location = location;
            hike.distance = distance;
            hike.rating = rating;
            hike.comments = comments;
            hikes.replaceAt(i, hike);
            break;
        }
    }
    Backend.updateHike(id, name, location, distance, rating, comments)
        .catch(function(error) {
            console.log("Couldn't update hike: " + id);
        });
}
```

마지막으로, 다음과 같이 모듈의 exports 에서 `hike` 와 `updateHike` 를 노출 합니다.:

```
module.exports = {
    hikes: hikes,

    updateHike: updateHike
};
```

우리는 `Context` 모듈을 완성했습니다!

## 모든 것을 연결하기 ##

이제 `Backend` 및 `Context` 모듈을 설정 했으므로, 이전에 가지고 있던 `hikes` 모듈 대신 뷰 모델을 사용해 리팩터링 할 때입니다.

우리는 `HomePage` 를 마이그레이션함으로써 시작할 것입니다. 이 페이지에는 이전 `hikes` 모듈의 하이킹 목록만 표시되었으므로 매우 간단합니다. 우리는 `Pages/HomePage.js` 에서 몇 가지 변경만 하면됩니다. 첫째, `HomePage.js` 를 살펴 보면, 첫 번째 라인은 오래된 `hikes` 모듈을 가져옵니다. 대신 `context` 모듈을 가져 오도록 변경합니다.

```
var Context = require("Modules/Context");
```

해야 할 또 다른 작업은 모듈 exports 에서 `hikes` 대신 `Context.hikes` 를 참조하도록 변경하는 뿐입니다.

```
module.exports = {
    hikes: Context.hikes,

    goToHike: goToHike
};
```

그리고 그것은 우리의 `HomePage` 를 커버 해야 합니다.

`EditHikePage` 차례입니다. 이 페이지는 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 로부터 하이킹 데이터를 받기 때문에, 이 페이지는 이 데이터를 표시하기 위해 어떤 변경도 필요하지 않습니다. 그러나 실제로 데이터를 편집하기 전에 몇 가지 사항을 변경해야 *할 것입니다* . 의미있는 연결을 위해, [마지막 챕터](https://www.fusetools.com/docs/tutorial/navigation-and-routing) 에서 만든 임시 `Back` 버튼 대신, 원래 디자인에서의 `Save` 및 `Cancel` 버튼을 설정하려고 합니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/9ad834ec45583c3d17817e1973f50449__media/hikr/tutorial/app-flow.webp)

`save` 버튼부터 시작하겠습니다. 이 버튼은 이전 페이지로 돌아갈 뿐만 아니라, 편집기에서 변경 한 내용을 데이터 모델에 커밋 한다는 점을 제외하곤 이전에 만든 `Back` 버튼과 거의 동일합니다. 이제는 기존의 `Back` 버튼의 이름을 `Save` 으로 바꾸는 것에서 시작하겠습니다.

먼저 `Pages/EditHikePage.ux` 에서 버튼 텍스트와 clicked 핸들러 를 모두 변경합니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Button Text="Save" Clicked="{save}" />
        </StackPanel>
```

다음으로 `Pages/EditHikePage.js` 에서 `goBack` 핸들러의 이름을 `save` 하도록 바꿉니다.:

```
function save() {
    router.goBack();
}
```

그리고 모듈의 exports 에서도 이름을 업데이트 할 것입니다.:

```
    rating: rating,
    comments: comments,

    save: save
};
```

마지막으로, 이 버튼은 뷰에서 수행 한 모든 수정을 커밋합니다. 이를 위해 `Context` 모듈의 `updateHike` 함수를 호출하여, 우리 `hike` [Observable](https://www.fusetools.com/docs/fusejs/observable) 의 값과 뷰 모델의 `Observable` 들에 포함 된 데이터를 전달합니다.:

```
function save() {
    Context.updateHike(hike.value.id, name.value, location.value, distance.value, rating.value, comments.value);
    router.goBack();
}
```

훌륭합니다! 이제 `Save` 버튼은 모두 연결되어 있어야 합니다. `Cancel` 버튼도 구현해 보겠습니다. `Cancel` 버튼은 `Save` 버튼과 매우 비슷합니다. 편집기에서 변경 한 내용을 어떻게든 취소해야합니다. 그러나 우리가 그 세부 사항에 대해 걱정하기 전에, `Cancel` 버튼과 해당 `cancel` 핸들러를 만들어 보겠습니다.

먼저 `Save` 버튼에 대해 작성한 코드 바로 아래에 있는, `Pages/EditHikePage.ux` 안 버튼의 UX 코드를 추가합니다.:

```
            <Button Text="Save" Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
        </StackPanel>
```

그런 다음 빈 `cancel` 함수를 추가하고 `Pages/EditHikePage.js` 에서 그것을 exports 합니다.:

```
function cancel() {
}

...

module.exports = {
    ...

    cancel: cancel,
    save: save
};
```

그런 다음 `save` 핸들러처럼, `Router` 인스턴스에서 `goBack` 을 호출하여 `cancel` 함수가 이전 페이지로 돌아갈 수 있는지 확인합니다.:

```
function cancel() {
    router.goBack();
}
```

마지막으로, 핸들러가 이전 페이지로 돌아 가기 전에 뷰 모델에서 변경한 사항을 되돌리고 싶습니다. 우리가 이 작업을 수행 할 수있는 몇 가지 방법이 있지만, 가장 쉬운 방법 중 하나는, 뷰 모델에 있는 모든 취소 `Observable` 들이 `EditHikePage` 의 `hike` `Observable` 의 `map` 의 결과 라는 점을 이용하는 것입니다. 이 때문에 `Observable` 의 값을 "새로 고침" 하면 모든 편집기 값이 원래 값으로 재설정 됩니다. 이는 다음과 같이 `hike` `Observable` 의 값을 그 자체에 할당함으로써 간단히 달성 할 수 있습니다 :

```
function cancel() {
    hike.value = hike.value;
    router.goBack();
}
```

쉽죠! 자, 이 코드는 지금 우리에겐 의미가 있지만, 코드를 읽지 않은 사람에겐 (또는 나중에 우리가 이 트릭에 대해 잊어 버린 경우) 왜 이것을 하는지 분명하지 않을 수 있습니다. 그러니 계속 진행하면서 주석을 달아주세요.:

```
function cancel() {
    // 의존하는 Observable 들의 값들을 리셋 하기 위해 hike 값을 새로고침 함
    hike.value = hike.value;
    router.goBack();
}
```

완벽합니다! 이제 모든 것이 연결되어야 있어야 합니다. 이제 우리는 서로 다른 파일들을 모두 저장할 수 있습니다. Fuse가 미리보기를 다시하면, 마침내 우리는 완전한 기능의 뷰를 시험해 볼 수 있습니다!

## 지금까지 우리의 진행 ##

결국, 우리는 기본적인 앱의 주요 기능 부분을 모두 갖추고 연결 했습니다. 이제 우리는 다양한 페이지들과 완벽히 조화롭고 멋진 확장 가능한 아키텍처로 연동된 모듈들을 구현했습니다. 다음과 같이 보입니다.:

![https://res.cloudinary.com/fusetools/image/upload/documentation_v2/3e1775cc8b7d386f31a387a5b250e58b__media/hikr/chapter-5/chapter-5.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/3e1775cc8b7d386f31a387a5b250e58b__media/hikr/chapter-5/chapter-5.mp4)

이 장에서 수정 한 다양한 파일들의 코드는 다음과 같습니다.:

`Modules/Backend.js` :

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

function getHikes() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(hikes);
        }, 0);
    });
}

function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            for (var i = 0; i < hikes.length; i++) {
                var hike = hikes[i];
                if (hike.id == id) {
                    hike.name = name;
                    hike.location = location;
                    hike.distance = distance;
                    hike.rating = rating;
                    hike.comments = comments;
                    break;
                }
            }

            resolve();
        }, 0);
    });
}

module.exports = {
    getHikes: getHikes,
    updateHike: updateHike
};
```

`Modules/Context.js` :

```
var Observable = require("FuseJS/Observable");
var Backend = require("./Backend");

var hikes = Observable();

Backend.getHikes()
    .then(function(newHikes) {
        hikes.replaceAll(newHikes);
    })
    .catch(function(error) {
        console.log("Couldn't get hikes: " + error);
    });

function updateHike(id, name, location, distance, rating, comments) {
    for (var i = 0; i < hikes.length; i++) {
        var hike = hikes.getAt(i);
        if (hike.id == id) {
            hike.name = name;
            hike.location = location;
            hike.distance = distance;
            hike.rating = rating;
            hike.comments = comments;
            hikes.replaceAt(i, hike);
            break;
        }
    }
    Backend.updateHike(id, name, location, distance, rating, comments)
        .catch(function(error) {
            console.log("Couldn't update hike: " + id);
        });
}

module.exports = {
    hikes: hikes,

    updateHike: updateHike
};
```

`Pages/HomePage.js` :

```
var Context = require("Modules/Context");

function goToHike(arg) {
    var hike = arg.data;
    router.push("editHike", hike);
}

module.exports = {
    hikes: Context.hikes,

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

            <Button Text="Save" Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
        </StackPanel>
    </ScrollView>
</Page>
```

`Pages/EditHikePage.js` :

```
var Context = require("Modules/Context");

var hike = this.Parameter;

var name = hike.map(function(x) { return x.name; });
var location = hike.map(function(x) { return x.location; });
var distance = hike.map(function(x) { return x.distance; });
var rating = hike.map(function(x) { return x.rating; });
var comments = hike.map(function(x) { return x.comments; });

function cancel() {
    // Refresh hike value to reset dependent Observables' values
    hike.value = hike.value;
    router.goBack();
}

function save() {
    Context.updateHike(hike.value.id, name.value, location.value, distance.value, rating.value, comments.value);
    router.goBack();
}

module.exports = {
    name: name,
    location: location,
    distance: distance,
    rating: rating,
    comments: comments,

    cancel: cancel,
    save: save
};
```

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
    "Modules/*.js:Bundle"
  ]
}
```

## 다음은 뭔가요? ##

이제 앱의 주요 부분이 모두 갖춰졌으므로, 앱을 돋보이게 만들기 위해 앱의 룩앤필(모양과 느낌) 을 반복적으로 조정해야 할 시간 입니다. [다음 챕터](https://www.fusetools.com/docs/tutorial/look-and-feel) 에서는 커스텀 look/feel 로 다양한 재사용 가능 컴포넌트들을 빌드하여, 앱을 멋지게 꾸미기 위해 그것들을 앱 전체에 뿌릴 것입니다. 그럼 [해 봅시다](https://www.fusetools.com/docs/tutorial/look-and-feel) !

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-5) 에서 볼 수 있습니다.
