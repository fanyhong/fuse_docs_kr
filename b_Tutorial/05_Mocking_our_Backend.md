원문: https://www.fusetools.com/docs/tutorial/mock-backend

# 소개 #

이전 장에서는 퓨즈의 네비게이터와 라우터 클래스를 살펴본 후 그것들을 사용하여 앱의 견해를 하나로 묶어 그들 사이에 데이터를 보냈습니다. 그러나 지금까지 EditHikePage는 실제로 데이터 모델을 영구적으로 변경할 수 없었습니다. 예를 들어, 하이킹을 선택하고 변경 한 다음 다른 하이킹으로 이동 한 후 다시 탐색하면 변경 한 내용이 손실됩니다. 이는 이러한 변경 사항이 EditHikePage의 뷰 모델에서만 로컬로 만들어지기 때문에 결코 모델로 돌아 가지 않기 때문입니다.

그러나 이제는 아키텍처에 대해 알아 냈으므로 이제이 문제를 해결할 때입니다. 실제 백엔드처럼 작동하는 모듈 인 모의 백엔드를 구현하면됩니다. 그러나 일부 데이터는 장치의 저장소 나 데이터베이스의 어느 곳에서나 유지하는 대신 실행중인 응용 프로그램에 로컬로 저장됩니다.

퓨즈를 사용하여 앱을 제작할 때 이와 같은 모의 백엔드를 만드는 것은 꼭 필요한 것은 아닙니다. 예를 들어, 기존 백엔드 솔루션을 선택할 수도 있고,이를 직접 구현할 수도 있습니다. 그러나이 튜토리얼은 가능한 한 일반화하기위한 것이므로 일반적으로 백엔드에 독립적 인 방식으로 이러한 개념을 제시하여 특정 백엔드의 세부 사항이 아닌 핵심 개념에 중점을 둘 수 있습니다. 이는 백엔드 솔루션이 실제로 사용되는지 여부에 관계없이 향후 앱을 실제 백엔드에 연결할 때 기대할 수있는 것을 이해할 수 있도록 적어도 한 번 이상 수행하는 것이 좋습니다.

> 참고 :이 튜토리얼 시리즈를 완성한 후에는 모의 백엔드를 특정 백엔드 솔루션 및 기타 멋진 기능과 통합하여 다양한 "트랙"으로 확장 할 예정입니다. !

또한 모의 백엔드를 직접 사용하는 대신 뷰 모델이 상호 작용할 모의 백엔드 위에 씬 추상화를 만듭니다. 이렇게하면 모의 백엔드 작동 방법을 변경하거나 실제 백엔드로 교체하는 경우 뷰 모델 중 어느 것도 변경해야합니다. 새로운 백엔드와 상호 작용하도록 추상화를 업데이트 할 수 있으며 이전과 동일한 인터페이스를 계속 제공 할 수 있습니다. 객체 캐싱과 같은 모의 백엔드가 가지고 있지 않은 기능을 채워서 이러한 추상화를 활용할 수도 있습니다.

그러나 우리가 무엇인가를 만들기 시작하기 전에 전형적인 백엔드와의 인터페이스가 어떤 것인지를 고려해야합니다. 그럼 잠수 해 보자!

이 장의 마지막 코드는 여기에서 볼 수 있습니다.

## 일반적인 백엔드 인터페이스 ##

백엔드는 매우 복잡 할 수 있으며 어떻게 보이고 / 작동하는지는 매우 다양 할 수 있지만 가장 기본적인 인터페이스는 전반적으로 비슷합니다. 특히 몇 가지 핵심 기능 만 수행하면 더욱 그렇습니다. 예를 들어, 초기화, 가입, 인증 등과 같은 것들은 무시할 수 있습니다. 왜냐하면 그 부분은 고도로 백엔드에 종속적이기 때문입니다. 우리가 기본 앱에서 관심을 두지 않는 기능이기 때문입니다. 간단한 앱 케이스의 경우 간단하게 간단한 데이터 저장 / 검색과 해당 데이터를 업데이트하는 방법 만 필요합니다. 이러한 기능을 염두에두고 간단한 백엔드 인터페이스는 다음과 같이 보일 수 있습니다.

