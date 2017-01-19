원본: https://www.fusetools.com/docs/scripting/scripting

스크립팅 및 데이터 바인딩

UX Markup의 <JavaScript> 태그는 포함 된 범위의 각 인스턴스에 대해 인스턴스화되는 모듈을 만듭니다.

퓨즈는 CommonJS 모듈 시스템을 구현합니다. 즉, 변수를 module.exports 객체에 첨부하여 변수를 외부로 내보낼 수 있습니다.

데이터 바인딩은 중괄호 구문 인 Property = "{variable}"을 사용하여 수행됩니다.

App-global 스크립트

<JavaScript /> 태그를 사용하여 (예 : <App> 태그에서 직접 응용 프로그램 전역 모듈을 만들 때) <JavaScript> 태그를 UX 마크 업에 인라인으로 배치 할 수 있습니다.

<App>
    <자바 스크립트>
        module.exports.foo = "bar";
    </ JavaScript>
    <패널>
        <텍스트 값 = "{foo}"/>
    </ Panel>
</ App>
코드가 UX 마크 업에서 인라인 일 필요는 없습니다. 우리는 그것을 별도의 파일에 넣고 이름으로 참조 할 수 있습니다. 이것은 samme 효과가 있습니다.

<App>
    <JavaScript File = "Main.js"/>
    <패널>
        <텍스트 값 = "{foo}"/>
    </ Panel>
</ App>
구성 요소 - 로컬 스크립트

또한 <JavaScript> 태그를 ux : Class 내에 배치 할 수 있습니다. 그러면 클래스의 각 인스턴스에 대한 모듈을 평가하여 구성 요소 로컬 상태와 데이터를 제공합니다.

<Panel ux : Class = "MyComponent">
    <자바 스크립트>
        var Observable = require ( "FuseJS / Observable");
        var foo = 관찰 가능 (10);
        module.exports = {foo}
    </ JavaScript>

    <텍스트 값 = "{foo}"/>
</ Panel>
'foo'변수는 이제 MyComponent의 각 인스턴스와 아래의 데이터 바인딩에 대해 존재합니다.

퓨즈의 JavaScript에 대한 중요 참고 사항

Fuse의 비즈니스 로직은 JavaScript 나 Babel이나 TypeScript 같은 외부 변환기를 사용하여 JavaScript로 컴파일되는 언어로 작성 될 수 있습니다.

퓨즈는 마크 업과 자바 스크립트 사용에도 불구하고 웹 또는 브라우저 기반 플랫폼이 아니라고 설명하는 것이 중요합니다. 퓨즈가 데이터 바인딩, 레이아웃 및 애니메이션을 처리하는 방법은 모두 퓨즈 플랫폼에 맞게 조정됩니다. process와 fs 같은 모듈에 의존하는 Node.js 라이브러리가 웹 페이지에서 작동하지 않을 것이라고 생각하는 것과 마찬가지로 Fuse의 JS 접근 방식은 동일합니다.

간단히 말해, 일반적인 DOM 기반 JS 프레임 워크가 아닌 새로운 것으로 퓨즈에 접근하면 더 큰 성공을 거둘 수 있습니다 (재미 있습니다!). :)

데이터 바인딩 예제

중괄호 구문은 바인딩 경로와 일치하는 데이터 컨텍스트에서 가장 가까운 개체에 바인딩합니다.

<App>
    <자바 스크립트>
        var hello = "world";

        function writeHello () {
            console.log ( "hello"+ hello);
        }

        module.exports = {
            안녕 안녕,
            writeHello : writeHello
        };
    </ JavaScript>
    <패널>
        <텍스트 값 = "{hello}"클릭 = "{writeHello}"/>
    </ Panel>
</ App>
배열에 대한 데이터 바인딩

각 비헤이비어를 사용하여 배열에 데이터 바인딩 할 수 있습니다.

<App>
    <자바 스크립트>
        var colors = [ "# f00", "# 0f0", "# 00f"];

        module.exports = {
            색상 : 색상
        };
        </ JavaScript>
    <StackPanel>
        <각 항목 = "{색상}">
            <사각형 색상 = "{}"높이 = "40"/>
        </ 각>
    </ StackPanel>
</ App>
각 비헤이비어는 바운드 배열의 각 항목에 대해 한 번씩 자식을 인스턴스화합니다. 그런 다음이 항목은 해당 인스턴스의 데이터 컨텍스트가되어 비어있는 중괄호 {} 세트로 색상 값에 데이터 바인딩 할 수 있습니다.

UI가 변화에 반응하게하기

대부분의 경우 앱의 수명주기 동안 변경되는 동적 데이터를 표시하려고합니다. 이는 Observable에 데이터를 저장함으로써 간단히 수행됩니다. Observable은 Fuse 반응 데이터 바인딩 시스템의 핵심 부분이며 Fuse 응용 프로그램의 모든 곳에서 사용됩니다.

Observables는 단일 값 또는 값 목록처럼 작동합니다. 모든 데이터 바인딩 Observable은 값이 변경되면 뷰를 자동으로 업데이트합니다.

<App>
    <자바 스크립트>
        var Observable = require ( 'FuseJS / Observable');
        var count = 관찰 가능 (0);
        함수 증가 () {
            count.value = count.value + 1;
        };
        module.exports = {
            count : count,
            증가 : 증가
        };
    </ JavaScript>
    <StackPanel>
        <텍스트 값 = "{count}"/>
        <버튼 텍스트 = "증가"클릭 = "{증가}"/>
    </ StackPanel>
</ App>
목록 변경

add 메소드를 사용하여 Observable에 항목을 추가 할 수도 있습니다.

<App>
    <자바 스크립트>
        var Observable = require ( 'FuseJS / Observable');
        var numbers = 관찰 가능 (0);
        함수 증가 () {
            numbers.add (numbers.length);
        };
        module.exports = {
            숫자 : 숫자,
            증가 : 증가
        };
    </ JavaScript>
    <DockPanel>
        <Button Text = "증가"클릭 = "{증가}"Dock = "위쪽"/>
        <StackPanel>
            <각 항목 = "{숫자}">
                <텍스트 값 = "{}"/>
            </ 각>
        </ StackPanel>
    </ DockPanel>
</ App>
각 동작은 데이터 바인딩 Observable의 내용이 변경되면 자체적으로 업데이트됩니다.
