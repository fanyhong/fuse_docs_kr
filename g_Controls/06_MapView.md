원본: https://www.fusetools.com/docs/fuse/controls/mapview

MapView 클래스

 고급 기능 표시
이동 :
UX
목차
기본지도보기를 표시합니다.

MapView를 사용하면 플랫폼의 고유 한 매핑 API 인 Android의 Google지도 및 iOS의 Apple지도를 사용하여 주석이 달린 양방향 세계지도를 사용자에게 표시 할 수 있습니다.

MapView는 네이티브 컨트롤이므로 표시 할 NativeViewHost에 포함되어야합니다. 다른 네이티브 모바일 컨트롤과 마찬가지로 현재 데스크톱 대상에 사용할 수있는 MapView가 없습니다.

참고 : .unoproj의 패키지 섹션에서 Fuse.Maps에 대한 참조를 추가해야합니다.

"패키지": [
    "퓨즈. 맵"
    "퓨즈",
    "퓨즈 (FuseJS)"
]
앱에 포함 된 MapView를 얻는 것은 간단합니다. 네이티브 컨트롤을 사용하여 평소처럼 UX에 노드를 포함하기 만하면됩니다.

<NativeViewHost>
    <MapView />
</ NativeviewHost>
지도 카메라를 초기화하고 조작하려면 위도, 경도, 확대 / 축소, 기울이기 및 방위 속성을 사용합니다. 모두 쌍방향 바인딩입니다. 확대 / 축소는 Google의 '확대 / 축소 수준'을 따릅니다. 자세한 내용은 여기를 참조하십시오.

스타일 속성을 사용하여 렌더링 스타일을 설정하여지도를 추가로 사용자 지정할 수 있습니다. 옵션으로는 Normal, Satellite 및 Hybrid가 있습니다.

라벨이 지정된 마커로지도에 주석을 추가하려면 MapMarker를 참조하십시오.

Android 용지도

Google지도에는 다음이 필요합니다.

Google Play 라이브러리 설치 지침은이 설명서를 참조하십시오.
유효한 Google지도 API 키입니다. 하나의 설정을 얻으려면 Google 문서를 따르십시오. 키를 얻은 후에는 아래 그림과 같이 프로젝트 파일에 키를 추가해야합니다
"Android": {
   "Geo": {
        "ApiKey": "your_key_here"
    }
}
이 예는 노르웨이 오슬로에있는 Fuse의 집에 집중된 평균 줌 레벨을 가진지도를 표시합니다.

UX

<App>
    <ClientPanel>
        <NativeViewHost>
            <MapView Latitude = "59.911567"경도 = "10.741030"Zoom = "10">
                <MapMarker Latitude = "59.911567"경도 = "10.741030"Label = "Fuse HQ"/>
            </ MapView>
        </ NativeviewHost>
    </ ClientPanel>
</ App>
MapView의 인터페이스