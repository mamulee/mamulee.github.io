---
title: 깃허브 블로그 만들다 안 될 때 (Chirpy 테마)
author: mamulee
date: 2022-03-30 14:50:00 +0900
categories: [Blogging, Tutorial]
tags: [github, blog, jekyll, chirpy]
img_path: /assets/img/github-blog/
render_with_liquid: false
---

## 서론

이 글은 검색만 해도 수두룩하게 나오는 깃허브 블로그를 개설 설명 글대로 똑같이 따라했지만 `어,,, 왜 안 되지? 간단한 것 같은데, 똑같이 했는데 왜 나만 안 되는 거야?` 라는 상황을 겪은 과거의 나와 같은 사람들을 위해 쓰는 글이다.
<br/>
<br/>

## 왜 안 돼?

<br/>

### 테마 적용

깃허브 블로그 개설 설명글을 보면 테마 적용은 정말 정말 쉽다. 각종 `Jekyll Themes` 사이트에서 원하는 테마를 다운받아 압축을 해제하고 그 안의 내용물을 그대로 로컬 블로그 폴더 안에 복붙하면 된다고 한다. 하지만 그 많고 많은 테마 중 [**Chirpy**](http://jekyllthemes.org/themes/jekyll-theme-chirpy/)를 골랐다면 <u> 축 당첨 </u>이다.
<br/>

제일 먼저 다운로드 후 git push를 할 때 아래와 같은 오류가 뜰 수 있다 :

![오류](1.png)

GitHub 설정에서 본인이 사용하고 있는 Personal access token의 scope에 workflow를 추가해주면 금방 해결된다.

> Github 로그인 -> 오른쪽 상단 프로필 사진 클릭 후 Settings -> Developer Settings -> Personal access tokens -> 사용 중인 토큰 이름 클릭 -> Select scopes 에서 workflow 체크 -> Update token

<br/>

**하지만 1**, 테마를 깃허브에 푸시 했다고 해서 바로 적용이 되는 것은 아니다.

이 `Chirpy` 테마를 이용하고자 한다면 복붙 이후에 해당 리포지터리에서 아래 명령어를 수행해야 한다 :

```console
$ bash tools/init.sh
```

> 위 명령어의 역할은
>
> 1. 아래 파일들 삭제 :
>
>    - `.travis.yml`
>    - `_posts` 폴더의 하위 파일들
>
> 2. `.gitignore`에서 `Gemfile.lock` 제거.
>
> 3. 수정 사항에 대한 새로운 `commit` 자동 생성.
>
> 등 이다.

성공했다면 아래와 같이 나올 것이다 :

![결과](2.png)

이후

```console
$ git push
```

를 다시 수행하면 깃허브의 리포지터리 Actions에서 어떤 `Automatic build`가 실행되는 것을 확인할 수 있다.

**하지만 2**, 실행이 끝난 후 초록불 대신 <span style="color : red">빨간불</span>이 들어올 수 있다.

이것도 간단히 해결할 수 있다.
<br/>
`init.sh`에 의해 제거 되었던 `Gemfile.lock`을 다시 `.gitignore`에 추가해주고 `Gemfile.lock` 파일을 리포지터리에서 삭제해 주자.
<br/>
그리고 다시 git push를 해주면 정상적으로 빌드가 진행되어 <span style="color:green">초록불</span>을 마주할 수 있다.

**마지막으로!!!** 빌드 성공 후 깃허브 리포지터리에 `gh-pages`가 새롭게 생긴 것을 확인했다면 `publishing source`를 이 브랜치로 변경해야 한다.

> GitHub 블로그 리포지터리의 Settings -> Pages -> Source 아래 `/(root)`의 브랜치 `gh-pages` 선택 -> Save

<br/>
<br/>
<br/>

이 작업을 끝으로 `Chirpy` 테마 적용은 끝!이라고 생각하고 두근대는 마음으로 [mamulee.github.io](http://mamulee.github.io)로 들어가봤지만 ~~`어라 왜 또 안 되는 거야@!!!@`~~

### 찐막 : \_config.yml 수정

진짜 진짜 마지막으로 `_config.yml` 파일을 열어 `url`에 본인의 블로그 주소를 넣자.

```yml
url: "https://<github-username>.github.io/"
```

`url` 이외의 항목들은 본인의 환경에 맞춰 알아서 수정하면 된다. 이후 모든 수정 사항들을 `git commit`, `git push` 해주면 드디어 내 블로그에 테마가 적용된 것을 확인할 수 있다.

> 수정 반영이 오래 걸릴 수 있으니 `Actions`에서 수시로 오류가 나진 않았는지 빌드가 완료되었는지 확인 후 블로그를 새로고침 해보는 것이 좋다

<br/>
<br/>

## 결론

혹여나 나처럼 장문이나 영어 울렁증이 있는 사람들을 위해 이 글을 작성했지만, `Chripy` 테마는 예쁜만큼 각 파일에 설명 또한 너무 잘 되어있기 때문에 [**Demo**](https://chirpy.cotes.page/) 페이지나 다운 받은 테마 파일의 `_posts` 폴더 안 5가지의 .md파일의 내용을 참고하면 더 많은 도움이 될 것이다.