```
// Returns an array of item objects
function getItems() { ... }
// Updates an item
function updateItem(...) { ... }
```

그렇다면 우리의 앱은이 인터페이스를 매우 직접적으로 사용합니다.:

```
// Get the item objects from the backend
var someItems = getItems();
// Update one of the items in the backend
updateItem(...);
```

이것은 충분히 직설적이어야합니다. 그러나 우리는 백엔드가 처리해야하는 대부분의 (전부는 아닐지라도) 배포에 대한 매우 중요한 세부 사항을 무시했습니다. 우리의 단순한 인터페이스는 이미 데이터를 로컬에 가지고 있다면 잘 작동 할 것입니다.하지만 데이터가 다른 곳에있는 서버에 저장된다면 어떻게 될까요? 백엔드 서버 (또는 디스크 등)가 요청한 데이터로 응답하기를 기다리면서 코드를 그냥 멈추게 할 수는 없습니다. 우리의 요청이 백엔드에 도착하고 백엔드가 데이터를 다시 전송하도록 요청하는 데는 미정의 시간이 걸릴 수 있습니다. 따라서 데이터 검색 및 업데이트는 비동기 적으로 이루어져야합니다. 그리고 자바 스크립트에서 비동기식 계산이 포함될 때 Promise 또는 그 근처에있는 두 개를 찾을 가능성이 큽니다.

약속에 대한 MDN의 기사를 바꾸어 말하면 "약속은 현재 또는 미래에 사용할 수있는 가치를 나타냅니다." 이것은 Promise가 우리에게 할 수있는 일에 대한 아주 기본적인 설명이지만, 이미이 설명에서 우리는 우리의 백엔드와 비동기 적으로 통신한다는 유즈 케이스에 부합한다는 것을 알 수 있습니다. Promises를 사용하면 일반적인 백엔드 인터페이스가 다음과 같이 보입니다.:

```
// Returns a Promise that represents an array of item objects
function getItems() { ... }
// Returns a Promise that will be fulfilled when the item is updated in the backend
function updateItem(...) { ... }
```

이것으로 우리 인터페이스가 정말로 바뀌지 않은 것처럼 보입니다! 물론, 실제로 이러한 함수를 사용하는 것은 (그리고 우리 모의 백엔드의 경우 구현하는 것) 약간 다르지만 많지는 않습니다. 예를 들어이 인터페이스를 사용하는 코드는 다음과 같습니다.

```
// Get the item objects from the backend asynchronously
var someItems = [];
getItems()
    .then(function(items) {
        someItems = items;
    })
    .catch(function(error) {
        console.log("Couldn't get items: " + error);
    });

// Update one of the items in the backend asynchronously
updateItem(...)
    .catch(function(error) {
        console.log("Couldn't update item: " + error);
    })
```

Promise를 제대로 사용하려면 비동기 코드 몇 가지를 도입해야합니다. Promise와 상호 작용하는 가장 간단한 방법은 그것이 제공하는 두 가지 함수, 즉 then과 catch를 호출하는 것입니다.

the then 함수를 호출함으로써 Promise가 수행 될 때 일어날 일을 기술 할 수 있습니다. 우리는 Promise가 충족시키는 값을 선택적으로 받아들이는 다른 함수를 전달하여이 작업을 수행합니다. 이 경우 백엔드의 항목 그러나이 인수는 선택 사항입니다. 예를 들어 updateItem 함수의 경우 Promise it returns는 업데이트가 완료되면 값을 반환하지 않습니다. Promise가 실제로 완료되었는지 여부 만 고려하므로이 경우 인수가 사용되지 않습니다.

catch 함수를 호출하여 Promise를 수행하는 동안 오류가 발생하면 어떻게되는지 설명 할 수 있습니다. 이 경우 실패의 원인에 대한 설명을 수락하는 함수를 전달할 수 있습니다. 예를 들어 실제 백엔드가 백엔드 서버에 연결할 수 없거나 인증에 실패하면 오류를보고 할 수 있습니다. 오류 감지 / 처리는 복잡한 주제이지만, 간단한 작업을 위해 이러한 세부 사항을 많이 훑어 보겠습니다. 그러나 나중에 우리는 실제 백엔드에 연결하는 경우 간단한 오류 처리기를 약속에 첨부하기를 원할 것입니다.

