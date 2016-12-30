원본: https://www.fusetools.com/docs/native-interop/preliminary-gradle-support

예비 거들 지원

돌아간 좋은 구글은 선택 도구로 ANT에서 Gradle로 전환했다. 불행히도 당시 그들은 NDK 응용 프로그램을 빌드하는 것을 지원하지 않았습니다. ndk 기능이 착륙 한 후에도 실험적으로 고려되었으며, 최근까지는 우리가 전환하기에 충분히 안정적이라고 여겨 질 수 없었습니다.

우리는 Gradle로부터 신뢰할 수있는 빌드를 얻을 수있는 시점에 있으며 가능한 한 빨리 기본값으로 사용하려고합니다.

이를 위해 빌드 플래그 -DGRADLE을 사용하여이 예비 기능을 사용할 수 있도록하고 있습니다.

Gradle 빌드 시도

gradle로 빌드하려면 빌드에서 -DGRADLE 플래그를 지정하십시오. 또한 프로젝트의 빌드 도구 버전과 대상 버전을 지정하는 것이 좋습니다.

예 :

fuse build -t = android -DGRADLE -r --set : SDK.BuildToolsVersion = "23.0.3"--set : SDK.CompileVersion = "23"--set : SDK.TargetVersion = "23"`
설치된 도구 버전을 확인하려면 다음을 실행하십시오.

우노 안드로이드
당신의 터미널에서. 그러면 Android SDK 관리자가 열립니다. 여기에서 우리는 다른 버전을 확인하고 설치할 수 있습니다.

종속성 지정

프로젝트에 그라디언트 종속성을 추가하려면 다음 특성을 Uno 클래스에 추가하십시오

[Require ( "Gradle.Dependency.Compile", "com.github.lecho : hellocharts-library : 1.5.8@aar")]
com.github.lecho : hellocharts-library : 1.5.8@aar를 원하는 라이브러리와 교체하십시오.

추가 저장소 추가

추가 저장소를 지정하려면 Uno 클래스에 다음 속성을 추가하십시오.

[Require ( "Gradle.Repository", "maven {url 'https://maven.fabric.io/public'}")]
build.gradle 파일의 model.repositories 섹션에 저장소를 추가해야하는 경우 다음을 사용하십시오.

[Require ( "Gradle.Module.Repository", "maven {url 'https://maven.fabric.io/public'}")]
Android Studio의 메모

기본적으로 Gradle을 사용할 때 Fuse는 Gradle을 지원하지만 Android Studio를 명시 적으로 지원하지는 않습니다. 이것으로 우리는 각 릴리스를 출시 할 때 우리가 알 수있는 지식 프로젝트가 올바르게 gradle로 컴파일되도록 보장하지만 Android Studio에 대해 동일한 주장을하지는 않습니다.

그 이유는 안드로이드 스튜디오가 너무 빨리 바뀌어서 변화가 일어나기 쉽지 않기 때문입니다. 변경 사항은 흔히 발생하며 현재 출시되는 리듬을 사용하여 이러한 변경 사항을 테스트하기가 어렵습니다.

그러나 우리는 -d 빌드 플래그를 사용하여 Android Studio에서 빌드를 열 수 있습니다. 업데이트가있는 경우 작업을 수행하려면 두세 개를 뛰어 넘을 필요가 있습니다.

