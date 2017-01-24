원본: https://www.fusetools.com/docs/tutorial/look-and-feel

## 소개 ##

[마지막 챕터](https://www.fusetools.com/docs/tutorial/mock-backend) 에서, 우리는 모의 백엔드를 구현했고 뷰 들을 함께 묶었으며, 우리 앱의 주요 부분의 흐름을 완료했습니다. 우리가 아직 우리 앱의 스플래시 화면을 다 완성하지는 않았지만, 우리 프로젝트의 다음 단계인 *룩앤필* (모양과 느낌)을 시도 하기엔 충분합니다. 앱의 룩앤필은 앱의 모양과 작동 방식을 나타냅니다. 앱의 일반적인 스타일/색상 뿐 아니라, 사용자 상호 작용을 기반으로한 애니메이션에 대한 부분도 설명합니다.

이 챕터에서는, 우리의 원래 디자인과 일치하도록 앱의 룩앤필 을 반복적으로 조정할 것입니다. 이렇게 하기 위해, 빠르고 반복 작업을 수행하기 위한 Fuse의 실시간 리로드 기능을 최대한 활용할 뿐만 아니라, 우리 앱의 모양과 동작이 얼마나 더 공통적인지를 나타내는 재사용 가능한 컴포넌트들을 만들 것입니다. 이 챕터를 마치면 앱을 완성하는데 한 챕터만 남을 것입니다. 그럼, 시작해 봅시다.

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-6) 에서 볼 수 있습니다.

## 배경색 ##

앱의 look/feel 을 조정하는 가장 좋은 방법은, 우리가 얻고자하는 일반적인 외형에 적합한 배경색을 심플하게 설정하는 것입니다. 이것은 매우 쉽고 빠르고 명확하게 우리가 원하는 다른 많은 조정들을 할 수 있도록 하므로, 우리는 배경색 설정부터 시작합니다.

우리 앱 화면들 중 하나(이 경우 편집 하이킹 페이지)의 디자인을 간략하게 살펴보면 :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

여기서 우리는 일반적으로 앱이 디자인 전체에서 어두운 녹색을 띈 푸른 빛 을 가지고 있음을 알 수 있습니다. 그것을 우리의 배경색을 고르는 어떤 가이드로 사용 할 것이고, 거기서부터 출발 할 것입니다. 우리 앱의 배경색을 변경하려면, [App](https://www.fusetools.com/docs/fuse/app) 클래스의 [Background](https://www.fusetools.com/docs/fuse/appbase/background) 속성을 설정하는 것이 해야 할 전부입니다. 이 경우 `MainView.ux` 에서 설정합니다. 이 색을 16진수 `#022328` 로 설정합시다.

```
<App Background="#022328">
    <Router ux:Name="router" />
```

우리의 앱은 이제 다음과 같이 보입니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/2364ea34339b4e639805aa68186d5bca__media/hikr/chapter-6/background-color.webp)

그것으로, 우리는 훌륭한 출발을 했습니다! 그러나 꽤 많은 UI 컴포넌트들이 지금 꽤 어색해 보입니다. 자, 이제 우리 화면들을 살펴보며 하나씩 그것들을 수정해 봅시다.

## 홈페이지 조정 ##

우리 `HomePage` 에서 시작합니다, 결국 다음과 같이 보이기를 원합니다 :

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/14954aa251367c1d687c1a0503190d21__media/hikr/tutorial/home.webp)

