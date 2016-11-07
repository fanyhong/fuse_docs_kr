# 미리보기 및 내보내기 #

## Android 설정 ##
안드로이드로 미리보기 및 내보내기가 가능하기 위해서는 요구되는 SDK 컴포넌트들을 설치할 필요가 있습니다. 터미널에서 다음을 실행합니다.
``` fuse install android ```
이 것은 Android SDK 컴포넌트가 존재하고 있는 위치에 시도 할 것이고, 필요하면 그것들을 설치 합니다.
만약 여러분이 윈도우즈를 실행하고 있다면 디바이스를 위해 맞는 USB 드라이버를 설치할 필요가 있습니다. 설치하기 위한 지침들 뿐 아니라 일반적인 벤더들의 드라이버 목록은 여기서 확인할 수 있습니다.


## iOS 설정 ## 
여러분은 Mac OSX 및 미리보기 할 Xcode 그리고 당신의 앱을 내보내기 할 iOS 가 필요할 겁니다. 
또한 무료 애플 개발자 계정 생성과 아래보여지는 것 같이 Xcode 설정 등록이 필요합니다.

![Alt text](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/401edb6f22e77628712f87ecc5b4bde4__media/preview-and-export-xcode-add-apple-id.webp)

## 미리보기 ##
여러분이 UX에 작성한 마크업과 자바스크립트 변경들에 대한 즉각적인 피드백을 얻으면서 로컬과 안드로이드/iOS 디바이스들에서 쉽게 미리보기 할 수 있습니다.

### 퓨즈 대시보드를 통한 미리보기 ###
대시보드에서 여러분의 프로젝트에 "Start app preview" 버튼을 클릭합니다. 여러분이 프리뷰를 시작할 타겟 플랫폼을 선택하는 모달 윈도우가 하나 나타납니다.

### Sublime Text 에서 미리보기 ###
서브라임 텍스트에서 미리보기를 위해서는 사이드바에 있는 `ux` 혹은 `unoproject` 파일을 우클릭하여 `Fuse: Preview` 아래에 있는 여러분의 타겟 플랫폼을 선택합니다.

![Sublime Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/04970d4c6ae8aa6d1827725e07de1844__media/preview-and-export-device-preview-osx-sublime-preview-menu.webp)

커맨드 팔레트로 미리보기를 시작할 수도 있습니다. `Cmd+Shift+P` (OSX) 혹은 `Ctrl+Shift+P` (윈도우즈) 로 팔레트를 열고, 미리보기 타겟 목록을 보기 위해서 `Fuse Preview` 를 타이핑합니다.

![Sublime Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/7548a0c1fe7bbef48688ceef464bb620__media/preview-and-export-device-preview-sublime-command-palette.webp)

### Atom 에서 미리보기 ###
커맨드 팔레트를 열기 위해 `Cmd+Shift+P` (OSX) 혹은 `Ctrl+Shift+P` (윈도우즈) 를 누릅니다, 그리고 미리보기 타겟 목록을 보기 위해서 `Fuse Preview` 를 타이핑합니다.

![Atom Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/2a7f70bfe13262201cda43410d1c3872__media/preview-and-export-device-preview-atom-command-palette.webp) 

### 커맨드라인에서 미리보기 ###
터미널에서 `cd` 로 프로젝트 폴더로 들어가서 여러분의 빌드 타겟에 부합하는 명령을 실행합니다.
```
fuse preview -t=Android
fuse preview -t=iOS
fuse preview -t=Local   # The -t=Local flag is optional in this case
```
`fuse help preview` 를 실행함으로써 `fuse preview` 관련 문서를 더 얻을 수 있습니다.

## 내보내기 ##
여러분의 앱을 내보낼때, UX 마크업 파일은 여러분 앱의 실시간 미리보기 가능성을 잃어버리는 걸 의미하는 c++코드로 컴파일 되어집니다.
하지만 여러분 앱이 여러분 컴퓨터와 네트워크 연결됨 없이 자체적으로 동작할겁니다. 여러분의 앱을 배포할때 원하는 것이 이겁니다. 

### Android ###
여러분의 쉘의 프로젝트 루트에서 아래 명령을 입력합니다.
``` fuse build --target=Android --run ```
이것은 연결되어진 안드로이드 디바이스로 배포되어 프로젝트를 시작할 것입니다.

릴리즈 빌드를 만들어 실행하기 위해사는:
``` fuse build --target=Android --configuration=Release ```
알림: 구글플레이 스토어에 앱을 내보내기 위해서는, 먼저 sign 할 필요가 있습니다.

### iOS ###
여러분의 쉘의 프로젝트 루트에서 아래 명령을 입력합니다.
``` fuse build --target=iOS --run ```
혹 XCode 안에서 생성된 프로젝트를 열기를 원한다면, `fuse build --target=iOS -adebug`를 실행합니다.

릴리즈 빌드를 만들어 실행하기 위해사는:
``` fuse build --target=iOS --configuration=Release ```
알림: 앱스토어에 앱을 내보내기 위해서는, 먼저 sign 할 필요가 있습니다.

## 서명 ##
릴리즈를 위한 여러분 앱을 sign 하기 위한 안내를 위한 서명하기 기사를 보십시오.