Promises가 제공하는 많은 기능이 있으며 매우 유용합니다. 그러나이 짧은 설명은 모의 백엔드를 만들고 사용하기 위해 알아야 할 모든 것을 명확히해야 할뿐만 아니라 나중에 실제 백엔드의 인터페이스를 grok에 추가해야합니다.

이제 여러분 중 일부는 흥미로운 것을 발견했을 것입니다. 그리고 그것은 우리가 이미 사용하고있는 Fuse의 Observables와 비슷한 약속입니다. Observable은 변경 및 관찰 할 수있는 값을 나타내며 비동기 인터페이스에도 적합합니다. 실제로 우리가 원한다면 우리의 백엔드 인터페이스를 모의하기 위해 약속 대신 Observables를 사용할 수 있습니다. 그 의미는 매우 유사합니다. 그리고 퓨즈를 특별히 목표로하는 백엔드를 제작한다면 백엔드와의 통합이 쉽기 때문에 이것이 권장됩니다! 그러나 퓨즈보다 더 많은 플랫폼을 지원하기 위해 많은 백엔드 솔루션이 구축 되었기 때문에 대신 우리에게 약속을 제공하는 인터페이스와 상호 작용할 가능성이 높습니다. 따라서 모의 사례를 마치 그런 것처럼 모의 할 것입니다. 다행스럽게도 Promises와 Observables 사이의 유사점 때문에이 두 가지 사이의 틈을 메우는 것은 나중에 간단히 알 수 있습니다.

참고 : MDN에서이 가이드의 약속에 대한 자세한 내용과 퓨즈가 구체적으로 준수하는 약속의 맛을 설명하는 A + 약속 표준을 배울 수 있습니다.
대체로 Promises를 사용하여 전형적인 JS 기반 백엔드 인터페이스가 어떻게 생겼는지에 대해 매우 근접한 결과를 얻었습니다. 따라서 모의 백엔드를 모델링하는 데 사용할 것입니다.

## 임시 모형 백엔드 구현 ##

참고 : 우리는 프로젝트에서 조금 움직이게 될 것이라는 점을 명심하십시오. 그래서 잠시 동안 컴파일이나 미리보기를 할 수는 없겠지만, 프로젝트가 끝날 때까지 백업하고 실행할 것입니다. 장.
모의 백엔드 구현을 시작하기 전에 JavaScript를 어떻게 구성 할 것인지에 대해 이야기하고자합니다. 현재 프로젝트 (물론 뷰 모델을 제외하고)에는 독립형 JS 모듈 하나만 있습니다. hikes.js 파일입니다. 더 많은 모듈을 추가 할 것이므로 우리는 프로젝트를 훌륭하고 체계적으로 유지해야합니다. 우리 페이지 폴더와 유사하게, 우리는 우리 애플리케이션의 다른 페이지들에 관한 모든 파일들을 저장했다. 우리 프로젝트의 루트에 Modules 폴더를 만들어서 우리의 모든 독립형 JS 모듈을 배치 할 것이다.:

```
.
|- MainView.ux
|- Modules
|- Pages
|  |- EditHikePage.js

...
```

다음으로 할 일은 퓨즈가이 디렉토리에있는 모듈에 대해 알고 있는지 확인하는 것입니다. 이전에는 hikes.js 파일을 프로젝트에 추가 할 때 프로젝트 파일 (hikr.unoproj)에이 파일에 대한 참조를 추가하여 응용 프로그램과 번들로 제공했습니다. Modules 폴더를 만들었으므로 이제는 해당 항목을 모듈 디렉토리의 모든 JavaScript 파일을 번들로 포함하는 항목으로 바꿉니다.

```
...

  "Includes": [
    "*",
    "Modules/*.js:Bundle"
  ]
}
```

