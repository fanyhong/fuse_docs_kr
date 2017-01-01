원본: https://www.fusetools.com/docs/ux-markup/resources

리소스 (ux : Key)

UX Markup의 ux : Key 속성은 해당 요소가 상주하는 요소를 하위 트리에 적용되는 동적 자원으로 변환합니다.

리소스는 {Resource resource_key} 바인딩 구문으로 생성 된 리소스 바인딩과 함께 사용할 수 있습니다.

통사론

<type ux : Key = "resource_key"[ux : Value = "value"] ... />
여기서 type은 UX Markup에서 사용할 수있는 모든 유형이며 resource_key는 임의의 문자열입니다.

엄격하게 요구되는 것은 아니지만 유효한 Uno 식별자로 구성된 resource_key를 마침표로 구분하여 사용하는 것이 좋습니다. 네임 스페이스 용.
유형이 값 유형 (예 : float4 또는 int)이면 ux : Value 속성을 지정해야합니다.

리소스 바인딩

리소스 키에 대한 바인딩을 만들려면 {Resource resource_key} 구문을 사용합니다.

예:

<패널 색상 = "{Resource MyApp.ThemeColor}"
글로벌 기본 리소스 (ux : Global)

ux : Global 속성은 리소스 키에 대한 전역 기본값을 정의하는 데 사용할 수 있습니다. UX 트리 범위에서 리소스 키가 확인되지 않으면 대신 리소스 바인딩 키를 계산하는 전역 리소스가 사용됩니다.

예

다음 예제는 MyApp.ThemeColor 리소스 키에 대한 ux : Global 기본값을 정의합니다.이 키는 다른 것을 지정하지 않은 앱 트리의 모든 부분에 적용됩니다.

<App>
    <float4 ux : Global = "MyApp.ThemeColor"ux : Value = "Green"/>

    <네비게이터>
        <HomePage ux : Template = "homePage"/>
        <ProfilePage ux : Template = "profilePage"/>
        <SettingsPage ux : Template = "settingsPage">
            <float4 ux : Key = "MyApp.ThemeColor"ux : Value = "Blue"/>
        </ SettingsPage>
    </ Navigator>
</ App>
SettingsPage에서 우리는 ux : Key 속성을 사용하여 자원 키를 Green 대신 Blue로 대체합니다. 즉, 설정 페이지에서 {Green} 대신 {Resource MyApp.ThemeColor}를 사용하면 트리를 훨씬 더 아래쪽으로 재정의하지 않는 한 녹색이 아닌 파란색으로 표시됩니다.
