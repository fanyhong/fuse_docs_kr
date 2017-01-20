원본: https://www.fusetools.com/docs/tutorial/splash-screen

## 소개 ##

[이전 챕터](https://www.fusetools.com/docs/tutorial/look-and-feel) 에서, 우리는 앱의 look and feel (모양과 느낌) 에 초점을 맞췄고, 이제는 멋지고 일관성있게 보이게 되었습니다. 이 과정에서 우리는 Fuse의 `ux:Class` 메커니즘도 활용하여, 앱 스타일링을 위한 재사용 가능 컴포넌트를 구축하는 방법을 학습했습니다.

이 챕터에서는, 마지막 화면(스플래시 페이지) 을 추가하고, 모든 것을 묶어 앱을 완성 할 것입니다! 사용자가 앱을 열때, 이것은 곧 우리 앱에 대한 분위기를 설정 것이고, 우리의 디자인과 일치시킬 수 있는 퍼즐의 마지막 부분입니다. 이전 챕터에서 배웠던 모든 툴들과 기술들을로 쉽게 할 수 있을 겁니다. 그럼 시작합시다!

앱의 전체 소스 코드는 [여기](https://github.com/fusetools/hikr) 에서 볼 수 있습니다.

## 스플래시 화면 만들기 ##

스플래시 화면을 위해 가장 먼저 해야 할 일은, 새로운 [Page](https://www.fusetools.com/docs/fuse/controls/page) 를 만드는 것입니다. 프로젝트의 `Pages` 디렉토리에서, 다음 내용으로 `SplashPage.ux` 라는 새 파일을 만듭니다.:

```
<Page ux:Class="SplashPage">
</Page>
```

우리가 지난 챕터에서 만들었던 `hikr.Page` 컴포넌트 대신, `Page` 를 기본 클래스로 사용하는 것에 주목하십시오. 우리는 결국 이 페이지에 비디오 배경을 추가 할 것이기 때문에, 지금은 배경에 대한 앱의 배경색만을 원합니다.

이제 `SplashPage` 를 얻었으니, `MainView.ux` 에 있는 우리 앱의 [Navigator](https://www.fusetools.com/docs/fuse/controls/navigator) 에 템플릿을 하나 추가해 봅시다. 우리는 또한 우리 앱에서 보여지길 원하는 첫 화면으로써, 이것을 기본 템플릿으로 만듭니다.:

```
        <Navigator DefaultTemplate="splash">
            <SplashPage ux:Template="splash" />
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
```

이렇게 저장하면 화면에 빈 `SplashPage` 가 나타납니다.

> 참고: Fuse는 리로드 간에 탐색 한 경로를 캐시하므로, 미리보기를 다시 시작해야 할 수도 있습니다. 미리보기를 다시 시작하면, 이 캐시가 지워 지므로 `SplashPage` 가 표시됩니다.

`SplashPage` 가 표시 되었으므로, 이제 디자인을 살펴 보겠습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/77d73cbf99a8449e743f59aed5a8af77__media/hikr/tutorial/splash.webp)

여기서 우리는 두 가지 주요 부분을 볼 수 있습니다. - 제목 텍스트와 슬로건, "Get Started" 버튼. 이 두 부분은 화면의 위/아래 반씩 각각 중앙에 배치됩니다. 먼저 `SplashScreen.ux` 에서 화면을 두 개의 셀로 분할하는 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 를 만듭니다.

```
<Page ux:Class="SplashPage">
    <Grid RowCount="2">
    </Grid>
</Page>
```

이 `Grid` 의 첫 번째 자식 노드는, `Grid` 의 맨 위 셀의 내용을 나타냅니다. 제목 텍스트와 슬로건을 위해 [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 을 만듭니다.:

```
    <Grid RowCount="2">
        <StackPanel>
        </StackPanel>
    </Grid>
```

다음으로 제목 텍스트를 추가합니다. 우리는 `hikr.Text` 인스턴스로 시작합니다 :

```
        <StackPanel>
            <hikr.Text>hikr</hikr.Text>
        </StackPanel>
```

이것을 저장하면 `hikr` 텍스트가 셀의 왼쪽 상단 모서리에 표시됩니다. 수평으로 가운데 위치 시키기 위해 정렬을 추가해 봅시다.:

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter">hikr</hikr.Text>
        </StackPanel>
```

우리는 곧 그것을 수직으로 가운데 위치시킬 것입니다. 지금은, 조금 더 `FontSize` 를 늘릴 것입니다 :

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
        </StackPanel>
```

자, 우리의 슬로건 텍스트를 추가합시다. 제목 텍스트와 동일한 `StackPanel` 에 배치합니다.:

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text>get out there</hikr.Text>
        </StackPanel>
```

또한 이 텍스트를 수평으로 가운데 배치하고, `Opacity` 를 설정하여 텍스트를 투명하게 만듭니다.

```
        <StackPanel>
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>
```

`StackPanel` 에서 두 항목을 모두 가져 왔고, 그것들의 `Alignment` 로 설정함으로써, ` StackPanel` 을 `Grid` 셀의 수직으로 가운데에 배치 할 수 있습니다.:

```
        <StackPanel Alignment="VerticalCenter">
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>
```

그게 전부입니다; 제목과 슬로건을 살펴봤습니다!

다음으로 "Get Started" 버튼을 추가 할 것입니다. 이전 챕터에서 작성한 `hikr.Button` 컴포넌트 덕분에 꽤 쉽습니다. 먼저 `StackPanel` 아래에 인스턴스를 하나 생성하는 것에서 시작합니다. `Grid` 에서 하위 셀을 채웁니다.:

```
    <Grid RowCount="2">
        <StackPanel Alignment="VerticalCenter">
            <hikr.Text Alignment="HorizontalCenter" FontSize="70">hikr</hikr.Text>
            <hikr.Text Alignment="HorizontalCenter" Opacity=".5">get out there</hikr.Text>
        </StackPanel>

        <hikr.Button Text="Get Started" />
    </Grid>
```

우리의 버튼을 수직 중앙에 두고, 여백을 주기 위해 왼쪽/오른쪽 여백을 추가합시다.:

```
        <hikr.Button Text="Get Started" Margin="50,0" Alignment="VerticalCenter" />
```

괜찮아 보이지만, 글꼴은 조금 작게 보입니다. `FontSize` 를 약간 늘립시다.:

```
        <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" />
```

아, 그런데 잠깐만요! `hikr.Button` 컴포넌트는 [Panel](https://www.fusetools.com/docs/fuse/controls/panel) 에서 작성 되었기 때문에, 실제로 `FontSize` 속성을 가지고 있지 않습니다. 그렇지만 별 문제가 되는건 아닙니다, 우리가 그것을 직접 만들 수 있습니다!

우리의 `Components/hikr.Button.ux` 파일에서, 지정된 `ux:Property` 와 함께 [float](https://www.fusetools.com/docs/uno/float) 인스턴스(폰트 사이즈는 숫자 값이어야 함) 를 생성함으로써, `FontSize` 속성을 먼저 추가합시다.:

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10">
    <string ux:Property="Text" />
    <float ux:Property="FontSize" />
```

이것은 속성 생성을 담당합니다; 이제 컴포넌트가 실제로 그것을 사용하는지 확인합시다. 컴포넌트의 `hikr.Text` 인스턴스에서 이전에 `Value` 를 사용했던 것처럼, 현재 하드 코딩 된 `FontSize` 를 우리의 속성에 바인딩 할 수 있습니다.:

```
    <hikr.Text Value="{Property Text}" FontSize="{Property FontSize}" TextAlignment="Center" />
```

이제, 이쯤에서 멈출 수도 있지만, 기억하십시오, 이 컴포넌트는 앱의 다른 곳에서 사용되며, 우리는 해당 인스턴스들에 대한 `FontSize` 속성을 지정하지 않았습니다. 따라서, 이 속성이 우리 컴포넌트에서 기본값을 가지고 있는지 확인해 봐야 합니다. 이 인스턴스들은 여전히 동일하게 보일 것입니다. 속성에 대한 기본값을 지정하려면, 인스턴스에 대해 설정하는 것처럼, 컴포넌트 정의에서 속성을 설정해야 합니다. `hikr.Button.ux` 에서 이것은 다음과 같이 보입니다 :

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10" FontSize="16">
```

이런식으로, 지정하지 않으면, `FontSize` 속성의 값은 이전과 같이 16이 될 것입니다. 멋집니다!

이제 버튼을 `HomePage` 로 이동하도록 합시다. 가장 먼저 할 일은, 내비게이션을 수행 할 콜백을 포함하는 `SplashPage` 에 대한 간단한 자바스크립트 모듈을 추가하는 것입니다. 그리고 우리는 우리 버튼의 Clicked 이벤트에 콜백을 바인딩 할 것입니다. 우리는 `Pages` 디렉토리 안에 `SplashPage.js` 라는 새로운 파일을 만들고 그 내부에 `goToHomePage` 라는 함수를 만들 것입니다 :

```
function goToHomePage() {
    // TODO
}
```

잠시 구현으로 돌아갑니다. 현재, 이 함수를 export 할 지 확인합니다.

```
function goToHomePage() {
    // TODO
}

module.exports = {
    goToHomePage: goToHomePage
};
```

그런 다음, `JavaScript` 태그를 사용하여 `SplashPage.ux` 파일에 이 모듈을 포함시킵니다.

```
<Page ux:Class="SplashPage">
    <JavaScript File="SplashPage.js" />

    <Grid RowCount="2">
```

마지막으로, 이 함수를 버튼에 연결합니다.

```
        <hikr.Button Text="Get Started" FontSize="18" Margin="50,0" Alignment="VerticalCenter" Clicked="{goToHomePage}" />
```

이제 `SplashPage` 의 JS 모듈을 설정했고 버튼에 대한 `Clicked` 핸들러를 갖게 되었으므로, 우리는 이 핸들러를 제대로 구현하여 `HomePage` 로 이동 할 수 있습니다. 그런데 [Navigation and routing](https://www.fusetools.com/docs/tutorial/navigation-and-routing) 챕터를 생각해 보면, 우리는 네비게이션 수행을 위해 하나 이상의 항목에 액세스 히도록 하기 위한 컴포넌트를 필요로 할 것입니다. - 우리 앱의 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스 입니다!

먼저 `router` 라 불리는 `SplashPage` 에 `Router` 디펜던시를 하나 추가합시다.

```
<Page ux:Class="SplashPage">
    <Router ux:Dependency="router" />

    <JavaScript File="SplashPage.js" />

```

그런 다음, 다른 `Page` 들과 마찬가지로, `Router` 인스턴스를 전달함으로써 `MainView.ux` 에서 해당 디펜던시를 충족시킬 수 있습니다.:

```
        <Navigator DefaultTemplate="splash">
            <SplashPage ux:Template="splash" router="router" />
            <HomePage ux:Template="home" router="router" />
            <EditHikePage ux:Template="editHike" router="router" />
        </Navigator>
```

이제 `Router` 인스턴스를 사용하여 원하는 탐색을 수행 할 수 있습니다. `SplashPage.js` 로 돌아가서, `goToHomePage` 함수가 `home` route 를 사용하는 `HomePage` 로 이동하도록 합시다.:

```
function goToHomePage() {
    router.push("home");
}
```

그렇게 하고 모든 것을 저장한 뒤, 테스트하여 작동하는지 확인할 수 있습니다! 그런데, 여기서는 `Router` 의 `Push` 함수를 사용하고 있다는 걸 유의하십시오. 정상적인 사용 중에 앱 사용자가 스플래시 화면으로 다시 돌아가게 되는 것은 좋지 않으므로, `goto` 를 사용하는 것이 더 이상적일 것 같습니다. 그러나 우리는 테스트가 더 쉽도록, 지금은 당분간 뒤로 이동할 수 있는 `push` 를 사용해도 됩니다. 그리고 나중에 `goto` 를 호출하는 것으로 교체 할 수 있습니다.

## 비디오 배경 추가 ##

이제 `SplashPage` 의 배경에 비디오를 추가 할 차례입니다! 우리가 사용할 비디오는 `nature.mp4` 라고 하며 여기에서 찾을 수 있습니다.:

[nature.mp4 on Github](https://github.com/fusetools/hikr/blob/master/Assets/nature.mp4)

우리가 이것을 앱에 넣기 전에, 이 비디오의 출처를 알아 두는 것이 중요합니다. 이 비디오는 [Graham Uhelski](https://vimeo.com/mankindfilms) 의 ["The Valley"](http://mazwai.com/#/videos/220) 의 수정 된 버전입니다. 이 버전은 [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/) 라이센스로 사용이 허가되었습니다. 즉, 원래 아티스트의 저작자 표시를 제공하는 한 상업적 목적으로도 콘텐츠를 공유하고 수정할 수 있습니다. 이를 위해서는 원작자에게 적절한 기여를 하고, 변경이 있었는지 여부를 표시해야합니다. 따라서 일단 앱에서 비디오를 얻으면, 이 작업을 수행하는 것이 중요합니다.

> 참고: 항상 권한 여부를 알고 있는 앱의 콘텐츠를 사용하십시오. 특히 쉽게 따라 할 수 있을때, 콘텐츠 라이센스를 무시하지 마십시오!

[비디오 파일](https://github.com/fusetools/hikr/blob/master/Assets/nature.mp4) 을 가져 와서 `Assets` 디렉토리에 저장하십시오. 그런 다음, `SplashPage.ux` 파일에서 [Video](https://www.fusetools.com/docs/fuse/controls/video) 태그를 사용하여 비디오를 표시 하고, [마지막 챕터](https://www.fusetools.com/docs/tutorial/look-and-feel) 의 `hikr.Page` 컴포넌트에 있는 [Image](https://www.fusetools.com/docs/fuse/controls/image) 와 마찬가지로, `Layer="Background"` 를 설정합니다.:

```
    <JavaScript File="SplashPage.js" />

    <Video Layer="Background" File="../Assets/nature.mp4" />
```

이것을 저장하고, Fuse가 연결된 모든 장치에 비디오 파일을 전송할 때까지 기다리면, 비디오 파일이 로드 되는 동안, 검은색 직사각형 같은 것으로 보이게 됩니다. 이는 `Video` 오브젝트가 기본적으로는 재생을 시작하지 않기 때문입니다. 우리는 실제 비디오를 재생하도록 지시해야 합니다. 이를 위해, `AutoPlay="true"` 로 설정합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" AutoPlay="true" />
```

이제 동영상이 재생되기 시작했습니다! 그런데 비디오의 끝까지 도달하면 재생이 멈추는 것을 볼 수 있습니다. 동영상을 반복하고 싶기 때문에, `IsLooping="true"` 를 설정합니다.

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" />
```

다음으로 해결해야 할 사항은, 디바이스/미리보기 가 동영상과 다른 종횡비를 가지고 있다면 동영상이 전체 화면을 채우지 않을 것입니다. [이전 챕터](https://www.fusetools.com/docs/tutorial/look-and-feel) 의 배경 이미지와 유사하게, 이는 `Video` 요소가 항상 사용 가능한 공간에 맞춰지기 위해 내용을 늘리거나 찌그러 뜨려 종횡비를 유지하기 때문입니다. 우리는 항상 비디오가 사용 가능 공간을 채우길 원하기 때문에, `hikr.Page` 컴포넌트의 배경 이미지와 마찬가지로, `StretchMode="UniformToFill"` 을 사용할 것입니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" />
```

이제 괜찮아 보입니다! 그런데, 우리의 배경은 그대로 있는 것이 너무 뚜렷하기 때문에, 약간의 흐림 효과를 추가하여 "push it back (뒤로 밀어)" 봅시다. `Video` (또는 실제로 Fuse의 모든 요소) 를 흐리게 처리하려면 [Blur](https://www.fusetools.com/docs/fuse/effects/blur) 요소를 추가해야 합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur />
    </Video>
```

이것으로 우리는 `Video` 가 멋지게 흐림 처리된 것을 볼 수 있습니다! 얼마나 흐릿하게 조정할 것인지 `Radius` 를 약간 조정 해 봅시다. 우리는 모양은 유지하면서 상당히 흐리게 보이기를 원합니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur Radius="4.75" />
    </Video>
```

좋아 보입니다! 마지막으로, 우리는 배경을 약간 투명하게 만들고, 앱의 배경색을 보여주기 위해 배경을 약간 뒤로 밀 것입니다.:

```
    <Video Layer="Background" File="../Assets/nature.mp4" Opacity="0.5" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill">
        <Blur Radius="4.75" />
    </Video>
```

그리고 그것으로, 우리의 비디오는 멋지게 보입니다! 이제 라이센스 요청와 같이, 원래 아티스트에 대한 저작자 표시를 추가해 보겠습니다. 화면 하단의 "original video by Graham Uhelski" 와 같은 텍스트는 아주 잘 처리됩니다. 먼저 `SplashPage` 컴포넌트의 모든 내용을 [DockPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) 에 배치 해 봅시다.

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

그런 다음 `DockPanel` 의 아래쪽에 도킹된 `hikr.Text` 컴포넌트를 추가합니다.: 

```
    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>

        <hikr.Text Dock="Bottom">original video by Graham Uhelski</hikr.Text>
```

수평 중앙으로도 정렬하고 약간의 margin(여백) 을 추가하여, 하단 가장자리를 완전히 터치하지 않도록 합시다.:

```
        <hikr.Text Dock="Bottom" Margin="10" TextAlignment="Center">original video by Graham Uhelski</hikr.Text>
```

그런 다음, 글꼴을 조금 더 작게 만들고 `Opacity` 를 조정하여, 너무 튀지 않도록 합니다.:

```
        <hikr.Text Dock="Bottom" Margin="10" Opacity=".5" TextAlignment="Center" FontSize="12">original video by Graham Uhelski</hikr.Text>
```

그리고 그것으로, 우리는 잘 조화되고 멋지게 보이는 우리 컨텐츠를 얻었습니다!

## 마무리 ##

이제 `SplashScreen` 이 기본적으로 완료 되었으므로, 앱에 몇 가지 마무리를 추가하여 최종 완성 할 수 있습니다. 예를 들면, 우리가 `HomePage` 로 이동해서 화면의 왼쪽 가장자리를 보면, `SplashScreen` 으로부터 생긴 우리 [Video](https://www.fusetools.com/docs/fuse/controls/video) 의 작은 비트 같은 것을 볼 수 있습니다. 이는 [Blur](https://www.fusetools.com/docs/fuse/effects/blur) 효과가 적용된 요소의 가장자리가 부드럽게 흐려지도록 하기 위해, 요소 주위에 약간의 공간을 추가하기 때문입니다. 그러나 우리의 경우, 이는 동영상 가장자리의 일부가 우리 앱의 다른 페이지에서 보일 수 있음을 의미하므로, 바람직하지 않습니다. 이 문제를 해결하려면, `SplashPage` 의 내용이 페이지의 경계들에서 잘려져 있는지 확인해야 합니다. 이 경우는, `SplashPage.ux` 의 [DockPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) ( `Video` 배경 포함) 에 `ClipToBounds="true"` 를 간단하게 추가하면 됩니다.

```
    <DockPanel ClipToBounds="true">
        <Video Layer="Background" File="../Assets/nature.mp4" IsLooping="true" AutoPlay="true" StretchMode="UniformToFill" Opacity="0.5">
            <Blur Radius="4.75" />
        </Video>
```

이제 해당 동영상은 `SplashPage` 경계 내에서만 표시됩니다. 쉽습니다!

잊기 전에 마지막으로 해야 할 일은, `SplashPage.js` 파일로 돌아가서 `Push` 대신 [Router](https://www.fusetools.com/docs/fuse/navigation/router) 인스턴스에서 `goto` 를 호출하는 것으로, 그렇게 하면 `SplashPage` 로 돌아갈 수 없습니다.

```
function goToHomePage() {
    router.goto("home");
}
```

모든 것이 끝났습니다! 이제 `SplashPage` 가 완성되었습니다.


## 우리의 진행 ##

보시다시피, 우리가 다 만들었습니다! 우리 hikr 컨셉 앱은 완전히 완료되었습니다. 훌륭합니다!

다음은 완성된 제품의 모습입니다.
[https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4)

다음은 이 챕터에서 변경한 코드 파일들 입니다.:

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

이제 우리 앱이 완료되었고, Fuse의 핵심 컨셉들을 이미 많이 다뤘지만, 항상 배울 것은 많습니다. [마지막 챕터](https://www.fusetools.com/docs/tutorial/final-thoughts) 에서는 우리가 배웠던 것과 Fuse 스킬들을 향상시키기 위해 다음으로 할 수 있는 것들에 대해, 빠르게 마무리 지을 것입니다. 그럼 그걸 [확인해 봅시다](https://www.fusetools.com/docs/tutorial/final-thoughts) !

앱의 전체 소스 코드는 [여기](https://github.com/fusetools/hikr) 에서 볼 수 있습니다.
