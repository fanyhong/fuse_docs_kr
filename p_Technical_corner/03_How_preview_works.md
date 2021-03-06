원본: https://www.fusetools.com/docs/preview-and-export/preview-details

퓨즈 미리보기 작동 원리

이 기사는 퓨즈 미리보기의 작동 방식과 퓨즈 빌드와의 차이점에 대한 시리즈 중 첫 번째 기사입니다. 우선, 몇 가지 UX 파일로 구성된 앱에서 작업한다고 가정 해 보겠습니다.



예를 들어 퓨즈 빌드를 사용하여 앱을 내보낼 때 해당 파일은 해당 플랫폼의 원시 코드로 컴파일됩니다. 결과는 기기에서 실행되는 일반 앱이며 App / Play 스토어 등에 업로드 할 수 있습니다.하지만이 기사의 주제는 퓨즈 미리보기가 다른 점과 차이가 있습니다.

예를 들어 퓨즈 미리보기로 앱 미리보기를 시작하면 앱에 코드를 추가하여 코드를 작성하는 동안 변경 사항을 가져와 표시 할 수 있습니다. 한 번에 여러 기기에서 앱을 미리 볼 수 있습니다. 이 예에서는 두 가지 미리보기를 시작합니다. 하나는 Android 기기에서, 다른 하나는 Mac에서 미리보기입니다. 퓨즈 미리보기 명령을 사용하여 여기에서 미리보기를 시작하지만 대시 보드 또는 편집기 플러그인에서 미리보기를 시작하면 똑같은 일이 발생합니다.

장치에 대한 실제 미리보기 앱을 만드는 것 외에도 Fuse 호스트가 시작되어 앱에 대한 실제 변경 사항을 가져와야합니다. 퓨즈 미리보기를 실행하고 미리보기를 퓨즈 한 후 --target = android를 실행하면 상황은 다음과 같습니다.



보시다시피 미리보기 기능이있는 앱이 이제 로컬 컴퓨터와 Android 기기에서 시작되었습니다. 퓨즈 호스트도 로컬 컴퓨터에서 시작되었으며 응용 프로그램의 일부인 파일을 모니터링합니다.

이제 미리보기 앱은 라이브 업데이트를 받으려면 호스트에 연결해야합니다. 퓨즈 호스트의 연결 세부 정보를 얻으려면 퓨즈 프록시에 문의하십시오. 이것이 미리보기 시작시 나타나는 신비한 "Connecting to proxy ..."메시지의 이유입니다. 이 모든 일은 로컬 컴퓨터에서 발생합니다. 앱이 인터넷에 연결되지 않고 프록시가 http 프록시와 혼동되어서는 안됩니다.

미리보기 앱은 퓨즈 프록시에 연결하고 호스트의 연결 세부 정보를 묻습니다.



프록시가 연결 세부 정보로 응답하고 미리보기 앱이 호스트에 연결됩니다.



이제 모든 것이 부팅되고, 미리보기 앱은 Fuse 호스트에 직접 연결되며 앱의 파일을 변경하면 모든 장치에 즉시 반영됩니다.
비즈니스용 Google 번역:번역사 도구웹사이트 번역기해외 시장 정보
