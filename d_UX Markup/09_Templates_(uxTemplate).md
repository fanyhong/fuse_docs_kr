원본: https://www.fusetools.com/docs/ux-markup/templates

템플릿 (ux : 템플릿)

UX Markup의 ux : Template 속성은 XML 요소에 배치 될 때 지정된 XML 요소와 전체 하위 트리가 템플리트 (Uno.UX.Template)임을 지정합니다. 템플릿은 런타임에 UX 마크 업 트리 복사본을 임의로 생성 할 수 있습니다.

통사론

<type ux : Template = "template_name"/>
여기서 type은 유효한 UX 마크 업 유형이고 template_name은 유효한 Uno 식별자입니다.

비고

템플릿은 다음과 같은 몇 가지 상황에서 사용됩니다.

<Each> repeater 내부에서 컨텐츠 노드는 암시 적으로 템플릿입니다.
<Navigator> 내에서 일반 노드 대신 페이지 템플리트를 제공하여 Navigator가 필요할 때마다 각 페이지 템플리트의 새 사본을 작성할 수 있습니다.
ux : 템플릿 인스턴스의 이름

ux : Template로 표시된 노드 내부에서, 현재 템플리트의 현재 사본 (인스턴스)은 ux : Name을 사용하여 같은 방법으로 명명 된 것과 같은 방식으로 template_name에 의해 참조 될 수 있습니다.

예를 들어, 포함 된 <JavaScript> 태그에서 템플릿 인스턴스를 이름으로 참조 할 수 있습니다.

<Page ux : Template = "homePage">
    <자바 스크립트>
        homePage.Parameter.onValueChanged (module, function () {...
또한 일반적으로 UX 마크 업에서 표현식에 페이지 이름을 사용할 수 있습니다.

<Page ux : Template = "homePage">
    <Rectangle LayoutMaster = "homePage"... />
구성 요소 사용자 정의 가능성을 위해 템플릿 사용

템플릿을 사용하여 모양에 사용되는 사용자 정의 요소를 가져 와서 구성 요소의 사용자 정의 가능성을 높일 수 있습니다. 예를 들어 PageIndicator 요소는 PageControl의 각 페이지에 대한 템플릿에서 요소를 생성합니다. 요소는 ux : Template 속성을 해당 템플릿을 식별 할 키로 설정하여 템플릿으로 만듭니다. 이 키는 적절한 템플릿을 찾을 때 템플릿 수용 요소가 사용합니다.

템플릿은 각각을 사용하여 그려집니다. 각각은 TemplateSource라는 속성을 가지고 있는데, 이는 Each가 템플릿을 가져 오는 요소를 지정합니다. 앞서 언급했듯이 템플릿은 키를 사용하여 자신을 식별합니다. 키 각 키는 TemplateKey 속성을 사용하여 지정된 템플릿을 찾습니다.

일반적인 TemplateSource는 ux입니다 : 각각의 자식 인 Class :

<StackPanel ux : Class = "CoolRepeater"Background = "# FAD">
    <각 TemplateSource = "이"TemplateKey = "항목"개수 = "20"/>
        <텍스트> 기본 템플릿 </ Text>
    </ 각>
</ StackPanel>
이 템플릿은 20 번 반복되는 템플릿을 허용하는 맞춤 구성 요소입니다. 템플릿이 제공되지 않으면 각 템플릿은 기본 템플릿으로 폴백됩니다. 그런 다음 사용자 지정 구성 요소를 다음과 같이 사용할 수 있습니다.

<CoolRepeater>
    <Text ux : Template = "Item"> 안녕하세요! </ Text>
</ CoolRepeater>

