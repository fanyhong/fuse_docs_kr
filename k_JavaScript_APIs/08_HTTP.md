원본: https://www.fusetools.com/docs/fusejs/http

HTTP

술책()

fetch는 HTTP 요청을 처리하는 주요 방법입니다.

다음은 JavaScript 객체를 JSON으로 게시하고 JSON 데이터를받는 방법에 대한 예입니다. JSON 데이터는 fetch를 사용하여 JavaScript 객체로 되돌려집니다.

var status = 0;
var response_ok = false;

fetch ( 'http://example.com', {
     방법 : 'POST',
     헤더 : { "Content-type": "application / json"},
     body : JSON.stringify (requestObject)
}). then (function (response) {
     status = response.status; // HTTP 상태 코드를 가져옵니다.
     response_ok = 응답 .ok; // response.status가 200 범위에 있습니까?
     return response.json (); // 이것은 약속을 반환합니다.
}). then (function (responseObject) {
     // 결과로 무언가를하십시오.
}). catch (function (err) {
     // Promise 체인 어딘가에서 오류가 발생했습니다.
});
가져 오기에 대한 전체 설명서는 여기에서 찾을 수 있습니다.

XMLHttpRequest

FuseJS는 XMLHttpRequest를 지원합니다. 자세한 정보를 보려면 여기로 이동하십시오.

