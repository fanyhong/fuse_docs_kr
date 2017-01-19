원본: https://www.fusetools.com/docs/fuse/reactive/with

클래스와 함께

  고급 기능 표시
이동 :
목차
현재 데이터 컨텍스트가 범위가 좁혀지는 범위를 나타냅니다.

With는 복잡한 데이터 컨텍스트가 있고 데이터 바인딩을 단순화하려는 경우에 유용합니다. 이 기능은 특히 중첩 된 개체 그래프의 일부를 보는 데 유용합니다.

예

<자바 스크립트>
     module.exports = {
         복합체 : {
             item1 : {
                 부제 1 : {이름 : "Spongebob", 나이 : 32}
             }
         }
     };
</ JavaScript>
<With Data = "{complex.item1.subitem1}">
     <텍스트 값 = "{name}"/>
     <텍스트 값 = "{연령}"/>
</ With>
의 인터페이스

