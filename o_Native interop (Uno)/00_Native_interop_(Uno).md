원본: https://www.fusetools.com/docs/native-interop/native-interop

네이티브 interop

퓨즈를 사용하여 앱을 개발하는 데는 UX와 JavaScript를 사용하는 것이 좋지만 퓨즈는 기본 코드, API 및 기술을 기반으로하며 사용자 정의 원시 코드 및 기능으로 확장 할 수 있습니다.

우노

우노 (Uno)는 퓨즈를 구동하는 C #과 같은 프로그래밍 언어입니다. 모든 퓨즈 코어 클래스는 Uno로 작성되며, Uno는 디바이스에서 실행될 때 C ++로 컴파일됩니다. Uno는 구문 및 형식 시스템의 대부분을 C #과 공유하므로 Uno를 배우는 것은 C #을 학습하는 것과 거의 동일합니다.

일반 UX 마크 업과 JavaScript 파일 외에도 프로젝트에 Uno 코드를 추가 할 수 있습니다.

Uno와 C #의 유사점과 차이점에 대한 정보는 Uno 언어 참조를 확인하십시오.

Uno를 사용하기 위해 무엇을 할 것인가?

새로운 네이티브 API 또는 타사 SDK 래핑 및 새 구성 요소를 UX 마크 업 또는 JavaScript에 대한 새 API에 표시
기존 Java 또는 Objective-C / Swift 코드와 상호 운용
극히 높은 성능을 필요로하는 작은 코드 조각
사용자 정의 GPU 그래픽 렌더링 및 효과
우노를 사용하지 않을 것

비즈니스 로직. 대신 JavaScript를 사용하십시오. JavaScript는 자체 스레드에서 실행되므로 UI ​​성능이 저하되지는 않습니다.
UX 구성 요소 작성. UX 마크 업을 대신 사용하십시오. 컴파일 할 때 모든 UX 마크 업은 Uno 코드로 변환되므로 걱정하지 마십시오.
Uno 파일을 프로젝트에 추가하기

Uno 파일을 추가하려면 프로젝트 폴더에 확장명이 .uno 인 빈 텍스트 파일을 만들고 프로젝트 파일 (.unoproj)에 Includes 배열에 추가하기 만하면됩니다. 항목은 "<filename> : SourceFile"형식이어야합니다.

또는 fuse create uno <파일 이름>을 입력하십시오. 이렇게하면 템플릿을 사용하여 자동으로 uno 파일이 만들어져 프로젝트에 추가됩니다.

자바 스크립트와 달리 Uno 코드는 C ++로 컴파일되며 앱이 미리보기에서 실행되는 동안 변경할 수 없습니다. Uno 파일을 변경하면 변경 사항을 적용하려면 퓨즈 미리보기를 다시 시작해야합니다.

Uno API 설명서

Uno API와 UX Markup API는 문자 그대로 똑같습니다. 전체 클래스 참조에서 네임 스페이스로 API를 탐색 할 수 있습니다.

기본적으로 문서 페이지에는 UX 마크 업과 JavaScript에서 액세스 할 수있는 멤버 만 표시됩니다. Uno 프로그래머 관점에서 볼 때 클래스의 모든 구성원을 표시하는 "고급 사항 표시"확인란이 있습니다.

외국 코드

Uno는 네이티브 API 또는 기존 코드 기반을 쉽게 래핑 할 수 있도록 외부 코드라는 C #의 수퍼 집합을 사용하여 Java, Objective-C 및 C ++와 원활하게 상호 작용합니다.

자세한 내용은 외부 코드 섹션을 참조하십시오.
