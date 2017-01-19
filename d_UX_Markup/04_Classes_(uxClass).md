원본: https://www.fusetools.com/docs/ux-markup/classes

클래스 (ux : 클래스)

ux : Class 속성은 XML 요소의 유형이 기본 클래스 인 새로운 UX (Uno) 클래스를 정의합니다. 즉, 새 클래스는 수퍼 클래스의 모든 공용 속성, 이벤트 및 동작을 상속받습니다.

UX 마크 업 요소의 모든 트리는 ux : Class 속성을 사용하여 쉽게 구성 요소로 변환 할 수 있습니다. 클래스에는 구성 요소의 내부 비즈니스 로직을 관리하는 JavaScript 태그가 포함될 수 있습니다.

통사론

<base_class ux : Class = "class_name"/>
여기서 base_class는 UX Markup에 액세스 할 수있는 클래스이고 class_name은 정규화 된 클래스 이름입니다 (필요한 경우 네임 스페이스 경로 포함).

예

다음 예제에서는 MyCheckBox라는 새 클래스를 정의합니다.이 클래스는 선택하지 않은 상태에서 파란색이고 선택하면 빨간색입니다. 확인란을 눌러 상태를 변경합니다.

<Panel ux : Class = "MyCheckBox"Color = "Blue">
    <bool ux : Property = "Value"/>
    <자바 스크립트>

        function toggle () {
            this.Value.value =! this.Value.value;
        }

        module.exports = {토글 : 토글};

    </ JavaScript>

    <While True 값 = "{Property Value}"/>
        <변경 사항 .Color = "Red"Duration = "0.2"/>
    </ WhileTrue>
    <태핑>
        <콜백 처리기 = "{토글}"/>
    </ Tapped>
</ Panel>
왜 수업을 사용합니까?

퓨즈는 몇 가지 이유로 앱을 구성 요소 (클래스)로 분리하도록 권장합니다.

올바른 방법 - 구성 요소 지향은 코드 기반을 깨끗하고 테스트 가능하며 확장 가능하고 유지 보수하기 쉽도록 유지합니다.
재사용 - 여러 위치에서 UI 및 로직을 재사용 할 수 있도록 구성 요소를 만드는 것이 유용합니다.
스타일링 - 프로젝트 전체에서 일관된 모양과 느낌을 만들기 위해서는 프리미티브를 기반으로 새로운 클래스를 만드는 것이 좋습니다.
InnerClass (ux : InnerClass)

ux : InnerClass는 ux : Class의 특별 버전입니다. ux : Class와 마찬가지로 새로운 UX (Uno) 클래스를 정의하고 기본 클래스의 모든 속성을 상속받습니다. ux : InnerClass의 특별한 점은 부모 범위의 이름에 액세스 할 수 있다는 것입니다. 이는 부모 범위 내에 만 인스턴스를 만들 수 있음을 의미합니다.

참고 : InnerClass는 처음에는 매우 편리한 기능처럼 보일 수 있지만 구성 요소의 긴밀한 결합으로 이어질 수 있으므로 사용시주의해야합니다. 대부분의 경우 ux : Class를 사용하는 것이 좋습니다.
통사론

<base_class ux : InnerClass = "class_name"/>
예

다음 예제에서 MyInnerClass의 클래스 정의 외부에 정의 된 toggleStatusPanel에 액세스합니다. 우리는 이것을 ux : Class 대신에 ux : InnerClass를 사용하여 선언하기 때문에 이것을 할 수 있습니다.

<App>
    <DockPanel>
        <Panel ux : Name = "statusPanel"Color = "# f00"Height = "80"Dock = "Top"/>
        <WhileTrue ux : Name = "toggleStatusPanel">
            <Change statusPanel.Color = "# 0f0"/>
        </ WhileTrue>

        <패널 ux : InnerClass = "MyInnerClass"Height = "80"Color = "Blue"Margin = "5">
            <클릭>
                <Toggle Target = "toggleStatusPanel"/>
            </ Clicked>
        </ Panel>

        <StackPanel>
            <MyInnerClass />
            <MyInnerClass />
            <MyInnerClass />
            <MyInnerClass />
        </ StackPanel>
    </ DockPanel>
</ App>
비고

ux : InnerClass는 퓨즈에서 매우 특별한 경우의 구성 요소를 포함하므로 일반적으로 필요하지 않습니다. ux : Property 및 ux : Dependency를 사용하여 구성 요소 안팎에 명시 적 인터페이스를 만드는 것이 좋습니다.