이제 모의 백엔드 구현을 시작할 준비가되었습니다. 이전 hikes.js 파일에는 이미 제시해야 할 모든 데이터가 포함되어 있으므로이를 시작점으로 사용할 수 있습니다. 먼저 파일을 Modules 디렉토리로 옮기고 그 이름을 Backend.js로 바꿉니다.

새로운 Backend.js 파일을 살펴보면 단순히 하이킹 어레이를 그대로 내보내는 것입니다. 그러나 이것이 우리의 모의 백엔드가 될 것이므로 이전에 논의한 인터페이스와 유사한 인터페이스를 노출시켜야합니다. 이것은 다음과 같이 보일 것입니다 :

```
// Returns a Promise that represents an array of hike objects
function getHikes() { ... }
// Returns a Promise that will be fulfilled when the hike is updated in the backend
function updateHike(...) { ... }
```

We'll create our getHikes function first. Below our hikes array, we'll start with an empty function:

```
function getHikes() {
}
```

If we wanted this function to return our hikes array, we could simply write something like this:

```
function getHikes() {
    return hikes;
}
```

대신 우리 함수가 Promise를 반환하기를 원합니다.이 약속은 하이킹이 백엔드에서 가져올 시뮬레이션을 준비 할 때 수행됩니다. 그래서 우리의 실제 getHikes 함수는 다음과 같이 보일 것입니다 :

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        resolve(hikes);
    });
}
```

이제 하이킹 배열을 반환하는 대신 새로운 Promise를 사용하여 Promise를 만듭니다. Promise 생성자는 약속을 수행하거나 거부하기 위해 호출 될 함수를 사용합니다. 이 함수는 resolve와 reject의 두 인수를 취합니다. 이러한 인수는 실제로는 함수 자체입니다. resolve는 우리가 만들고있는 Promise를 수행하는 함수이며, then 함수를 사용하여 Promise에 핸들러를 연결하면 해당 핸들러가 호출됩니다. 우리 코드에서는 Promise를 하이킹 컬렉션으로 해결하기 위해 호출해야하는 모든 기능이 있습니다. 오류가 발생하고 catch를 통해 Promise에 첨부 된 핸들러가 이후에 호출되면 reject 함수가 대신 호출 될 수 있습니다.

JS의 내장 setTimeout 함수를 사용하여 실제로 밀리 초 단위로 처리가 완료 될 때 연기 할 수 있습니다. setTimeout은 두 개의 인수도 취합니다. 첫 번째 함수는 미래에 언젠가 호출 될 함수이고, 두 번째 함수는 해당 함수를 호출하기 전에 지연되는 시간 (밀리 초)입니다. 예를 들어,이 코드는 30 초 후에 Promise를 해결합니다.:

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(hikes);
        }, 500);
    });
}
```

이 코드는 우리의 앱이 백엔드에서 오는 데이터를 기다릴 필요가있는 방법을 테스트하고자 할 때 매우 유용합니다. 그러나, 테스트하는 동안 스스로를 단순하게 유지하려면 지연 대신 0을 사용하십시오.

```
function getHikes() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(hikes);
        }, 0);
    });
}
```

완벽한 인터페이스와 적절한 시뮬레이션 지연을 가진 멋진 getHikes 함수가 있습니다!

이제 updateHike 함수를 사용하십시오. 이것은 모의 백엔드에서 업데이트 할 특정 하이킹에 대한 정보를 취하고 업데이트가 완료되면 완료 될 Promise를 반환하는 함수입니다. 우리는 빈 함수로 시작합니다 :

```
function updateHike() {
}
```

