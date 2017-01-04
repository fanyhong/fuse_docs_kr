원본: https://www.fusetools.com/docs/ux-markup/literals

UX 마크 업 리터럴

이 기사에서는 숫자, 벡터, 색상 및 문자열이 UX 마크 업에서 어떻게 해석되는지 자세히 설명합니다.

번호

모든 숫자 유형 (float, int, double, Size 등)은 JavaScript의 숫자와 같은 표기법이나 Uno의 이중 숫자로 표현할 수 있으며 마침표 (.)는 선택적 소수 구분 기호로 사용할 수 있습니다.

유효한 수 :

1
1.3
13
.4
예:

<패널 너비 = "10.5"/>
단위

일부 속성은 단위 % (%), pt (점) 또는 px (픽셀)를 허용합니다. 이것은 단순히 숫자 리터럴 뒤에 장치를 배치함으로써 나타납니다.

예 :

<Panel Width = "50 %"Height = "200px"/>
기본 단위는 지정되지 않았으므로 지정된 단위의 값으로 산술에 관련되지 않는 한 pt (포인트)로 해석됩니다.

벡터

벡터 유형 (float2, float3, float4, Size2 등)은 다음과 같은 규칙에 따라 해석되는 다양한 방법으로 기록 할 수 있습니다.

단일 숫자 값이 제공된 경우이 값은 각 구성 요소에 대해 반복됩니다.

예를 들어;

<패널 여백 = "10"/>
다음과 같습니다.

<패널 여백 = "10, 10, 10, 10"/>
쉼표 (,)로 구분 된 두 개의 숫자 (X 및 Y)가 제공되면이 두 구성 요소는 [X, Y, X, Y]와 같이 대상 벡터의 크기까지 반복됩니다.

<패널 여백 = "4, 8"/>
다음과 같습니다.

<패널 여백 = "4, 8, 4, 8"/>
이것은 여백의 경우 두 개의 구성 요소가 각각 수평 (왼쪽 / 오른쪽) 및 수직 (위쪽 / 아래) 여백을 지정한다는 것을 의미합니다.

쉼표 (,)로 구분 된 세 개의 숫자 (X, Y 및 Z)가 제공되는 경우이 구성 요소는 필요한 경우 추가 1.0으로 채워집니다.

<패널 색상 = "1,0,1"/>
다음과 같습니다.

<패널 색상 = "1,0,1,1"/>
즉, 색상의 경우 알파 채널 (W)은 기본적으로 1로 설정되고 달리 지정되지 않으면 불투명 한 색상이 지정됩니다.

표현의 벡터

표현식의 일부로 벡터를 작성하려면 괄호 안에 벡터를 캡슐화하여 올바르게 구문 분석하도록하십시오.

<패널 색상 = "(1,0,1,1) / {fadeValue}"/>
크기 벡터

일부 속성은 Size2 유형이며 X 및 Y에 대해 별도의 단위를 지원합니다. 이러한 속성의 예는 Element.Offset입니다. 이러한 값은 벡터와 같이 표시되며, 각 벡터 구성 요소는 단위를 지정할 수 있습니다.

<패널 오프셋 = "10 %, 20px"/>
그림 물감

색상은 float4 유형 (또는 알파 채널이없는 경우 float3)으로 표시됩니다. 이것은 벡터와 동일한 방법으로 주목할 수 있음을 의미합니다 (위 참조).

색상 작업을 쉽게하기 위해 float4 및 float3 벡터는 # 기호를 사용하여 16 진수 표기법으로 표시 할 수 있습니다.

<패널 색상 = "# 18f"/>
16 진수 색상 (벡터)은 3, 4, 6 또는 8 개의 16 진수로 주어질 수 있으며 다음과 같이 해석됩니다.

#RGB (또는 #YYZ)
#RGBA (또는 #XYZW)
#RRGGBB (또는 #XXYYZZ)
#RRGBBABA (또는 #XXYYZZWW)
알파 채널 (A 또는 W)가 생략되면 1.0 (f 또는 ff)의 알파 값이 가정됩니다.

문자열

문자열 속성은 다른 속성과 약간 다르게 파싱됩니다. 문자열 속성은 기본적으로 원시 문자열 리터럴로 취급됩니다.

<텍스트 값 = "Hello, World!" />
바인딩 표현식을 삽입하려면 다음과 같이 문자열 중간에 중괄호 표현식을 삽입하면됩니다.

<텍스트 값 = "안녕하세요, {사용자 이름}!" />
표현식에서 문자열을 계산하려면 인라인 표현식 구문을 사용하십시오.

<텍스트 값 = "Hello {= toLower ({username})}"/>