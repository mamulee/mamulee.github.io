---
title: 깃허브 블로그 글 조회수 기능 추가 (Chirpy 테마)
author: mamulee
date: 2022-03-31 18:20:00 +0900
categories: [Blogging, Tutorial]
tags: [github, blog, jekyll, chirpy]
img_path: /assets/img/page-views/
---

## 서론
`내가 쓴 글을 과연 나 말고 다른 사람들이 보기는 할까?` 문득 궁금해져서 귀찮아서 추가하지 않으려던 조회수 기능을 추가해 보려고 한다. [Chirpy 공식 설명 문서](https://chirpy.cotes.page/posts/enable-google-pv/)를 따라 해보자. 이 문서에는 `조회수가 낮다면 블로그 글 쓸 맛이 안 날 수 있으니 주의하라는` 재밌는 경고가 쓰여져 있지만, 나는 조회수에 딱히 집착하지 않으니 가뿐히 무시하고 시작한다. ~~아무도 안 보면 내가 얼마나 들락날락하는지 알아보는 용도로 쓰지 뭐..,,~~
<br/>
<br/>
## 시작~~~~
### Google Analytics 계정 만들기
<https://analytics.google.com/> 들어가서 `측정 시작` :

1. 계정 설정 : 원하는 계정 이름, 계정 데이터 공유 설정을 한다.
   > 나는 그냥 체크된 대로 진행했다.
   {: .prompt-tip }
2. 속성 설정 : 속성 이름, 보고 시간대, 통화 설정
   > 나중에 수정이 가능하다고 하니 대충 설정하면 될 듯하다. 속성 이름은 알아보기 쉽게 블로그 이름으로 했다.
   {: .prompt-tip }
3. 비지니스 정보
   > 딱 봐도 귀찮아 보여서 굳이 써야하나 하고 바로 만들기를 눌렀더니 그대로 진행됐다. `설정 안 해도 괜찮은 것 같다.`
   {: .prompt-tip }

만들기를 누르면 아래와 같이 약관 계약 창이 뜨고 동의함 누르면 완료!

![약관 계약 창](1.png)

### 데이터 스트림 만들기
계정을 만들면 바로 데이터 스트림 설정 창으로 넘어갈 것이다. 그렇지 않다면 직접 찾아서 들어가자.
> 좌측 상단 관리자 -> 원하는 속성 클릭 (방금 만든 속성 이름) -> 데이터 스트림
> 
자 이제 플랫폼을 `웹`으로 선택하면 데이터 스트림 설정 창이 뜬다.
본인의 `블로그 주소`와 `스트림 이름`을 적자. 난 그냥 똑같이 블로그 이름 썼다.
그리고 `스트림 만들기`를 누르면,,,,...

![웹 스트림 세부정보](2.png)

짜잔~~ 위와 같이 웹 스트림 세부 정보 창이 뜬다. `G-XXXXXXXXXX` 형식의 측정 ID를 복사해서 `_config.yml` 파일로 가보자.

```yaml
google_analytics:
  id: 'G-XXXXXXXXXX'   # 본인의 측정 ID
  # Google Analytics pageviews report settings
  pv:
    proxy_endpoint:   # fill in the Google Analytics superProxy endpoint of Google App Engine
    cache_path:       # the local PV cache data, friendly to visitors from GFW region
```
{: file="_config.yml"}

`_config.yml` 파일 내 위 부분을 찾아 측정 ID를 넣고 저장 후 `git push`를 해주면,
일정 시간이 지난 후 아래와 같이 실시간으로 통계를 확인할 수 있다.

![실시간 보고서](3.png)

### 조회수 설정
[여기](https://developers.google.com/analytics/solutions/google-analytics-super-proxy)서 상새한 설명을 확인할 수 있지만 난 그냥 빨리 `Chirpy`를 이용해 블로그 글 조회수만 확인하고 싶을 뿐이다.

### Google App Engine 셋업

1. <https://console.cloud.google.com/appengine> 접속, 약관 동의 후,
2. 프로젝트 만들기
   > 흠, 프로젝트 이름을 적은 후 가까운 데이터 센터를 지정해야 했지만 이번에도 역시 그냥 만들기를 누르니까 만들어졌다. 일단 계속 해보자.
   {: .prompt-tip }
3. 앱 만들기
   > 문서에는 앱 만들기 후 프로젝트 만들기라 되어있는데 난 왜 반대지? 일단 내 맘대로 진행해 본다.
   {: .prompt-warning }
   
   > ![앱 만들기](4.png)

   > 한국으로 설정하고 다음

   > ![시작하기](5.png)

   > 언어 = `Python`, 환경 = `표준`으로 하고 `나중에 배포`

4. API 및 서비스 (좌측 상단 탐색 메뉴 안에 있음)
   >1. `+ API 및 서비스 사용 설정` -> `Google Analytics API` 검색 ![Google Analytics API](6.png) -> 사용 

   >2. `OAuth 동의 화면` -> User Type `외부` 선택 -> 만들기
   
   > OAuth 동의 화면 만들기에 대한 자세한 설명은 없다. 대충 Google Analytics API를 사용하기 위해 사용자에게 보여지는 동의 화면에 대한 설정인 것 같은데, 이전과 마찬가지로 나는 그냥 `필수 항목`만 채워넣고 계속 다음 단계로 진행하여 완료했다.
   {: .prompt-tip }

   >3. `사용자 인증 정보` -> `+ 사용자 인증 정보 만들기` -> `OAuth 클라이언트 ID` -> 애플리케이션 유형 `웹 애플리케이션` -> 이름 작성 -> 승인된 리디렉션 URI :
   `https://<project-id>.<region>.r.appspot.com/admin/auth`
   >> - `project-id` : 홈의 프로젝트 정보나 프로젝트 선택 시 ID를 확인할 수 있다.
   >> - `region` : 앱 만들기에서 선택했던 `리전` 같다,, 스크린샷 해놓은 덕분에 살았다,...

   >그럼 아래와 같이 OAuth 클라이언트가 생성된 것을 확인할 수 있다. 딱 봐도 중요해 보이니 잘 적어 놓자.

   > ![OAuth Client](7.png)

5. Cloud SDK 다운로드 :
   <https://cloud.google.com/sdk/docs/quickstart>
   > 이전에 프로젝트에 결제 정보를 등록하지 않았다면, 구글 클라우드 플랫폼 최상단에 계속 뜨는 광고 

   > ![광고](7.png)

   > 활성화를 누르거나 탐색 메뉴 -> 결제 -> 결제 계정 연결에서 등록해주면 된다.
   
   > 블로그에 사용하기엔 무료로 제공되는 크레딧이 충분하기 때문에 추가 결제에 대한 걱정은 안 해도 된다고 한다.
   {: .prompt-tip }

   > 링크에서 본인에게 맞는 패키지를 다운하고 압축 파일 해제 후 

   ```console
   $ ./google-cloud-sdk/install.sh
   ```
   까지 실행해야 완료된다. ~~`다운로드 과정을 캡처로 다 떠놨지만 귀찮아서 생략한다. 필요한 분이 생긴다면 추후에 업데이트 하겠다`.~~

6. 명령어 실행 :
   > ```console
   > $ gcloud init
   > ```
   > 를 실행하면

   > ![콘솔창 1](9.png)
   
   > 위와 같이 뜨고 , `y` 엔터를 하면 크롬 창이 열리면서 로그인 인증이 진행된다. 그 이후,

   > ![콘솔창 2](10.png)

   > 콘솔창에서 위와 같이 확인할 수 있고, `1`을 입력하면

   > `Your current project has been set to: [프로젝트 ID].`

   > 라는 문구와 함께 주르륵 어쩌구 저쩌구 안내 문구들이 뜬다.

   > ```console
   > $ gcloud info
   > ```
   > 마지막으로 위 명령어를 입력하면 현재 프로젝트에 관한 정보들을 확인할 수 있다.
<br/>
<br/>

### Google Analytics superProxy 셋업
1. 깃허브에서 **Google Analytics superProxy**를 로컬에 클론 해오자 : 
   ```console
   $ git clone https://github.com/googleanalytics/google-analytics-super-proxy
   ```
2. [`src/app.yaml`{: .filepath}](https://github.com/googleanalytics/google-analytics-super-proxy/blob/master/src/app.yaml#L1-L2) 파일의 첫 2줄 삭제 :
    ```yaml
    - application: your-project-id
    - version: 1
    ```
    {: file="src/app.yml"}

3. In `src/config.py`{: .filepath}, 아까 잘 적어놓은 `OAUTH_CLIENT_ID` 와 `OAUTH_CLIENT_SECRET`를 입력하자.
4. `XSRF_KEY`에는 자기만 알 수 있는 아무말을 적는다.
   `config.py`{: .filepath} 파일 예시 : 

    ```python
    #!/usr/bin/python2.7

    __author__ = 'pete.frisella@gmail.com (Pete Frisella)'

    # OAuth 2.0 Client Settings
    AUTH_CONFIG = {
      'OAUTH_CLIENT_ID': 'YOUR_CLIENT_ID',
      'OAUTH_CLIENT_SECRET': 'YOUR_CLIENT_SECRET',
      'OAUTH_REDIRECT_URI': '%s%s' % (
        'https://chirpy-test-XXXXXX.ue.r.appspot.com',
        '/admin/auth'
      )
    }

    # XSRF Settings
    XSRF_KEY = 'OnceUponATimeThereLivedALegend'
    ```
    {: file="src/config.py"}
5.  클론한 폴더 내 `src/`{: .filepath} 에서, 아래와 같이 콘솔에 명령어 수행 :

    ```console
    [root@bc96abf71ef8 src]# gcloud app deploy
    Services to deploy:

    descriptor:      [/tmp/google-analytics-super-proxy/src/app.yaml]
    source:          [/tmp/google-analytics-super-proxy/src]
    target project:  [chirpy-test-XXXX]
    target service:  [default]
    target version:  [VESRION_NUM]
    target url:      [https://chirpy-test-XXXX.ue.r.appspot.com]


    Do you want to continue (Y/n)? Y

    Beginning deployment of service [default]...
    ╔════════════════════════════════════════════════════════════╗
    ╠═ Uploading 1 file to Google Cloud Storage                 ═╣
    ╚════════════════════════════════════════════════════════════╝
    File upload done.
    Updating service [default]...done.
    Setting traffic split for service [default]...done.
    Deployed service [default] to [https://chirpy-test-XXXX.ue.r.appspot.com]

    You can stream logs from the command line by running:
    $ gcloud app logs tail -s default

    To view your application in the web browser run:
    $ gcloud app browse
    ```
    > 위와 같이 순조롭게 마치면 좋겠지만, 난 `3`번의 시도 끝에 성공했다. ERROR 메세지에 안내된 주소로 가봐도 `Cloud Build API` 사용이 이미 설정되어 있었기에 시스템에 적용될 때까지 시간이 필요하다길래 기다리면서 계속 시도했다.
    {: .prompt-warning }
    > 근데 또 한가지 찜찜한 점이 있다. 내가 작성한 url과 target url의 `region` 부분이 다르다. 난 `asia-northeast3`이라 썼는데 `du`로 바뀌어 있다.
    {: .prompt-warning }
6. 어쨌든 콘솔에 뜬 `target url`을 인터넷 주소창에 입력한 후 맨 마지막에 `/admin`을 추가하고 이동한다.
7. `Manage Users`에서 자신의 이메일 주소를 추가한다.
8. `Authorize Access` 클릭.

> `Authorize Access`를 클릭하고 내 구글 아이디로 로그인을 하니 웬걸 `403 오류: access_denied` 가 계속 떴다. 아니 내가 관리자인데 왜 자꾸 액세스 권한이 없다는 거야???? 심지어 액세스 권한 요청은 또 개발자인 `나 (내 이메일 주소)`한테 연락을 하란다. 정말 답답해 죽는 줄 알았다. 도대체 왜 안 되는 걸까....여기서 시간 소모를 엄청 했다.
{: .prompt-danger }

> 드디어 찾아낸 해결 방법 :
>> `API 및 서비스`의 `OAuth 동의 화면`에서 `테스트 사용자`에 내가 사용할 구글 계정을 추가해야 한다. 앱 게시 상태가 일단 `테스트`인데, 이건 나중에 필요하면 `앱 게시` 해봐야겠다.

> 하지만 곧바로 또 다시 다른 난관에 봉착..,..`Privacy error`로 사이트가 안전하지 않아 연결을 안 해준다... 뭐가 잘못된 걸까..... 오늘 다 끝내고 싶었는데 안 되겠다,,, 내일 다시 이어서 해야지....
{: .prompt-danger }

**다시 해결!!!!!!**

> **'연결이 비공개로 설정되어 있지 않습니다.'** 가 뜬 창의 주소 `url`의 `https`를 `http`로 바꿔준다. 또, 이전에 내가 찜찜하다고 한 `du` 부분,,,분명 난 `Redirect URI`에 `asia-northeast3`을 썼지만,,, `gcloud app deploy`에서는 바뀌었기 때문에 주소창에서 이 부분을 수정하니 드디어! ![성공](11.png)
> `continue`를 누르면 : 
   {: .prompt-tip }

![Google Analytics superProxy](12.png)

다음 단계로 넘어갈 수 있는 창이 뜬다.

### Google Analytics Query 만들기
1. `Create Query` -> [Query Explorer](https://ga-dev-tools.web.app/query-explorer/) 를 이용해 `query`를 만들어 보자.
   > - id : `ga:XXXX` 형식
   > - start-date : 블로그 첫 글 개시 날짜
   > - end-date : `today`
   > - metrics : `ga:pageviews`
   > - dimensions : `ga:pagePath`
   > - filters : `ga:pagePath=~^/posts/.*/$;ga:pagePath!@=`
   >> `site.baseurl`을 지정했다면, `ga:pagePath=~^/BASE_URL/posts/.*/$`