그런 다음 특정 하이킹을 식별하고 업데이트하기위한 몇 가지 인수를 추가합니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
}
```

이 경우 id 인수를 사용하여 업데이트 할 하이킹을 식별하고 나머지 인수는 해당 하이킹의 해당 필드를 모두 덮어 쓰게됩니다.

다음으로 Promise 생성자와 setTimeout을 getHikes처럼 사용하여 선택적인 시간 지연으로 Promise를 반환합니다.:

```
function updateHike(id, name, location, distance, rating, comments) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
        }, 0);
    });
}
```

좋아 보여! 이제 ID로 하이킹을 실제로 식별하고 멤버를 업데이트하는 코드를 추가합니다. 일을 간단하게하기 위해, 우리는 우리가 찾고있는 하이킹을 찾기 위해 간단한 선형 검색을 할 것입니다.:

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

하이킹이 확인되면 함수의 인수로 데이터를 업데이트하고 검색 루프를 빠져 나옵니다.:

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

거의 끝났어. 마지막으로 하이킹 개체가 업데이트 된 후에 Promise를 해결할 것입니다. 우리가 만드는 약속은 어떤 데이터도 반환하지 않으므로 매개 변수없이 resolve 만 호출하면됩니다.

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

거의 끝났어. 마지막으로 하이킹 개체가 업데이트 된 후에 Promise를 해결할 것입니다. 우리가 만드는 약속은 어떤 데이터도 반환하지 않으므로 매개 변수없이 resolve 만 호출하면됩니다.:

```
module.exports = {
    getHikes: getHikes,
    updateHike: updateHike
};
```

그리고 그걸로 모의 백엔드가 완성되었습니다!

## Our Context abstraction ##

earler를 언급하면서 우리는 뷰 모델이 우리 모의 백엔드와 직접 상호 작용할 수있었습니다. 기술적으로는 아무 문제가 없지만, 뷰 모델이 대신 상호 작용하는 백엔드 위에 작은 추상화 레이어를 만드는 것이 유리합니다. 이렇게하면 모델에보다 일관된 인터페이스를 제공 할 수 있으며 캐싱과 같은 기능을 구현하여 앱에서 사용하는 대역폭과 배터리 양을 줄일 수 있습니다. 따라서 우리가 할 다음 작업은 Context라고 부르는 추상화를 만드는 것입니다.

Context 모듈을 만들려면 Modules 디렉토리에서 Context.js라는 새 파일을 만듭니다.

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

이 파일에서 FuseJS Observable 모듈을 가져옵니다.

```
var Observable = require("FuseJS/Observable");
```

또한 백엔드 모듈을 다음과 같이 가져옵니다.

```
var Observable = require("FuseJS/Observable");
var Backend = require("./Backend");
```

백엔드 모듈을 ./Backend로 가져온 방법에 유의하십시오. 일반적으로 앱에 번들로 제공되는 JS 모듈을 가져올 때 모듈을 해결하기 위해 require 표현식을 사용하고 프로젝트의 루트 디렉토리에 상대적인 경로를 지정합니다. 그러나 상대 경로를 사용하여 모듈을 확인할 수도 있습니다. 이 경우 정확히 그게 우리가하는 일입니다. 우리는 Backend.js가 Context.js 파일과 같은 디렉토리 인 Modules 디렉토리에 있다는 것을 알고 있습니다. 따라서 ./Backend를 사용하면 문제가 없습니다.

이제 Context 모듈의 고기를 위해. 컨텍스트는 우리의 뷰 모델에 간단한 인터페이스를 제공하여 앱의 데이터를 쉽게 소비하고 수정할 수 있도록해야합니다. 우리는 뷰가 결국 데이터 바인딩을 통해 뷰 모델의 데이터를 표시 할 것이므로 컨텍스트는 하나 이상의 Observables를 통해 데이터를 노출하는 것이 이상적입니다. 그래서, 우리는 간단한 인상으로 시작할 것입니다 Observable :

```
var hikes = Observable();
```

이 Observable은 뷰 모델에서 사용 가능한 모든 하이킹을 표시하는 데 사용됩니다. 앱이 시작되면 우리는 백엔드의 데이터를 사용하여 Observable을 채 웁니다. 앱이 실행될 때이 컬렉션은 본질적으로 백엔드에서 같은 컬렉션의 로컬 미러가됩니다. 변경 사항을 적용하면 뷰 모델이 즉시 업데이트되도록이 컬렉션의 내용을 업데이트합니다. 또한 백엔드와 통신하여 정보를 비동기 적으로 업데이트 할 수 있습니다.

우리의 응용 프로그램이 시작되면, 우리는 백엔드 모듈의 getHikes 함수를 호출하고 그것을 반환하는 Promise를 사용하여 초기 데이터로 관찰 가능 항목을 채 웁니다.:

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

우리의 약속에서 replaceAll 함수를 사용하는 방법에 주목하여 Promise가 수행 될 때 얻을 수있는 배열의 내용으로 내용을 덮어 씁니다. 우리는 또한 좋은 측정을 위해 작은 오류 처리기를 첨부했습니다. 시작시 백엔드의 초기 데이터로 관찰 할 수있는 하이킹을 처리합니다.

다음으로 뷰 모델이 데이터를 업데이트하는 메커니즘을 제공해야합니다. 모의 백엔드와 마찬가지로, 업데이트 할 하이킹을 식별하는 id와 업데이트 할 데이터를 취하는 updateHike 함수를 제공 할 수 있습니다. 이 함수는 Observable (추가 검색을 사용하여 간단하고 일관성있게 유지하는 데 사용되는 백엔드) 로컬 업데이트를 업데이트하고 백엔드에 데이터 업데이트를 지시합니다.:

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

마지막으로 다음과 같이 모듈의 내보내기에서 인상과 updateHike를 노출합니다.:

```
module.exports = {
    hikes: hikes,

    updateHike: updateHike
};
```

마지막으로 다음과 같이 모듈의 내보내기에서 인상과 updateHike를 노출합니다.

## Hooking everything up ##

이제 백엔드 및 컨텍스트 모듈을 설정 했으므로 이전에 가지고 있던 하이킹 모듈 대신에 뷰 모델을 사용하여 리팩터링 할 때입니다.

우리는 HomePage를 마이그레이션함으로써 시작할 것입니다. 이 페이지에는 이전 하이킹 모듈의 하이킹 목록 만 표시되었으므로 매우 간단합니다. 우리는 Pages / HomePage.js에서 몇 가지 변경 만하면됩니다. 첫째, 우리가 HomePage.js를 살펴 본다면 첫 번째 라인은 오래된 하이킹 모듈을 가져옵니다. 대신 컨텍스트 모듈을 가져 오도록 변경하십시오.

```
var Context = require("Modules/Context");
```

우리가해야 할 또 다른 유일한 일은 모듈 내보내기에서 Context.hikes를 참조하도록 하이 레벨 참조를 변경하는 것입니다.

```
module.exports = {
    hikes: Context.hikes,

    goToHike: goToHike
};
```

그리고 그것은 우리의 홈페이지를 커버해야합니다.

이제 EditHikePage를 사용하십시오. 이 페이지는 라우터에서 하이킹 데이터를 받기 때문에이 페이지는이 데이터를 표시하기 위해 어떤 변경도 필요하지 않습니다. 그러나 실제로 데이터를 편집하기 전에 몇 가지 사항을 변경해야합니다. 그러나 의미있는 일을하기 위해 마지막 장에서 만든 임시 Back 단추 대신 원래 디자인에서 Save 및 Cancel 단추를 설정하려고합니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/9ad834ec45583c3d17817e1973f50449__media/hikr/tutorial/app-flow.webp)


저장 버튼부터 시작하겠습니다. 이 버튼은 이전 페이지로 돌아갈뿐만 아니라 편집기에서 변경 한 내용을 데이터 모델에 커밋한다는 점을 제외하고 이전에 만든 뒤로 버튼과 거의 동일합니다. 이제는 기존의 뒤로 버튼의 이름을 저장으로 바꾸는 것으로 시작하겠습니다.

먼저 Pages / EditHikePage.ux에서 버튼 텍스트와 클릭 핸들러를 모두 변경합니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Button Text="Save" Clicked="{save}" />
        </StackPanel>
```

