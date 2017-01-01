원본: https://www.fusetools.com/docs/ux-markup/globals

글로벌 (ux : 글로벌)

UX Markup의 ux : Global 속성은 해당 요소가 정적 전역 자원으로 변환됩니다.

정적 전역 리소스는 프로젝트에서 로컬로 사용되거나 다른 프로젝트에서 참조 할 수있는 리소스 라이브러리를 정의하는 데 사용됩니다.

통사론

<type ux : Global = "resource_key"[ux : Value = "value"] ... />
여기서 type은 UX Markup에서 사용할 수있는 모든 유형이며 resource_key는 임의의 문자열입니다.

엄격하게 요구되는 것은 아니지만 유효한 Uno 식별자로 구성된 resource_key를 마침표로 구분하여 사용하는 것이 좋습니다. 네임 스페이스 용.
유형이 값 유형 (예 : float4 또는 int)이면 ux : Value 속성을 지정해야합니다.

예제들

예를 들어, Fuse는 Red 및 Blue와 같은 공통 색상 이름에 대한 전역 자원을 정의합니다. 이름은 다음과 같이 나타낼 수 있습니다.

<패널 색상 = "파랑"/>
ux : Global attribtue를 사용하여 모든 유형의 사용자 정의 글로벌 리소스를 정의 할 수 있습니다.

<float4 ux : Global = "MyProject.WarmBlue"ux : Value = "# 18f"/>
그런 다음 어디에서나 사용할 수 있습니다.

<직사각형>
    <스트로크 너비 = "3"Color = "MyProject.WarmBlue"/>
</ Rectangle>
글로벌 리소스 이름에는 마침표가 포함될 수 있습니다. 프로젝트, 회사 또는 컨텍스트에 따라 그룹화 할 때 리소스 이름에 마침표를 사용하는 것이 좋습니다.

글로벌 리소스는 컴파일시 해결되며 동적으로 변경할 수 없습니다. 동적 리소스의 경우 참고 자료를 참조한다.

기본 리소스로 전역

ux : Global 속성은 또한 자원 바인딩에 대한 전역 기본값을 정의합니다. 자세한 정보는 ux : Key의 docs를 참조하십시오.
