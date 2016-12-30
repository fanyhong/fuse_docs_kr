원본: https://www.fusetools.com/docs/tutorial/splash-screen

## 소개 ##

이전 장에서 우리는 앱의 모양과 느낌에 초점을 맞추 었으며 멋지고 일관성있게 보였습니다. 이 과정에서 우리는 또한 Fuse의 ux : Class 메커니즘을 활용하여 앱 스타일링을위한 재사용 가능한 구성 요소를 구축하는 방법을 배웠습니다.

이 장에서는 최종 화면 (스플래시 페이지)을 추가하여 모든 것을 묶어 앱을 완성합니다! 사용자가 열면 우리의 디자인과 일치시킬 수있는 퍼즐의 마지막 부분입니다. 이전 장에서 배웠던 모든 도구와 기술을 통해 케이크 조각이 될 것입니다. 그럼 시작합시다!

앱의 전체 소스 코드는 여기에서 볼 수 있습니다.

스플래시 화면 만들기

스플래시 화면에서 가장 먼저해야 할 일은 새로운 Page를 만드는 것입니다. 프로젝트의 Pages 디렉토리에서 다음 내용으로 SplashPage.ux라는 새 파일을 만듭니다.:

```
<Page ux:Class="SplashPage">
</Page>
```

우리가 마지막 장을 만든 hikr.Page 구성 요소 대신 Page를 기본 클래스로 사용하는 방법에 주목하십시오. 결국이 페이지에 비디오 배경을 추가 할 것이므로 지금은 배경에 대한 앱의 배경색 만 원할 것입니다.

SplashPage가 생겼으니, MainView.ux의 앱 네비게이터에 템플릿을 추가해 봅시다. 또한이 템플릿을 앱에 표시하려는 첫 번째 스크린으로 기본 템플릿으로 만듭니다.:

```
        <Navigator DefaultTemplate="splash">
            <SplashPage ux:Template="splash" />
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
```

이렇게 저장하면 화면에 빈 SplashPage가 나타납니다.

> 참고 : 퓨즈는 재로드간에 탐색 한 경로를 캐시하므로 미리보기를 다시 시작해야 할 수도 있습니다. 미리보기를 다시 시작하면이 캐시가 지워 지므로 SplashPage가 표시됩니다.

SplashPage가 표시되었으므로 이제 디자인을 살펴 보겠습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/77d73cbf99a8449e743f59aed5a8af77__media/hikr/tutorial/splash.webp)

여기서 우리는 제목 텍스트와 슬로건, 그리고 "시작하기"버튼의 두 가지 주요 부분을 볼 수 있습니다. 이 두 부분은 화면의 위 / 아래 반쪽 중앙에 각각 배치됩니다. 먼저 SplashScreen.ux에서 화면을 두 개의 셀로 분할하는 Grid를 만드는 것입니다.

```
<Page ux:Class="SplashPage">
    <Grid RowCount="2">
    </Grid>
</Page>
```

이 그리드의 첫 번째 하위 노드는 그리드의 맨 위 셀의 내용을 나타냅니다. 우리의 제목 텍스트와 슬로건을 위해 StackPanel을 만듭니다.:

```
    <Grid RowCount="2">
        <StackPanel>
        </StackPanel>
    </Grid>
```

다음으로 제목 텍스트를 추가합니다. 우리는 hikr.Text 인스턴스로 시작합니다 :

```
        <StackPanel>
            <hikr.Text>hikr</hikr.Text>
        </StackPanel>
```

이것을 저장하면 hikr 텍스트가 셀의 왼쪽 상단 모서리에 표시됩니다. 수평 정렬을 위해 정렬을 추가해 보겠습니다.:

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter">hikr</hikr.Text>
        </StackPanel>
```

우리는 조만간 그것을 수직으로 센터링 할 것입니다. 지금은 조금 더 폰트 크기를 늘릴 것입니다 :

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
        </StackPanel>
```

