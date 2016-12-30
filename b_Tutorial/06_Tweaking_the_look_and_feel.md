원본: https://www.fusetools.com/docs/tutorial/look-and-feel

## 소개 ##
마지막 장에서 우리는 모의 백엔드를 구현하고 우리의 의견을 묶어서 앱의 주요 부분의 흐름을 마무리했습니다. 아직 앱 스플래시 화면을 작성하지는 않았지만 프로젝트의 다음 단계 인 모양과 느낌을 해결하기에 충분합니다. 앱의 모양과 느낌은 앱의 모양과 작동 방식을 나타냅니다. 앱의 일반적인 스타일 / 색상 및 사용자 상호 작용을 기반으로 애니메이션이 적용되는 방식에 대해 설명합니다.

이 장에서는 독창적 인 디자인과 일치하도록 앱의 모양과 느낌을 반복적으로 조정할 것입니다. 이를 달성하기 위해 퓨즈의 실시간 리로드 기능을 최대한 활용하여 빠른 반복 작업을 수행 할뿐만 아니라 앱의 일반적인 모습과 모양을 나타내는 재사용 가능한 구성 요소를 만듭니다. 이 장을 마치면 앱을 완성하는 데 한 발자국 만 남았으니 이제 시작하겠습니다.

이 장의 마지막 코드는 여기에서 볼 수 있습니다.

## 배경색 ##

앱의 모양 / 느낌을 조정하는 가장 좋은 방법은 우리가 성취하고자하는 일반적인 모양에 맞는 배경색을 설정하는 것입니다. 이것은 매우 쉽고 빠르며, 우리가 아주 분명히하고 싶어하는 많은 개조를 할 것입니다. 그래서 우리는 그것을 시작할 것입니다.

우리 앱의 스크린 중 하나 (이 경우 편집 하이킹 페이지)의 디자인을 간략하게 살펴보면 :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

여기서 우리는 일반적으로 그 디자인 전체에 짙은 녹색 - 짙은 푸른 색조가 있음을 알 수 있습니다. 그래서 우리는 배경색을 선택하고 거기에서 시작하는 가이드로 사용합니다. 우리 앱의 배경색을 변경하려면 우리 앱의 App 클래스의 Background 속성을 설정해야합니다. 이 경우 MainView.ux에서 수행 할 것입니다. 이 색을 16 진수 # 022328로 설정합시다.

```
<App Background="#022328">
    <Router ux:Name="router" />
```

우리의 응용 프로그램은 이제 다음과 같이 보입니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/2364ea34339b4e639805aa68186d5bca__media/hikr/chapter-6/background-color.webp)

그리고 그걸로 우리는 훌륭한 출발을했습니다! 그러나 꽤 많은 UI 구성 요소가 이제는 꽤 어색해 보입니다. 자, 이제 우리의 스크린을 살펴보고 조각을 하나씩 고쳐 봅시다.

## 홈페이지 조정 ##

우리는 우리 홈페이지에서 시작할 것인데, 우리는 결국 다음과 같이 보이기를 원할 것입니다 :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/14954aa251367c1d687c1a0503190d21__media/hikr/tutorial/home.webp)

현재 구현과 디자인 이후의 가장 분명한 차이점 중 하나는 선택기의 모양과 느낌입니다. 현재 선택기는 간단한 버튼을 사용하여 구현됩니다. 그러나 우리는 우리의 디자인과 일치하는 대신 사용자 정의 무언가를 만들고 싶습니다.

제일 먼저 할 일은 버튼을 가장 간단한 커스텀 UX로 대체하는 것입니다. 우리의 Pages / HomePage.ux 파일을 보면 Button은 다음과 같이 보입니다 :

```
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{goToHike}" />
            </Each>
```

Instead of a Button, we'll simply use a Panel with a Text element inside it. We'll start with just that:

```
            <Each Items="{hikes}">
                <Panel>
                    <Text Value="{name}" />
                </Panel>
            </Each>
```

We'll want this custom Panel to be clickable just like our Button was. As it turns out, the Clicked attribute is not exclusive to the Button class at all; many other elements, such as Panels, also have a Clicked attribute. So, we'll add that to our Panel:

```
                <Panel Clicked="{goToHike}">
                    <Text Value="{name}" />
                </Panel>
```

이 내용을 저장하고 미리보기를 업데이트하면 Text 요소를 클릭하는 동안이 새로운 선택기가 대부분 작동하는 것을 볼 수 있습니다. 이는 기본적으로 Fuse가 Clicked와 같은 사용자 상호 작용을 위해 요소에서 적중 테스트를 수행 할 때 요소의 투명도를 유지하려고하기 때문입니다. Panel은 대부분 투명하기 때문에 대부분의 요소를 클릭 할 수 없습니다. 이 문제를 해결하기 위해 패널의 HitTestMode를 간단히 재정의 할 수 있습니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Value="{name}" />
                </Panel>
```

이 방법으로 포인터가 Panel의 로컬 경계 또는 Panel 요소 (예 : Text 요소) 중 하나에서 눌러지면 포인터는 "히트"로 계산되며 요소는 예상대로 클릭 가능하게됩니다.

다음으로해야 할 일은 텍스트 색상을 변경하는 것입니다. 우리는 꽤 어두운 배경을 가지고 있기 때문에 단순한 흰색이 트릭을해야합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" />
                </Panel>
```

또한 텍스트 주위에 여백을 추가합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
```

이제 상황이 꽤 좋아 보이기 시작했습니다! 그러나 한 셀렉터가 멈추고 다른 셀렉터가 시작하는 곳을보기가 약간 어렵 기 때문에 두 셀렉터 사이에 구분 기호를 추가합니다. 이를 위해 우선 모든 작은 요소 위에 투명도가있는 작은 흰색 사각형을 배치합니다.:

```
            <Each Items="{hikes}">
                <Rectangle Height="1" Fill="#fff4" />

                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
            </Each>
