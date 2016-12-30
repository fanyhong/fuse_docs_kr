원본: https://www.fusetools.com/docs/fuse/effects/halftone

하프 톤 클래스

 고급 기능 표시
이동 :
목차
예제들
요소에 클래식 하프 톤 효과를 적용합니다.

하프 톤의 인터페이스

DotTint : float UX
도트가 원본 요소의 원래 색조에 채색되어야하는 정도를 제어합니다.
Intensity : float UX
각 점의 기준선 크기입니다.
PaperTint : float UX
배경이 원본 요소의 원래 색조로 채워지는 정도를 제어합니다.
부드러움 : float UX
점 가장자리의 매끄러움
간격 : float UX
각 점 사이의 공간입니다.
Effect에서 상속 된
RenderBoundsChanged : 액션 <효과> (효과) UX
RenderingChanged : 액션 <효과> (효과) UX
노드에서 상속됩니다.
findData (기호) JS
노드의 데이터 컨텍스트에서 주어진 심볼에 대한 데이터의 관찰 가능 항목을 반환합니다.
바인딩 : IList Binding의 IList
이 노드에 속하는 바인딩 목록입니다.
이름 : Selector UX
노드의 런타임 이름. 이 등록 정보는 ux : Name 속성을 사용하여 자동으로 설정됩니다.
첨부 된 UX 속성
GlobalKey (리소스에 의해 첨부 됨) : string UX
ux : Global 속성은 UX 마크 업에서 모든 곳에서 액세스 할 수있는 글로벌 리소스를 만듭니다.
예제들

아래 예제는 이미지에 적용된 하프 톤 효과를 보여줍니다.

<Image File = "cat.jpg"StretchMode = "UniformToFill">
    <하프 톤 DotTint = "0"PaperTint = "0.95"Intensity = "0.9"Smoothness = "2"Spacing = "15"/>
</ Image>
