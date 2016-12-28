
원본: https://www.fusetools.com/docs/tutorial/tutorial

# 소개 #
저희 튜토리얼에 오신 것 환영합니다.
우리가 중단했던 퀵스타트를 개선하는 이 튜토리얼은 우리가 스크래치로부터 Fuse 앱을 만드는 단계들 거칠 것입니다.
저희가 함께 만들 앱은 우리가 있었던 하이킹장소들을 모아서 보여주고, 그 것들의 평가로 변화를 만들고, 코멘트를 추가할 수 있는 기본적인 하이크 트래킹앱 "hikr" 입니다.
전체 소스코드는 여기에 있습니다.
hikr는 간단한 앱이고, 의도적으로 그렇게 한것입니다. 우리는 다양한 조각들로부터 하나의 앱을 빌딩하고, 너무 세세한 것들에 얽매이지 않고 함께 적합한 방법을 찾아봄으로써 Fuse의 컨셉들을 배우기를 바랍니다. hikr는 이것을 위해 특별히 디자인되어졌으며, 앱을 만들어 봄으로써, 우리는 Fuse에서 현실-세계를 빌드하는 많은 주요한 부분들을 배울 것입니다. 

추가적으로, 원래 튜토리얼을 중단했던 곳에서 이어서 다양한 "tracks" 를 추가할 것입니다, 그리고 특정 백엔드들이 추가되는 것, 어떤 에디터들을 향상시키는것, 애니메이션들이 추가되는것, 그리고 그 이상의 것들을 다룰 것입니다. 우리가 가장 먼저 핵심 아이디어들에 포커스를 맞추는 이런 방법은, 나중에 더 향상되고 구체적인 것들을 파헤칠 것입니다. 그러므로 이 것들을 계속 주목해주십시오!

## 핵심 컨셉들 ##  
부분적으로, hikr 는 이 핵심 컨셉들을 다룹니다.
- Fuse에서 하나의 앱 구조를 잡는 최선의 연습들
- 옵져버블과 데이터바인딩
- 데이터를 변경을 하고 백엔드와 이 변화에 대한 커뮤니케이션 하기
- 재사용 가능한 컴포넌트들을 빌드하기위한 시간과 방법
- 라우팅과 네이게이션으로 컴포넌트들을 조합하기
그리고 많은 다른 것들 처럼 우리는 이 핵심 컨셉들로 함께 앱을 빌드하는 것으로써 실제 만들어본다는 경험을 얻을 것입니다.

## 챕터 ##
이 튜토리얼은 다음과 같은 챕터들로 분리되어 있습니다.
1. Hike 뷰 편집
2. 다양한 hikes
3. 컴포들 나누기
4. 네이게이션과 라우팅
5. 백엔드 모형만들기
6. 룩앤필 수정하기
7. 스플래시 스크린
8. 마지막 생각들과 다음은 무엇일지

각 챕터는 이전에 멈춘 곳에서부터 진행되므로, 너무 많이 뒤로 건너뛰지 않고 순차적으로 진행할 것을 추천합니다.


# hikr's Design #
hikr의 디자인은 꽤 쉽습니다. 앱은 비디오 백그라운드와 모든 것이 완전한, 아름다운 스플래시 화면으로 시작할 겁니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/77d73cbf99a8449e743f59aed5a8af77__media/hikr/tutorial/splash.webp)

"Get Started" 버튼을 누르면 최근 하이킹 목록들을 보여주는 홈페이지로 갑니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/14954aa251367c1d687c1a0503190d21__media/hikr/tutorial/home.webp)

마침내, 이렇게 보이는 Edit Hike 화면에서 이 하이킹들중 어떤 것을 탭할 수 있습니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/3c206c491b98db55ab1a4e3c08ab2d2a__media/hikr/tutorial/edit-hike.webp)

이 페이지는 특정 하이킹을 편집하게 해줍니다, 그리고 "Save" 버튼으로 이 변경사항들을 저장하거나, "Cancel" 버튼으로 취소할 수 있습니다. 그러고나면 두 버튼들은 홈페이지로 돌아게 될것입니다.

![](https://res.cloudinary.com/fusetools/image/upload/w_450%2Ch_450%2Cdpr_1.0%2Cc_limit/documentation_v2/9ad834ec45583c3d17817e1973f50449__media/hikr/tutorial/app-flow.webp)

그리고 여기서 모션으로 전체앱을 볼 수 있습니다.
https://res.cloudinary.com/fusetools/image/upload/documentation_v2/a6ee74459f1d4e432228b59b1f08f778__media/hikr/tutorial/hikr.mp4

## 다음은 무엇입니까 ##
이제 기본적인 뷰 모델을 따라서 Edit Hike 화면을 만드는 것으로부터 시작할 기본 아이디어를 얻었습니다.
앱의 전체소스코드는 여기 있습니다.