```

이것은 꽤 잘 보이고 우리 선별 자의 경계선처럼 잘 작동하지만 그 아래에 경계선이없는 마지막 경계선을 포함하지 않습니다. 우리는 모든 선택 자 아래에 직사각형을 추가 할 수 있습니다. 그러나 테두리가 다른 요소에 닿는 지 여부에 따라 경계가 다른 두께를 갖는 것처럼 보입니다. 대신 다음과 같이 선택기 아래에 Rectangle을 추가 할 수 있습니다.:

```
        <StackPanel>
            <Each Items="{hikes}">
                <Rectangle Height="1" Fill="#fff4" />

                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
            </Each>

            <Rectangle Height="1" Fill="#fff4" />
        </StackPanel>
```

저기, 멋지다! 이 시점에서 우리 셀렉터는 디자인에서와 같이 보일 것입니다. 그러나 우리는 분리 기호에 대한 몇 가지 코드를 복제했습니다. 두 개의 동일한 직사각형을 만드는 대신, Separator라는 재사용 가능한 구성 요소를 만들어 두 번 인스턴스화 할 수 있습니다. StackPanel에서는 다음과 같이 Separator 구성 요소를 만듭니다.

```
        <StackPanel>
            <Rectangle ux:Class="Separator" Height="1" Fill="#fff4" />

            <Each Items="{hikes}">
                <Rectangle Height="1" Fill="#fff4" />

                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
            </Each>

            <Rectangle Height="1" Fill="#fff4" />
        </StackPanel>
```

그런 다음 Rectangle 인스턴스를 Separator 인스턴스로 대체 할 수 있습니다.:

```
        <StackPanel>
            <Rectangle ux:Class="Separator" Height="1" Fill="#fff4" />

            <Each Items="{hikes}">
                <Separator />

                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
            </Each>

            <Separator />
        </StackPanel>
```

완전한!

> 참고 : 종종 ux : Class를 사용하여 재사용 가능한 구성 요소를 만들면 구성 파일을 자체 파일에 저장하여 정리할 수 있습니다. 그러나 재사용이 필요한 다른 파일 내부에 작은 구성 요소를 만드는 것이 좋으며 파일에서 파일을 옮기는 것이 조각이 어떻게 잘 맞는지 더 자세히 알 수 있습니다. Separator 클래스의 경우, 필요한 곳에서 구성 요소를 선언하고 사용하는 것이 가장 좋습니다.

선택자가 디자인 에서처럼 보이게되었으므로 기본 애니메이션을 추가하여 누를 때 누를 때처럼 느껴지도록합니다.

우리가 영어로보고 싶었던 애니메이션을 정확하게 설명하려면 다음과 같이 말할 것입니다. "우리 항목의 경우 누르는 동안 짧은 시간에 적은 양으로 항목을 부드럽게 확장합니다." 우리에게 운좋게도 퓨즈는 트리거와 애니메이터를 통해 영어 문장과 매우 ​​흡사 한 애니메이션과 상호 작용을 설명하는 강력하고 간결한 방법을 제공합니다. 즉, 트리거는 퓨즈 앱에서 이벤트, 제스처 또는 기타 사용자 입력 또는 상태 변경을 감지하는 데 사용됩니다. 트리거는 애니메이터를 사용하여 이러한 입력에 응답하고 상태 변경, 애니메이션 수행 등을 수행 할 수 있습니다. 트리거 및 애니메이터는 처음부터 고도로 구성 가능하고 사용하기 쉽도록 디자인되었습니다.

예를 들어 위에서 설명한 애니메이션을 UX에서 표현해 보겠습니다. 처음에는 셀렉터 항목을 "누르는 동안"움직이기를 원했습니다. 그래서 Panel에 WhilePressed라고하는 트리거를 추가합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />

                    <WhilePressed>
                    </WhilePressed>
                </Panel>
```

다음으로, 우리는 "항목의 크기를 조정"하고 싶습니다. 이를 위해 WhilePressed 트리거 내부에 Scale 애니메이터를 추가합니다.

```
                    <WhilePressed>
                        <Scale />
                    </WhilePressed>
```

위의 영어 문장과 같이 읽는 방법을 보시라. 다음으로, 아이템이 눌려지는 동안 얼마나 커야 하는지를 설명 할 것입니다. Scale의 경우 스케일링 양을 설명하기 위해 1에 관련된 Factor를 지정합니다. Factor가 1이면 요소의 크기가 변경되지 않습니다. 2 인 경우 요소의 크기가 두 배로 확장되고 .5 크기가 절반으로 조정됩니다.이 경우 요소가 원래 크기보다 약간 작아지기를 원합니다. .95의 계수 :

```
                    <WhilePressed>
                        <Scale Factor=".95" />
                    </WhilePressed>
```

이것을 저장하면 셀렉터를 누르는 동안 실제로 작아집니다. 그러나이 스케일링은 즉시 발생합니다. 우리는 그것이 "단기간에"일어나길 원했습니다. 이것을 설명하기 위해 Animator에 Duration을 추가합니다.이 Duration은 요소가 일반 크기에서 Factor 속성으로 지정된 크기까지 초 단위로 조정되는 데 걸리는 시간을 나타냅니다. 우리가 누르는 애니메이션이 상당히 빠르기를 원하기 때문에, 지속 시간을 .08 초로 지정합시다 :

```
                    <WhilePressed>
                        <Scale Factor=".95" Duration=".08" />
                    </WhilePressed>
```

이것을 저장하면 셀렉터를 누르는 동안 실제로 작아집니다. 그러나이 스케일링은 즉시 발생합니다. 우리는 그것이 "단기간에"일어나길 원했습니다. 이것을 설명하기 위해 Animator에 Duration을 추가합니다.이 Duration은 요소가 일반 크기에서 Factor 속성으로 지정된 크기까지 초 단위로 조정되는 데 걸리는 시간을 나타냅니다. 우리가 누르는 애니메이션이 상당히 빠르기를 원하기 때문에, .08 초의 지속 시간을 지정합시다 :

