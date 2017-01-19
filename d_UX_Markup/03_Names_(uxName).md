원본: https://www.fusetools.com/docs/ux-markup/names

로컬 이름 (ux : Name)

UX Markup의 ux : Name 속성은 UX 객체에 로컬 이름을 제공하는 데 사용됩니다.

통사론

<type ux : Name = "node_name"/>
여기서 type은 UX 마크 업에서 액세스 할 수있는 모든 유형이며 node_name은 유효한 Uno 식별자입니다.

비고

UX 마크 업의 모든 객체 (XML 요소)에 ux : Name 속성을 지정하여 로컬 이름을 지정할 수 있습니다.

<Panel ux : Name = "panel1"/>
이름이 주어지면 동일한 범위 내에서 해당 이름으로 객체를 참조 할 수 있습니다. 모든 UX 표현식은 일반 이름으로이 객체를 참조 할 수 있습니다. 로컬 이름은 동일한 이름을 가진 글로벌 리소스보다 우선합니다.

<Reactangle LayoutMaster = "panel1"/>
이름은 다음과 같이 UX Express에서 direclty로 사용할 수 있습니다.

<사각형 너비 = "너비 (패널 1) / 2"
ux : Name은 컴파일 타임 속성이므로 데이터 바인딩 될 수 없습니다. 데이터 바인딩은 런타임에 발생합니다.
유효한 이름 (Uno 식별자)

이름은 유효한 Uno 식별자 여야합니다. 이는 문자 (A-Z 또는 a-z), 숫자 및 밑줄 만 포함 할 수 있으며 숫자로 시작할 수 없음을 의미합니다.
