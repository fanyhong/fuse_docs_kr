원본: https://www.fusetools.com/docs/fuse/transform

Transforms Transform 클래스

 고급 기능 표시
이동 :
목차
비고
변환은 퓨즈 레이아웃 엔진에 의해 할당 된 배치를 넘어서 요소를 이동, 회전, 크기 조정 및 기울이기 위해 사용됩니다.

변환은 다른 요소 및 트리거와 마찬가지로 요소에 추가됩니다.

예

이 예에서는 원래 크기의 3 배가되도록 원의 배율을 조정합니다.

<Circle Color = "Green"Width = "50"Height = "50">
    <스케일링 계수 = "3"/>
</ Circle>
사용 가능한 변환

로테이션 UX
요소를 지정된 각도만큼 회전합니다.
UX 확장
지정된 요소로 요소를 확대하거나 축소합니다.
Shear UX
시각적으로 전단력을 적용합니다 (비뚤어 짐). 전단을 애니메이션으로 만들려면 대신 Skew 애니메이터를 사용하십시오.
번역 UX
공간의 선형 오프셋을 나타냅니다. 애니메이션 번역의 경우이 클래스의 속성에 애니메이션을 적용하는 대신 Move 애니메이터를 사용하는 것이 좋습니다.
변환 인터페이스

노드에서 상속됩니다.
findData (기호) JS
노드의 데이터 컨텍스트에서 주어진 심볼에 대한 데이터의 관찰 가능 항목을 반환합니다.
바인딩 : IList Binding의 IList
이 노드에 속하는 바인딩 목록입니다.
이름 : Selector UX
노드의 런타임 이름. 이 등록 정보는 ux : Name 속성을 사용하여 자동으로 설정됩니다.
비고

왜 그냥 이동, 크기 조정 및 회전하지 않습니까?

동일한 요소에 대해 여러 변형을 수행하려는 경우 적용되는 순서가 중요합니다. 변환 추가에 대해 명시 적으로 설명하면이 사실을 악용 할 수 있습니다.

<사각형 색상 = "녹색"너비 = "50"높이 = "50">
    <번역 X = "100"/>
    <회전 각도 = "45"/>
</ Rectangle>

<Rectangle Color = "Red"Width = "50"Height = "50">
    <회전 각도 = "45"/>
    <번역 X = "100"/>
</ Rectangle>
위쪽 직사각형이 오른쪽으로 100 포인트 이동 한 다음 45도 회전합니다. 그것은 원래 위치의 오른쪽에 100 점을 놓고 끝납니다.

그러나 두 번째 사각형은 먼저 회전 한 다음 이동합니다. 초기 회전 때문에 양의 X 방향이 이제 오른쪽 하단으로 향하게됩니다. 이 때문에 직사각형은 오른쪽 아래쪽으로 100 포인트 위로 끝납니다.

주의 사항

요소를 너무 많이 확장하면 앨리어싱 효과가 발생할 수 있습니다. 이는 스케일 된 요소가 먼저 텍스처로 렌더링되고 그 다음 스케일이 조정되기 때문입니다. 이렇게하면 요소 Width 및 Height에 애니메이션을 적용하는 것과 비교하여 Scaling 애니메이션을 매우 빠르게 애니메이트 할 수 있습니다.