우리가 다시 구해서 이것을 시험해 보면, 짧은 시간에 변화가 일어나고 훨씬 나아 졌음을 알 수 있습니다. 그러나 애니메이션은 충분히 부드럽지 않습니다. 이는 기본적으로 이와 같은 애니메이션이 선형이기 때문에 애니메이션이 처음부터 끝까지 일정한 속도로 발생하기 때문입니다. 이것은 매우 예측 가능하지만 우리의 경우에는 이상적이지 않습니다. 우리가 애니메이션을 만들고 있기 때문에, 우리의 애니메이션이 다소 선형으로 시작되었지만 부드럽게 끝난다면 더 좋아할 것입니다. 이를 위해 많은 애니메이터가 애니메이션 시작 및 중지 방법을 설명하는 여유 공간 지정을 지원합니다. 애니메이션을 설명 할 때 Easings은 매우 일반적이며 많은 도구 (Fuse 포함)가 지원하는 많은 표준 곡선이 있습니다 (사용되는 이름 지정 규칙이 약간 다를 수 있음).

우리가 누르는 애니메이션의 경우 QuadraticOut 여유를 지정할 수 있습니다.이 여유는 애니메이션이 시작될 때 선형 여유 (기본값)와 유사하지만 곡선이 애니메이션의 끝으로 가면서 부드럽게됩니다.

```
                    <WhilePressed>
                        <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                    </WhilePressed>
```

이제 우리가 이것을 시도 할 때 우리 셀렉터는 우리가 원하는 것처럼 눌렀을 때 매우 자연스럽게 확장됩니다. 굉장해!

> 참고 : 트리거 및 애니메이터에 대한 자세한 내용은 트리거 및 애니메이션 기사 및 비디오를 확인하십시오!

이제 항목을 자세히 설명 했으므로 완성되기 전에이 페이지에 추가 할 사항이 하나 더 있습니다.이 항목은 "최근 하이킹"제목 텍스트입니다. 이것은 꽤 쉬워야합니다. 우리는 기본적으로 약간의 여백 등을 가지고 화면 상단에 도킹 된 흰색 텍스트를 원합니다. 먼저, 도킹 부분을 해결해 보겠습니다. 우리 항목의 UX 코드를 DockPanel로 옮깁니다.

```
    <DockPanel>
        <ScrollView>
            <StackPanel>
                <Rectangle ux:Class="Separator" Height="1" Fill="#fff4" />

                <Each Items="{hikes}">
                    <Separator />

                    <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                        <Text Color="White" Value="{name}" Margin="20" />

                        <WhilePressed>
                            <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                        </WhilePressed>
                    </Panel>
                </Each>

                <Separator />
            </StackPanel>
        </ScrollView>
    </DockPanel>
```

그런 다음 DockPanel 상단에 도킹 할 "최근 하이킹"텍스트에 Text 요소를 추가합니다.

```
    <DockPanel>
        <Text Color="White" FontSize="30" TextAlignment="Center" Dock="Top" Margin="0,50">Recent Hikes</Text>

        <ScrollView>
```

큰! 그렇게 쉬웠다. 그러나 코드를 살펴보면이 파일에는 Color = "White"를 지정하는 두 개의 다른 Text 요소가 이미 있습니다. 우리 앱이 어두운 배경을 가지고 있기 때문에 이것은 아마도이 한 페이지뿐만 아니라 꽤 많이 사용하기를 원할 것입니다. 자, 이것을 위해 재사용 가능한 컴포넌트를 만들자!

재사용 가능한 구성 요소를 응용 프로그램 전체에서 사용하기를 원할 것이므로이 파일에이 재사용 가능 구성 요소를 지정하고 싶지 않을 것입니다. 대신, 우리는 그것을 자신의 파일에 넣기를 원할 것입니다. 프로젝트 dir의 루트에서 Components 폴더를 만듭니다.

```
.
|- MainView.ux
|- Components
|- Modules
|- Pages
```

이 Components 디렉토리는 다양한 재사용 가능한 구성 요소를 배치하기에 완벽한 장소입니다. 흰색 텍스트 구성 요소가 응용 프로그램 전체에서 사용되기를 원할 것이므로이 구성 요소가 hikr 응용 프로그램에 특정한 텍스트 요소임을 나타 내기 위해 hikr.Text를 호출 할 것입니다. 구성 요소를 만들려면 다음 텍스트가 포함 된 구성 요소 디렉토리에 hikr.Text.ux라는 파일을 만듭니다.:

```
<Text ux:Class="hikr.Text" Color="White" />
```

우리는 그걸 저장하고, 그 구성 요소는 우리 프로젝트에서 사용할 수있게 될 것입니다. 우리의 HomePage로 돌아가서 Text Color = "White"대신에 hikr.Text를 사용하도록합시다. "최근 하이킹"텍스트의 닫기 태그를 업데이트하는 것을 잊지 마십시오!

```
    <DockPanel>
        <hikr.Text FontSize="30" TextAlignment="Center" Dock="Top" Margin="0,50">Recent Hikes</hikr.Text>

        <ScrollView>
            <StackPanel>
                <Rectangle ux:Class="Separator" Height="1" Fill="#fff4" />

                <Each Items="{hikes}">
                    <Separator />

                    <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                        <hikr.Text Value="{name}" Margin="20" />

                        <WhilePressed>
                            <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                        </WhilePressed>
                    </Panel>
                </Each>

                <Separator />
            </StackPanel>
        </ScrollView>
    </DockPanel>
```

그리고 그 배경과는 별도로 (우리는이 장의 뒷부분에서 설명 할 것입니다), 우리는 HomePage의 내용을 완전히 완성했으며 재사용 가능한 텍스트 구성 요소도 가지고 있습니다. 시원한!

