원본: https://www.fusetools.com/docs/ux-markup/resources

# 리소스 (ux:Key) #

UX 마크업의 `ux:Key` 속성은 그것이 속해 있는 해당 요소를 서브트리에 적용되는 동적 리소스로 변환합니다.

리소스들은 `{Resource resource_key}` 바인딩 구문으로 생성된, *리소스 바인딩* 들로 사용될 수 있습니다.

## 구문 (syntax) ##

```
<type ux:Key="resource_key" [ux:Value="value"] ... />
```

여기서 `type` 은 UX 마크업에서 사용할 수 있는 모든 타입이 가능하며, `resource_key` 는 임의의 문자열 입니다.

> 엄격하게 요구되는 것은 아니지만, 네임스페이싱을 위해 마침표( `.` )로 구분된 유효한 Uno 식별자들로 구성되는 `resource_key` 를 사용하는 것이 좋습니다.

해당 타입이 값 타입( `float4` 또는 `int` 같은)이라면 `ux:Value` 속성을 지정되어야 합니다.

## 리소스 바인딩 (Resource bindings) ##

리소스 키에 대한 바인딩을 만들려면 `{Resource resource_key}` 구문을 사용합니다.

예:

```
<Panel Color="{Resource MyApp.ThemeColor}"
```

## 글로벌 기본 리소스 (ux:Global) ##

`ux:Global` 속성은 리소스 키(resource key)들에 대한 전역 기본값을 정의하는데 사용할 수 있습니다. UX 트리 범위(scope)에서 리소스 키가 확인되지 않으면, 대신 리소스 바인딩 키를 계산하는 전역 리소스(global resource)가 사용됩니다.

### 예 ###

다음 예제는 `MyApp.ThemeColor` 리소스 키에 대한 `ux:Global` 기본값을 정의합니다. 이 키는 다른 지정되지 않은 앱 트리의 모든 부분에 적용됩니다.

```
<App>
    <float4 ux:Global="MyApp.ThemeColor" ux:Value="Green" />

    <Navigator>
        <HomePage ux:Template="homePage" />
        <ProfilePage ux:Template="profilePage" />
        <SettingsPage ux:Template="settingsPage">
            <float4 ux:Key="MyApp.ThemeColor" ux:Value="Blue" />
        </SettingsPage>
    </Navigator>
</App>
```

`SettingsPage` 에서 우리는 `ux:Key` 속성을 사용하여 리소스 키를 `Green` 대신 `Blue` 로 대체합니다. 즉, 설정 페이지에서 `{Resource MyApp.ThemeColor}` 를 사용하면, 해당 트리 하위에서 재정의하지 않는 한 `Green` 이 아닌 `Blue` 으로 표시될 것입니다.