다음으로 Pages / EditHikePage.js에서 GoBack 핸들러의 이름을 저장하도록 바꿉니다.:

```
function save() {
    router.goBack();
}
```

그리고 모듈의 내보내기에서도 이름을 업데이트 할 것입니다.:

```
    rating: rating,
    comments: comments,

    save: save
};
```

마지막으로,이 버튼은 뷰에서 수행 한 모든 편집을 커밋합니다. 이를 위해 Context 모듈의 updateHike 함수를 호출하여 우리 하이브 Observable의 값과 뷰 모델의 Observables에 포함 된 데이터를 전달합니다.:

```
function save() {
    Context.updateHike(hike.value.id, name.value, location.value, distance.value, rating.value, comments.value);
    router.goBack();
}
```

큰! 이제 저장 버튼을 모두 연결해야합니다. 이제 취소 버튼도 구현해 보겠습니다. 취소 단추는 저장 단추와 매우 비슷합니다. 편집기에서 변경 한 내용을 어떻게 든 취소해야합니다. 그러나 우리가 그 세부 사항에 대해 걱정하기 전에, 취소 버튼과 해당 취소 핸들러를 만들어 보겠습니다.

먼저 Save 버튼에 대해 작성한 코드 바로 아래에있는 Pages / EditHikePage.ux에있는 버튼의 UX 코드를 추가합니다.:

