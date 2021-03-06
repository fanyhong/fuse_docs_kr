원본: https://www.fusetools.com/docs/fuse/animations/animator

Animators Animator 클래스

 고급 기능 표시
이동 :
목차
Animators는 트리거가 트리거 될 때 요소가 애니메이션으로 표시되는 방법과 방법을 지정하는 데 사용됩니다. 애니메이션의 정확한 결과를 제어하는 ​​데 중요한 세 쌍의 속성이 있습니다.

예

애니메이터 유형의 예는 다음 예에서 사용 된 변경 및 이동입니다.

<패널 ux : Name = "panel1"Color = "Blue">
    <잠시 후>
        <Change panel1.Color = "# 0f0"Duration = "1"/>
        <이동 X = "100"지연 = "1"지속 시간 = "1"/>
    </ WhilePressed>
</ Panel>
패널에서 포인터를 누를 때 위의 WhilePressed 트리거가 활성화되면 애니메이터는 지연 및 기타 속성에 따라 재생됩니다.

재생 시간 / 재생 시간

애니메이터는 활성화 된 트리거에 대한 응답으로 요소 및 속성을 애니메이션화하는 데 사용됩니다. 선택할 수있는 많은 애니메이터가 있으며, 모두 다른 용도로 사용됩니다. 일반적인 애니메이터에는 Move, Rotate, Scale 및 Change가 있습니다. 이러한 애니메이터는 활성화 될 때 앞으로 움직이고 비활성화되면 뒤로 움직이는 반면, 스핀 및 사이클과 같은 일부 애니메이터는 활성화 된 상태에서 계속 반복되는 애니메이션을 만듭니다.

지연 / 지연

Delay 속성을 설정하면 실제 애니메이션이 그 초만큼 지연됩니다. DelayBack은 역방향 애니메이션에 다른 지연을 설정하는 데 사용됩니다. 애니메이션의 총 재생 시간은 지연 + 재생 시간이됩니다. 다음 변경 애니메이터의 총 지속 시간은 7 초입니다. 그것은 활성화 된 후 5 초를 기다린 다음 대상 요소를 2 초 이상 움직입니다.

<Change Delay = "5"Duration = "2"someElement.Height = "100"/>
이징 / EasingBack

퓨즈에는 미리 정의 된 여유 곡선의 표준 세트가 제공됩니다. 이징 곡선은 애니메이션이 시간 경과에 따라 어떻게 진행되는지 제어하는 ​​데 사용됩니다. 기본 여유는 선형으로 설정됩니다. 선형 이징을 사용하면 애니메이션이 전체 시간 동안 동일한 속도로 진행됩니다. 이것은 대개 아주 부자연스럽고 가짜로 보입니다. 보다 자연스러운 느낌을 얻기 위해 다음과 같이 QuadraticInOut으로 이징을 변경할 수 있습니다.

<Easing 변경 = "QuadraticInOut"Duration = "2"someElement.Property = "SomeValue"/>
이 애니메이터는 처음에는 천천히, 중간에서는 더 빠르게, 그리고 마지막에는 다시 느리게 진행됩니다.

애니메이터 추적

TrackAnimator 클래스에는 Duration과 정의 된 대상 값이 있습니다. 이징 곡선 또는 맞춤 키 프레임을 사용하여 애니메이션을 추가로 조정할 수 있습니다.