자, 우리의 슬로건 텍스트를 추가합시다. 제목 텍스트와 동일한 StackPanel에 배치합니다.:

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text>get out there</hikr.Text>
        </StackPanel>
```

또한이 텍스트를 가운데에 배치하고 불투명도를 설정하여 텍스트를 투명하게 만듭니다.

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>
```

StackPanel에서 두 항목을 모두 가져 왔으므로 StackPanel을 Grid 셀의 가운데에 세로로 배치 할 수 있습니다. 정렬 :

```
        <StackPanel Alignment="VerticalCenter">
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>
```

그리고 그게 전부입니다. 우리의 제목과 구호 텍스트는 돌봐!

다음으로 "시작하기"버튼을 추가 할 것입니다. 이전 장에서 작성한 hikr.Button 구성 요소 덕분에 매우 쉽습니다. 먼저 StackPanel 아래에 인스턴스를 생성하여 Grid의 하위 셀을 차지합니다.:

```
    <Grid RowCount="2">
        <StackPanel Alignment="VerticalCenter">
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>

        <hikr.Button Text="Get Started" />
    </Grid>
```

우리의 버튼을 수직으로 중심에두고 여백을주기 위해 왼쪽 / 오른쪽 여백을 추가합시다.

```
        <hikr.Button Text="Get Started" Margin="50,0" Alignment="VerticalCenter" />
```

이것은 좋아 보인다, 그러나 글꼴은 조금 작게 본다. FontSize를 약간 늘리십시오.:

```
        <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" />
```

아,하지만 기다려! hikr.Button 구성 요소는 Panel에서 작성 되었기 때문에 실제로 FontSize 속성을 가지고 있지 않습니다. 그러나 이것은 아무런 문제가 아닙니다. 우리는 단지 그것을 스스로 만들 수 있습니다!

우리의 Components / hikr.Button.ux 파일에서 먼저 ux로 float 인스턴스를 생성하여 FontSize 속성을 추가해 봅시다 (글꼴 크기는 숫자 값이어야 함) : Property specified :

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10">
    <string ux:Property="Text" />
    <float ux:Property="FontSize" />
```

이것은 속성 생성을 담당합니다. 이제 구성 요소가 실제로 그것을 사용하는지 확인합시다. 컴포넌트의 hikr.Text 인스턴스에서 이전에 Value를 사용했던 것처럼 현재 하드 코딩 된 FontSize를 우리의 속성에 바인딩 할 수 있습니다.:

```
    <hikr.Text Value="{Property Text}" FontSize="{Property FontSize}" TextAlignment="Center" />
```

이제는 여기서 멈출 수 있지만이 구성 요소는 응용 프로그램의 다른 곳에서 사용되며 해당 인스턴스의 FontSize 속성을 지정하지 않았습니다. 따라서이 속성은 구성 요소에 기본값이 있으므로 해당 인스턴스가 여전히 동일하게 보이도록해야합니다. 속성에 대한 기본값을 지정하려면 인스턴스에 대해 설정하는 것처럼 구성 요소 정의에 속성을 설정해야합니다. hikr.Button.ux에서 이것은 다음과 같이 보입니다 :

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10" FontSize="16">
```

이렇게 지정하지 않으면 FontSize 속성의 값은 이전과 같이 16이됩니다. 굉장해!

이제 버튼을 홈 페이지로 이동하도록합시다. 가장 먼저 할 일은 탐색을 수행 할 콜백을 포함하는 SplashPage 용 간단한 자바 스크립트 모듈을 추가하는 것입니다. 콜백을 버튼의 Clicked 이벤트에 바인딩합니다. 우리는 Pages 디렉토리 안에 SplashPage.js라는 새로운 파일을 만들 것입니다. 그리고 내부에 goToHomePage라는 함수를 만들 것입니다 :

```
function goToHomePage() {
    // TODO
}
```

구현으로 돌아갑니다. 지금은이 함수를 내보낼 지 확인하십시오.

