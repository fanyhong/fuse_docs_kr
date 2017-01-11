원본: https://www.fusetools.com/docs/tutorial/tutorial

# 소개 #

시작부터 끝까지 앱을 만들어 보는 저희 튜토리얼에 오신 것 환영합니다!

우리의 [quickstart](https://www.fusetools.com/docs/basics/quickstart) 가 중단 된 곳에서, 이 튜토리얼은 처음부터 퓨즈 앱을 만드는 과정을 안내합니다. 우리가 함께 할 앱은 "hikr" 라고 불리는 것입니다. 기본적인 하이킹 추적 앱 으로, 우리가 해왔던 하이킹들을 모아서 보여주고 그것들을 등급를 매기거나 댓글을 달아 변경할 수 있습니다.

앱의 전체 소스 코드는 [여기](https://github.com/fusetools/hikr) 에서 볼 수 있습니다.

hikr는 간단한 응용 프로그램이며 의도적으로 그렇게 한 것입니다. 우리는 여러 조각으로 응용 프로그램을 작성하고, 세부 사항에 얽매이지 않고 함께 어울리는 방법을 확인하여 퓨즈의 개념을 배우기를 원합니다. hikr는 이것을 위해 특별히 디자인되어졌으며, 앱을 만들어 봄으로써, Fuse에서 현실 세계를 빌드하는 많은 핵심 요소들을 배울 것입니다. 

또한 원본 튜토리얼에서 벗어나지 않고 계속 진행되는 다양한 "트랙" 을 추가하고 특정 백엔드 추가, 편집기 개선, 애니메이션 추가 등과 같은 것들을 다룰 예정입니다. 이렇게하면 우선 핵심 아이디어에 초점을 맞추고 나중에 한 단계 더 나아간 세부적인 사항들에 깊게 들어갈 수 있습니다. 그러니 계속 지켜봐주십시오!

## 핵심 컨셉 ##

특히, hikr 는 다음의 핵심 컨셉들을 다룹니다.

- Fuse에서 하나의 앱 구성을 위한 모범 사례
- Observables 와 데이터바인딩
- 데이터를 변경을 하고 백엔드와 이 변화에 대한 커뮤니케이션 하기
- 재사용 가능한 컴포넌트들을 빌드하기 위한 방법 및 시기
- 라우팅과 네이게이션으로 컴포넌트들을 조합하기

또한 앱을 함께 만들면 이러한 핵심 개념뿐 아니라 다른 많은 기능을 통해 실제 경험을 쌓게 됩니다.

## 챕터 ##

이 튜토리얼은 다음과 같은 챕터들로 분리되어 있습니다.

1. [Hike 뷰 편집](https://www.fusetools.com/docs/tutorial/edit-hike-view)
2. [다양한 hikes](https://www.fusetools.com/docs/tutorial/multiple-hikes)
3. [컴포넌트들 나누기](https://www.fusetools.com/docs/tutorial/splitting-up-components)
4. [네이게이션과 라우팅](https://www.fusetools.com/docs/tutorial/navigation-and-routing)
5. [백엔드 모형만들기](https://www.fusetools.com/docs/tutorial/mock-backend)
6. [룩앤필 수정](https://www.fusetools.com/docs/tutorial/look-and-feel)
7. [스플래시 스크린](https://www.fusetools.com/docs/tutorial/splash-screen)
8. [마지막 생각들 / 다음은 무엇일지](https://www.fusetools.com/docs/tutorial/final-thoughts)

각 챕터는 이전에 멈춘 곳에서부터 진행되므로, 너무 많이 뒤로 건너뛰지 않고 순차적으로 진행할 것을 추천합니다.


# hikr 디자인 #

hikr의 디자인은 매우 간단합니다. 이 앱은 비디오 배경과 모든 것을 갖춘 아름다운 스플래시 화면으로 시작됩니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/77d73cbf99a8449e743f59aed5a8af77__media/hikr/tutorial/splash.webp)

"Get Started" 버튼을 누르면 우리의 최근 하이킹 목록을 보여주는 홈 페이지로 이동합니다

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/14954aa251367c1d687c1a0503190d21__media/hikr/tutorial/home.webp)

마지막으로 이러한 하이킹 중 하나를 눌러 다음과 같은 Edit Hike 뷰 로 이동할 수 있습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

이 페이지에서 특정 하이킹을 수정할 수 있으며 "Save"버튼을 사용하여 변경 사항을 저장하거나 "Cancel"버튼을 사용하여 변경 사항을 되돌릴 수 있습니다. 버튼을 누르면 모두 홈페이지로 돌아가게 됩니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/9ad834ec45583c3d17817e1973f50449__media/hikr/tutorial/app-flow.webp)

다음은 전체 앱이 움직이는 모습입니다.

https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4

## 다음은 무엇입니까? ##

이제 우리가해야 할 일에 대한 기본적인 아이디어를 얻었으므로 기본 뷰 모델과 함께 하이라이트 편집 편집을 [시작합니다](let's get started) . 그럼 시작합시다!

앱의 전체 소스 코드는 [여기](https://github.com/fusetools/hikr) 에서 볼 수 있습니다.
