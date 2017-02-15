원본: https://www.fusetools.com/docs/ux-markup/classes

# 클래스 (ux:Class) #

`ux:Class` 속성은 XML 요소 타입이 *기본 클래스(base class)* 인 새로운 UX (Uno) 클래스를 정의합니다. 즉, 새 클래스는 수퍼 클래스로부터 모든 공용 속성들, 이벤트들 및 동작들을 상속받습니다.

UX 마크업 요소들의 모든 트리는 `ux:Class` 속성을 사용하여 쉽게 컴포넌트로 변환할 수 있습니다. 클래스들은 컴포넌트의 내부 비즈니스 로직을 관리하는 [JavaScript](https://www.fusetools.com/docs/fuse/reactive/javascript) 태그들을 포함할 수 있습니다.

## 구문 (Syntax) ##

```
<base_class ux:Class="class_name" />
```

여기서 `base_class` 는 UX 마크업에 액세스 할 수 있는 `class` 이고, `class_name` 은 fully qualified 클래스 이름(필요한 경우 `.` 으로 구분된 네임 스페이스(namespace) 경로를 포함하는) 입니다.

## 예제 (Examples) ##

다음 예제에서는 선택되지 않으면 파란색이고 선택되면 빨간색이 되는 `MyCheckBox` 라는 새 클래스를 정의합니다. checkbox 를 탭해서 상태를 변경합니다.

```
<Panel ux:Class="MyCheckBox" Color="Blue">
    <bool ux:Property="Value" />
    <JavaScript>

        function toggle() {
            this.Value.value = !this.Value.value;
        }

        module.exports = { toggle: toggle };

    </JavaScript>

    <WhileTrue Value="{Property Value}" />
        <Change this.Color="Red" Duration="0.2" />
    </WhileTrue>
    <Tapped>
        <Callback Handler="{toggle}" />
    </Tapped>
</Panel>
```

## 왜 클래스(class)를 사용합니까? ##

fuse 는 몇 가지 이유로 앱을 컴포넌트(클래스)로 분리하는 것을 권장합니다.

- 좋은 훈련 - 컴포넌트 지향은 코드 기반을 깨끗하고, 테스트 가능하며 확장 가능하고 유지 보수하기 쉽도록 유지합니다.
- 재사용 - 다양한 위치에서 UI 및 로직을 재사용 할 수 있도록 하기 위해서는 컴포넌트를 만드는 것이 유용합니다.
- 스타일링 - 프로젝트 전체에서 일관된 룩앤필(모양과 느낌)을 만들기 위해서는 원시타입(primitives)들을 기반으로 새로운 클래스들을 만드는 것이 좋습니다.

## InnerClass (ux:InnerClass) ##

`ux:InnerClass` 는 `ux:Class` 의 특별한 버전입니다. `ux:Class` 와 마찬가지로, 새로운 UX (Uno) 클래스를 정의하고 *기본 클래스(base class)* 의 모든 속성들을 상속받습니다. `ux:InnerClass` 의 특별한 점은 부모 범위(scope)의 이름에 액세스 할 수 있다는 것입니다. 이는 또한 여러분이 부모 범위(scope) 내에만 인스턴스를 만들수 있음을 의미합니다.

> 참고: `InnerClass` 는 처음에는 매우 편리한 기능처럼 보일 수 있지만, 컴포넌트들의 타이트한 결합으로 이어질 수 있으므로 사용시 주의해야 합니다. 대부분의 경우 `ux:Class` 를 사용하는 것이 좋습니다.

## 구문 (Syntax) ##

```
<base_class ux:InnerClass="class_name" />
```

## 예제 (Example) ##

다음 예제에서 `MyInnerClass` 의 클래스 정의 밖에 정의된 `toggleStatusPanel` 에 액세스 합니다. `ux:Class` 대신에 `ux:InnerClass` 를 사용해 선언하기 때문에 이렇게 할 수 있습니다.

```
<App>
    <DockPanel>
        <Panel ux:Name="statusPanel" Color="#f00" Height="80" Dock="Top"/>
        <WhileTrue ux:Name="toggleStatusPanel">
            <Change statusPanel.Color="#0f0" />
        </WhileTrue>

        <Panel ux:InnerClass="MyInnerClass" Height="80" Color="Blue" Margin="5">
            <Clicked>
                <Toggle Target="toggleStatusPanel" />
            </Clicked>
        </Panel>

        <StackPanel>
            <MyInnerClass />
            <MyInnerClass />
            <MyInnerClass />
            <MyInnerClass />
        </StackPanel>
    </DockPanel>
</App>
```

## 비고 (Remarks) ##

`ux:InnerClass` 는 fuse 에서 컴포넌트화 중 매우 특별한 경우에 다루므로 일반적으로는 필요하지 않습니다. `ux:Property` 및 `ux:Dependency` 를 사용하여 컴포넌트 안팎에 명시적 인터페이스를 만드는 것이 더 좋습니다.