```
function goToHomePage() {
    // TODO
}

module.exports = {
    goToHomePage: goToHomePage
};
```

그런 다음 JavaScript 태그를 사용하여 SplashPage.ux 파일에이 모듈을 포함시킵니다.

```
<Page ux:Class="SplashPage">
    <JavaScript File="SplashPage.js" />

    <Grid RowCount="2">
```

마지막으로이 함수를 버튼에 연결합니다.

```
        <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" Clicked="{goToHomePage}" />
```

이제 SplashPage의 JS 모듈을 설정하고 버튼에 대한 Clicked 처리기를 갖게되었으므로이 핸들러를 제대로 구현하여 HomePage로 이동할 수 있습니다. 그러나 네비게이션과 라우팅의 장을 생각해 보면, 우리는 앱의 라우터 인스턴스 인 네비게이션을 수행하기 위해 하나의 컴포넌트에 액세스 할 수 있어야합니다!

먼저 router라는 SplashPage에 Router 의존성을 추가합시다.

```
<Page ux:Class="SplashPage">
    <Router ux:Dependency="router" />

    <JavaScript File="SplashPage.js" />

```

그런 다음 다른 페이지와 마찬가지로 Router 인스턴스를 전달하여 MainView.ux에서 이러한 종속성을 충족시킬 수 있습니다.:

```
        <Navigator DefaultTemplate="splash">
            <SplashPage ux:Template="splash" router="router" />
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
```

이제 라우터 인스턴스를 사용하여 원하는 탐색을 수행 할 수 있습니다. SplashPage.js로 돌아가서 goToHomePage 함수가 홈 경로를 사용하여 HomePage로 이동하도록합니다.:

```
function goToHomePage() {
    router.push("home");
}
```

그렇게하면 모든 것을 저장하고 테스트하여 작동하는지 확인할 수 있습니다. 그러나 여기서는 라우터의 푸시 기능을 사용하고 있습니다. 이상적으로는 정상적인 사용 중에 앱 사용자가 스플래시 화면으로 다시 돌아가는 것이 좋지 않으므로 goto를 사용하는 것이 이상적입니다. 그러나 우리가 테스트하기가 더 쉽도록 푸시를 사용할 수 있으므로 당분간 뒤로 이동할 수 있으며 나중에 goto를 호출하여 스왑 할 수 있습니다.

## 비디오 배경 추가 ##

이제 SplashPage의 배경에 비디오를 추가 할 차례입니다! 우리가 사용할 비디오는 nature.mp4라고하며 여기에서 찾을 수 있습니다.

nature.mp4 on Github

우리가 이것을 앱에 집어 넣기 전에,이 비디오의 출처를 알아 두는 것이 중요합니다. 이 비디오는 Graham Uhelski의 "The Valley"의 수정 된 버전입니다.이 버전은 CC BY 3.0 라이센스로 사용이 허가되었습니다. 즉, 원래 아티스트의 저작자 표시를 제공하는 한 상업적 목적으로조차도 콘텐츠를 공유하고 수정할 수 있습니다. 이를 위해서는 원저자에게 적절한 기여를하고 변경이 있었는지 여부를 표시해야합니다. 따라서 일단 앱에서 비디오를 얻으면이 작업을 수행하는 것이 중요합니다.

> 참고 : 권한이 있음을 알고있는 앱의 콘텐츠를 항상 사용하십시오. 콘텐츠 라이센스를 무시하지 마십시오. 특히 쉽게 따라 할 수 있습니다!

비디오 파일을 가져 와서 Assets 디렉토리에 저장하십시오. 그런 다음 SplashPage.ux 파일에서 Video 태그를 사용하여 비디오를 표시 할 수 있으며 마지막 챕터의 hikr.Page 구성 요소에있는 Image와 마찬가지로 Layer = "Background"를 설정합니다.:

```
    <JavaScript File="SplashPage.js" />

    <Video Layer="Background" File="../Assets/nature.mp4" />
```