> 참고 :이 장에서 살펴볼 바와 같이 퓨즈의 기존 요소에 스타일 / 기능을 추가 한 다음 재사용이 필요할 때 재사용 가능한 구성 요소를 추출하는 것이 일반적입니다. 이는 일관되고 빠른 워크 플로우를 제공하며 퓨즈에서 사물을 조정할 수있는 좋은 습관을 개발할 수있는 좋은 방법입니다. 이 장의 나머지 부분에서는 우리가 "손가락으로"이해할 수 있도록이 패턴을 계속 수행 할 것입니다.

## EditHikePage 조정 ##

이제는 EditHikePage를 살펴볼 차례입니다. 우리의 디자인은 다음과 같습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

마지막 페이지보다이 페이지에 대한 작업이 조금 더 많았지 만, 이미 앱의 모양과 느낌을 조정하고 있기 때문에 아무 문제가 없습니다.

우리의 EditHikePage는 기본적으로 우리가해야 할 일을 취소 / 저장 버튼과 편집자의 두 부분으로 나눌 수 있습니다. 우리는 버튼으로 시작할 것입니다.

취소 및 저장 버튼 조정하기

HomePage와 마찬가지로 Cancel 버튼과 Save 버튼은 현재 단순한 버튼입니다. 그러나 우리는 이러한 것들을 위해 자신 만의 맞춤형 디자인을 만들고 싶어하기 때문에 대신 자체적 인 구성 요소를 만들 것입니다. 저장 버튼을 사용자 정의하여 시작한 다음, 그 부분에서 재사용 가능한 구성 요소를 추출합니다.

저장 버튼을 Pages / EditHikePage.ux의 Text 요소가 포함 된 패널로 대체하여 HomePage의 하이킹 선택자와 동일한 방법으로 시작합니다. 이번에는 사용자 정의 hikr.Text를 사용합니다. 일반 Text 요소 대신 생성 된 것입니다.:

```
            <Panel Clicked="{save}">
                <hikr.Text Value="Save" />
            </Panel>
            <Button Text="Cancel" Clicked="{cancel}" />
```

텍스트가 버튼의 중앙에 있기를 원하기 때문에 hikr.Text 요소에 정렬을 추가합니다. 또한 기본 글꼴 크기를 지정합니다.

```
            <Panel Clicked="{save}">
                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

다음으로, 사용자 정의 단추의 배경을 디자인과 일치하도록 변경하려고합니다. 우리는 컴포넌트에 Rectangle을 추가하는 것으로 시작하고, Layer = "Background"를 지정하여 컴포넌트의 배경으로 설정합니다. 사각형의 색상은 디자인의 버튼과 동일하게 적용됩니다.

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" />

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

것은 꽤 좋아 보이지만, 우리 디자인의 버튼은 모서리가 둥글다. 그래서이 Rectangle에 작은 CornerRadius를 추가해 보겠습니다.:

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4" />

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

사용자 정의 버튼의 배경에서 마지막으로 할 일은 버튼 바로 아래에있는 약간의 DropShadow를 추가하는 것입니다. 우리는 그림자를 약간 미묘하게하고 앱의 테마에 맞게 투명도를 사용하여 검정색으로 만들고 Angle, Distance, Spread 및 Size 값을 지정합니다.:

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

큰! 이제 단추의 배경과 텍스트를 설정 했으므로 가장 바깥 쪽 패널 주위에 여백을 추가하고 안쪽 여백을 추가합니다. 이것은 호흡 할 여지가 있습니다.

```
            <Panel Clicked="{save}" Margin="10" Padding="10">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

이 맞춤 검색 버튼의 모양을 다루고 있습니다. 그러나 그 느낌은 어떨까요? 이 버튼은 우리 셀렉터가 홈 페이지에서했던 것처럼 누르는 애니메이션을 가져야합니다. 사실, 우리는 왜 같은 것을 사용하지 않습니까? 코드를 복사하여 사용자 정의 버튼에 붙여 넣으십시오.:

```
            <Panel Clicked="{save}" Margin="10" Padding="10">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />

                <WhilePressed>
                    <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                </WhilePressed>
            </Panel>
```

우리가 모든 것을 저장하고 시도한다면, 꽤 좋은 느낌입니다. 굉장해!

저장 단추가 잘 설정되었으므로 재사용 가능한 hikr.Button 구성 요소를 추출하고이 스타일을 취소 단추에도 적용 해 봅시다.

여기에 ux : Class = "hikr.Button"속성을 추가하고 Save 버튼에 hikr.Button의 새 인스턴스를 추가하고 Panel에서 Clicked 핸들러를 다음과 같이 옮깁니다.:

```
            <Panel ux:Class="hikr.Button" Margin="10" Padding="10">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />

                <WhilePressed>
                    <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                </WhilePressed>
            </Panel>

            <hikr.Button Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
```

다음으로해야 할 일은 하드 코딩 된 "저장"텍스트를 구성 요소에서 인스턴스로 옮기는 것입니다. 그러나 우리 hikr.Button에 단순히 Text = "Save"를 썼다면 hikr.Button 클래스에는 그러한 속성이 없기 때문에 컴파일에 실패합니다. 퓨즈는 UX 만 사용하는 구성 요소에 대한 자체 속성을 정의 할 수있는 ux : Property라는 메커니즘을 제공하므로 쉽게 해결할 수 있습니다.

hikr.Button 구성 요소의 Text 속성을 만들려면 우리 속성에 대한 이름을 정의하는 ux : Property 특성을 사용하여 문자열 인스턴스 (텍스트에 문자열을 포함하므로)를 클래스에 추가하면됩니다.:

```
            <Panel ux:Class="hikr.Button" Margin="10" Padding="10">
                <string ux:Property="Text" />

                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
```

