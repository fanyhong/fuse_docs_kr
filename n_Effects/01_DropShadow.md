원본: https://www.fusetools.com/docs/fuse/effects/dropshadow

DropShadow 클래스

 고급 기능 표시
이동 :
목차
예제들
DropShadow는 기본 그림자를 요소에 적용합니다.

DropShadow의 인터페이스

각도 : float UX
빛이 요소에 닿는 각도 (도)입니다.
색상 : float4 UX
그림자의 색입니다.
거리 : float UX
그림자가 소스에서 오프셋되어야하는 거리 (포인트)입니다.
크기 : float UX
포인트의 그림자의 크기.
스프레드 : float UX
그림자가 떨어지는 방법을 제어합니다. 0에 가까울수록 선형이 커집니다. 이 값을 낮게 유지 (1.0 이하로 실험)하면 아티팩트가 생성됩니다.
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

다음 예제에서는 그림자가있는 사각형을 표시합니다.

<사각형 여백 = "40"Color = "# 9b59b6">
    <DropShadow Size = "10"Distance = "3"Spread = "0.05"Color = "# 0008"Angle = "90"/>
</ Rectangle>
또는 가장 간단한 형태로 :

<직사각형 여백 = "40"Color = "# fff">
    <DropShadow />
</ Rectangle>