이것을 저장하고 퓨즈가 연결된 모든 장치에 비디오 파일을 전송할 때까지 기다리면 비디오 파일이로드되는 동안 검은 색 직사각형처럼 보입니다. 이는 비디오 객체가 기본적으로 재생을 시작하지 않기 때문입니다. 우리는 실제로 비디오를 재생하도록 지시해야합니다. 이를 위해 AutoPlay = "true"로 설정합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" AutoPlay="true" />
```

이제 동영상이 재생되기 시작했습니다. 그러나 비디오의 끝까지 도달하게되면 재생이 멈추는 것을 볼 수 있습니다. 동영상을 반복하고 싶기 때문에 IsLooping = "true"로 설정합니다.

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" />
```

다음으로 해결해야 할 사항은 기기 / 미리보기가 동영상과 다른 측면을 가지고 있다면 동영상이 전체 화면을 채우지 않을 것입니다. 이전 장의 배경 이미지와 비슷하게, 이는 비디오 요소가 내용을 늘리거나 찌그러 뜨려 가로 세로 비율을 유지하면서 항상 사용 가능한 공간에 맞춰지기 때문입니다. 우리는 항상 비디오가 사용 가능한 공간을 채우길 원하기 때문에 hikr.Page 구성 요소가 배경 이미지와 마찬가지로 StretchMode = "UniformToFill"을 사용합니다.

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" />
```

이제 우리는 좋아 보인다! 그러나 우리의 배경은 그대로있는 것이 너무 뚜렷하기 때문에 약간의 흐림 효과를 추가하여 "되돌려 보겠습니다". 비디오 (또는 실제로 퓨즈의 모든 요소)를 흐리게 처리하려면 블러 요소를 첨부해야합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur />
    </Video>
```

이것으로 우리는 비디오가 잘 보이지 않는 것을 볼 수 있습니다! 얼마나 흐릿하게 조정할 것인지 반경을 약간 조정 해 봅시다. 우리는 모양을 유지하면서 상당히 흐리게 보이기를 원합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur Radius="4.75" />
    </Video>
```

좋아 보이는구나! 마지막으로 우리는 배경을 약간 투명하게 만들고 앱의 배경색을 보여주기 위해 배경을 약간 뒤로 밀면됩니다 :

```
    <Video Layer="Background" File="../Assets/nature.mp4" Opacity="0.5" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur Radius="4.75" />
    </Video>
```

그리고 그것으로 우리의 비디오는 멋지게 보입니다! 이제 라이센스 필요와 같이 원래 아티스트에게 저작자 표시를 추가해 보겠습니다. 화면 하단의 "Graham Uhelski의 원본 비디오"와 같은 텍스트는 아주 잘 처리됩니다. 먼저 SplashPage 구성 요소의 모든 내용을 DockPanel에 배치 해 봅시다.

```
<Page ux:Class="SplashPage">
    <Router ux:Dependency="router" />

    <JavaScript File="SplashPage.js" />

    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>

        <Grid RowCount="2">
            <StackPanel Alignment="VerticalCenter">
                <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
                <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
            </StackPanel>

            <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" Clicked="{goToHomePage}" />
        </Grid>
    </DockPanel>
</Page>
```

그런 다음 DockPanel의 아래쪽에 도킹 된 hikr.Text 구성 요소를 추가합니다.: 

```
    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>

        <hikr.Text Dock="Bottom">original video by Graham Uhelski</hikr.Text>
```

화면의 중앙에 수평으로 정렬하고 약간의 여백을 추가하여 하단 가장자리를 완전히 만지지 않도록하십시오.:

```
        <hikr.Text Dock="Bottom" Margin="10" TextAlignment="Center">original video by Graham Uhelski</hikr.Text>
```

그런 다음 글꼴을 조금 더 작게 만들고 불투명도를 조정하여 눈에 잘 띄지 않도록하십시오.:

```
        <hikr.Text Dock="Bottom" Margin="10" Opacity=".5" TextAlignment="Center" FontSize="12">original video by Graham Uhelski</hikr.Text>