그리고 그걸로 우리 재산을 얻었습니다. 그러나 클래스의 hikr.Text 인스턴스에 여전히 하드 코딩 된 문자열 "Save"가 표시됩니다. 방금 생성 한 Text 속성의 값을 대신 표시하고 속성 바인딩을 통해이 작업을 수행 할 수 있습니다. 속성 바인딩은 이전에 UX에서 값을 JavaScript의 값에 바인딩하는 데 사용한 적이있는 데이터 바인딩과 비슷하며 구문도 거의 같습니다. 단추의 텍스트를 Text 속성에 바인딩하려면 hikr.Text 인스턴스의 Value를 다음과 같이 변경합니다.

```
                <hikr.Text Value="{Property Text}" FontSize="16" TextAlignment="Center" />
```

여기에서 우리는 ux에 바인딩을 볼 수 있습니다 : Property는 JS 데이터 바인딩과 거의 동일하게 보이지만 ReadProperty를 앞에 추가했습니다. 이것은 퓨즈가 서로 다른 바인딩 유형의 차이점을 알려주는 방식입니다.

새로운 Text 속성을 만들고 구성 요소 내부에 바인딩하면 이제 저장 단추에 맞게 Text 속성을 설정할 수 있습니다.:

```
            <hikr.Button Text="Save" Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
```

이것을 저장하면 버튼이 이전과 같이 보이지만 지금은 Text를 구성 요소 외부에 원하는대로 설정할 수 있습니다. 이것은 우리 구성 요소를 진정으로 재사용 할 수있게합니다!

> 참고 : ux : Property 및 퓨즈가 구성 요소를 만드는 데 제공하는 기타 유용한 도구에 대한 자세한 내용은 구성 요소 만들기 문서를 확인하십시오.

새로운 구성 요소를 hikr.Button.ux라는 구성 요소 디렉토리에서 자체 파일로 이동해 보겠습니다.

```
<Panel ux:Class="hikr.Button" Margin="10" Padding="10">
    <string ux:Property="Text" />

    <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
        <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
    </Rectangle>

    <hikr.Text Value="{Property Text}" FontSize="16" TextAlignment="Center" />

    <WhilePressed>
        <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
    </WhilePressed>
</Panel>
```

마지막으로 취소 버튼의 스타일도 지정합니다. 이 경우, Button을 hikr.Button으로 대체하면됩니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <hikr.Button Text="Save" Clicked="{save}" />
            <hikr.Button Text="Cancel" Clicked="{cancel}" />
        </StackPanel>
```

그리고 이제 우리는 재사용 가능한 hikr.Button 구성 요소와 두 개의 멋진 버튼을 갖게되었습니다! 이제는 디자인에 따라 화면의 올바른 영역에 배치해야합니다. 특히 두 개의 버튼을 화면 맨 아래에 나란히 배치해야합니다.

단추를 나란히 배치하려면 해당 단추를 Grid에 배치합니다. Grid는 지정된 수의 행과 열을 기반으로 사용 가능한 공간을 나누고 그 결과로 생성 된 셀에 자식을 배치하는 패널입니다. 기본적으로 이러한 행과 열의 크기는 동일합니다. 따라서 단일 행과 두 개의 열이있는 Grid를 지정하면 두 개의 버튼에 대한 완벽한 컨테이너를 얻게됩니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Grid ColumnCount="2">
                <hikr.Button Text="Save" Clicked="{save}" />
                <hikr.Button Text="Cancel" Clicked="{cancel}" />
            </Grid>
        </StackPanel>
```

여기서 ColumnCount를 2로 지정합니다. 그리드에 두 개의 열이 필요합니다. 우리는 어쨌든 하나만 필요하기 때문에이 경우에 필요한 행 수를 지정할 필요가 없습니다. 이렇게하면 버튼이 나란히 배치되지만 버튼의 순서를 디자인과 일치시켜 보겠습니다.:

```
            <Grid ColumnCount="2">
                <hikr.Button Text="Cancel" Clicked="{cancel}" />
                <hikr.Button Text="Save" Clicked="{save}" />
            </Grid>
```

완전한! 여기서는 그리드가 지정된 순서대로 자식 요소를 배치하는 것을 볼 수 있습니다. 그리드의 왼쪽 위 셀부터 시작합니다. 우리 자식 요소가 차지하는 셀을 변경하려면 다른 순서로 지정해야합니다. 멋지고 쉬운!

마지막으로이 그리드를 페이지 맨 아래에 놓습니다. 이를 위해 우리는 Page의 현재 내용을 모두 DockPanel에 배치하고 Dock = "Bottom"을 사용하여 DockPanel의 아래쪽에 Grid를 고정시킵니다. 전체적으로, 우리의 EditHikePage.ux 파일은 다음과 같습니다 :

```
<Page ux:Class="EditHikePage">
    <Router ux:Dependency="router" />

    <JavaScript File="EditHikePage.js" />

    <DockPanel>
        <Grid ColumnCount="2" Dock="Bottom">
            <hikr.Button Text="Cancel" Clicked="{cancel}" />
            <hikr.Button Text="Save" Clicked="{save}" />
        </Grid>

        <ScrollView>
            <StackPanel>
                <Text Value="{name}" />

                <Text>Name:</Text>
                <TextBox Value="{name}" />

                <Text>Location:</Text>
                <TextBox Value="{location}" />

                <Text>Distance (km):</Text>
                <TextBox Value="{distance}" InputHint="Decimal" />

                <Text>Rating:</Text>
                <TextBox Value="{rating}" InputHint="Integer" />

                <Text>Comments:</Text>
                <TextView Value="{comments}" TextWrapping="Wrap" />
            </StackPanel>
        </ScrollView>
    </DockPanel>
</Page>
```

그리고이를 통해 취소 및 저장 버튼의 모양과 느낌 및 배치를 모두 사용자 정의했습니다.

## 에디터 조정하기 ##

이제 EditHikePage에서 다양한 값 편집기의 모양 / 느낌을 살펴보고 조정 해 봅시다. 우리의 기억을 새롭게하려면이 페이지의 디자인을 다시 한번 살펴 보겠습니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

