원본: https://www.fusetools.com/docs/fuse/controls/circle

서클 클래스

 고급 기능 표시
이동 :
목차
원을 표시합니다.

원은 채우기 및 획을 가질 수있는 셰이프입니다. 기본적으로 서클에는 크기, 채우기 또는 획이 없습니다. 표시되도록하려면 일부를 추가해야합니다.

StartAngle / EndAngle

StartAngle과 EndAngle을 사용하여 원의 조각 만 그릴 수 있습니다. 여러 가지 방법으로이를 제어하는 ​​데 사용할 수있는 6 가지 속성이 있습니다.

StartAngle - 슬라이스가 시작되는 각도 (라디안 단위)입니다.
StartAngleDegrees - 슬라이스가 시작되는 각도의 각도입니다.
EndAngle - 슬라이스가 끝나는 각도 (라디안 단위)입니다.
EndAngleDegrees - 슬라이스가 끝나는 각도 (도)입니다.
LengthAngle - StartAngle의 라디안 오프셋입니다. 이것은 EndAngle 대신 사용할 수 있습니다.
LengthAngleDegrees - StartAngle에서도 단위의 오프셋입니다. 이것은 EndAngleDegrees 대신 사용할 수 있습니다.
예를 들어 동일한 Circle에서 StartAngle과 StartAngleDegrees를 모두 사용하면 정의되지 않은 동작이 발생합니다.

예제들

빨간색 원 표시 :

<Circle Width = "100"Height = "100"Color = "# f00"/>
획 및 선형 그래디언트로 멋지게보기 :

<Circle Width = "100"Height = "100">
    <선형 그래디언트>
        <GradientStop Offset = "0"Color = "# cf0"/>
        <GradientStop Offset = "1"Color = "# f40"/>
    </ LinearGradient>
    <스트로크 폭 = "1">
        <SolidColor Color = "# 000"/>
    </ 스트로크>
</ Circle>
원을 그리기 :

<Circle Width = "150"Height = "150"Color = "# f00"StartAngleDegrees = "135"LengthAngleDegrees = "145"/>

