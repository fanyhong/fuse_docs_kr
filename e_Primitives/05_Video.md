원본: https://www.fusetools.com/docs/fuse/controls/video

비디오 클래스

 고급 기능 표시
이동 :
목차
비디오를 표시합니다.

비디오는 File 또는 Url 속성을 통해 파일 또는 스트림에서 비디오를 재생할 수 있습니다. 이미지와 비슷합니다. StretchMode, StretchDirection 및 ContentAlignment 속성을 공유하며 두 클래스 모두 동일한 방식으로 작동합니다.

유용한 속성

비디오에는 Image와 공유되는 속성 외에 속성을 구성하거나 제어하는 ​​데 사용할 수있는 속성 집합이 함께 제공됩니다.

볼륨 : 0.0 ~ 1.0의 범위, 기본값은 1.0입니다.
재생 시간 : 동영상 재생 시간 (초)
위치 : 동영상의 현재 위치 (초)
IsLooping : 비디오가 반복되는지 여부를 지정하는 부울입니다. 기본값은 false입니다.
비디오와 함께 사용할 수있는 유용한 트리거

<비디오>
    <WhilePlaying /> <! - 동영상 재생 중 활성 ->
    <WhilePaused /> <! - 동영상 일시 중지 중 활성 ->
    <WhileCompleted /> <! - 비디오 재생이 끝나면 활성입니다. ->
    <WhileLoading /> <! - 동영상로드 중 활성 ->
    <WhileFailed /> <! - 동영상로드 실패 또는 오류 발생시 활성 ->
</ Video>
비디오를 제어하는 ​​데 사용할 수있는 유용한 작업

퓨즈에는 비디오 재생을 제어하는 ​​데 사용할 수있는 일련의 작업이 있습니다. 그들은 모두 자신이 제어하는 ​​Video 요소를 지정하는 공통의 Target 속성을 가지고 있습니다.

<Pause /> <! - 현재 위치를 그대로두고 재생을 일시 중지합니다. ->
<Stop /> <! - 재생을 중지하고 비디오 시작 부분으로 돌아갑니다. ->
<Resume /> <! - 현재 위치에서 재생 재개 ->
지원되는 형식

비디오는 내보내기 대상에서 제공하는 비디오 코드를 사용하여 구현되므로 플랫폼이 지원하는 모든 것을 지원합니다. Windows, OS X, Android 및 iOS는 일부 형식에 대한 지원을 공유하지 않을 수 있습니다.

Android 지원 형식
iOS 및 OS X에서 지원하는 형식 ( 'public.movie'아래에 있음)
Windows 지원 형식
로컬 파일 시스템에서 재생

동영상은 앱이 실행되는 기기의 로컬 파일 시스템에서 재생할 수도 있습니다. file : //을 비디오의 절대 경로 앞에 추가하여 수행 할 수 있습니다.

<Video File = "file : ///data/data/com.fuse.app/video.mp4"/>
시작시 세 개의 슬래시를 확인하십시오. 이것은 유닉스 파일 시스템 경로가 항상 /

예

다음 예제는 비디오를 재생하고, ProgressAnimation을 사용하여 재생 진행률을 표시하고, 일시 중지 및 다시 시작 애니메이터를 사용하여 비디오를 일시 중지 / 다시 시작하는 방법을 보여줍니다.

<DockPanel>
    <Video ux : Name = "video"Dock = "Fill"File = "fuse_video.mp4"IsLooping = "true"StretchMode = "UniformToFill">
        <ProgressAnimation>
            <progressBar.Width = "100"/> 변경
        </ ProgressAnimation>
    </ Video>
    <Rectangle ux : Name = "progressBar"Dock = "Bottom"Fill = "# f00"Width = "0 %"Height = "10"/>
    <Grid Dock = "Bottom"ColumnCount = "2"RowCount = "1">
        <Button Text = "재생">
            <클릭>
                <Resume Target = "video"/>
            </ Clicked>
        </ Button>
        <Button Text = "일시 중지">
            <클릭>
                <Pause Target = "video"/>
            </ Clicked>
        </ Button>
    </ Grid>
</ DockPanel>