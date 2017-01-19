원본: https://www.fusetools.com/docs/fuse/geolocation/geolocation

퓨즈 JS / GeoLocation 모듈 (JS)

 고급 기능 표시
이동 :
목차
지구 위치 서비스를 제공합니다.

위치 정보 서비스를 사용하려면 장치 인증이 필요합니다. 프로젝트에 Fuse.GeoLocation 패키지를 포함하면 앱이 실행될 때이 인증에 대한 프롬프트가 표시됩니다.

startListening을 사용하면 계속적인 위치 업데이트를 얻을 수 있습니다. 일회성 위치 요청에는 location 또는 getLocation을 사용하십시오.

이 기능을 사용하려면 프로젝트 파일에서 "Fuse.GeoLocation"에 대한 참조를 추가해야합니다.

이 모듈은 EventEmitter이므로 EventEmitter의 메서드를 사용하여 이벤트를 수신 할 수 있습니다.

예

다음 예제에서는 다양한 작동 모드를 사용하는 방법을 보여줍니다.

<자바 스크립트>
    var Observable = require ( "FuseJS / Observable");
    var GeoLocation = require ( "FuseJS / GeoLocation");

    // 즉시
    var immediateLocation = JSON.stringify (GeoLocation.location);

    // Timeout
    var timeoutLocation = 관찰 가능 ( "");
    var timeoutMs = 5000;
    GeoLocation.getLocation (timeoutMs) .then (function (location) {
        timeoutLocation.value = JSON.stringify (location);
    }). catch (function (실패) {
        console.log ( "getLocation fail"+ 실패);
    });

    // 연속
    var continuousLocation = GeoLocation.observe ( "changed"). map (JSON.stringify);

    함수 startContinuousListener () {
        var intervalMs = 1000;
        var desiredAccuracyInMeters = 10;
        GeoLocation.startListening (intervalMs, desiredAccuracyInMeters);
    }

    function stopContinuousListener () {
        GeoLocation.stopListening ();
    }

    module.exports = {
        즉각적인 위치 : 즉각적인 위치,
        timeoutLocation : timeoutLocation,
        continuousLocation : continuousLocation,

        startContinuousListener : startContinuousListener,
        stopContinuousListener : stopContinuousListener
    };
</ JavaScript>

<StackPanel>
    <Text> 즉시 : </ Text>
    <텍스트 값 = "{immediateLocation}"/>

    <텍스트> 시간 초과 : </ 텍스트>
    <텍스트 값 = "{timeoutLocation}"/>

    <텍스트> 연속 : </ 텍스트>
    <텍스트 값 = "{continuousLocation}"/>

    <Button Text = "연속 리스너 시작"Clicked = "{StartContinuousListener}"/>
    <Button Text = "연속 청취자 중지"Clicked = "{StopContinuousListener}"/>
</ StackPanel>
이 모듈이 반환하는 위치는 다음 형식의 JavaScript 객체입니다.

{
    위도 : 십진수로 측정 한 숫자,
    경도 : 십진수로 측정 한 숫자,
    정확도 : 미터로 측정 한 숫자
}
GeoLocation의 인터페이스
