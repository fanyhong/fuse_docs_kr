원본: https://www.fusetools.com/docs/fuse/selection/selection

선택 수업

 고급 기능 표시
이동 :
목차
예제들
이 엔티티는 실험적이며 이후 릴리스에서 변경되거나 제거 될 수 있습니다.
Selection은 항목 목록, 라디오 단추 또는 선택기와 같은 선택 컨트롤을 만드는 데 사용됩니다. Selection 자체는 선택을 정의하고 고급 동작을 관리하며 현재 값을 추적합니다. 다양한 선택 가능한 객체는 선택할 수있는 항목을 정의합니다.

선택 항목은 노드가 나타나는 노드와 연관됩니다. 예 :

<패널>
    <선택 />
</ Panel>
패널은 이제 선택 컨트롤로 간주됩니다. 이 패널의 하위 항목 인 선택 가능 및 선택 항목과 같은 동작 및 트리거는이 선택 동작을 찾습니다.

Selection API의 기능은 사용자 상호 작용 API와 프로그래밍 API로 나뉩니다. 사용자 상호 작용 기능은 MaxCount 및 MinCount와 같이 Selection의 요구 사항에 따라 제한됩니다. 프로그래밍 방식의 함수는 원하는 상태로 설정할 수 있습니다. 그들은 제약받지 않습니다. 따라서 초기화 순서를 염려하지 않고도 값 바인딩 및 JavaScript 기반 동작을 쉽게 만들 수 있습니다.

선택 인터페이스

MaxCount : int UX
사용자가 선택할 수있는 최대 항목 수입니다. 그들이 더 많은 항목을 선택하려고하면 교체가 어떻게되는지 결정합니다.
MinCount : int UX
사용자가 선택할 수있는 항목의 최소 수입니다. 그들이 선택을 취소하고 항목을 시도하고이 수를 밑도는 경우 선택 취소가 무시됩니다.
대체 : SelectionReplace UX
사용자가 MaxCount를 초과하는 항목을 선택할 때 수행 할 작업을 지정합니다.
SelectionChanged : EventHandler (객체, EventArgs) UX
선택 상태가 변경 될 때마다 발생합니다.
값 : 문자열 UX
현재 선택된 항목의 문자열 값입니다. 여러 항목을 선택하면 선택된 가장 오래된 항목의 값이됩니다.
값 : object UX
선택한 값의 현재 목록입니다. 선택한 항목에 대한 양방향 인터페이스를 만들려면 JavaScript의 Observable 배열에 바인딩되어야합니다.
노드에서 상속됩니다.
findData (기호) JS
노드의 데이터 컨텍스트에서 주어진 심볼에 대한 데이터의 관찰 가능 항목을 반환합니다.
바인딩 : IList Binding의 IList
이 노드에 속하는 바인딩 목록입니다.
이름 : Selector UX
노드의 런타임 이름. 이 등록 정보는 ux : Name 속성을 사용하여 자동으로 설정됩니다.
첨부 된 UX 속성
GlobalKey (리소스에 의해 첨부 됨) : string UX
ux : Global 속성은 UX 마크 업에서 모든 곳에서 액세스 할 수있는 글로벌 리소스를 만듭니다.
예제들

다음 예제에서는 Selection을 사용하여 간단한 옵션 목록을 만듭니다. 항목을 눌러 선택 항목을 토글합니다. 값은 현재 선택된 항목을 추적하기 위해 JavaScript Observable에 바인딩됩니다.

<Panel ux : Class = "MyItem"Color = "# aaa">
    <string ux : Property = "Label"/>
    <string ux : Property = "Value"/>

    <선택 가능 값 = "{ReadProperty this.Value}"/>
    <텍스트 값 = "{ReadProperty this.Label}"/>

    <WhileSelected>
        <변경 this.Color = "# ffc"/>
    </ WhileSelected>
    <태핑>
        <ToggleSelection />
    </ Tapped>
</ Panel>

<자바 스크립트>
    var Observable = require ( "FuseJS / Observable")
    exports.values ​​= Observable ()

    exports.list = 관찰 가능 ( "")
    exports.values.onValueChanged (module, function () {
        exports.list.value = exports.values.toArray (). join ( ",")
    })
</ JavaScript>
<StackPanel>
    <선택 값 = "{값}"/>

    <MyItem Label = "Big Red One"값 = "sku-01"/>
    <MyItem Label = "작은 초록색 2"값 = "sku-02"/>
    <MyItem Label = "셋째 마지막 값"= "sku-03"/>
    <MyItem Label = "For Four For"Value = "sku-04"/>
    <MyItem Label = "Point Oh-Five"Value = "sku-05"/>

    <텍스트 값 = "선택됨 :"여백 = "0,10,0,0"/>
    <텍스트 값 = "{list}"/>
</ StackPanel>
