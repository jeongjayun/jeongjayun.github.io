---
title: 깃허브 블로그 Chripy theme 적용기 💥
date: 2023-05-27 11:17:08 +/- 0900
categories: [BLOG]
tags: [gitblog]     # TAG names should always be lowercase
---

>이 글은 Chirpy 테마 적용하는 방법이 아니라 제가 적용하던 중 겪었던 에러와 테마를 적용한 후기를 담고 있습니다. 
{: .prompt-info }

## 00. 시작
 나는 에러노트 작성용 및 쫌쫌따리 기록용으로 벨로그를 사용하고 있었다. 평소 여러 플랫폼을 사용하는 것을 즐겨서 (노션도 사용했었음.) 여기저기 기웃거리다 예전에 만들었다 적응 못하고 던져놨던 깃허브 블로그를 다시 사용해보려고 한다.

 벨로그의 큰 장점으로 웹에서 바로 마크다운 형식을 보면서 글을 작성하고 올릴 수 있는 점, 같은 플랫폼의 다른 기술벨로그들의 글을 함께 볼수도 검색할 수도 있다는 점, 이미지를 복사해서 에디터에 붙여넣기하면 이미지 주소가 저절로 임포트 되는 점 👍👍👍 이 좋았으나 학원 동기가 vscode로 깃블로그에 글을 쓰는 모습은 정말 멋져보였기에(...)  멋져 보이면 또 따라해야했음. 

