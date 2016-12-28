원문: https://www.fusetools.com/docs/basics/quickstart

# 퀵스타트 #
설치, 생성하기, 미리보기 과정을 통해 한 단계씩 튜토리얼을 진행하여 Fuse로 첫 번째 앱을 만들어 낼 수 있는 튜토리얼입니다.

## 설정 ##
간단하게 [Fuse의 최종버전을 다운로드](https://www.fusetools.com/downloads) 한 뒤, 설치하여 시작할 준비를 합니다.
설치는 순조로워야 합니다만 뭔가 잘 안된다면, [우리에게 알려주십시오](https://www.fusetools.com/contact). [macOS](https://www.fusetools.com/docs/basics/installation/setup-install-osx) 와 [윈도우즈](https://www.fusetools.com/docs/basics/installation/setup-install-win) 에 대한 설정 명령들 또한 있습니다. 추가로, 필요한 경우에 사용할 수 있는 [macOS를 위한 설치제거 명령들](https://gist.github.com/Tapped/daa78c08882f33b0c7c3) 입니다.

## 새로운 프로젝트 생성하기 ##
Fuse 커맨트라인 도구를 사용하여 새 프로젝트의 디렉토리를 생성합니다.
```
fuse create app <projectname> [optional path]
```
경로가 생략되면, 현재 디렉토리의 하위에 디렉토리가 생성될겁니다.
최근 생성된 Fuse 프로젝트는 기본적으로 `MainView.ux` 라는 .ux 파일을 반드시 하나 포함해야 합니다.
> Fuse 대시보드의 프로젝트 메뉴에서 "New Project" 를 클릭해 프로젝트를 생성할 수도 있습니다.

## 앱 실행하기 ##
Fuse는 앱을 실행하기 위한 두가지 방법을 제공합니다.
- `fuse build` - 앱스토어에 옮겨 지거나 개인적 테스트를 위해 사용될 수 있는 독립적으로 실행 가능한 앱으로 빌드합니다.
- `fuse preview` - 실행 도중 변경 가능한 미리보기 버전의 앱으로 빌드합니다. 이것은 보통 개발 중에 사용 됩니다.

*로컬 미리보기* 를 시작하기 위해서는, `cd` 로 새로 생성한 프로젝트 안으로 이동하여 아래 명령을 실행합니다.
``` 
fuse preview
```
이 것은 여러분의 맥 혹은 PC 에서 미리보기 로컬 세션을 시작합니다.
> 또한 Fuse 대시보드에서 프로젝트를 선택 후 미리보기를 클릭함으로써 미리보기를 시작할 수 있습니다.
더 굉장한 점은 휴대폰이나 타블렛 위에서 실행하는 미리보기로 정확하게 같은 일을 할 수 있다는 겁니다.
이런 작업을 위해서는 디바이스가 USB로 연결되어 있어야 하고 개발 컴퓨터와 같은 무선 네트워크에 있어야 합니다.
아래 명령은 iOS용 앱 미리보기를 위한 Xcode 프로젝트 생성합니다.
```
fuse preview -tios
```
안드로이드는, [적절한 디바이스 드라이버들](https://developer.android.com/studio/run/oem-usb.html#Drivers) 이 설치 되었는지 확인한 뒤에 진행 합니다.
```
fuse preview -tandroid
```

USB 연결은 처음에 미리보기 앱을 휴대 전화에 설치하고 설치할 때만 필요합니다. 미리보기 앱이 설치되면 USB 연결이 필요하지 않습니다. 그 미리보기 앱은 당신이 필요할 때마다 여러번 당신의 폰에서 재시작 할 수 있습니다. 앱은 미리보기 앱을 만든 컴퓨터의 로컬 IP 주소를 기억하고 무선으로 연결합니다. 이를 통해 개발 중에 원하는만큼의 장치로 미리보기를 실행할 수 있습니다.

만약 프로젝트에서 네이티브코드 (Uno, Java 또는 Objective-C)를 변경하거나 컴퓨터의 IP 주소를 변경하면 USB 케이블을 다시 연결하고 `fuse preview` 명령을 다시 실행해야합니다

## UX 마크업로 작업하기 ##
텍스트 편집기에서 .ux 파일을 열고 내부의 마크 업을 살펴볼 차례입니다. 구문 강조 및 자동 완성을 위해 [Fuse 플러그인](https://www.fusetools.com/docs/basics/installation/sublime-plugin) 과 함께 [Sublime Text 3](https://www.sublimetext.com/3) 을 사용하는 것이 좋습니다.

`MainView.ux`의 내용은 아래와 같아야 합니다:
```
<App>
</App>
```   

몇개의 UI 요소들을 추가:
```
<App>
    <DockPanel>
        <StatusBarBackground Dock="Top" />
        <ScrollView ClipToBounds="true">
            <StackPanel>
                <Text FontSize="30">Hello, world!</Text>
                <Slider />
                <Button Text="Button" />
                <Switch Alignment="Left" />
            </StackPanel>
        </ScrollView>
    </DockPanel>
</App>
```

앱이 미리보기에서 실행되는 동안 UX 마크 업 파일 (Ctrl / Cmd + S를 누름)을 저장할 때마다 변경 사항이 모든 미리보기 세션 (로컬 및 장치 모두)에 즉시 반영됩니다.

## 마크업 이해하기 ##
UX 파일 안의 각 XML 요소는 Uno 오브젝트를 나타냅니다. 요소의 이름은 클래스의 이름을 나타냅니다.

`MainView.ux` 현재 구성들:
- [App](https://www.fusetools.com/docs/fuse/app) - 앱의 진입점. 앱에서 `App`은 오직 하나만 가질 수 있습니다.
- [DocPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) - 자식들을 위, 아래, 왼쪽, 오른쪽에 배치하는 레이아웃을 수행합니다. 마지막 자식은 기본적으로 남아있는 공간을 채웁니다. 
- [StatusBarBackground](https://www.fusetools.com/docs/fuse/controls/statusbarbackground) - 상태 표시 줄이 투명 할 경우를 대비하여 상태 표시 줄의 공간을 예약합니다. 이를 통해 iOS 및 Android에서 상태 표시 줄의 배경을 제어 할 수 있습니다. Fuse는 아직 상태 표시 줄을 에뮬레이션하지 않지만 모바일로 내보내면 적용된 효과를 확인 할 수 있습니다.
- [ScrollView](https://www.fusetools.com/docs/fuse/controls/scrollview) - 스크롤은 기본적으로 수직 방향으로 처리합니다.
- [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) - StackPanel은 자식을 가로 또는 세로로 쌓아서 레이아웃을 수행합니다. 이 예제에서와 같이 세로가 기본값입니다.
- [Slider](https://www.fusetools.com/docs/fuse/controls/slider) - 사용자가 주어진 범위 내에서 값을 선택하게하는 컨트롤입니다.
- [Button](https://www.fusetools.com/docs/fuse/controls/button) - 클릭 / 탭 할 수있는 컨트롤입니다.
- [Switch](https://www.fusetools.com/docs/fuse/controls/switch) - 스위치를 켜거나 끌 수있는 컨트롤입니다.

[전체 UX 클래스 레퍼런스](https://www.fusetools.com/docs/full-ux-class-reference)에서 UX 에서 사용 가능한 클래스들의 전체 목록을 볼 수 있습니다.  

## 로직 과 스크립팅 ##
Fuse를 사용하면 JavaScript를 사용하여 앱에 로직을 추가 할 수 있습니다. UX 마크 업과 스크립팅을 결합하여 모양과 느낌, 데이터 모델, 논리 및 백엔드 통합에 이르기까지 앱을 구현하는 데 필요한 모든 기능을 갖추게됩니다.
그러나 이 첫 번째 예제에서는 단추를 클릭 할 때 증가하는 카운터를 간단히 만들 것입니다.
먼저 DockPanel 안 쪽에 아래의 자바스크립트 코드 조각을 추가합니다.
```
<JavaScript>
    var Observable = require('FuseJS/Observable');
    var buttonText = Observable('Button');
    var clickCount = 0;

    function onClick() {
        clickCount += 1;
        buttonText.value = 'Clicks: ' + clickCount;
    }

    module.exports = {
        buttonText: buttonText,
        onClick: onClick
    }
</JavaScript>
```
그리고 나서 .ux 안에 이미 존재하는 Button 에 `onClick` 핸들러를 연결하고 `buttonText` 변수를 바인딩합니다.
```
<Button Text="{buttonText}" Clicked="{onClick}"/>
```
UX 마크업을 변경할 때와 마찬가지로 JavaScript를 변경하면 변경 사항을 적용 할 때, 모든 미리보기 세션에서 즉시 업데이트됩니다.

## 재사용 가능한 스타일들과 컴포넌트들 ##
Fuse에서 모든 클래스들은 새로운 클래스로 만들기 위해 하위클래스화 할 수 있습니다.
예를 들어 모든 텍스트 요소의 글꼴 크기를 지정하지 않으려 한다고 가정해 봅시다. 모든곳에 `<Text FontSize = "40">` 을 사용하는 대신 원하는 텍스트 유형을 설명하는 하위 클래스를 만들거나 소유하고 있어야 합니다.
```
<Text ux:Class="BigHeader" FontSize="40" />
```
그러고 나면, 언제든 big header 를 보여줄 수 있습니다.
```
<BigHeader>Welcome!</BigHeader>
```
`BigHeader`의 정의는 `MainView.ux` 안 아무 위치 에나 넣어도 되고, 여러분 프로젝트 임의의 `.ux` 파일에 넣울 수도 있습니다.

## 앱 내보내기 ##  
앱이 최소한 하나의 가치 있는 특색을 가진 지금, 안드로이드와 iOS 로 내보낼 때가 되었습니다. 명령 줄에서 다음을 입력하여이 작업을 수행 할 수 있습니다.
```
fuse build --target=<iOS or Android> --run
```

## 좋습니다. 쉽죠! 다음은 뭘까요? ##
위의 기본 사항에 익숙해지면 처음부터 hikr 라는 하이킹 추적 응용 프로그램을 빌드하는 [전체 튜토리얼](https://www.fusetools.com/docs/tutorial/tutorial) 을 살펴 보는 것이 좋습니다. [예제 섹션](https://www.fusetools.com/examples) 또한 확인해 보셔야 합니다.

우리는 또한 [포럼](https://www.fusetools.com/community/forums) 에서 여러분의 모든 질문에 답하는 것을 기쁘게 생각합니다.


