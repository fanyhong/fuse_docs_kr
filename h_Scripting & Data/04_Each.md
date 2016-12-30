원본: https://www.fusetools.com/docs/fuse/reactive/each

각 클래스

 고급 기능 표시
이동 :
목차
각 항목에 대해 지정된 템플릿을 사용하여 개체 모음을 표시합니다.

각 태그의 자식 요소는 Items 속성에 지정된 컬렉션의 각 항목에 대해 "투영"될 템플릿을 나타냅니다. 그러면 투영 된 항목이 해당 인스턴스의 데이터 컨텍스트가되므로 컬렉션을 명시 적으로 인덱싱하지 않고 항목 자체를 기준으로 데이터 바인딩을 지정할 수 있습니다.

각자가 투영 한 각 하위 트리는 자체 범위에 존재합니다. 즉, 각각의 자녀는 그 외부에서 액세스 할 수 없습니다. 그러나 내부에서 선언 된 외부 노드에 액세스 할 수 있습니다.

예

<자바 스크립트>
    module.exports = {
        항목 : [
            {name : "Jake", 나이 : 24},
            {name : "Julie", 나이 : 25},
            {name : "Jerard", 나이 : 26}
        ]
    };
</ JavaScript>
<StackPanel>
    <각 항목 = "{items}">
        <StackPanel>
            <텍스트 값 = "{name}"/>
            <텍스트 값 = "{연령}"/>
        </ StackPanel>
    </ 각>
</ StackPanel>
각각을 ux : Template와 함께 사용하기

사용자 정의 구성 요소에서 각각을 사용하는 경우, 각 구성 요소가 사용하는 기본 템플리트 대신 사용할 수있는 사용자 정의 템플리트 오브젝트를 가져 와서 해당 구성 요소의 가용성을 높일 수 있습니다. 이렇게하려면 두 가지 작업을 수행해야합니다.

TemplateSource 속성에 템플릿을받을 수있는 요소를 지정합니다 (사용자 지정 구성 요소의 경우 사용자 지정 구성 요소의 클래스)
TemplateKey 속성을 사용하여 각각이 찾고자하는 템플릿 이름을 지정하십시오.
템플릿을 지정하지 않으면 각 템플릿의 자식 요소가 사실상 템플릿으로 사용됩니다.

예

다음 예제에서는 사용자 정의 템플릿을 클래스에 전달하여 각 템플릿을 사용할 수 있음을 보여줍니다.

<StackPanel ux : Class = "CoolRepeater"Background = "# FAD">
    <각 TemplateSource = "this"TemplateKey = "Item"Count = "20">
        <텍스트> 템플릿이 제공되지 않습니다. </ Text>
    </ 각>
</ StackPanel>
<CoolRepeater>
    <Text ux : Template = "Item"> 안녕하세요! </ Text>
</ CoolRepeater>
"Hello, world!"를 제거하면주의해야합니다. 텍스트는 사용자 정의 템플리트이며, 각각은 하위를 템플리트로 사용합니다.

각 인터페이스