```
            <Button Text="Save" Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
        </StackPanel>
```

그런 다음 빈 취소 기능을 추가하고 Pages / EditHikePage.js로 내 보냅니다.:

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

그런 다음 저장 핸들러처럼 Router 인스턴스에서 goBack을 호출하여 cancel 함수가 이전 페이지로 돌아갈 수 있는지 확인합니다.:

```
function cancel() {
    router.goBack();
}
```

마지막으로, 핸들러가 이전 페이지로 돌아 가기 전에 뷰 모델에서 변경 한 사항을 되돌리고 싶습니다. 우리가이 작업을 수행 할 수있는 몇 가지 방법이 있지만, 가장 쉬운 방법 중 하나는 뷰 모델에있는 모든 후원 Observables가 EditHikePage의 관찰 가능한 Observable지도의 결과라는 점을 이용하는 것입니다. 이 때문에 Observable의 값을 "새로 고침"하면 모든 편집기 값이 원래 값으로 재설정됩니다. 이는 다음과 같이 하이브 Observable의 값을 그 자체에 할당함으로써 간단히 달성 할 수 있습니다 :

```
function cancel() {
    hike.value = hike.value;
    router.goBack();
}
```

쉬운! 자,이 코드는 지금 우리에게 의미가 있지만, 코드를 읽지 않은 사람 (또는이 트릭에 대해 잊어 버린 경우에는 미래에 우리)에게 왜 이것을하고 있는지 분명하지 않을 수 있습니다. 계속 진행하고 댓글을 달아주세요.:

```
function cancel() {
    // Refresh hike value to reset dependent Observables' values
    hike.value = hike.value;
    router.goBack();
}
```

완전한! 이제 모든 것이 연결되어야합니다. 이 시점에서 우리는 서로 다른 파일을 모두 저장할 수 있습니다. 퓨즈가 미리보기를 다시 읽으면 마침내 우리는 완전한 기능의보기를 시험해 볼 수 있습니다!

## Our progress so far ##

마지막으로, 우리는 기본적으로 우리 앱의 주요 기능 부분을 모두 갖추고 있으며 연결되어 있습니다. 이제 우리는 다양한 페이지와 모듈을 완벽하게 조화롭게 연동시켜 멋지고 확장 가능한 아키텍처를 구현했습니다. 다음과 같이 보입니다.:

![https://res.cloudinary.com/fusetools/image/upload/documentation_v2/3e1775cc8b7d386f31a387a5b250e58b__media/hikr/chapter-5/chapter-5.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/3e1775cc8b7d386f31a387a5b250e58b__media/hikr/chapter-5/chapter-5.mp4)

이 장에서 수정 한 다양한 파일의 코드는 다음과 같습니다.:

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

이제 앱의 주요 부분이 모두 갖추어 졌으므로 앱을 돋보이게 만들기 위해 앱의 모양과 느낌을 반복적으로 조정해야 할 때입니다. 다음 장에서는 사용자 정의 모양 / 느낌으로 다양한 재사용 가능한 구성 요소를 빌드하고 응용 프로그램을 멋지게 꾸미기 위해 응용 프로그램 전체에 뿌립니다. 그래서 그것에 가자!

이 장의 마지막 코드는 여기에서 볼 수 있습니다.
