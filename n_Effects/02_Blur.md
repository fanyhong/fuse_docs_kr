원본: https://www.fusetools.com/docs/fuse/effects/blur

흐림 클래스

 고급 기능 표시
이동 :
목차
비고
예제들
요소에 가우시안 블러를 적용합니다.

블러의 인터페이스

반경 : float UX
블러의 반경 / 크기
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
비고

Radius는 애니메이션 가능한 속성이지만 잠재적으로 값 비싼 작업입니다. 느린 장치에서 테스트하여 성능이 크게 저하되지 않도록하십시오.

예제들

다음 예제에서는 흐려진 사각형을 표시합니다.

<사각형 여백 = "40"Color = "# 9b59b6">
    <흐림 반경 = "15"/>
</ Rectangle>