```

그리고 그걸로 우리는 우리의 컨텐츠가 적절하게 만들어지고 멋지게 보입니다!

## 마감 처리 ##

이제 SplashScreen이 기본적으로 완료되었으므로 앱에 몇 가지 마무리 터치를 추가하여 거래를 성사시킬 수 있습니다. 예를 들어, HomePage로 이동하여 화면의 왼쪽 가장자리를 보면 SplashScreen에서 비디오의 작은 부분을 볼 수 있습니다. 이는 흐림 효과가 적용된 요소 주위에 약간의 공간을 추가하여 요소의 가장자리가 부드럽게 흐려지기 때문입니다. 그러나 우리의 경우, 이는 동영상 가장자리의 일부가 앱의 다른 페이지에서 볼 수 있음을 의미하므로 바람직하지 않습니다. 이 문제를 해결하려면 SplashPage의 내용이 페이지의 경계에 잘려져 있는지 확인해야합니다. 우리의 경우 SplashPage.ux에서 DockPanel (Video 배경 포함)에 ClipToBounds = "true"를 간단히 추가 할 수 있습니다.

```
    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>
```

이제 동영상은 SplashPage 경계 내에서만 표시됩니다. 쉬운!

잊지 않기 전에 마지막으로해야 할 일은 SplashPage.js 파일로 돌아가서 Push 대신 Router 인스턴스에서 goto를 호출하여 SplashPage로 돌아갈 수 없습니다.

```
function goToHomePage() {
    router.goto("home");
}
```

그게 다야! 이제 SplashPage가 완성되었습니다.


## 우리의 진보 ##

우리가 그걸 끝까지 만든 것처럼 보입니다! 우리 hikr 개념 앱은 완전히 끝났습니다. 굉장해!

다음은 완성 된 제품의 모습입니다.
[https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4)

다음은이 장에서 변경 한 코드 파일입니다.:

`Components/hikr.Button.ux` :

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10" FontSize="16">
    <string ux:Property="Text" />
    <float ux:Property="FontSize" />

    <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
        <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
    </Rectangle>

    <hikr.Text Value="{Property Text}" FontSize="{Property FontSize}" TextAlignment="Center" />

    <WhilePressed>
        <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
    </WhilePressed>
</Panel>
```

`MainView.ux` :

```
<App Background="#022328">
    <iOS.StatusBarConfig Style="Light" />
    <Android.StatusBarConfig Color="#022328" />

    <Router ux:Name="router" />

    <ClientPanel>
        <Navigator DefaultTemplate="splash">
            <SplashPage ux:Template="splash" router="router" />
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
    </ClientPanel>
</App>
```

`Pages/SplashPage.ux` :

```
<Page ux:Class="SplashPage">
    <Router ux:Dependency="router" />

    <JavaScript File="SplashPage.js" />

    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>

        <hikr.Text Dock="Bottom" Margin="10" Opacity=".5" TextAlignment="Center" FontSize="12">original video by Graham Uhelski</hikr.Text>

        <Grid RowCount="2">
            <StackPanel Alignment="VerticalCenter">
                <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
                <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
            </StackPanel>

            <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" Clicked="{goToHomePage}" />
        </Grid>
    </DockPanel>
</Page>
```

`Pages/SplashPage.js` :

```
function goToHomePage() {
    router.goto("home");
}

module.exports = {
    goToHomePage: goToHomePage
};
```

## 다음은 뭔가요? ##

이제 앱이 끝났으므로 Fuse의 핵심 개념을 이미 다뤘지만 항상 배울 점이 많습니다. 마지막 장은 우리가 배운 것에 대해 그리고 퓨즈 기술을 향상시키기 위해 우리가 다음에 갈 수있는 부분에 대해 빨리 마무리 할 것입니다. 그래서 그것을 확인하자!

앱의 전체 소스 코드는 여기에서 볼 수 있습니다.