우선, 현재 구현에는 하이킹 이름을 표시하는 추가 Text 요소가 있습니다. 우리가 이미 이것을위한 편집자를 가지고 있기 때문에, 우리는 앞서 가서 그것을 제거 할 수 있습니다 :

```
            <StackPanel>
                <Text>Name:</Text>
                <TextBox Value="{name}" />
```

다음으로 모든 편집자의 제목 텍스트를 조정 해 봅시다. 먼저 이름 필드의 Text 요소를 hikr.Text 요소로 바꾸기 시작합니다.:

```
                <hikr.Text>Name:</hikr.Text>
                <TextBox Value="{name}" />
```

이것은 꽤 잘 보이지만이 hikr.Text 요소의 불투명도를 미세 조정하여 흰색이 아닙니다.:

```
                <hikr.Text Opacity=".6">Name:</hikr.Text>
                <TextBox Value="{name}" />
```

그런 다음 제목 텍스트와 편집기 필드 사이에 여백을 추가합니다.:

```
                <hikr.Text Opacity=".6" Margin="0, 0, 0, 5">Name:</hikr.Text>
                <TextBox Value="{name}" />
```

이것은 꽤 좋아 보이므로이 스타일을 TitleText라고 부르는 자체 사용자 정의 구성 요소로 추출해 내고 이름 제목 필드를 업데이트하여 사용할 것입니다.:

```
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />

                <TitleText>Name:</TitleText>
                <TextBox Value="{name}" />
```

이미 깨끗해 보인다! ux : Class를 다시 사용하여 서브 클래스 중 하나를 서브 클래스화할 수있는 방법에 주목하십시오. 이제 다른 편집기의 제목도 업데이트 해 보겠습니다.:

```
            <StackPanel>
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />

                <TitleText>Name:</TitleText>
                <TextBox Value="{name}" />

                <TitleText>Location:</TitleText>
                <TextBox Value="{location}" />

                <TitleText>Distance (km):</TitleText>
                <TextBox Value="{distance}" InputHint="Decimal" />

                <TitleText>Rating:</TitleText>
                <TextBox Value="{rating}" InputHint="Integer" />

                <TitleText>Comments:</TitleText>
                <TextView Value="{comments}" TextWrapping="Wrap" />
            </StackPanel>
```

멋지고 쉬운! 다음으로 다양한 편집기 / 제목 주위의 간격을 조정할 것입니다. 우리가 할 수있는 첫 번째 일은 StackPanel 테두리에 약간의 간격을두기 위해 편집기가 포함 된 StackPanel에 약간의 여백을 적용하는 것입니다.:

```
            <StackPanel Padding="10">
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />
```

This helps with the spacing between the editors and the borders of the Page, but not between the various different elements. Let's add some ItemSpacing as well to the StackPanel, which (as the name implies) should add some space between the different items in the StackPanel:

```
            <StackPanel ItemSpacing="10" Padding="10">
```

거의; 이제 요소들 사이에 간격이 생겼지 만 제목과 편집자 사이에 더 많은 간격이 있습니다. 이를 해결하기 위해 각 타이틀 / 에디터 쌍을 자체 StackPanel로 포장합니다. 이렇게하면 주변 StackPanel의 ItemSpacing이 제목 / 편집기 쌍 사이에만 적용되며 제목과 편집기는 서로 관련하여 적절하게 간격을 유지합니다. 그러면 다음과 같이 보일 것입니다.:

```
            <StackPanel ItemSpacing="10" Padding="10">
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />

                <StackPanel>
                    <TitleText>Name:</TitleText>
                    <TextBox Value="{name}" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Location:</TitleText>
                    <TextBox Value="{location}" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Distance (km):</TitleText>
                    <TextBox Value="{distance}" InputHint="Decimal" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Rating:</TitleText>
                    <TextBox Value="{rating}" InputHint="Integer" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <TextView Value="{comments}" TextWrapping="Wrap" />
                </StackPanel>
            </StackPanel>
```

이를 통해 모든 편집자는 적절한 간격을두고 있으며 간단한 속성과 패널을 구성하여이를 수정할 수있었습니다. 좋은!

이제 편집자를 돌 봅시다. 이전과 마찬가지로, 처음 반복적으로 수정 한 다음 재사용 가능한 구성 요소를 만든 다음 모든 구성 요소에 적용합니다. 이 경우 유일한 예외는 다른 편집기와 마찬가지로 TextBox가 아닌 TextView 인 주석 편집기입니다. 자체적 인 스타일링이 필요하므로 먼저 다른 편집기의 TextBox를 처리 한 다음 다시 돌아와야합니다.

이제 이름 편집기의 TextBox를 살펴 보겠습니다. 먼저 hikr.Text 구성 요소와 마찬가지로 TextColor를 White로 설정합니다. 또한 CaretColor를 White로 설정하여 깜박이는 캐럿 인디케이터의 색상을 조절할 수 있습니다. 그리고 우리가 그것에 착수하는 동안 Padding을 추가해 보겠습니다.

```
                <StackPanel>
                    <TitleText>Name:</TitleText>
                    <TextBox TextColor="White" CaretColor="White" Padding="10,10,0,10" Value="{name}" />
                </StackPanel>
```

좋아 보여! 이제 hikr.TextBox라는 재사용 가능한 구성 요소에 압축을 풀고 hikr.TextBox 인스턴스로 바꾸어 다른 모든 TextBox 기반 편집기에 스타일을 적용 해 봅시다 (해당되는 경우 바인딩 및 InputHints는 그대로 둡니다).

