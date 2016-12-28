원본: https://www.fusetools.com/docs/basics/faq

# 자주 묻는 질문 #

## 지원하는 플랫폼 ##
Fuse 및 내보내기가 지원되는 플랫폼 전체 목록은 [여기](https://www.fusetools.com/docs/basics/supported-platforms) 서 찾을 수 있습니다.

## 일반적인 팁 ##

### Fuse 업그레이드하기 ###
프로젝트를 마지막으로 빌드한 이 후에 Fuse를 업그레이드 하셨다면, 빌드 전에 프로젝트를 정리하십시요. (터미널의 프로젝트 폴더에서 `uno clean`을 실행하십시오.)   

---

## 로그들 ##
이슈들에 대한 조사를 할때 Fuse와 에디터 플러그인들로부터 작성된 로그들을 확인하는 것은 유용한 경우가 많습니다. 
- macOS: `~/.fuse/logs`
- Windows: `%localappdata%\Fusetools\Fuse\logs`

---

### Text.Padding이 텍스트에 패딩을 추가하지 않는 이유는 무엇입니까? ###
`Padding` 은 요소의 자식에 적용됩니다. 텍스트는 `<Text>` 요소 자체의 일부이며 자식 요소는 아닙니다. 텍스트 주변에 패딩을 넣으려면 포함하는 요소에 `Padding` 을 대신 넣으십시오.
```
<Panel Padding="10" Color="#9f9" Alignment="Center">
    <Text Alignment="Center" Value="text"/>
</Panel>
```
![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/e017310d47eaae649f808ae4c9febe06__media/faq-text-padding.webp)

(위의 `<Panel>` 로 부터 `Padding`을 제거하고 대신 `<Text>` 에서 `Margin` 을 설정하는 것은 같은 효과를 가집니다.) 

## 연결 관련 이슈들 ##

### 네트워크 에러 발생 ###
이런 메시지를 받습니다. `fuse: A network error occurred: Could not resolve host '<your hostname>' Please check your network setup and try again.`

#### 해결책 ####
네트워크에 문제가 있습니다. 터미널에서 `ping $(hostname)`을 시도하세요. 오류 메시지가 나타나면 계속하기 전에 네트워크 설정을 수정해야합니다.

---

### 데몬에 접속 실패 ###
어딘가에서 `FailedToConnectToDaemon` 로 스택 트레이스를 얻습니다.

#### 해결책 ####
- 먼저, Fuse 데몬을 중지합니다.
   - Windows: Fuse 트레이아이콘을 마우스 우클릭한 뒤 "Exit"를 클릭합니다.
   - macOS: 메뉴바에서 Fuse를 컨트롤 클릭 한뒤 "Quit"를 클릭합니다.
- 새로운 데몬은 다음에 여러분이 Fuse를 사용하려할때 할때 자동적으로 시작됩니다.
- 만약 도움이 되지 않았다면, `fuse kill-all` 을 실행해 보십시오. 모든 실행중인 Fuse 프로세스들이 종료됩니다.
- 만약 도움이 되지 않았다면, Windows / macOS 에 로그아웃 했다가 다시 로그인해 보십시오.
- 만약 도움이 되지 않았다면, 컴퓨터를 다시 시작해 보십시오.

---

### 미리보기 - 접속 실패 ###
iOS 와 안드로이드 디바이스에서 미리보기 하는 동안에, "Failed to connect" 메시지가 표시됩니다.

#### 해결책 ####
- 디바이스 WiFi가 켜져 있는지 확인합니다.
- 디바이스가 Fuse를 실행하고 있는 컴퓨터와 같은 WiFi 에 연결되어 있는지 확인합니다.
- 방화벽(윈도우즈 방화벽 같은)이 동작하고 있는 컴퓨터 라면, Fuse를 incoming 연결로 접속하도록 허용했는지 확인합니다. 
- 안드로이드 디바이스에서 테더링을 사용하고 있다면(USB를 통해 모바일 네트워크를 공유하는 경우), 끄고 시도합니다.
- 여전히 문제가 있다면, 트레이아이콘이나 메뉴바에서 Fuse를 종료하고 미리보기를 다시 시작합니다. 

---

## 안드로이드로 미리보기와 내보내기 불가 ##

### 증상들 ###
- 안드로이드 빌드가 멈추고 "Trying to uninstall existing version of APK" 메시지가 표시됩니다.
- 빌드가 "ERROR: No android devices found." 와 함께 완료됩니다.

#### 해결책 ####
- USB 케이블로 디아이스가 연결되어 있는지 확인합니다.
- 디바이스에서 USB 디버깅을 사용하도록 설정했는지 확인합니다. 이 작업은 디바이스들과 OS 버전들 간에 다를 수 있으므로 아마 여러분 디바이스를 위한 특정 명령들을 검색해보셔야 할 수도 있습니다. [여기](http://developer.android.com/tools/device.html#setting-up) 두번째 포인트 지점이 시작하기에 좋은 지점입니다.
- `uno adb devices`를 실행할때 여러분의 디바이스가 나타나는지 확인하십시오.
- 여러분이 윈도우즈 사용자라면 여러분 디바이스의 최신 USB 드라이버가 설치되어있는지 확인하십시오. [여기](http://developer.android.com/tools/extras/oem-usb.html) 이 것에 대해 더 읽어보십시오. 

---

## iOS 빌드 불가 ##

### 증상들 ###
- iOS 빌드가 이런 메시지와 함께 멈춥니다: `No code signing identities found: No valid signing identities (i.e. certificate and private key pair) were found.`

#### 해결책 ####
- Xcode 를 실행합니다.
- 새 iOS blank 프로젝트를 하나 생성하고 그것을 빌드해 봅니다.
- 다음과 같은 에러 `No provisioning profiles matching an applicable signing identity were found.` 가 나타나면, `Fix Issue` 를 클릭하고 창을 닫습니다.
- Xcode 를 닫습니다.
- 이제 Fuse에서 여러분의 빌드와 미리보기를 다시 시작합니다.

---

## Sublime 플리그인 동작 불가 ##

### 증상들 ###
- "Error loading: Packages/Fuse/UX.tmLanguage" 메시지가 표시됩니다.
- 서브라임에서 문법 하이라이팅 기능이 동작하지 않습니다.

#### 해결책 ####
- 대시보드를 열어, "Sublime Text Setup" 을 클릭하고 팝업 윈도우을 따릅니다.
- Sublime Text 가 기본 경로에 설치되어 있는지 확인합니다. `/Applications/Sublime\ Text.app`
- 만일 동작하지 않았다면, `%APPDATA%\Sublime Text 3\Installed Packages` (Windows) 나 `~/Library/Application Support/Sublime Text 3/Installed Packages` (macOS) 에서 `Fuse`로 시작하는 파일들을 삭제하고 다시 시도합니다.

---

## Sublime 플러그인이 Fuse를 찾을 수 없음 ##

### 증상들 ###
- "Fuse could not be found" 메시지가 표시됩니다.

### 해결책 ###
- Fuse가 설치되어 있는지 확인합니다.
- Fuse가 최근에 설치된 경우 Sublime을 다시 시작해 봅니다.
- Fuse가 올바른 경로에 있는지 확인합니다. 터미널을 열고 `fuse --version` 을 실행한 뒤, fuse 버전이 출력되는지 확인합니다.
- 이전 단계가 작동하지 않으면 로그아웃 했다가 다시 로그인해서 시도해 봅니다.
- Fuse가 Sublime이나 명령 줄에서 여전히 발견되지 않고 Windows를 사용중인 경우 설치 프로그램이 `PATH` 환경 변수를 업데이트하지 못했을 수 있습니다. 일반적으로 `C:\Users\<your username>` 의 `<user directory>` 에 위치한 `PATH` 환경 변수에 `<user directory>\AppData\Local\Fusetools\Fuse\App\Bin` 를 [추가](http://www.computerhope.com/issues/ch000549.htm#windows8) 합니다.

---

## 미리보기에서 Uno 코드 실행 및 업데이트 불가 ##

### 증상들 ###
- 미리보기에서 여러분의 Uno 코드 변경이 반영되지 않습니다.
- Uno 코드가 미리보기에서 실행되어지지 않았습니다.

#### 이유 ####
- UX/JavaScript 와 달리, Uno-code 는 저장하고 자동 새로고침 할 때 미리보기를 새로고침 하지 않으므로 앱을 다시 빌드해야 합니다.
   - Windows: Fuse 트레이아이콘을 우클릭한 뒤 "Exit"를 클릭합니다.
   - macOS: 메뉴바에서 Fuse를 컨트롤 클릭 한뒤 "Quit"를 클릭합니다.
- 추가적으로 `ux.uno` 파일들 안의 Uno-code는 미리보기에서 전혀 실행되지 않습니다.
- 결과적으로 `uno build` 를 통한 Uno 코드 작업이 미리보기를 통한 것 보다 더 쉬울 수 있습니다. 이를 위한 더 나은 해결책이 곧 제공 될 것입니다.

---

## 미리보기에서 "Oops! Something went wrong here" 발생 ##
여러분 앱을 미리보기 하고 있을때, 화면에 "Oops! Something went wrong here" 가 나타납니다.

#### 해결책 ####
- 그 문제에 대한 자세한 것을 보기 위해 Fuse [모니터](https://www.fusetools.com/docs/preview-and-export/monitor) 를 엽니다.

---

## Windows 에서 로컬 미리보기가 시작되지 않음 ##
만약 콘솔에서의 출력이 `GL_VERSION: 1.1.0` 및 `GL_RENDERER: GDI Generic` 이 포함되어 있으면, 대개 OpenGL 드라이버들이 없거나 만료된 것과 같은 문제입니다. 여러분의 그래픽 어댑터를 가장 최신 드라이버로 업그레이드하고 다시 시도해 보십시오.
이 문제는 또한 Intel HD Graphics 2000 / 3000 / 4000 그래픽스 어댑터들에서 윈도우즈10 하위 드라이버 문제로 인해 발생할 수도 있습니다. 이 경우에는 즉각적인 업데이트들로 로컬 미리보기를 하는 것은 불가능하지만, 여러분의 PC에서 일반 빌드(`fuse build -t=dotnetexe --run`) 를 함으로써 테스트가 가능 합니다.
물론 [안드로이드 및 iOS 디바이스 미리보기](https://www.fusetools.com/docs/basics/preview-and-export) 를 사용 할 수도 있습니다.

## 버그 리포트하는 방법 ##
버그를 발견했다고 생각하시는 분은 [포럼](https://www.fusetools.com/community/forums/bug_reports) 으로 버그 리포트를 보내 주시면 알려 주시면 감사하겠습니다. 문제에 대한 정보가 많을수록 문제를 쉽게 해결할 수 있습니다. 따라서 우리는 버그 보고서에 포함시켜야하는 작은 정보 목록을 작성했습니다.  
1. 여러분이 사용하고 있는 Fuse 버전은 무엇입니까? 터미널에서 `fuse --version`을 실행한 출력을 복사해 주십시오.
2. 여러분의 운영체제와 그것의 버전은 무엇입니까?
3. 여러분이 모바일 디바이스에서 테스트하고 있다면 - 그것은 어떤 디바이스 이고 어떤 OS 버전이 돌아가고 있습니까?
4. 문제가 발생하는 대상은 무엇입니까? 예를 들어 Android와 iOS 모두에서 테스트 중이라면 두 플랫폼 모두에서 문제가 발생합니까?
5. 미리보기와 빌드를 내보내기 할때 모두 여러분의 이슈가 발생되는지 확인하십시오.
    - 로컬 미리보기 - `fuse preview`를 실행합니다.
    - 디바이스 미리보기 - `fuse preview -tios` 혹은 `fuse preview -tandroid`를 실행합니다.
    - 빌드 내보내기 - `fuse build -tios` 혹은 `fuse build -tandroid`를 실행합니다.
6. 버그를 재현할 수 있는 최소한의 프로젝트 코드 복사 및 지침들(구체적으로)  
    - 복제의 경우는 이 링크로 저희에게 안전하게 업로드 될 수 있습니다: https://www.dropbox.com/request/ZgndLtJQm5eGzG9cicGK
    - 복제의 경우 가 꽤 작다면, 여러분의 포럼에 스페이스 4칸을 사용한 포스트로 여러분의 코드를 붙여 넣을 수 있습니다.
    - 가능하다면 프로젝트를 작게 만들어 주십시오: 이슈를 재현 할 수 있는데 필요한 코드들만 포함해 주십시오. 
    - zip 등으로 압축하기 전에, 여러분의 터미널에서 캐시된 빌드와 다른 파일크기를 늘리는 것들을 제거하는 `uno clean`을 실행하여 주십시오.  

---
