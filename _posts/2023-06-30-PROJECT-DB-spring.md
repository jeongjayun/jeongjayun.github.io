---
title: Team Project) DB ~ Spring 쇼핑몰 프로젝트 ~ 
date: 2023-06-22 17:00:00 +/- TTTT
categories: [PROJECT, DB]
tags: [project, spring] # TAG names should always be lowercase
---
# DB 💎🖤
jsp, MVC2 패턴으로 구현했던 쇼핑몰을 Spring Framework로 옮기는 작업을 진행했다.

> **두번째 팀 프로젝트**   
> 깃허브 : [https://github.com/jeongjayun/DBspring](https://github.com/jeongjayun/DBspring)      
> 블로그 : [http://jeongjayun.github.io/posts/PROJECT-DB-jsp/](http://jeongjayun.github.io/posts/PROJECT-DB-jsp/)
{: .prompt-info }

## 목차
1. 프로젝트 개요
1. Database 설계
1. 맡은 부분
1. 느낀점

## 1. 프로젝트 개요

### - 프로젝트명 : Diamond Black 💎🖤
### - 인원 : 4명
- **정자윤** (본인)
- 박권능
- 이규동
- 임종민

### - 기간 : 2023.05.11 ~ 2023.06.13
### - 기능
#### (1) 사용자
1. 회원 가입
1. 로그인
1. 자유, 질문 게시판 게시글 등록, 수정, 삭제
1. 자유, 질문 게시판의 게시글에 댓글 작성, 수정, 삭제
1. 마이페이지
    1. 주문 내역
    1. 나의 작성 글 확인
    1. 내 정보 수정 및 탈퇴
    1. 보유쿠폰 확인
1. 상품 구매
    1. 경매 및 세일상품 등 상품 구매
1. 장바구니 기능 및 결제
1. 쿠폰 적용
1. 상품 검색

#### (2) 관리자
1. 회원 관리
    - 회원 정보 수정
    - 회원 탈퇴
1. 게시판 관리(자유, 질문, 공지사항 게시판)
    - 게시글 등록, 수정, 삭제
1. 상품 관리
    - 상품 등록, 삭제
    - 브랜드 관리
    - 브랜드 추가, 삭제
1. 옥션
    - 옥션 상품 관리
    - 시간 설정 및 시작가격 설정 등
1. 게시판
    - 자유&질문&공지 게시판
    - CRUD기능, 조회수, 페이징 처리, 댓글, 멀티 이미지 첨부, 게시글 검색
    - QnA
    - 자주 묻는 질문 확인
1. 상품
    - 상품 등록
        - 가격, 이미지, 상품 설명, 사이즈, 브랜드
    - 장바구니 담기
1. 장바구니
    - 장바구니 담긴 상품 수량 조정, 삭제
    - 총 상품 가격 확인
1. 결제
    - 구매자 정보 확인
    - 배송받을 배송지 변경
    - 총 결제 금액 확인
    - 결제 API를 이용하여 결제
1. Event
    - 등급에 따른 쿠폰 수령
    - 반복수령 불가
1. Contact
    - 지도 API를 이용하여 회사 위치 확인
    - 회사 정보 확인
1. 옥션(경매)
    - 어드민이 설정한 상품, 시간, 시작 가 확인
    - 입찰이 끝나면 낙찰자에게 구매 권한 부여
1. 세일
    - 어드민이 상품 등록 시 할인율을 1% 이상 설정 후 등록

### - 개발 환경
- 개발 언어 : Java 11, HTML, JavaScript, JSP, JQuery
- 개발 환경 : Spring, Apache Tomcat 9.0
- Database : Oracle

### - 간단 소개
- 해외 명품 직구 샵

## 2. Database 설계
![DBdiagram](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/ad258204-f0aa-43fa-b71d-ef765d1bd2e6)

## 3. 맡은 부분 : 상품관리 📦
이전 프로젝트 시 맡았던 장바구니 및 결제 페이지를 구현하려고 했으나 (5/13 ~ 5/23) 테이블에 값이 제대로 넘어가지 않아 그만 두고 '관리자 - 상품관리' 단을 구현하였다. (5/23 ~ 6/13)
 
### - 상품관리 기능구현 목표
- [x] 상품 등록
    - [x] 유효성검사
        - [x] 필수값(브랜드, 상품 이름, 성별, 카테고리, 사이즈, 가격, 수량, 상품정보) 미입력 시 작성 페이지 유지하며 경고메세지를 띄운다.
        - [x] 상품 가격 변경 및 할인율 변경 시 값이 바로 변동된다.
    - [x] 브랜드 관리에서 등록한 브랜드들을 팝업창으로 목록 확인하여 값을 넣는다.
    직접 타이핑하여 입력되지 않는다.
    - [x] 파일 업로드 시 운영체제 OS 와 Mac을 구분하여 이미지 저장경로를 달리한다.
    - [x] 위지윅 플러그인을 사용하여 글을 작성할 수 있다.
    - [ ] json을 이용하여 글 작성 중 업로드한 이미지를 확인할 수 있다.
- [x] 상품 정보 확인
    - [x] 작성한 상품 글을 확인할 수 있다.
    - [x] Ajax를 이용하여 상품 등록 시 업로드한 이미지를 바로 확인 할 수 있다.
- [x] 상품 수정
    - [x] 상품 등록 시의 유효성검사를 그대로 유지한다.
    - [x] 상품 가격 변경 및 할인율 변경 시 값이 바로 변동된다.
    - [x] 이미지 변경 후 상품 정보를 읽으면 변경된 이미지를 바로 확인 할 수 있다.
- [x] 상품 삭제
    - [x] 삭제할 때 스크립트로 경고 메시지를 띄운다.
- [x] 상품 검색
    - [x] 상품 관리단에서도 상품명 검색한다.
    - [ ] 검색 후 페이지 이동해도 검색어 유지한다.
- [x] 상품 관리 페이지 페이징
- [ ] 카테고리 기능을 구현하여 브랜드 별 / 상품 종류 별로 글을 나눠 볼 수 있다.

### - 구현 모습

#### 1) 상품 관리 목록
<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/015f08a7-5407-4b4a-ace9-7b92cdc71f4e">

- 페이징은 구현되었으나 게시글 번호가 꼬여있는 점이 아쉽다.
- 사이즈 별로 같은 상품을 여러 번 등록해야 하는 점이 아쉽다.

<img width="1282" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/40e94061-e46b-4c3d-b037-efcd9519b035">

- 상품명 검색이 가능하다.

#### 2) 상품 글쓰기
<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/224958ca-81c0-47eb-9cfb-640a8a41ad67">

- 필수 값을 입력하지 않으면 경고 메시지를 보인다. (유효성 검사)
- Ajax 통하여 이미지 업로드는 되나 json을 통해서 이미지를 받아오는 것은 안된다. 글 작성 중에 업로드 할 이미지를 볼 수 없는 점은 아쉽다.

<img width="1061" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/80ab184f-a530-4144-969a-84693cb7697e">

- 브랜드는 직접 입력 할 수 없고 브랜드 목록에 존재하는 값만 선택하여 입력한다.

<img width="1012" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/721b5531-4303-4c34-a96c-cf46d0aee077">

- 원가와 할인율에 따라 판매가가 계산되어 변경된다.

<img width="868" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/4503c391-db57-4e94-8fb3-5af1ae406dff">

- 파일 업로드 시 운영체제에 따라 설정해놓은 다른 경로로 이미지가 저장된다.
- 최대한 상대경로를 적용하고 싶었는데 이게 최선이었다.
```java
String osName = System.getProperty("os.name").toLowerCase();
		String uploadFolder;

		System.out.println("접속한 운영체제 : " + osName);

		if (osName.contains("win")) { // Windows
			uploadFolder = "C:\\upload";
		} else if (osName.contains("nix") || osName.contains("nux") || osName.contains("mac")) { // Linux, macOS
			uploadFolder = System.getProperty("user.home") + "/workspace/upload/tmp";
		} else {
			throw new UnsupportedOperationException("지원되지 않는 운영체제입니다.");
		}
```

#### 3) 상품 수정
<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/10d45372-6a05-4054-8186-5f6c6e2fe8fa">

- 필수값을 입력하지 않으면 경고 메시지를 보인다. (유효성 검사)

#### 4) 상품 수정 완료
<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/b697fa6a-b6b5-424c-aa0e-38b37c464fdf">

<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/b52dc24b-3ff2-414c-a5e1-24b0f8385cab">

<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/95acff47-cec3-4cc6-8eff-05b2d9b723f9">

- 업로드한 이미지가 깨짐없이 바로 적용된다.

#### 5) 상품 삭제

<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/8f541564-8e8e-458b-8c36-5abe950c879f">

<img width="1491" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/eaf94ff3-0ecb-4b64-ab5c-392387bd71b6">

- 확인 버튼을 눌러야 삭제된다.

## 4. 느낀 점

이전에 작업했던 장바구니 및 결제 페이지, 구매내역 부분에 아쉬운 점이 많아서 이번에 작업할 때에도 그 부분을 개선하려고 했으나 기간 내에 하고자 한 기능 구현을 못하여 미구현된 다른 부분을 뒤늦게 작업 시작했다. 저번에는 어찌저찌 구현했지만 이번엔 잘 안되서 스스로에게 화도 많이 났는데 다른 부분을 맡고 저번에 아쉬웠던 CRUD를 연습할 기회로 좋게 생각했다. 

본래 프로젝트 기간은 5월 25일까지 였으나 상품 관리를 만들면서 꼼꼼하게 작업하고 싶은 욕심이 생겨서 다른 팀원에 비해 시간을 많이 초과해버렸다. 개발자로 취업하게 되면 시간엄수는 필수인데 두마리 토끼를 놓치지 않고 작업할 수 있도록 시간 분배를 잘해야 겠다고 크게 느꼈다.

특히 Ajax를 통한 파일 업로드하는 부분이 어려웠는데 json으로 이미지 받아오는 게 잘안되서 추가로 Ajax 및 json, 비동기 작업에 대해서 공부를 더 해야겠다. 프로젝트를 하면서 이전 프로젝트보다 개선되는 부분도 확실히 있는데 할 때마다 개선사항이 생기는 건 어쩔 수 없는 걸까. 만족스러운 부분이 있는 만큼 아쉬운 부분도 커진다. 다음에는 개선사항을 더 줄일 수 있도록 노력해야겠다.


#### 이전에 작업했던 부분
<img width="1440" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/107238b4-90d3-4e3d-aebd-abaea2d4f938">

<img width="1440" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/c90bb54a-32e4-482c-b4c3-c7a91ea202d4">

생각도 못하고 있었는데 저번 mvc2 패턴에서 상품관리/글쓰기를 내가 구현했더라고 ...   
어쩌다보니 이렇게 비교할 기회가 생겼는데 이전에 비해 많이 성장한 내가 너무 신기하다.