```
            <StackPanel ItemSpacing="10" Padding="10">
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />

                <TextBox ux:Class="hikr.TextBox" TextColor="White" CaretColor="White" Padding="10,10,0,10" />

                <StackPanel>
                    <TitleText>Name:</TitleText>
                    <hikr.TextBox Value="{name}" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Location:</TitleText>
                    <hikr.TextBox Value="{location}" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Distance (km):</TitleText>
                    <hikr.TextBox Value="{distance}" InputHint="Decimal" />
                </StackPanel>

                <StackPanel>
                    <TitleText>Rating:</TitleText>
                    <hikr.TextBox Value="{rating}" InputHint="Integer" />
                </StackPanel>
```

마지막으로이 구성 요소를 Components 디렉토리로 옮길 수 있습니다. 나중에이 구성 요소를 응용 프로그램의 다른 화면에 사용할 수 있습니다. 그래서 우리는 ux : Class를 Components hir의 새로운 hikr.TextBox.ux 파일로 옮길 것입니다 :

```
<TextBox ux:Class="hikr.TextBox" TextColor="White" CaretColor="White" Padding="10,10,0,10" />
```

이제이 페이지의 내용을 마무리하기 전에 주석 편집기를 시작하겠습니다. 이것은 여러 줄을 표시 할 수있는 TextView이므로 이전에 TextBox에 대한 스타일링과는 다른 스타일을 적용하고 싶습니다. 그러나 기본적으로 TextColor, CaretColor 및 Padding을 편집기에 적용하여 동일한 프로세스로 시작합니다.:

```
                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <TextView TextColor="White" CaretColor="White" Padding="5" Value="{comments}" TextWrapping="Wrap" />
                </StackPanel>
```

다음으로 우리는 TextView 배경에 약간의 불투명도가있는 둥근 사각형을 추가하여 디자인과 일치 시키길 원할 것입니다. hikr.Button 구성 요소에서했던 것처럼 Layer = "Background"로 Rectangle 인스턴스를 추가하고 Color 및 CornerRadius를 설정합니다.:

```
                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <TextView TextColor="White" CaretColor="White" Padding="5" Value="{comments}" TextWrapping="Wrap">
                        <Rectangle Layer="Background" Color="#fff2" CornerRadius="4" />
                    </TextView>
                </StackPanel>
```

좋아 보여! 이 시점에서 우리는이 스타일의 TextView를 다른 하나와 마찬가지로 자체 구성 요소로 분할 할 필요가 없습니다.이 구성 요소는이 인스턴스에만 사용되므로 다른 요소와 마찬가지로 구성해야합니다. 그러나, 좋은 측정을 위해, 나아가고 그렇게하십시오. 이전과 마찬가지로 hikr.TextView.ux라는 Components 디렉토리에 파일을 만들고 거기에 스타일이 지정된 TextView 코드를 추가하여 ux : Class 정의를 추가합니다. 그런 다음 EditHikePage를 업데이트하여이 hikr.TextView를 주석 편집기에 사용합니다. TextWrapping = "Wrap"및 값 바인딩을 구성 요소에 배치하는 대신 인스턴스에 남겨 두십시오. 구성 요소를 사용할 때 설정해야 할 가능성이 매우 높은 속성입니다.

`hikr.TextView.ux` :

```
<TextView ux:Class=”hikr.TextView” TextColor="White" CaretColor="White" Padding="5">
    <Rectangle Layer="Background" Color="#fff2" CornerRadius="4" />
</TextView>
```

`EditHikePage.ux` :

```
                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <hikr.TextView Value="{comments}" TextWrapping="Wrap" />
                </StackPanel>
```

그리고 이것으로 우리의 EditHikePage에있는 내용을 마칩니다!

## 페이지에 배경 이미지 추가 ##

페이지의 내용이 끝나면이 페이지의 디자인을 마무리하는 데 집중하십시오. 우리는 단색 배경색으로는 꽤 멀었지만, 이제 우리의 디자인과 일치하는 배경 이미지를 페이지에 추가 할 차례입니다. 이 이미지는 앱에서 사용할 정적 애셋이므로 먼저 프로젝트의 루트 디렉토리에 애셋 폴더를 만듭니다.:

```
.
|- MainView.ux
|- Assets
|- Components
|- Modules
|- Pages
```

다음으로 필요한 배경 이미지를 다운로드하십시오. 이것이 우리가 사용할 배경 이미지입니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/15353ef83e1e4a42b9456fe23166a8f4__media/hikr/chapter-6/background.webp)

그 이미지를 background.jpg로 Assets 디렉토리에 저장하십시오.

이제 프로젝트 폴더에 이미지가 생겼으므로 앱에서 이미지를 사용하고 싶습니다. 우리의 디자인에 따라 우리는이 이미지를 HomePage와 EditHikePage의 배경으로 사용하고자합니다. 우리는 전체 앱에 적용 할 수 있지만 각 페이지에 적용하면 페이지에서 발생하는 전환이 배경 이미지에 적용되므로 디자인 방식을 선호합니다. 자, 각 페이지에 우리의 HomePage부터 시작해 보겠습니다.

HomePage.ux에서이 이미지를 페이지의 배경으로 적용하기 전에 레이어에 "Background"세트가있는 이미지 요소를 추가하여 둥근 사각형을 이전에 구성 요소에 추가 한 방법과 비슷합니다.

```
<Page ux:Class="HomePage">
    <Image Layer="Background" File="../Assets/background.jpg" />

    <Router ux:Dependency="router" />
```

File 속성의 값이 HomePage.ux 파일과 어떻게 관련되어 있는지 확인하십시오. 이 세트를 사용하면 배경에 이미지가 생기지 만 이미지 내용은 예상대로 페이지의 전체 공간을 채우지 못합니다. 이는 기본적으로 이미지 요소가 콘텐츠의 종횡비를 변경하지 않고 사용 가능한 공간에 맞게 내용을 축소하거나 늘리거나 이미지 파일의 페이지가 페이지와 약간 다르기 때문에 약간 줄어들 수 있기 때문입니다. 이 문제를 해결하기 위해 Image 요소에 대해 StretchMode = "UniformToFill"을 지정할 수 있습니다. 이렇게하면 내용이 항상 사용 가능한 전체 공간을 채우고 가로 세로 비율을 유지합니다.

