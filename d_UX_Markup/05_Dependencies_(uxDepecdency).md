원본: https://www.fusetools.com/docs/ux-markup/dependencies

종속성 (ux : Dependency)

UX Markup의 ux : Dependency 속성은 생성 가능한 ux : Class를 포함하는 새로운 하드 종속성 (Uno 생성자 인수)을 정의합니다.

통사론

<type ux : Dependency = "dependency_name"/>
여기서 type은 UX 마크 업에서 액세스 할 수있는 모든 유형이며 depdendency_name은 유효한 Uno 식별자입니다.

비고

구성 요소 (ux : Class로 정의 됨)는 작업 환경에서 특정 객체 나 서비스에 대한 액세스가 필요할 수 있습니다. 예를 들어, 구성 요소가 App의 라우터에 액세스해야 할 수 있습니다.

다음과 같이 ux : Dependency 속성을 사용하여 구성 요소에 종속성을 선언 할 수 있습니다.

<Panel ux : Class = "MyBackButton">
    <라우터 ux : 종속성 = "라우터"/>
    <Panel ux : Dependency = "panel"/>

    <자바 스크립트>
        function clicked () {
            router.goBack ();
        }

        module.exports = {클릭 된 : 클릭};
    <자바 스크립트>

    <클릭>
        <콜백 핸들러 = "{clicked}">
    </ Clicked>

    <잠시 후>
        <패널 변경 .Opacity = "0.5"Duration = "0.3"/>
    </ WhilePressed>
</ Panel>
위의 예는 router와 panel의 두 종속 관계를 선언합니다. 구성 요소를 클릭하면 라우터가 .goBack ()에 사용됩니다. 패널을 더빙 한 패널은 구성 요소를 누른 상태에서 반투명으로 사라집니다.

종속성은 Uno의 생성자 인수와 동일하며 읽기 전용 필드에 저장됩니다. 이것은 객체가 초기화시에 항상 알려지며 절대로 변경되지 않기 때문에, 우리는 JavaScript 나 애니메이터에서 객체를 해당 이름으로 Change와 같이 안전하게 사용할 수 있음을 의미합니다.

종속성이있는 구성 요소를 인스턴스화 할 때 각 종속성 (즉, 종속성 주입)에 대해 객체를 제공해야합니다. 그렇지 않으면 컴파일 시간 오류가 생성됩니다.

<App>
    <라우터 ux : Name = "라우터"/>
    <패널 ux : Name = "p1"/>
    <MyBackButton router = "router"panel = "p1"/>
</ App>
구성 요소는 종속성에 대한 기본값을 제공 할 수 없습니다.

종속성 상속

하위 클래스를 만들 때 종속성은 전달되지 않습니다. 따라서 서브 클래 싱하는베이스 클래스로 수동으로 전달해야합니다.

<Page ux : Class = "A">
    <라우터 ux : 종속성 = "라우터"/>
</ Page>
<A ux:Class="B">
    <라우터 ux : 종속성 = "라우터"ux : 바인딩 = "라우터"/>
</A>
ux는 어떻습니까? ux : Dependency는 Property와 다릅니다.

종속성은 속성과 비슷하게 작동하지만 몇 가지 주요 차이점이 있습니다.

종속성은 변경할 수 없으므로 시간이 지남에 따라 값이 변하지 않습니다.
종속성에는 인스턴스화시 값이 제공되어야합니다.
종속성은 기본값을 가질 수 없습니다.
종속성은 <JavaScript> 태그 (Observables가 아님)의 로컬 명명 된 객체 direclty로 사용할 수 있습니다.
종속성은 클래스 범위에서 로컬 이름으로 사용할 수 있습니다. 즉, {Property foo}와 같은 바인딩을 사용하여 액세스하지 않아도되므로 foo를 사용하면됩니다.
