원본: https://www.fusetools.com/docs/fuse/controls/textinput

TextInput 클래스

 고급 기능 표시
이동 :
목차
단일 행 텍스트 입력 제어.

TextInput은 사용자 이름, 비밀번호, 숫자, 전자 메일, 검색 필드 등과 같이 한 줄만 입력해야하는 입력 필드를 만들 때 일반적으로 사용하거나 서브 클래스로 사용합니다. 기본적으로 표시되지 않으므로 사용자가 제공 할 때까지 보이지 않습니다. 보기 또는 텍스트 값.

기본 모양으로 텍스트 입력 컨트롤을 사용하려면 TextBox를 참조하십시오. 여러 줄의 텍스트를 허용하려면 TextView를 사용하십시오.
예제들

이 예제는 스타일이있는 기본 TextInput과 내용을 지우는 버튼을 보여줍니다.

<패널>
    <Button Alignment = "CenterRight"Text = "Clear"여백 = "5">
        <클릭>
            <Set text.Value = ""/>
        </ Clicked>
    </ Button>
    <TextInput ux : Name = "text"PlaceholderText = "텍스트 필드"PlaceholderColor = "# ccc"Height = "50"Padding = "5">
        <사각형 레이어 = "배경">
            <스트로크 너비 = "2"Brush = "# BBB"/>
        </ Rectangle>
    </ TextInput>
</ Panel>
다음 예제에서는 TextInput의 하위 클래스를 사용하여 앱 전체에서 일관된 모양을 구현하는 방법을 보여줍니다.

<! - TextInput 서브 클래 싱 ->
<TextInput ux : Class = "MyTextInput"FontSize = "20"PlaceholderColor = "# ccc"Padding = "5">
    <사각형 레이어 = "배경"CornerRadius = "3">
        <스트로크 너비 = "1"Color = "# ccc"/>
        <SolidColor Color = "흰색"/>
    </ Rectangle>
</ TextInput>

<! - 예제 사용법 ->
<StackPanel Margin = "10"ItemSpacing = "10">
    <MyTextInput PlaceholderText = "사용자 이름"/>
    <MyTextInput PlaceholderText = "암호"IsPassword = "true"/>
    <MyTextInput PlaceholderText = "암호 반복"IsPassword = "true"/>
    <MyTextInput />
</ StackPanel>
이 예제는 InputHint, AutoCorrectHint, AutoCapitalizationHint 및 ActionStyle 속성을 사용하여 TextInput에 포커스가있을 때 온 스크린 키보드의 레이아웃과 동작을 구성하는 방법을 보여줍니다.

<TextInput PlaceholderText = "검색 ..."ActionStyle = "검색"AutoCapitalizationHint = "없음"/>
<TextInput PlaceholderText = "이메일"InputHint = "이메일"ActionStyle = "보내기"AutoCorrectHint = "사용 안 함"AutoCapitalizationHint = "없음"/>
<TextInput PlaceholderText = "http : //"InputHint = "URL"ActionStyle = "이동"AutoCorrectHint = "사용 안 함"AutoCapitalizationHint = "None"/>
<TextInput PlaceholderText = "+ 47 123 456 789"InputHint = "Phone"/>
<TextInput PlaceholderText = "1234"InputHint = "Number"/>
<TextInput PlaceholderText = "1.234"InputHint = "Decimal"/>
<TextInput PlaceholderText = "1"InputHint = "정수"/>
TextInput의 인터페이스