```
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" />
```

> 참고 : 이미지 요소가 이미지를 표시 할 수있는 다양한 방법에 대한 자세한 내용은 이미지 기본 사항 비디오를 확인하십시오!

마지막으로이 이미지 요소를 약간 투명하게 만들어 이미지에 어두운 청록색 배경색으로 "색조"를 적용 해 봅시다.

```
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" Opacity=".7" />
```

큰! 이제 백그라운드 이미지가 우리 홈 페이지에 올바르게 적용되었으므로 EditHikePage에도 적용 해 보겠습니다. 아마 우리가 어떻게 할 것인가를 추측 할 수 있습니다. 우리는 먼저이 배경 이미지가 적용된 재사용 가능한 hikr.Page 구성 요소를 만든 다음 HomePage 및 EditHikePage 구성 요소를 Page 클래스가 아닌 해당 클래스에서 직접 파생시킵니다.

따라서 이전과 마찬가지로 hikr.Page.ux라는 Components 디렉토리에 다음을 포함하는 새 파일을 만듭니다.:

```
<Page ux:Class="hikr.Page">
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" Opacity=".7" />
</Page>
```

그런 다음 Pages / HomePage.ux 및 Pages / EditHikePage.ux에서 코드를 업데이트합니다. 여는 태그와 닫는 태그를 모두 업데이트하는 것을 잊지 마십시오. 이 것들은 놓치기 쉽습니다.:

`HomePage.ux`

```
<hikr.Page ux:Class="HomePage">
    <Router ux:Dependency="router" />

    ...
</hikr.Page>
```

`EditHikePage.ux`

```
<hikr.Page ux:Class="EditHikePage">
    <Router ux:Dependency="router" />

    ...
</hikr.Page>
```

멋지다. 이제 페이지가 완전히 완성되어 멋지게 보입니다.

## 상태 표시 줄 조정 ##

이제 앱의 전체 모양과 느낌으로 거의 끝났습니다! 좀 더 살펴볼 필요가있는 것은 앱의 상태 표시 줄뿐입니다. 기기에서 앱을 실행하면 상태가 좋지 않을 것입니다. 그러나 상태 표시 줄은 실제로 테마에 맞지 않습니다. 특히 어두운 배경 때문에 iOS의 상태 표시 줄 텍스트와 아이콘이 너무 어두워 읽을 수 없으며 Android에서 상태 표시 줄의 배경색이 단순히 잘못된 색상으로 표시됩니다.

이러한 문제를 해결하기 위해 Fuse는 iOS 및 Android의 상태 표시 줄을 각각 사용자 정의하는 데 사용할 수있는 iOS.StatusBarConfig 및 Android.StatusBarConfig 클래스를 제공합니다. 이를 사용하려면 MainView.ux의 App 태그 상단에 각각의 인스턴스를 추가하고 각 플랫폼에 적절한 속성을 설정해야합니다. iOS부터 시작하여 스타일 = "빛"으로 지정하여 퓨즈에 어두운 텍스트가 아닌 밝은 텍스트와 아이콘이있는 상태 표시 줄이 필요하다고 말합니다.:

```
<App Background="#022328">
    <iOS.StatusBarConfig Style="Light" />

    <Router ux:Name="router" />
```

다음으로 Android 상태 표시 줄을 살펴 보겠습니다. 이 경우 앱의 배경색과 일치하는 배경색을 지정합니다.:

```
<App Background="#022328">
    <iOS.StatusBarConfig Style="Light" />
    <Android.StatusBarConfig Color="#022328" />
```

그리고 그것으로, 우리의 애플 리케이션도 잘 우리의 장치에 보인다!

> 참고 : 각 플랫폼에 대한 StatusBarConfig 클래스의 각 인스턴스 중 최대 하나만 갖는 것이 중요합니다. 그렇지 않으면 올바르게 작동하지 않을 수 있습니다. 상태 표시 줄 및 이와 유사한 구성에 대한 자세한 내용은 상태 표시 줄 기본 사항 비디오를 확인하십시오.

## 우리의 진보는 지금까지 ##

휴우, 끝내야 할 것이 많았 어! 하지만 이제는 앱의 모양과 느낌이 풀렸고 퓨즈에서 재사용 가능한 구성 요소를 추출하는 연습이 많이있었습니다. 굉장해!

현재 우리의 앱은 다음과 같습니다.:

![https://res.cloudinary.com/fusetools/image/upload/documentation_v2/2cb85e19bf765c87c7b6b63d69e727db__media/hikr/chapter-6/chapter-6.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/2cb85e19bf765c87c7b6b63d69e727db__media/hikr/chapter-6/chapter-6.mp4)

코드에 관해서는 이번에는 꽤 많은 파일을 다루었 기 때문에 여기에 나열하는 대신 Github의 다양한 파일에 간단하게 링크 할 것입니다.

- Assets/Background.jpg
- Components/hikr.Button.ux
- Components/hikr.Page.ux
- Components/hikr.Text.ux
- Components/hikr.TextBox.ux
- Components/hikr.TextView.ux
- Pages/EditHikePage.ux
- Pages/HomePage.ux
- MainView.ux

## 다음은 뭔가요? ##

이제 앱의 주요 부분이 기능적으로 훌륭해 졌으므로 모든 것을 하나로 묶을 차례입니다. 다음 장에서는 비디오 배경을 가진 아름다운 스플래시 스크린을 구축하여 앱을 완성 할 것입니다. 우리는 이미 앱의 아키텍처와 재사용 가능한 재사용 가능한 컴포넌트를 가지고 있기 때문에 아무런 문제가 없어야합니다. 그럼 파헤 치자!

이 장의 마지막 코드는 여기에서 볼 수 있습니다.