이전에는 [Hydejack Theme](https://hydejack.com)를 적용해뒀었는데 적용했을 당시에는 보기에 무척!! 예뻤으나 루비 및 지킬테마에 대한 지식이 아예 없었던 터라 조금 만지작 거리다 끝났었다. ~~물론 지금봐도 하이드잭 테마는 예쁘다 🥹.~~ 그러나 이번엔 정말 정착하고 싶다.

 깃허브 블로그 만들 때 중요한 건 뭐 ‼️ <br>
 정말 지킬 맨땅에서 시작할 자신은 없기 때문에 적당한 테마를 찾는 것이었다.

## 01. Chripy Theme
#### 테마를 고른 이유
이 테마는 처음 깃허브 블로그 만들면서 Hydejack Theme 검색할 당시에도 많이 보였던 테마다. 그 당시에는 투박한 디자인이라고 생각해서 패스했는데 몇 개월 지나고 보니 이만한 테마가 또 없더라. 역시 jekyll theme 검색할 때마다 자주 보이는 베스트 테마에는 다 이유가 있다. 

벨로그에 없는 카테고리를 일단 쓸 수 있고 벨로그처럼 태그따라 글 분류, 특히 마음에 들었던 TOC(Title of Contents) 기능, 다크/라이트 모드도 지원해주니 (Hydejack에서는 결제 후 확장 / 원한다면 직접 코딩해서 추가도 가능한 듯 했지만 아직 내겐 그럴 능력부족 😅) , 예쁜 코드블럭과 바로 복사 가능한 버튼도 있으니 이거다 싶었다.

사람 마음이 참 청개구리 같은게 벨로그처럼 그냥 글만 쓰면 됩니다! 하고 멍석 다 깔아주면 "아 ~ 커스텀 할 수 있는 나만의 페이지가 갖고 싶다 ~ " 싶고 처음부터 레포지토리를 만들어서 md글 하나하나 push 하라면 "아 ~ 누가 좀 쉽게 쓸 수 있게 안만들어 주냐 ~ " 하니 그때 그때 원하는 플랫폼 쓰는 게 내게 맞겠더라.

### Theme 적용
Chirpy 테마를 블로그에 적용하는 방법은 **3가지** 있다.
1. theme 배포해주는 깃허브 주소에서 fork 받기
1. zip 파일을 다운받아 로컬에서 환경설정 한 뒤에 push 하기
1. chirpy starter 를 이용

나는 위 방법들 모두 해봤고 3번째 방법으로 재시도 했을 때 적용에 성공했다.

## 02. 테마 적용하던 중 마주친 Error
Hydejack 테마를 처음 적용할 때 너무 힘들었다보니 jekyll 테마 zip으로 다운 받아서 적용하는 건 눈감고도 할 수 있게 되어서(...) Chripy 테마도 쉽게 설치할 수 있을 줄 알았다. 당시엔 _'이게 왜 안돼? 🤯 '_ 화가 나서 에러 페이지와 메세지 캡쳐 할 생각 못했는데 되새김질 하려고 보니 역시 캡쳐를 해둘 걸 그랬다... <br>


### Error 1. Gemfile dependencies.
2번 방법으로 진행 후 initial commit은 push 되는데 이후에 하는 push들은 actions에서 실패했다는 이메일이 날아왔다. 

> **pages build and deployment** <br> 
>Github can't satisfy your Gemfile dependencies.
{: .prompt-danger }

**Github - Actions** 에서 에러에 대한 내용을 볼 수 있다. 이게 뭔가 싶어서 구글링 하던 중 <https://github.com/alshedivat/al-folio/issues/1208> 글을 읽고 위의 에러에 대한 자세한 내용도 볼 수 있다는 걸 뒤늦게 알았다. 아무튼 중요한 건 이 글의 리플.

<img width="924" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/e03e627d-2040-46e4-85f1-245e1f0fb262">

<https://docs.github.com/ko/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site> 이 링크를 따라 설정 페이지로 돌아갔으나 나는 main 브랜치만 있어서 직접 gh-pages 와 똑같은 이름으로 브랜치를 만들어서 메인 브랜치를 변경해도 테마 적용은 되지 않았다.

이후 구글링을 통해서 bundle install 후 생기는 Gemfile.lock 을 삭제 후 bundle update 해보라 / bundler 삭제 후 재설치, ruby 삭제 후 재설치, 깃허브 블로그용 레포지토리도 삭제 후 재생성 등 여러 시도 했으나 그 이후로도 Gemfile dependencies 는 계속 된다.

### Error 2. gh-pages 자동생성 안됨
 내가 모르는 다른 설정사항이 더 있나 싶어 다른분들의 글을 참고했다.

> 하얀눈길님 블로그 <https://www.irgroup.org/posts/jekyll-chirpy/> <br>
> hashnsalt.log <https://velog.io/@hashnsalt/Github-Blog-만들기-2#github-blog-테마-적용> <br>
> J1mmyson님 깃블로그 <https://j1mmyson.github.io/posts/StartingBlog/>

Chriphy 테마는 push 하면 자동으로 gh-pages 라는 브랜치가 생성되어 settings - pages 에서 메인 브랜치를 변경해주면 바로 테마가 적용이 가능하다는데 **나는 자동으로 gh-pages 브랜치가 생성되지 않아서 안되는 거**다. _config 파일도 크게 바꾼 점 없는데도(username, url 정도) 적용이 안됐다.

tools/init.sh 파일이 관리자 초기 설정을 초기화 해준다하여 터미널에서 실행하려고 했으나
```terminal
tools/init.sh 
bash tools/init.sh
```
<img width="560" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/c5b7fbcd-c9d5-4ac2-a95b-6c91f9c81756">

아예 폴더에서 파일 자체를 실행시킨 뒤에 push 하는 등 여러 시도를 더 했으나 gh-pages 가 생성되지 않았다.
결국 zip 파일로 테마를 설치하는 방법은 포기하고 깃허브에서 fork 받았어도 자동으로 gh-pages 가 생성되지 않았다...

gh-pages doc 문서를 다시 보고 github action도 켜봤는데 gh-pages가 생성되는지도 모르겠고 여전히 push 하면 위의 error 메세지만 actions 창에 가득해서 대체 어떻게 적용하는 거람하고 며칠을 블로그 레포지토리만 지웠다가 새로 만들었다가 반복했는지 모른다 🫨 .

그래서 포기하고 hydejack theme를 다시 잘 써봐야겠다 싶어 Chripy 대신 Hydejack을 다시 적용했더니 원격에서는 적용이 잘된 모습인데 local에서는 bundle serve 하면 터미널 가득 conflict( index, 404 파일 중복 등) 및 Chripy 테마에 있던 일부 파일들(_post 밑의 예시글 md들)을 찾을 수 없다는 노란 메세지가 가득하고 로컬에서 접속한 jekyll 블로그는 새하얀 빈 창이라 이거 망했다 😱😱😱😱 싶은거다.

_'Chripy 꼭 써라!!!'_ 하는 것 같아서 마지막으로 Chripy Starter로 다시 테마를 적용했다. 

## 03. 해결? 일단 theme 적용 성공 🎉
Starter 적용하니 바로 원격에서 Chripy theme가 적용된 내 깃블로그 보인다!! 🥳🎉 물론 로컬에서도 잘 실행된다.  적용만 되어선 안된다 ... 내가 글을 쓰고 push 했을 때 제대로 원격에 올라가야 정말 적용 완료인 것!

```terminal
bundle lock --add-platform x86_64-linux
```
[Chripy - Getting Started (데모페이지) ](https://chirpy.cotes.page/posts/getting-started/) 의 [이 부분](https://chirpy.cotes.page/posts/getting-started/#deploy-by-using-github-actions)을 읽어보고 터미널에서 위 명령 실행 후 push하니 글이 잘 올라간다.<br>
이렇게 쉽게 적용되다니 조금 허무하고 황당하고 🤦‍♀️ ... 나는 MacOS가 linux라고 생각했는데 다른건가?

에러노트 작성할 때도 느끼는 거지만 삽질하고 고민한 것에 비해 문제가 간단하게 해결되면 허무할 때가 왕왕 있다. 이번 같은 경우는 공식문서의 영어를 외면하고 다른 분들이 정리한 글을 참고하려는 약은 수를 썼다가 그저 고생했을 뿐 ... 당연한 말이지만 앞으로는 영어를 외면할 것이 아니라 공식 문서를 찾아보도록 노력해야겠다.


### 📌 이후에 해야 할 것 리스트
- [x] 블로그 css 변경
- [ ] 운영체제 MacOS와 Linux 의 차이 호기심 해결하기 
