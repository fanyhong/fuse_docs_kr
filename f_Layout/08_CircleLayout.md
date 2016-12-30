원본: https://www.fusetools.com/docs/fuse/layouts/circlelayout

CircleLayout 클래스

  고급 기능 표시
이동 :
목차
순환 방식으로 요소를 배치합니다.

CircleLayout은 패널 내부에 배치되어야하며 내부 요소에 적용됩니다.

우리는 StartAngleDegrees와 EndAngleDegrees를 사용하여 얼마나 많은 원을 다룰 지 정의 할 수 있습니다. 여기서 0 도는 3시와 같습니다.

레이아웃 계산 문제를 피하려면 EndAngleDegrees가 StartAngleDegrees보다 커야합니다.
예

<Panel Color = "# 000000">
     <CircleLayout />
     <원형 채우기 = "# ff00ff"/>
     <원형 채우기 = "# 7f7fff"/>
     <원형 채우기 = "# 00ffff"/>
     <원형 채우기 = "# 7fff7f"/>
     <원형 채우기 = "# ffff00"/>
     <원형 채우기 = "# ff7f7f"/>
</ Panel>
레이아웃 계산은 원을 큰 원에 맞추면됩니다. 안쪽의 요소는 모두 원으로 처리되므로 모든 요소를 반경 엣지 및 서로 닿으면 정렬됩니다 (원호 간격은 0입니다).

CircleLayout의 인터페이스

