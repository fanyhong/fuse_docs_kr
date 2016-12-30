원본: https://www.fusetools.com/docs/assets/sketch-import

스케치에서 애셋 가져 오기

스케치는 Mac 용 훌륭한 벡터 디자인 도구입니다. 해보지 않았다면 확인해보십시오!.

퓨즈 수 :

.sketch 파일, 조각 이미지, 글꼴 및 바로 사용할 수있는 UX 태그에서 에셋 가져 오기.
초기 가져 오기 후 스케치 파일을 변경하면 실시간으로 자산을 업데이트합니다.
참고 : .sketch 파일 가져 오기는 Mac OS X (Sketch 자체와 유사)에서만 작동하며 현재 실험 중입니다.
모범 사례

Fuse에서 사용할 .sketch 파일을 작업 할 때는 다음 사항에 유의하십시오.

퓨즈 태그 이름은 스케치의 레이어 이름과 일치하므로 레이어 이름을 의미있게 유지하십시오.
대상 장치와 일치하는 대지 크기를 사용하십시오. 예를 들어, iPhone 6 용으로 설계하는 경우 Sketch에서 iPhone 6 대지 템플릿을 사용하십시오. 이렇게하면 내보낼 때 자산이 올바른 크기가되도록합니다.
스케치의 대지는 퓨즈의 페이지와 일치 할 수 있지만 (반드시 필요는 없습니다)
가져 오는 중

스케치 가져 오기 도구를 사용하려면 먼저 보헤미안 코딩으로 SketchTool을 설치해야합니다. 이 파일은 여기에서 다운로드 할 수 있습니다. 압축을 풀고 설치 스크립트를 실행하십시오.

스케치 문서를 가져 오려면, MyDesign.sketch라고 말하고 먼저 파일을 Fuse 프로젝트 폴더에 복사하십시오.

그런 다음 터미널 유형을 사용합니다.

퓨즈 가져 오기 MyDesign.sketch
가져 오기는 문서의 자산 금액에 따라 다소 시간이 걸릴 수 있습니다.

드래그 앤 드롭을 사용하여 가져 오기

터미널을 사용하는 대신에 .sketch 파일을 Fuse 미리보기 창으로 끌어서 놓을 수 있습니다. 이렇게하면 스케치 파일이 프로젝트의 루트 폴더에 복사되고 (아직없는 경우) 동일한 작업이 수행됩니다. 같은 이름을 가진 기존 .sketch 파일을 덮어 씁니다.

리소스 라이브러리

가져 오기가 완료되면 - 모두 정상적으로 진행된 경우 - 퓨즈가 .sketch 파일 옆에 MyDesign.sketch.ux라는 파일을 생성했습니다. 텍스트 편집기에서 열어서 내부 내용을 확인하십시오.

이 파일은 퓨즈가 스케치 문서에서 추출 할 수 있었던 모든 자산을 포함하는 리소스 라이브러리입니다. 프로젝트에 자동으로 포함되며 리소스를 사용할 수 있습니다.

자체적으로 리소스 라이브러리는 아무 작업도 수행하지 않고 내 보낸 앱 크기에 추가하지 않습니다. 다른 UX 파일에서 참조하는 자산 만 최종 응용 프로그램에 포함되며 나머지는 제거됩니다.

글꼴

퓨즈는 시스템에서 필요한 글꼴을 자동으로 추출하여 .sketch 파일 옆의 -assets / 폴더에 넣습니다. 또한 리소스 라이브러리의 전역 리소스로 선언합니다.

리소스 라이브러리에서 다음과 같이 보입니다.

<Font File = "MyDesign.sketch-assets / HelveticaNeue.ttf"ux : Global = "HelveticaNeue"/>
우리가 다음과 같이 사용할 수 있다는 것을 의미합니다 :

<Text Font = "HelveticaNeue"> 안녕하세요! </ Text>
이미지

스케치 문서의 이미지는 Image를 확장하고 MultiDensityImageSource를 사용하여 렌더링 된 에셋을 가리키는 클래스 (ux : Class)로 표시됩니다.

클래스 이름은 스케치의 도면층 이름에 해당합니다. MyDesign.sketch라는 파일에 SomeGroup이라는 레이어 그룹과 SomeLayer라는 레이어가 포함 된 Screen1이라는 아트 보드가 포함 된 경우이 이미지의 인스턴스에서 다음과 같이 만들 수 있습니다.

