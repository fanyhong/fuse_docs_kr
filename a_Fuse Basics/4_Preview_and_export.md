
원본: https://www.fusetools.com/docs/basics/preview-and-export

# 미리보기 및 내보내기 #

## Android 설정 ##
미리보기 또는 Android로 내보내기를 수행하려면 필수 SDK 구성 요소를 설치해야합니다. 터미널에서 다음을 실행합니다.
```
fuse install android
```
이렇게하면 기존 Android SDK 구성 요소를 찾고 필요한 경우 설치합니다.
Windows를 실행하는 경우 장치에 적합한 USB 드라이버를 설치해야합니다. 일반적인 공급 업체의 드라이버 목록과 드라이버를 설치하는 방법은 [여기에서 찾을 수 있습니다](https://developer.android.com/studio/run/oem-usb.html#Drivers).

또한 Android 기기 자체에서 '개발자 옵션'및 'USB 디버깅'을 사용 설정해야합니다. 이 작업을 수행하는 방법에 대한 정보는 [공식 문서](https://developer.android.com/studio/run/device.html)를 참조하십시오.

## iOS 설정 ## 
앱을 미리보고 iOS로 내보내려면 Mac OS X 및 Xcode가 필요합니다.
무료 Apple Developer 계정을 만들어 아래와 같이 Xcode의 설정 등록해야합니다.

![Alt text](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/401edb6f22e77628712f87ecc5b4bde4__media/preview-and-export-xcode-add-apple-id.webp)

## 미리보기 ##
로컬 및 Android / iOS 장치에서 응용 프로그램을 쉽게 미리 볼 수 있으며 UX 마크 업과 JavaScript의 변경 사항에 대한 즉각적인 피드백을 얻을 수 있습니다.

### Fuse 대시보드를 통한 미리보기 ###
대시 보드에서 프로젝트로 이동하여 "Start app preview" 버튼을 클릭하십시오. 미리보기를 시작할 대상 플랫폼을 선택하는 모달 창이 나타납니다.

### Sublime Text 에서 미리보기 ###
서브라임 텍스트에서 미리보기를 위해서는 사이드바에 있는 `ux` 혹은 `unoproject` 파일을 우클릭하여 `Fuse: Preview` 아래에 있는 타겟 플랫폼을 선택합니다.

![Sublime Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/04970d4c6ae8aa6d1827725e07de1844__media/preview-and-export-device-preview-osx-sublime-preview-menu.webp)

커맨드 팔레트로 미리보기를 시작할 수도 있습니다. `Cmd+Shift+P` (macOS) 혹은 `Ctrl+Shift+P` (Windows) 로 팔레트를 열고, 미리보기 타겟 목록을 보려면 `Fuse Preview` 를 타이핑합니다.

![Sublime Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/7548a0c1fe7bbef48688ceef464bb620__media/preview-and-export-device-preview-sublime-command-palette.webp)

### Atom 에서 미리보기 ###
커맨드 팔레트를 열기 위해 `Cmd+Shift+P` (macOS) 혹은 `Ctrl+Shift+P` (Windows) 를 누릅니다, 그리고 미리보기 타겟 목록을 보려면 `Fuse Preview` 를 타이핑합니다.

![Atom Preview](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/2a7f70bfe13262201cda43410d1c3872__media/preview-and-export-device-preview-atom-command-palette.webp) 

### 커맨드라인에서 미리보기 ###
터미널에서 `cd` 로 프로젝트 폴더로 들어가서 빌드 타겟에 해당하는 명령을 실행합니다.
```
fuse preview -t=Android
fuse preview -t=iOS
fuse preview -t=Local   # The -t=Local flag is optional in this case
```
`fuse help preview` 를 실행하여 `fuse preview` 관련 문서를 얻을 수 있습니다.

## 내보내기 ##
앱을 내보낼 때 UX 마크 업은 기본 C++ 코드로 컴파일되므로 앱을 실시간으로 미리 볼 수 없게 됩니다. 그러나 여러분의 응용 프로그램은 컴퓨터에 다시 네트워크 연결할 필요없이 자체적으로 작동합니다. 앱 배포시 여러분이 원하는 방식입니다.

### Android ###
쉘의 프로젝트 루트에서 아래 명령을 입력합니다.
```
fuse build --target=Android --run
```
이것은 연결되어진 안드로이드 디바이스로 배포되어 프로젝트를 시작할 것입니다.

릴리즈 빌드를 만들어 실행하기 위해사는:
```
fuse build --target=Android --configuration=Release
```
알림: 구글플레이 스토어에 앱을 내보내기 위해서는, [먼저 sign](https://www.fusetools.com/docs/preview-and-export/signing) 할 필요가 있습니다.

### iOS ###
쉘의 프로젝트 루트에서 아래 명령을 입력합니다.
```
fuse build --target=iOS --run
```
혹 XCode 안에서 생성된 프로젝트를 열기를 원한다면, `fuse build --target=iOS -adebug`를 실행합니다.

릴리즈 빌드를 만들어 실행하기 위해서는:
```
fuse build --target=iOS --configuration=Release
```
알림: 앱스토어에 앱을 내보내기 위해서는, [먼저 sign](https://www.fusetools.com/docs/preview-and-export/signing) 할 필요가 있습니다.

## 서명 ##
릴리즈를 위한 여러분 앱을 sign 하기 위한 안내를 위한 [서명하기 기사](https://www.fusetools.com/docs/preview-and-export/signing) 를 보십시오.
