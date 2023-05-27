---
title: Chripy theme 커스텀
date: 2023-05-27 19:50:00 +/- 0900
categories: [BLOG]
tags: [gitblog]     # TAG names should always be lowercase
---
> 이전 글 <br>
> <https://jeongjayun.github.io/posts/BLOG-Blogging-00/>
{: .prompt-info }

## 00. 시작
<img width="794" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/da2d57c6-51a7-413c-8238-3d95af209ae3">
Chripy Starter 는 쉽게 설치하여 글쓰기에 집중이 가능하다는 장점이 있지만 나는 기본 디자인은 싫다! 설치하고 보니 CSS 폴더가 없다?! ~~이게 뭐지!!~~ 😂😂😂 그러나 방법은 다 있음. 하여 내 입맛대로 CSS 변경 부분 메모해두는 글이다.

### 참고
> 52log <https://ryeowon.github.io/posts/customizing_font/><br>

세상에 내가 제일 뒷북은 아닐까 싶을 정도로 검색하면 다 나온다. 😅👍<br>

#### 구글 개발자 도구 
<img width="1490" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/d7eb67eb-00c0-4bfb-8b65-959554595f00">
구글의 개발자 도구를 사용하면 내가 변경해야 하는 부분이 어느 scss 에 있는지 쉽게 확인할 수 있다. 로컬 실행 후에 개발자 도구로 변경점 확인 -> scss 변경 후 새로고침하면 변경된 부분이 바로 보이는 점이 좋다.

