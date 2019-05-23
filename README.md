# Jenkins 사용법 정리 : 기본 빌드, 배포 자동화


#### 머리말
- 현재 맡고 있는 시스템의 빌드, 배포 자동화를 구축하기 위해서, CI/CD와 관련된 지식들을 공부해놓고, 다른 사람들이 참고할 수 있게 정리해놓으려 한다.
- 자동화 테스트는, 현재 테스트 주도 방식 개발이 몸에 익으면 서서히 도입해 넣을 예정이다.
- 밑에 스크린 샷들 중 기본 젠킨스에는 안나오는 것들이 있을 것이다. 그런 것들은 플러그인을 설치해서 그런건데, 구글에서 젠킨스 플러그인 검색하면 설치법이나 필수적으로 까는게 좋은 플러그인 목록등이 많이나온다. 나도 유명한 플러그인들 몇개 깐거임.

#### 현재 상황
- 패키징, 정적분석, 배포, 톰캣 재시작 까지
![image](https://user-images.githubusercontent.com/21155325/58252717-b916da00-7da1-11e9-90e0-cc3fe7cc0c08.png)

#### 1. 패키징
- 개요 : 원격 repository에서 소스들을 가져와서 war로 마는것. 
- 처음에는 war로 말았는데, 시간이 너무 오래걸려서, 압축풀린 폴더 상태로 파일만 전송하는 스타일로 바꿈.

#### 1.1 프로젝트 만들기
- new item을 눌러서 프로젝트를 한개 만든다. Freestyle project로 만들었다.
- 프로젝트는 내가 자동화하고자 하는 행위의 한단위로 보면 될듯하다. 이런 프로젝트들을 이어붙여서 거대한 자동화의 흐름을 만들어낸다. 
![image](https://user-images.githubusercontent.com/21155325/58253319-1eb79600-7da3-11e9-9381-0814af077145.png)

#### 1.2. 프로젝트 설정화면
- 이런 화면이 뜰거다. description에 프로젝트 설명이나 소개 같은거 메모하면된다.
![image](https://user-images.githubusercontent.com/21155325/58253697-f2504980-7da3-11e9-8f8f-70bc84a68501.png)
- 소스 레포지토리 설정
#### 1.3. 소스 레포지토리 설정 : 어디서 소스 가져올것인가?
-나는 git써서 가져올테니 source code management를 git으로 선택하고.
- repository url이랑 credentials(로그인계정&비번), 무슨 브랜치 가져올건지 선택하면 된다. 
- master 브랜치 가져올거라 master 적어놓음.
- 빨간색이 뜨는 이유는 실제로 저 레포지토리 주소가 없기때문이다. 예제때문에 막지어낸 레포지토리 url이라 그런다.
![image](https://user-images.githubusercontent.com/21155325/58254051-e2853500-7da4-11e9-98e3-089d997f549b.png)

#### 1.4. build trigger : 동작을 누가 시작시켜 줄 것인가?
- Build Triggers, 말그대로 빌드를 누가 촉발시켜 줄것인가 결정하는 것이다. 
- 빌드는 말그대로 이 프로젝트가 수행해야할 행동이라 보면 될듯하다.  
- 나는 그냥 시작버튼 눌러서 할것이라 아무것도 선택안했다.
- 주기적으로, 본인 repository에 push가 일어나면 촉발되는 등 여러 선택지가 있다. 
![image](https://user-images.githubusercontent.com/21155325/58254832-94713100-7da6-11e9-9018-fdd0def31bc3.png)

#### 1.5. build environment & build : 어떤 걸가지고 뭘할 것인가?
- 이 프로젝트가 어떤 tool을 활용해서, 무슨 행동 하는지 선택하면 된다. 
- 나는 ant 스크립트를 실행시킬거니까 invoke ant를 선택했다.
- ANT_HOME, JAVA8 이런거는 내가 쓸 도구들 닉네임 지어준건데, JENKINS 환경 설정에서 설정할 수 있다.

![image](https://user-images.githubusercontent.com/21155325/58255650-70aeea80-7da8-11e9-880b-d40326c8b300.png)

![image](https://user-images.githubusercontent.com/21155325/58254706-4c520e80-7da6-11e9-9436-2e682c9988a6.png)
- build.xml을 compile을 타겟으로 실행하겠다는 의미.
![image](https://user-images.githubusercontent.com/21155325/58255769-ace24b00-7da8-11e9-8c07-3ecf54d64080.png)

#### 1.6. post-build actions : 이 프로젝트의 빌드 수행되면 다음에는 뭘하지?
- 현재 프로젝트의 핵심인 build (행동)이 끝나면 또 어떤 행동할 것인지 정하는 것이다.
- 보통 여러 프로젝트끼리 체인처럼 엮고, 선후관계를 정하는 걸 많이한다.
- 그 외에도 선택할수 있는 것들이 많은 데 한번 열어보길 바란다.
- 다음에 더 정리해야지.
![image](https://user-images.githubusercontent.com/21155325/58256102-5295ba00-7da9-11e9-9d3a-5b644f4db9a4.png)