현재 구현과 이 후 구현될 디자인 사이에 가장 분명한 차이점 중 하나는 selector (하이킹을 선택하도록 하는) 들의 룩앤필 입니다. 현재 selector 들은 간단한 [Button](https://www.fusetools.com/docs/fuse/controls/button) 을 사용하여 구현되었습니다; 그러나 우리는 우리의 디자인과 일치하는 어떤 커스텀을 대신 만들기를 원합니다.

가장 먼저 할 일은, `Button` 을 가장 간단한 커스텀 UX로 대체하는 것입니다. 우리의 `Pages/HomePage.ux` 파일을 보면 `Button` 은 다음과 같이 보입니다 :

```
            <Each Items="{hikes}">
                <Button Text="{name}" Clicked="{goToHike}" />
            </Each>
```

`Button` 대신에, [Text](https://www.fusetools.com/docs/fuse/controls/text) 요소가 있는 [Panel](https://www.fusetools.com/docs/fuse/controls/panel) 을 사용합니다. 다음과 같이 시작합니다.

```
            <Each Items="{hikes}">
                <Panel>
                    <Text Value="{name}" />
                </Panel>
            </Each>
```

`Button` 과 마찬가지로 이 사용자 정의 `Panel` 을 클릭 할 수 있게 하려고 합니다. `Clicked` 속성은 `Button` 클래스에만 독점적으로 존재하는 것은 아닙니다; `Panel` 과 같은 많은 다른 요소에도 `Clicked` 속성이 있습니다. `Panel` 에 `Clicked` 속성을 추가 합니다.:

```
                <Panel Clicked="{goToHike}">
                    <Text Value="{name}" />
                </Panel>
```

이 내용을 저장하고 미리보기를 업데이트 하면, 정확하게 `Text` 요소를 클릭하는 경우 만큼만, 이 새로운 selector 들이 *거의* 작동하는 것을 볼 수 있습니다. 이는 Fuse가 `Clicked` 같은 사용자 상호 작용들을 위한 요소의 hit testing (클릭 가능한 부분에 대한 설정) 을 수행 할 때, 해당 요소의 투명도를 그대로 유지하려고 하는 것이 기본으로 설정되어 있기 때문에 그렇습니다. `Panel` 은 대부분 투명하기 때문에, 이것은 요소의 대부분을 클릭 할 수 없음을 의미합니다. 이 문제를 해결하기 위해, 우리는 Panel 의 [HitTestMode](https://www.fusetools.com/docs/fuse/elements/element/hittestmode) 를 다음처럼 간단하게 재정의 할 수 있습니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Value="{name}" />
                </Panel>
```

이 방법으로, 포인터가 `Panel` 의 로컬 바운드 안 혹은 `Panel` 의 자식 요소 (예: `Text` 요소) 중 하나에서 눌러지면, 포인터는 "hit" 로 계산되며, 요소는 예상대로 클릭 가능하게 됩니다.

다음으로해야 할 일은 텍스트 색상을 변경하는 것입니다. 우리는 꽤 어두운 배경을 가지고 있기 때문에, 단순한 흰색으로 꾸며야 합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" />
                </Panel>
```

또한 `Text` 주위에 [Margin](https://www.fusetools.com/docs/fuse/elements/element/margin) 을 추가합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
```

이제 꽤 나아지기 시작했습니다! 그러나, 한 selctor 끝나면서 다른 selector 가 시작하는 부분을 구분하기가 약간 어렵기 때문에, 두 selector 사이에 구분 기호를 추가할 것입니다. 이를 위해, 우선 우리의 모든 요소들에 위에 약간 투명한 작은 흰색 [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) (사각형) 을 배치합니다.:

```
            <Each Items="{hikes}">
                <Rectangle Height="1" Fill="#fff4" />

                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />
                </Panel>
            </Each>
```

이렇게 하면 꽤 잘 보이고, 우리 모든 selector 들 위에서 경계선으로써 잘 동작하지만, 아래 경계선이 없는 마지막 selelctor 까지는 커버 되지 않습니다. 우리는 모든 selector 들 아래에 `Rectangle` 을 추가 *할 수도* 있지만, 그렇게 하면 테두리가 다른 요소에 닿는지 여부에 따라, 경계가 서로 다른 두께를 갖는 것처럼 보이게 됩니다. 대신 다음과 같이 seletor 들 아래에 Rectangle 을 추가 할 수 있습니다.:

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

괜찮아 보입니다! 이 시점에서 우리의 selector 들은 디자인 했던 것과 같게 보일 것입니다. 그러나 우리는 수정하길 원하는 분리 기호들에 대한 코드를 일부 복제해야 했습니다. 우리는 두 개의 동일한 `Rectangle` 을 만드는 대신, `Separator` 라는 재사용 가능한 컴포넌트를 만들어 두 번 인스턴스화 할 수 있습니다. [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 에서 다음과 같이 `Separator` 컴포넌트를 만듭니다.

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

그런 다음 `Rectangle` 인스턴스를 `Separator` 인스턴스로 대체 할 수 있습니다.:

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

완성했습니다!

> 참고: 종종 ux:Class를 사용하여 재사용 가능한 컴포넌트를 만들때, 구조적인 유지를 위해 자체 파일로 배치 할 수도 있습니다. 그러나 재사용이 필요한 다른 파일들 내부에 작은 컴포넌트들을 만드는 것도 전혀 문제될게 없으며, 오히려 파일로 옮기는 작업이 코드 조각들이 어떻게 동작되는지 보기 더 어렵게 만들 수도 있습니다. `Separator` 클래스의 경우, 필요한 곳에 해당 컴포넌트를 선언하고 사용하는 것이 더 좋습니다.

selector 들이 디자인에서 처럼 *보이게 (look)* 되었으므로, 기본 애니메이션을 추가하여 눌렸을 때 누르는 것처럼 *느껴지도록 (feel)* 하겠습니다.

우리가 보고 싶었던 애니메이션을 영어로 정확하게 설명한다면, 다음과 같을 것입니다: "For our item, while it's pressed, scale the item smoothly by small amount over a short period of time." 운좋게도, Fuse는 트리거와 애니메이터를 통해 영어 문장과 매우 ​​흡사하게 애니메이션과 상호 작용을 설명하는 강력하고 간결한 방법을 제공합니다. Fuse 앱에서 *트리거 (trigger)* 들은 이벤트, 제스처 또는 기타 사용자 입력 또는 상태 변경을 *감지* 하는 데 사용됩니다. 트리거들은 이러한 입력에 *응답* 하고, 상태 변경 및 애니메이션 수행 등을 수행 하기 위한 *애니메이터 (animator)* 들을 사용합니다. 트리거들과 애니메이터들은 처음부터 고도로 구성 가능하고 사용하기 쉽도록 디자인되었습니다.

예를 들어, 위에서 설명한 애니메이션을 UX에 표현해 보겠습니다. 처음에는 selector 아이템들을 "while they're pressed" ("누르는 동안") 애니메이션 되기를 원했습니다. 그래서 `Panel` 에 [WhilePressed](https://www.fusetools.com/docs/fuse/gestures/whilepressed) 라고 하는 트리거를 추가합니다.:

```
                <Panel HitTestMode="LocalBoundsAndChildren" Clicked="{goToHike}">
                    <Text Color="White" Value="{name}" Margin="20" />

                    <WhilePressed>
                    </WhilePressed>
                </Panel>
```

다음으로, "scale the item" ("항목의 크기를 조정") 하고 싶습니다. 이를 위해 `WhilePressed` 트리거 내부에 [Scale](https://www.fusetools.com/docs/fuse/animations/scale) 애니메이터를 추가합니다.

```
                    <WhilePressed>
                        <Scale />
                    </WhilePressed>
```

위를 영어 문장을 읽는 방법으로 보세요. 다음으로, 아이템이 눌려지는 동안 *얼마나* 커지거나 작아지게 되어야 하는지를 설명 할 것입니다. `Scale` 의 경우, 스케일 할 크기를 구현하기 위해, 1을 기준으로 한 `Factor` 를 지정합니다. Factor가 1 이면 요소의 크기가 변경되지 않습니다. 2 인 경우 요소의 크기가 두 배로 확장되고, .5 인 경우 크기가 절반으로 조정됩니다. 우리의 경우, 요소가 원래 크기보다 약간 작아지기를 원하므로, factor를 .95 로 설정합니다.:

```
                    <WhilePressed>
                        <Scale Factor=".95" />
                    </WhilePressed>
```

이것을 저장하면, 셀렉터를 누르는 동안 실제로 작아집니다. 그러나 이 스케일링은 즉rkr 발생 됩니다; 우리는 그것이 "over a short period of time." ("짧은 시간동안") 일어나길 원했습니다. 이것을 구현하기 위해 Animator에 `Duration` 을 추가합니다. 이 `Duration` 은 요소가 일반 크기에서 `Factor` 속성으로 지정된 크기까지 초 단위로 조정되는 데 걸리는 시간을 나타냅니다. 우리가 누르는 애니메이션이 상당히 빠르기를 원하기 때문에, 지속 시간을 .08 초로 지정합시다.:

```
                    <WhilePressed>
                        <Scale Factor=".95" Duration=".08" />
                    </WhilePressed>
```

우리가 다시 저장해서 이것을 테스트 해 보면, 짧은 시간에 변화가 일어나고 훨씬 나아 졌음을 알 수 있습니다. 그러나 애니메이션은 충분히 부드럽지 않습니다. 이는 기본적으로 이와 같은 애니메이션이 *선형* 이기 때문에, 애니메이션이 처음부터 끝까지 일정한 속도로 발생하기 때문입니다. 이것은 쉽게 예측 가능하지만, 우리의 경우에는 이상적이지 않습니다. 우리가 누르는 애니메이션을 만들고 있기 때문에, 우리의 애니메이션이 다소 선형으로 시작되었지만 부드럽게 끝난다면 더 좋을 겁니다. 이를 위해 많은 애니메이터들이 어떻게 애니메이션 시작 및 중지 를 구현 할 것인지에 대한, *easing* 지정을 지원합니다. 애니메이션을 구현 할 때 Easing 은 아주 일반적으로 사용되는 것으로, 많은 도구(Fuse 포함) 가 지원하는 [다양한 표준 곡선](http://easings.net/) 이 있습니다 (사용되는 이름 지정 규칙이 약간 다를 수 있음).

우리가 누르는 애니메이션의 경우 `QuadraticOut` easing 을 지정할 수 있습니다. 이 easing 은 애니메이션이 시작될 때 `Linear (선형)` easing (기본값)과 유사하지만, 곡선이 애니메이션의 끝으로 가면서 부드럽게 처리 됩니다.

```
                    <WhilePressed>
                        <Scale Factor=".95" Duration=".08" Easing="QuadraticOut" />
                    </WhilePressed>
```

이제 이렇게 해보면, 우리 selector 들은 우리가 원하는 것처럼, 눌렸을 때 아주 자연스럽게 스케일 됩니다. 멋집니다!

> 참고: 트리거들 및 애니메이터들 에 대한 자세한 내용은 [트리거 및 애니메이션](https://www.fusetools.com/docs/fuse/triggers/trigger) 기사 및 비디오를 확인하십시오!

이제 아이템들을 상세히 구현 했는데, 완성 전에 이 페이지에 추가 해야 할 아이템이 하나 더 있습니다. 바로 "Recent Hikes" ("최근 하이킹들") 제목 텍스트입니다. 이것은 꽤 쉽습니다.; 우리는 기본적으로 약간의 margin(여백)과 함께 화면 상단에 도킹된 흰색 텍스트를 원합니다. 먼저, 도킹 부분을 해결해 보겠습니다. 우리 아이템들에 대한 UX 코드를 [DockPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) 안으로 옮깁니다.

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

그런 다음 `DockPanel` 상단에 도킹 할 "Recent Hikes" 텍스트에 `Text` 요소를 추가합니다.

```
    <DockPanel>
        <Text Color="White" FontSize="30" TextAlignment="Center" Dock="Top" Margin="0,50">Recent Hikes</Text>

        <ScrollView>
```

훌륭합니다! 쉬웠습니다. 그러나 코드를 살펴보면, 우리는 `Color="White"` 를 지정하는 두 개의 다른 `Text` 요소들을 이미 이 파일에 가지고 있습니다. 우리 앱이 어두운 배경을 가지고 있기 때문에, 이것은 아마도 이 한 페이지 뿐만 아니라 다른곳에서도 꽤 많이 사용하게 될 것입니다. 자, 이것을 재사용 가능한 컴포넌트로 만듭시다!

재사용 가능한 컴포넌트를 앱 전체에서 사용하기를 원할 것이므로, 우리는 아마도 이 파일에 지정하고 싶지 않을 겁니다. 대신 우리는 그것을 자체 파일로 만들어 넣기를 원합니다. 프로젝트 루트 디렉토리에서 `Components` 폴더를 만듭시다.:

```
.
|- MainView.ux
|- Components
|- Modules
|- Pages
```

이 `Components` 디렉토리는 다양한 재사용 가능 컴포넌트들을 배치하기에 완벽한 장소입니다. 흰색 텍스트 컴포넌트가 앱 전체에서 사용되기를 원할 것이므로, 우리는 이 컴포넌트가 `hikr` 앱에 특정한 텍스트 요소임을 나타 내기 위해 그것을 `hikr.Text` 라고 호출 할 것입니다. 이 컴포넌트를 만들려면, `Comments` 디렉토리에, 아래 텍스트가 포함 된 `hikr.Text.ux` 라는 파일을 만듭니다.:

```
<Text ux:Class="hikr.Text" Color="White" />
```

저장을 하면, 그 컴포넌트는 우리 프로젝트에서 사용할 수 있게 됩니다. 우리의 `HomePage` 로 돌아가서 `Text Color="White"` 대신에 `hikr.Text` 를 사용하도록 합시다. "Recent Hikes" 텍스트의 닫는 태그를 넣는 것을 잊지 마십시오!

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

이렇게 하면, 그 백그라운드(우리는 이 챕터의 뒷부분에서 설명 할 것입니다) 와는 별도로, 우리는 `HomePage` 의 내용을 완전히 완성했으며, 재사용 가능한 텍스트 컴포넌트도 얻었습니다. 멋집니다!

> 참고: 이 챕터에서 살펴 본 바와 같이, Fuse의 기존 요소에 스타일/기능 을 추가 한 다음, 재사용이 필요할 때 재사용 가능한 컴포넌트를 추출하는 것이 일반적입니다. 이는 일관적이면서 빠른 워크플로우를 제공하며, Fuse에서 다루는 것들을 조정할 수있는 좋은 습관을 개발 할 수있는 좋은 방법입니다. 이 챕터의 나머지 부분에서는 우리가 "in the fingers" ("손가락들로") 를 이해할 수 있도록, 이 패턴을 계속 수행 할 것입니다.

## EditHikePage 조정 ##

이제는 `EditHikePage` 를 살펴볼 차례입니다. 우리의 디자인은 다음과 같습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

이 페이지가 지난 페이지보다 작업은 조금 더 많지만, 우리는 이미 앱의 룩앤필을 조정하는 방법을 알아가고 있기 때문에, 별 문제는 없습니다.

`EditHikePage` 에서, 우리는 `cancel`/`save` 버튼들 과 에디터들 - 이렇게 두 부분으로 나눌 필요가 있습니다. 우리는 버튼들부터 시작합니다.

## Cancel 및 Save 버튼 조정 ##

`HomePage` 와 마찬가지로 `Cancel` 버튼과 `Save` 버튼은 현재 단순한 [Button](https://www.fusetools.com/docs/fuse/controls/button) 들 입니다. 그러나 우리는 이러한 것들에 대해 자신만의 커스텀 디자인을 만들고 싶기 때문에, 대신할 자체 컴포넌트를 만들 것입니다. `Save` 버튼을 커스터마이징 하는 것으로 시작한 다음, 거기서 재사용 가능한 컴포넌트를 추출할 것입니다.

`Save` `Button` 을 `Pages/EditHikePage.ux` 의 `Text` 요소가 포함된 `Panel` 로 대체함으로써 `HomePage` 의 하이킹 selector 들에 했던 것과 동일한 방법으로 합니다. 이번에는 커스텀 `hikr.Text` 를 사용합니다. 일반 `Text` 요소 대신 생성 된 것입니다.:

```
            <Panel Clicked="{save}">
                <hikr.Text Value="Save" />
            </Panel>
            <Button Text="Cancel" Clicked="{cancel}" />
```

텍스트가 버튼의 중앙에 있기를 원하기 때문에, `hikr.Text` 요소에 정렬을 추가합니다. 또한 기본 글꼴 크기를 지정합니다.

```
            <Panel Clicked="{save}">
                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

다음으로, 커스텀 버튼의 배경을 디자인과 일치하도록 변경하려고 합니다. 우리는 컴포넌트에 [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) 을 추가하는 것으로 시작해서, `Layer="Background"` 를 지정하여 컴포넌트의 배경으로 설정 할 것입니다. `Rectangle` 색상은 디자인의 버튼과 동일하게 적용됩니다.

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" />

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

꽤 괜찮아 보이지만, 우리 디자인의 버튼은 모서리가 둥급니다. `Rectangle` 에 `CornerRadius` 를 조금 추가해 보겠습니다.:

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4" />

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

커스텀 버튼 배경에서 마지막으로 할 일은, 버튼 바로 아래에 있는 약간의 [DropShadow](https://www.fusetools.com/docs/fuse/effects/dropshadow) 를 추가하는 것입니다. 우리는 그림자를 약간 투먕한 검정으로 만들고, 앱의 테마에 맞게 `Angle` , `Distance` , `Spread` 및 `Size` 값을 지정 할 것입니다.:

```
            <Panel Clicked="{save}">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

훌륭합니다! 이제 버튼의 배경과 텍스트를 설정 했으므로, 가장 바깥 쪽 `Panel` 주위에 margin(여백) 을 추가하고, 마찬가지로 안쪽으로 padding(여백) 을 추가합니다. 이것은 자연스러운 여백을 줄 것입니다.

```
            <Panel Clicked="{save}" Margin="10" Padding="10">
                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
            </Panel>
```

이 커스텀 버튼의 look(모양) 을 다루고 있습니다. 그러나 그 feel(느낌)은 어떨까요? 이 버튼은 우리 `HomePage` 에서 selector 들에 했던 것처럼 누르는 애니메이션을 가져야 합니다. 실제로, 같은 것을 사용합시다. 코드를 복사하여 커스텀 버튼에 붙여 넣으십시오.:

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

모두 저장 해보면, 꽤 좋은 feel(느낌) 입니다. 멋집니다!

`Save` 버튼이 잘 설정되었으므로, 재사용 가능한 `hikr.Button` 컴포넌트를 추출하고, 이 스타일을 `Cancel` 버튼에도 적용해 봅시다.

여기 `Panel` 에 `ux:Class="hikr.Button"` 속성을 추가하고, `Save` 버튼에 `hikr.Button` 의 새 인스턴스를 추가한 뒤, `Panel` 의 `Clicked` 핸들러를 다음과 같이 옮깁니다.:

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

다음으로 해야 할 일은, 하드코딩 된 `"Save"` 텍스트를 컴포넌트에서 인스턴스로 옮기는 것입니다. 그러나 우리 `hikr.Button` 에 단순히 `Text="Save"` 라고 작성 한다면, `hikr.Button` 클래스에는 그런 속성이 없기 때문에 컴파일에 실패합니다. Fuse는 UX 만 사용하는 컴포넌트에 대한 자체 속성을 정의 할 수 있는 `ux:Property` 라는 메커니즘을 제공하므로, 이 것으로 쉽게 해결할 수 있습니다.

`hikr.Button` 컴포넌트의 `Text` 속성을 만들려면, 우리 속성에 대한 이름을 정의하는 `ux:Property` 속성을 사용하여 `string` 인스턴스( `Text` 는 하나의 text 문자열을 포함 할 것이므로) 를 우리 클래스에 추가하면 됩니다.:

```
            <Panel ux:Class="hikr.Button" Margin="10" Padding="10">
                <string ux:Property="Text" />

                <Rectangle Layer="Background" Color="#125F63" CornerRadius="4">
                    <DropShadow Angle="90" Distance="1" Spread="0.2" Size="2" Color="#00000060" />
                </Rectangle>

                <hikr.Text Value="Save" FontSize="16" TextAlignment="Center" />
```

그것으로, 우리는 속성을 얻었습니다. 그러나 클래스의 `hikr.Text` 인스턴스에 여전히 하드 코딩 된 문자열 `"Save"` 가 표시됩니다. 우리는 방금 생성 한 `Text` 속성의 값을 대신 표시하길 원하고, *속성 바인딩* 을 통해 이 작업을 수행 할 수 있습니다. 속성 바인딩은 이 전에 UX 에서의 값을 JavaScript의 값에 바인딩 하는데 사용한 적이 있었던 데이터 바인딩과 비슷하며, 구문도 거의 같습니다. 버튼의 텍스트를 `Text` 속성에 바인딩 하려면 `hikr.Text` 인스턴스의 `Value` 를 다음과 같이 변경합니다.

```
                <hikr.Text Value="{Property Text}" FontSize="16" TextAlignment="Center" />
```

여기에서, 우리는 `ux:Property` 에 바인딩 하는 것이 JS 데이터에 바인딩 하는 것과 거의 동일한 것을 볼 수 있습니다. 그러나 우리는 그 앞에 `ReadProperty` 를 추가했습니다. 이 것이 Fuse가 서로 다른 바인딩 타입들 간의 차이점을 알려주는 것입니다.

새로운 `Text` 속성을 만들고 컴포넌트 내부에 바인딩하면, 이제 `Save` 버튼에 맞게 `Text` 속성을 설정할 수 있습니다.:

```
            <hikr.Button Text="Save" Clicked="{save}" />
            <Button Text="Cancel" Clicked="{cancel}" />
```

이것을 저장하면, 버튼이 이 전과 같게 보이지만, 이제는 `Text` 를 컴포넌트 외부에서 원하는대로 설정할 수 있습니다. 이것은 우리 컴포넌트를 진정으로 재사용 할 수 있도록 만듭니다!

> 참고: `ux:Property` 및 Fuse가 컴포넌트를 만드는데 제공하는 기타 유용한 도구에 대한 자세한 내용은 [컴포넌트 만들기](https://www.fusetools.com/docs/basics/creating-components) 문서를 확인하십시오.

새로운 컴포넌트를 우리 `Components` 디렉토리 내, `hikr.Button.ux` 라는 자체 파일로 옮겨 보겠습니다.

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

마지막으로 `Cancel` 버튼의 스타일도 지정합니다. 이 경우, `Button` 을 `hikr.Button` 으로 대체하면 됩니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <hikr.Button Text="Save" Clicked="{save}" />
            <hikr.Button Text="Cancel" Clicked="{cancel}" />
        </StackPanel>
```

그럼 이제 우리는 재사용 가능한 `hikr.Button` 컴포넌트와 두 개의 멋진 버튼을 얻게 되었습니다! 이제는 디자인에 따라 화면의 올바른 영역에 배치해야합니다. 특히 두 개의 버튼을 화면 맨 아래에 나란히 배치해야 합니다.

버튼들을 나란히 배치하기 위해, 그것들을 [Grid](https://www.fusetools.com/docs/fuse/controls/grid) 에 배치합니다. `Grid` 는 지정된 수의 *행* 과 *열* 을 기반으로 사용 가능한 공간을 나누고, 그 결과로 생성 된 *셀* 에 자식을 배치하는 `Panel` 입니다. 기본적으로 이러한 행과 열의 사이즈는 동일합니다. 따라서 단일 행과 두 개의 열이있는 `Grid` 를 지정하면, 두 개의 버튼에 대한 완벽한 컨테이너를 얻게됩니다.:

```
            <Text>Comments:</Text>
            <TextView Value="{comments}" TextWrapping="Wrap" />

            <Grid ColumnCount="2">
                <hikr.Button Text="Save" Clicked="{save}" />
                <hikr.Button Text="Cancel" Clicked="{cancel}" />
            </Grid>
        </StackPanel>
```

여기서 `Grid` 에 두 개의 열이 필요하므로, `ColumnCount` 를 `2` 로 지정합니다. 우리는 하나의 행만 필요하기 때문에, 이 경우에 행이 얼마나 많이 필요한지를 지정할 필요가 없습니다. 이렇게하면 버튼이 나란히 배치됩니다. 버튼의 순서를 디자인과 일치시켜 보겠습니다.:

```
            <Grid ColumnCount="2">
                <hikr.Button Text="Cancel" Clicked="{cancel}" />
                <hikr.Button Text="Save" Clicked="{save}" />
            </Grid>
```

완료했습니다! 여기서는 `Grid` 가 지정된 순서대로 자식 요소들을 배치하는 것을 볼 수 있습니다. `Grid` 의 왼쪽 위 셀부터 시작합니다. 우리 자식 요소들이 차지하는 셀을 변경하려면, 다른 순서로 지정해야 합니다. 멋지고 쉽습니다!

마지막으로, 이 `Grid` 를 `Page` 맨 아래에 베치합니다. 이를 위해, 우리는 `Page` 의 현재 내용을 모두 [DockPanel](https://www.fusetools.com/docs/fuse/controls/dockpanel) 에 배치하고, `Dock="Bottom"` 을 사용하여 `DockPanel` 의 아래쪽에 `Grid` 를 고정시키도록 합니다. 전체적으로, 우리의 `EditHikePage.ux` 파일은 다음과 같습니다 :

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

그리고 이를 통해 `Cancel` 및 `Save` 버튼의 look and feel (모양과 느낌) 및 배치를 모두 커스터마이징 했습니다.

## 에디터들 조정하기 ##

이제 `EditHikePage` 에서 다양한 값 에디터들의 look/feel (모양/느낌) 을 살펴보고 조정 해 봅시다. 우리의 기억을 새롭게하기 위해, 이 페이지의 디자인을 다시 한번 살펴 보겠습니다.:

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

우선, 현재 구현에는 하이킹 이름을 표시하는 추가 `Text` 요소가 있습니다. 우리가 이미 이것을 위한 에디터를 가지고 있기 때문에, 우리는 서둘러 그것을 제거 하도록 합니다.:

```
            <StackPanel>
                <Text>Name:</Text>
                <TextBox Value="{name}" />
```

다음으로 모든 에디터의 제목 텍스트 를 조정 해 봅시다. 먼저 `Name` 필드의 `Text` 요소를 `hikr.Text` 요소로 바꾸기 시작합니다.:

```
                <hikr.Text>Name:</hikr.Text>
                <TextBox Value="{name}" />
```

이것도 꽤 괜찮아 보이지만, 이 `hikr.Text` 요소의 불투명도를 약간 조정합시다. 이젠 흰색이 아닙니다.:

```
                <hikr.Text Opacity=".6">Name:</hikr.Text>
                <TextBox Value="{name}" />
```

그런 다음 제목 텍스트와 에디터 필드 사이에 margin(여백) 을 추가합니다.:

```
                <hikr.Text Opacity=".6" Margin="0, 0, 0, 5">Name:</hikr.Text>
                <TextBox Value="{name}" />
```

이것은 꽤 괜찮아 보이므로, 이 스타일을 `TitleText` 라고 부르는 자체 커스텀 컴포넌트로 추출해 냅니다. 그리고 `Name` 제목 필드를 수정 합니다.:

```
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />

                <TitleText>Name:</TitleText>
                <TextBox Value="{name}" />
```

이미 잘 정리되어 보입니다! `ux:Class` 를 다시 사용함으로써, 우리 서브클래스들 중 하나를 서브클래스화(상속) 할 수 있게 된 것을 잘 보십시오. 이제 다른 에디터들의 제목들도 수정해 보겠습니다.:

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

멋지고 쉽습니다! 다음으로, 다양한 에디터들/제목들들 주변 간격을 조정할 것입니다. 우리가 할 수 있는 첫 번째 작업은, [StackPanel](https://www.fusetools.com/docs/fuse/controls/stackpanel) 테두리에 약간의 간격을 두기 위해, 에디터가 포함된 StackPanel 에 약간의 padding(여백) 을 적용하는 것입니다.:

```
            <StackPanel Padding="10">
                <hikr.Text ux:Class="TitleText" Opacity=".6" Margin="0,0,0,5" />
```

이것은 에디터들과 `Page` 의 경계선들 사이의 간격에는 도움이 되지만, 다양한 다른 요소 사이에는 해당되지 않습니다. `StackPanel` 에 [ItemSpacing](https://www.fusetools.com/docs/fuse/controls/stackpanel/itemspacing) 을 추가해 봅시다. (이름에서 알 수 있듯이) `StackPanel` 의 여러 항목 사이에 간격을 추가해야합니다.:

```
            <StackPanel ItemSpacing="10" Padding="10">
```

거의 다 되었습니다; 이제 요소들 사이에 간격이 생겼지만, 제목들과 에디터들 사이에 더 많은 여백이 생겼습니다. 이를 해결하기 위해, 각 제목/에디터 쌍 을 자체 `StackPanel` 로 감쌉니다. 이렇게 하면, 주변 `StackPanel` 의 `ItemSpacing` 이 제목/에디터 쌍 사이에만 적용되며, 제목과 에디터는 서로 관련하여 적절한 간격을 유지합니다. 그러면 다음과 같이 보일 것입니다.:

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

이로써, 모든 에디터들은 적절한 간격을 두고 있으며, 우리는 간단한 속성들과 `Panel` 들을 구성하여 이를 수정할 수 있었습니다. 멋집니다!

이제 에디터들을 봅시다. 이전과 마찬가지로, 처음엔 반복적으로 수정한 다음, 재사용 가능한 컴포넌트를 만든 뒤, 다른 것들에 전부 적용할 것입니다. 이 경우 유일한 예외는, 다른 것들처럼 [TextBox](https://www.fusetools.com/docs/fuse/controls/textbox) 를 사용하지 않고, [TextView](https://www.fusetools.com/docs/fuse/controls/textview) 를 사용한 `Comments` 에디터 입니다. 이것은 자체적인 스타일링이 필요하므로, 먼저 다른 에디터의 `TextBox` 를 처리 한 다음, 다시 돌아올 것 입니다.

이제 `Name` 에디터의 `TextBox` 를 살펴 보겠습니다. 먼저 `hikr.Text` 컴포넌트와 마찬가지로 `TextColor` 를 `White` 로 설정합니다. 또한 `CaretColor` 를 `White` 로 설정하여, 깜박이는 캐럿 인디케이터 의 색상을 조절할 수 있습니다. 그리고 거기에 약간의 `Padding` 을 추가해 봅시다.

```
                <StackPanel>
                    <TitleText>Name:</TitleText>
                    <TextBox TextColor="White" CaretColor="White" Padding="10,10,0,10" Value="{name}" />
                </StackPanel>
```

괜찮아 보입니다! 이제 `hikr.TextBox` 라는 재사용 가능 컴포넌트로 그걸 추출합시다. 그리고 `hikr.TextBox` 인스턴스들로 그것들을 대체해서, 다른 모든 `TextBox` 기반 에디터들에 스타일을 적용해 봅시다. (그러나 그것들의 바인딩 및 `InputHints` 에 해당하는 부분은 그대로 둡니다).

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

마지막으로, 우리는 아마도 나중에 다른 화면들에서 이 컴포넌트를 사용할지도 모르므로, `Components` 디렉토리로 옮길 수 있습니다. 그래서 우리는 `ux:Class` 를 `Components` 디렉토리의 새로운 `hikr.TextBox.ux` 파일로 옮길 것입니다 :

```
<TextBox ux:Class="hikr.TextBox" TextColor="White" CaretColor="White" Padding="10,10,0,10" />
```

이제 이 페이지의 내용을 마무리하기 전에, `Comments` 에디터를 작업할 시간 입니다. 이것은 여러 줄을 표시 할 수 있는 `TextView` 이므로, 이전에 `TextBox` 들에 대한 스타일링과는 다른 스타일을 적용하길 원합니다. 그러나 일부 `TextColor` , `CaretColor` 및 `Padding` 을 해당 에디터에 적용함으로써, 기본적으로 동일한 프로세스로 시작 합니다.:

```
                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <TextView TextColor="White" CaretColor="White" Padding="5" Value="{comments}" TextWrapping="Wrap" />
                </StackPanel>
```

다음으로, 우리는 `TextView` 배경에 약간의 불투명도가 있는 둥근 [Ractangle](https://www.fusetools.com/docs/fuse/controls/rectangle) 을 추가하여, 디자인과 일치 시키길 원합니다. `hikr.Button` 컴포넌트에서 했던 것처럼 `Layer="Background"` 로 `Rectangle` 인스턴스를 추가하고, `Color` 및 `CornerRadius` 를 설정합니다.:

```
                <StackPanel>
                    <TitleText>Comments:</TitleText>
                    <TextView TextColor="White" CaretColor="White" Padding="5" Value="{comments}" TextWrapping="Wrap">
                        <Rectangle Layer="Background" Color="#fff2" CornerRadius="4" />
                    </TextView>
                </StackPanel>
```

좋아 보입니다! 이것은 이 인스턴스 하나에만 사용되므로, 우리는 이 스타일의 `TextView` 를 다른 것들처럼 자체 컴포넌트로 분리할 필요는 없습니다.  그러나 보다 나은 관리를 위해, 분리 합시다. 이 전과 마찬가지로 `Components` 디렉토리에 `hikr.TextView.ux` 라는 파일을 만들고, 거기에 `ux:Class` 정의를 추가하는 스타일이 지정된 `TextView` 코드를 배치합니다. 그런 다음, `Comments` 에디터에 대한 이 `hikr.TextView` 를 사용하도록 `EditHikePage` 를 수정 합니다. `TextWrapping="Wrap"` 과 해당 컴포넌트에 그것들을 배치하는 대신 해당 *인스턴스* 를 바인딩 하는, `Value` 를 남겨 두어야 합니다. 이것들은 해당 컴포넌트를 사용할때, 우리 스스로 설정 할 가능성이 아주 높은 속성들입니다.

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

이것으로 `EditHikePage` 의 내용을 마칩니다!

## 페이지들에 배경 이미지 추가 ##

완료된 [Page](https://www.fusetools.com/docs/fuse/controls/page) 들의 내용으로 이 `Page` 들의 디자인을 마무리하는데 집중합시다. 우리는 이제 막 단색 배경색과 꽤 멀어졌지만, 이번에는 우리의 디자인과 일치하는 배경 이미지를 페이지들에 추가 할 차례입니다. 이 이미지는 앱에서 사용할 *static asset (정적 에셋)* 이므로, 먼저 프로젝트의 루트 디렉토리에 `Assets` 폴더를 만듭니다.:

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

해당 이미지를 `Assets` 디렉토리에 `background.jpg` 로 저장하십시오.

이제 프로젝트 폴더에 이미지를 얻었으므로, 우리 앱에서 이를 사용하길 원합니다. 우리의 디자인에 따라 이 이미지를 `HomePage` 와 `EditHikePage` 의 배경으로 사용할 것입니다. 우리 앱 전체에 적용 할 *수도* 있지만, 각 `Page` 에 적용하면 `Page` 에서 발생하는 트랜지션들이 배경 이미지에도 적용될 것을 보장하므로, 이게 더 나은 현명한 디자인이 됩니다. 자, 우리 `HomePage` 부터 시작해 각 `Page` 에 적용해 보겠습니다.

`HomePage.ux` 에서, `Layer="Background"` 설정으로 [Image](https://www.fusetools.com/docs/fuse/controls/image) 요소를 하나 추가함으로써, 이전 우리 컴포넌트들에 둥근 `Rectangle` 들을 추가한 것과 유사하게, 이 이미지를 `Page` 의 배경으로 적용할 것입니다.:

```
<Page ux:Class="HomePage">
    <Image Layer="Background" File="../Assets/background.jpg" />

    <Router ux:Dependency="router" />
```

`File` 속성의 값이 `HomePage.ux` 파일과 어떻게 관련되어 있는지 보십시오. 이 설정을 하면 배경에 이미지가 생기지만, 이미지 내용은 예상대로 `Page` 의 전체 공간을 채우지 못합니다. 이는 `Image` 요소가 해당 내용의 종횡비 변경없이 사용 가능한 공간에 맞춰 내용을 축소하거나 늘리는 것이 기본값으로 되어있기 때문이고, 이미지 파일의 가로 세로가 우리의 `Page` 와는 약간 다르기때문에, 내용이 약간 줄어들 것입니다. 이 문제를 해결하기 위해, `Image` 요소에 대해 `StretchMode = "UniformToFill"` 을 지정할 수 있습니다. 이렇게 하면 내용이 항상 종횡비를 유지하면서, 사용 가능한 전체 공간을 채우는 것을 보장합니다.

```
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" />
```

> 참고: `Image` 요소가 이미지를 표시 할 수 있는 다양한 방법에 대한 상세 내용은 [이미지 기본](https://www.youtube.com/watch?v=d5lA0yyq9g0) 비디오를 확인하십시오!

마지막으로, 우리가 가진 어두운 청록 배경색으로 이미지에 "tint (색조)" 를 적용하기 위해, 이 `Image` 요소를 약간 투명하게 만들어 봅시다.

```
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" Opacity=".7" />
```

훌륭합니다! 이제 백그라운드 이미지가 우리 `HomePage` 에 올바르게 적용되었으므로, `EditHikePage` 에도 적용 해 보겠습니다. 아마 우리가 어떻게 할지 추측 할 수 있을 겁니다 - 우리는 먼저 이 배경 이미지가 적용된 재사용 가능한 `hikr.Page` 컴포넌트를 만든 다음, `Page` 클래스 대신 해당 클래스에서 직접 파생되는 `HomePage` 및 `EditHikePage` 컴포넌트를 만들 것입니다.

따라서, 이 전과 마찬가지로 `Components` 디렉토리에 다음을 포함하는 `hikr.Page.ux` 라는 새 파일을 만듭니다.:

```
<Page ux:Class="hikr.Page">
    <Image Layer="Background" File="../Assets/background.jpg" StretchMode="Fill" Opacity=".7" />
</Page>
```

그런 다음, `Pages/HomePage.ux` 및 `Pages/EditHikePage.ux` 에서 코드를 업데이트합니다. 여는 태그와 닫는 태그를 모두 수정하는 것을 잊지 마십시오; 이것들은 놓치기 쉽습니다.:

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

좋습니다, 이제 `Page` 가 완전히 완성되어 멋지게 보입니다.

## 상태 표시 줄 조정 ##

이제 우리 앱의 전체 look and feel (모양과 느낌) 은 거의 끝났습니다! 좀 더 살펴볼 필요가 있는 것은, 앱의 상태표시줄 뿐입니다. 디바이스에서 앱을 실행하면 꽤 괜찮아 보이겠지만, 우리의 상태 표시 줄은 사실 이 테마에 적합하지 않습니다. 특히 어두운 배경 때문에, iOS의 상태 표시줄 텍스트와 아이콘을 읽기 힘들 수 있으며, Android에서 상태 표시줄의 배경색은 단순하게 잘못된 색상으로 표시될 겁니다.

이러한 문제를 해결하기 위해, Fuse는 iOS 및 Android의 상태 표시줄을 각각 커스터마이징 하는데 사용할 수 있는 [iOS.StatusBarConfig](https://www.fusetools.com/docs/fuse/ios/statusbarconfig) 및 [Android.StatusBarConfig](https://www.fusetools.com/docs/fuse/android/statusbarconfig) 클래스를 제공합니다. 이를 사용하려면, `MainView.ux` 의 `App` 태그 상단에 각각의 인스턴스를 추가하고 각 플랫폼에 적절한 속성을 설정해야 합니다. Fuse에 어두운 텍스트가 아닌 밝은 텍스트와 아이콘이 있는 상태 표시줄이 필요하다고 말하기 위한 `Style="Light"` 를 지정하는 것을, iOS부터 시작해 봅시다.:

```
<App Background="#022328">
    <iOS.StatusBarConfig Style="Light" />

    <Router ux:Name="router" />
```

다음으로 Android 상태 표시 줄을 살펴 보겠습니다. 이 경우, 앱의 배경색과 일치하는 배경색을 지정합니다.:

```
<App Background="#022328">
    <iOS.StatusBarConfig Style="Light" />
    <Android.StatusBarConfig Color="#022328" />
```

그리고 그것으로, 우리 앱이 디바이스에서도 멋지게 잘 보입니다!

> 참고: 각 플랫폼에 대한 `StatusBarConfig` 클래스의 각 인스턴스는 최대 하나만 갖는 것이 중요합니다. 그렇지 않으면 올바르게 작동하지 않을 수 있습니다. 상태 표시줄 및 이와 유사한 구성에 대한 자세한 내용은 [상태 표시줄 기본](https://www.youtube.com/watch?v=EBopndSkk5c) 비디오를 확인하십시오.

## 지금까지의 진행 ##

휴우, 해야 할 것이 많았습니다! 하지만 이제는 앱의 look and feel(모양과 느낌) 을 해결했고, Fuse에서 재사용 가능한 컴포넌트를 추출하는 연습을 많이 했습니다. 훌륭합니다!

현재, 우리의 앱은 이렇게 보입니다.:
[https://res.cloudinary.com/fusetools/image/upload/documentation_v2/2cb85e19bf765c87c7b6b63d69e727db__media/hikr/chapter-6/chapter-6.mp4](https://res.cloudinary.com/fusetools/image/upload/documentation_v2/2cb85e19bf765c87c7b6b63d69e727db__media/hikr/chapter-6/chapter-6.mp4)

코드에 관해서는, 이번에 꽤 많은 파일을 다루었기 때문에, 여기에 나열하는 대신 Github의 다양한 파일들을 간단히 링크합니다.

- [Assets/Background.jpg](https://github.com/fusetools/hikr/blob/chapter-6/Assets/background.jpg)
- [Components/hikr.Button.ux](https://github.com/fusetools/hikr/blob/chapter-6/Components/hikr.Button.ux)
- [Components/hikr.Page.ux](https://github.com/fusetools/hikr/blob/chapter-6/Components/hikr.Page.ux)
- [Components/hikr.Text.ux](https://github.com/fusetools/hikr/blob/chapter-6/Components/hikr.Text.ux)
- [Components/hikr.TextBox.ux](https://github.com/fusetools/hikr/blob/chapter-6/Components/hikr.TextBox.ux)
- [Components/hikr.TextView.ux](https://github.com/fusetools/hikr/blob/chapter-6/Components/hikr.TextView.ux)
- [Pages/EditHikePage.ux](https://github.com/fusetools/hikr/blob/chapter-6/Pages/EditHikePage.ux)
- [Pages/HomePage.ux](https://github.com/fusetools/hikr/blob/chapter-6/Pages/HomePage.ux)
- [MainView.ux](https://github.com/fusetools/hikr/blob/chapter-6/MainView.ux)

## 다음은 뭔가요? ##

이제 앱의 주요 부분들이 기능적이고 보기 좋아 졌으므로, 모든 것을 하나로 묶을 시간입니다. [다음 챕터](https://www.fusetools.com/docs/tutorial/splash-screen) 에서는, 비디오 배경을 가진 아름다운 스플래시 스크린을 구축함으로써, 우리 앱을 완성 할 것입니다. 우리는 이미 앱의 아키텍처와 재사용 가능 컴포넌트들 가지고 있기 때문에, 아무런 문제가 없을 겁니다. 그럼 [파 봅시다](https://www.fusetools.com/docs/tutorial/splash-screen) !

이 장의 마지막 코드는 [여기](https://github.com/fusetools/hikr/tree/chapter-6) 에서 볼 수 있습니다.