## 01. 폰트변경
#### 폰트 변경 방법
[눈누](https://noonnu.cc/)에서 적용하고 싶은 폰트를 찾아 @import 부분의 내용을 복사해서 scss 상단에 붙여넣기

### 사이트 제목
```scss
.site-title {
    font-family: 'OKGUNG'; /* 추가사항 */
    /* 원본과 변경내용 없음 */
  }
```
+ common.scss
+ 사용 폰트 : 읏맨-궁서체 <https://noonnu.cc/font_page/947>

### 메뉴
```scss
  ul {
    /* 원본과 변경내용 없음 */

    li.nav-item {
      font-family: 'OKGUNG';  /* 추가사항 */
      /* 원본과 변경내용 없음 */
```
+ common.scss
+ 사용 폰트 : 읏맨-궁서체 <https://noonnu.cc/font_page/947>

### 포스트 제목
```scss
.post {
  h1 {
    font-family: 'LeferiPoint-SpecialItalicA'; /* 추가사항 */
  }
}
```
+ common.scss
+ 사용 폰트 : 레페리포인트 Special Italic <https://noonnu.cc/font_page/821>

### Achives 년도
```scss
.year {
    font-family: 'LeferiPoint-SpecialItalicA';
    /* 폰트 외 변경사항 없음 */

    /* Year dot */
    &::after {
      left: 10px;
    /* 폰트 변경하며 dot 위치 조절 */
    }
  }
```
+ archives.scss
+ 사용 폰트 : 레페리포인트 Special Italic <https://noonnu.cc/font_page/821>

### 블로그 기본 폰트와 heading 폰트
```scss
$font-family-base: 'NEXON Lv2 Gothic';
$font-family-heading: 'LeferiPoint-SpecialItalicA';
```
+ variables.scss
+ 사용 폰트 : 넥슨 Lv.2 고딕 <https://noonnu.cc/font_page/435>
+ 사용 폰트 : 레페리포인트 Special Italic <https://noonnu.cc/font_page/821>

### topbar 폰트
```scss
#topbar-title { 
  /* font-family만 변경 */
  font-family: 'OKGUNG';
}
```
+ commons.scss
 : 바꾸면 모바일단 상단의 폰트가 바뀜
+ 사용 폰트 : 읏맨-궁서체 <https://noonnu.cc/font_page/947>

## 02. 색상 변경
[Color Palette](https://colorhunt.co/) <https://colorhunt.co/palette/f9f5f6f8e8eefdcedff2bed1> 에서 색 조합 찾음 🔍<br>
근데 생각보다 번거로워서 라이트모드에서만 색을 바꾸기로 했다.
+ 변경 위치 : light-typography.scss // 라이트 모드

### Side bar
```scss
  /* Sidebar */
  --sidebar-bg: linear-gradient(to right, rgb(249, 245, 246), rgb(255,255,255, 0));
  /* 옆의 사이드바 배경색 그라데이션 */
  --sidebar-muted-color: #EEEEEE;
  --sidebar-active-color:#F2BED1; /* 활성화된 버튼의 폰트색 */
  --sidebar-hover-bg: rgb(248, 232, 238, 0.5); /* 사이드바 오버 배경 */
  --sidebar-btn-bg: white;
  --sidebar-btn-color: #F2BED1;
```

### --avatar-border-color
```scss
--avatar-border-color: rgb(206, 206, 206, 0); //dark-typograhpy.scss
--avatar-border-color: rgb(255, 255, 255, 0); //light-typograhpy.scss

/* 나는 변덕이 심해 우선 opacity 0 으로 해두었다. */
```

### Title of Content highlight
```scss
--toc-highlight: #D5B4B4;
```

### 링크
```scss
/* Common color */
  --link-color: #867070; /* 링크 */
  --link-underline-color: #E4D0D0; /* 링크 밑줄 */
```
+ 변경 위치 : light-typography.scss // 라이트 모드 

```scss
%link-hover {
  color: #D5B4B4 !important;
  border-bottom: 1px solid #E4D0D0;
  text-decoration: none;
}
```
+ 변경 위치 : module.scss

### 체크박스
```scss
  --checkbox-checked-color: #D5B4B4; /* 체크박스 */
```

## 03. 댓글 기능
### 참고
> da-in님 블로그 <https://da-in.github.io/posts/Blog-Comments/>

나는 처음에 utteranc 을 적용하고 싶었다. 적용하고자 구글링을 해봤으니 _layout 폴더 밑의 post 양식을 일부 수정하면 연결할 수 있다고 했는데 나는 starter 로 적용하여 layout 폴더가 없었고 혹시나 싶어서 include 폴더를 다운 받아봤으나 소용없었던 찰나 단비같은 글을 발견.

Chripy 테마의 config 파일에는 comments 영역이 있긴 한데 어떻게 써야할 지 몰랐는데 다인님께서 쉽게 써주셔서 나도 적용할 수 있었다.

```yml
comments:
  active: giscus
  # The global switch for posts comments, e.g., 'disqus'.  Keep it empty means disable
  # The active options are as follows:
  #disqus:
  #  shortname: # fill with the Disqus shortname. › https://help.disqus.com/en/articles/1717111-what-s-a-shortname
  # utterances settings › https://utteranc.es/
  # utterances:
  #  repo:  jeongjayun/jeongjayun.github.io
  #  issue_term:  pathname
  # Giscus options › https://giscus.app
  giscus:
    repo: jeongjayun/jeongjayun.github.io
    repo_id: /*  */
    category: /*  */
    category_id: /*  */
    mapping: # optional, default to 'pathname'
    input_position: # optional, default to 'bottom'
    lang: # optional, default to the value of `site.lang`
    reactions_enabled: # optional, default to the value of `1`

```

1. 활성화하고자 하는 댓글 추가 기능의 이름을 active 옆에 써주기
1. giscus 앱을 github에 install
1. <https://giscus.app/ko> 에서 giscus 설정해주기 
1. config에 채울 내용은 3번 사이트에서
<img width="629" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/9d7cbcb9-b991-4354-915b-cb4cee907d16"><br>
내용을 기입 및 설정한대로 밑에 스크립트에 내용이 차오른다.<br>
스크립트에 적힌 내용대로 복사해서 config 파일에 붙여넣기<br>
<img width="740" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/04a5ac38-3b31-4bcb-aec9-670e7c159397">

1. 로컬에서 실행하여 잘 적용됐는지 확인
1. git push

## 04. 더 수정하고 싶은 부분
- [ ] favicon 
- [x] 모바일 페이지에서 폰트 변경
- [x] utteranc 적용 -> giscus 로 대체
- [ ] Trending Tags 밑의 태그 버튼 hover 색 : _buttons.scss 밑 btn-outline-primary ?? 😂