<MyDesign.Screen1.SomeGroup.SomeLayer />
이 태그는 이미지이므로 Image와 동일한 속성을 지원합니다 (예 : Alignment, Margin, StretchMode 및 Scale9Margin.

이것은 리소스 라이브러리 파일 (.sketch.ux)에서 다음과 같은 생성 된 클래스를 갖기 때문에 가능합니다.

<Image ux : Class = "MyDesign.Screen1.SomeGroup.SomeLayer">
  <MultiDensityImageSource>
   <FileImageSource File = "MyDesign.sketch-assets/MyDesign.Screen1.SomeGroup.SomeLayer@1x.png"Density = "1.0"/>
   <FileImageSource File = "MyDesign.sketch-assets/MyDesign.Screen1.SomeGroup.SomeLayer@2x.png"Density = "2.0"/>
  </ MultiDensityImageSource>
</ Image>
이미지 파일은 .sketch 파일 옆의 MyDesign.sketch-assets / 폴더에 저장됩니다.

자동 생성 된 앱 레이아웃

퓨즈는 가져온 리소스 라이브러리를 기반으로 응용 프로그램 레이아웃을 생성 할 수 있으므로 스케치 디자인과 똑같은 퓨즈 스크린을 얻을 수 있습니다. 여기에는 도형 및 이미지 외에도 Text 요소가 포함되어있어 반응 형 앱 레이아웃을 만드는 데 좋은 출발점이 될 수 있습니다.

가져 오기를 통합하기 위해 --app 또는 (--overwrite-app) 옵션을 지정하여이 작업을 수행 할 수 있습니다. 예:

퓨즈 가져 오기 MyDesign.sketch --app MyDesign.ux
그러면 MyDesign.ux가 생성됩니다. MyDesign.ux에는 App 태그와 PageControl이 있고 스케치 파일에 각 대지에 대해 하나의 페이지가 포함됩니다.

퓨즈는 스케치 파일에서 가능한 한 정확하게 레이아웃을 재현합니다. 이는 현재 스케치 파일을 정확하게 일치시키는 모든 요소의 절대 위치 지정을 의미합니다.

퓨즈는 레이아웃 규칙을 자동 감지하도록 UX 코드를 최적화하는 도구 등에서 작업하고 있습니다. 업데이트를 다시 확인하십시오!

자산의 실시간 업데이트

원래 가져 오기 후 fuse 가져 오기를 다시 실행하면 리소스 라이브러리가 최신 자산으로 업데이트 (덮어 쓰기)되고 프로젝트에서 사용중인 모든 자산이 모든 미리보기 세션에서 자동으로 업데이트됩니다.

스케치 도면층의 이름이 변경되지 않은 경우 클래스의 이름은 동일하게 유지됩니다.

이미지 밀도

우리는 옵션을 가져 오기 명령에 전달하여 스케치 문서를 가져올 때 퓨즈가 생성하는 이미지 밀도를 제어 할 수 있습니다. 예 :

퓨즈 가져 오기 MyDesign.sketch - 1x - 1.5x - 2x
모든 이미지 애셋을 각각 1.0, 1.5 및 2.0 밀도로 가져옵니다. 아무것도 지정하지 않으면 퓨즈는 기본적으로 1.0 및 2.0 밀도를 렌더링합니다.

자산 앨리어싱

때로는 .sketch 파일을 가져올 때 자산에 길고 비실용적 인 이름이 많이 나타날 수 있습니다. 이는 아무도 문서가 만들어 졌을 때 퓨즈에 문서를 가져 와서 레이어 이름을 제대로 지정하지 않았다는 것을 아무도 생각하지 않았기 때문입니다.

일반적으로 다음과 같은 것들을 얻습니다 :

<이미지 ux : Class = "activity.Activity ___ 6.Rectangle_91 ___ Bitmap_2">
    <MultiDensityImageSource>
        <FileImageSource File = "activity.sketch-assets / Activity ___ 6.Rectangle_91___Bitmap_2@1x.png"밀도 = "1"/>
        <FileImageSource File = "activity.sketch-assets / Activity ___ 6.Rectangle_91___Bitmap_2@2x.png"밀도 = "2"/>
    </ MultiDensityImageSource>
</ Image>
이렇게하면 UX 코드를 사용하기 시작할 때 UX 코드가 좋지 않게 보이게됩니다. 동시에 클래스의 이름을 바꾸고 싶지는 않습니다. 왜냐하면 .sketch 파일에 대한 링크가 없어지고 더 이상 실시간 업데이트를 얻을 수 없기 때문입니다.

이를 처리하기 위해, 예를 들어 AssetAliases.ux와 같은 추가 UX 파일을 만드십시오. 예를 들어 Panel을 루트 태그로 사용할 수 있습니다.

위의 예에서이 비트 맵은 실제로 전체 화면의 배경임을 알고 있습니다. 그러므로 ActivityBackground와 같이 더 합리적인 이름을 지정할 수 있습니다.

AssetAliases.ux에서 :

<패널>
    <activity.Activity ___ 6.Rectangle_91 ___ Bitmap_2 ux : Class = "ActivityBackground"/>
</ Panel>
앨리어스하려는 다른 모든 애셋은이 목록에 추가 할 수 있습니다.

그런 다음 실제 앱 코드에서이 훨씬 좋은 태그를 사용할 수 있습니다.

<ActivityBackground />
