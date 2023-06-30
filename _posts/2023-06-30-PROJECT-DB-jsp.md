---
title: Team Project) DB ~ jsp, mvc2 쇼핑몰 프로젝트 ~ 
date: 2023-05-30 20:00:00 +/- TTTT
pin: true
categories: [PROJECT, DB]
tags: [project, jsp, mvc2] # TAG names should always be lowercase
---

# DB 💎🖤

첫 프로젝트를 보안해서 웹 프로젝트로 이어나가려 했으나 반 전체적으로 조원 변동이 있었고 첫 프로젝트를 웹으로 옮기기에는 구현할 기능의 부족하다는 강사님의 조언이 있었다. 회의 끝에 학원에서 배운 java 및 db를 연결한 CRUD의 기능들을 활용하며 우리만의 **특색**이 담긴 쇼핑몰 웹을 구현하기로 하였다.

우리들의 프로젝트명 'DB'는 DataBase의 DB 이면서 우리가 만들 테마인 [해외 명품 직구샵]의 이미지에 맞게 DirectBuying, DiamondBlack이라는 두가지 의미를 갖고 있다.

 > **첫 프로젝트**
 > [petTalk](https://github.com/jeongjayun/petTalk) : java swing 이용하여 만든 애완인들의 커뮤니티 애플리케이션을 구현하고자 하였음.
 {: .prompt-info }

## 목차
1. 프로젝트 개요
1. 프로젝트 진행순서
1. 기능별 요구사항
1. Database 설계
1. 팀원 및 역할분담
1. 화면 설계
1. 개선사항 및 느낀점

## 1. 프로젝트 개요
![image](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/4444af38-484a-45ff-8f33-47ac73d92faf)

### - 프로젝트명 : Diamond Black 💎🖤
### - 인원 : 4명
- **정자윤** (본인)
- 박권능
- 이규동
- 임종민

### - 기간 : 2023.04.10 ~ 2023.04.21
### - 기능
#### (1) 사용자
1. 회원 가입
1. 로그인
1. (자유 게시판)게시글 등록, 수정, 삭제
1. 마이페이지
    - 주문 내역
    - 내가 쓴 글 확인
    - 회원 정보 수정 및 탈퇴
1. 상품 구매
    - 경매 및 핫딜 등 상품 구매
1. 장바구니 기능 및 결제

#### (2) 관리자
1. 회원 관리
    - 회원 정보 수정
    - 회원 탈퇴
1. 게시판 관리(자유 게시판, QnA 게시판)
    - 게시글 등록, 수정, 삭제
1. 상품 관리
    - 상품 등록, 삭제
1. 옥션, 핫딜 관리
    - 옥션 및 핫딜 상품 관리
    - 시간 설정 및 할인율 설정 등

#### (3) 게시판
1. 자유 게시판
    - CRUD
    - 조회수, 페이징 처리, 댓글, 이미지 첨부
1. QnA 게시판
    - CRUD기능
    - 조회수, 페이징 처리, 이미지 첨부

#### (4) 상품
1. 상품 등록
    - 가격, 이미지, 상품 설명, 사이즈, 브랜드
1. 장바구니 담기

#### (5) 장바구니
1. 장바구니 담긴 상품 삭제
1. 총 상품 가격 확인

#### (6) 결제
1. 구매자 정보 확인
1. 총 결제 금액 확인
1. 결제 API를 이용하여 결제

#### (7) Contact
1. 지도 API를 이용하여 회사 위치 확인
1. 회사 정보 확인

#### (8) 옥션(경매)
1. 관리자가 설정한 상품, 시간, 시작가 확인
1. 입찰이 끝나면 낙찰자에게 구매 권한 부여

#### (9) 핫딜(타임세일)
1. 관리자가 설정한 상품 갯수, 시간, 할인율 확인
1. 해당 시간 안에 할인된 임의의 상품을 구매 가능


### - 개발환경
- 개발 언어 : Java 11, JavaScript, JSP, HTML
- IDE : Eclipse(JDK11)
- Data Base : Oracle

### - 간단소개
기본적인 쇼핑몰에 추가기능(옥션, 핫딜)을 구현하였다.

## 2. 프로젝트 진행순서
1. DB 구성 및 설계.
1. 팀원들에게 배포 후, 각자 맡은 역할(회원 가입과 로그인, 게시판, 상품 등록 ~ 구매 등) 맞춰 CRUD 기능을 구현한다.
1. 기본 기능 구현 후 핫딜(타임세일), 옥션(경매)같은 특색있는 기능을 구현한다.
1. 구현한 기능들을 테스트를 하며 세세한 부분을 수정 및 보안한다.

## 3. 기능별 요구사항
- 회원 가입
    - 유효성 검사
    - 이름은 한글(자음+모음)의 조합 2글자 이상
    - 아이디는 영문 대소문자와 숫자 4~12 자리의 조합으로 입력
    - 아이디 중복 체크 후 사용 가능
        - 아이디 중복 시 “이미 사용 중인 아이디입니다.” 메시지 출력
    - 이메일 형식은 Emailid 뒤에 @ 과 ###.### 의 형식으로 입력
    - 우편번호는 주소API를 이용하여 자신의 집 검색 후 자동 입력
    - 비밀번호는 영문과 숫자 조합으로 입력
    - 비밀번호는 sha256 알고리즘으로 암호화 되어 DataBase에 저장
<br>
<br>

- 로그인
    - 로그인을 하지 않은 경우 아래 페이지만 이용 가능
        - 회원 가입 페이지
        - 로그인 페이지
        - 게시글 목록 조회 페이지
        - 게시글 상세 보기 페이지
        - 상품 페이지
        - 옥션 페이지
        - 핫딜 페이지
        - 로그인을 하지 않고 위에 페이지를 제외한 다른 페이지를 이용하려 할 시 “로그인 후 사용 가능 합니다.” 메시지 출력 후 기능 이용 제한

    - 로그인 검사
        - 아이디와 비밀번호가 일치하지 않을 경우 “비밀 번호가 맞지 않습니다.” 메시지 출력
        - 아이디가 존재하지 않을 경우 “존재하지 않는 아이디 입니다.” 메시지 출력
        - 아이디와 비밀번호가 일치할 시 메인 페이지로 이동
        - 어드민 계정으로 로그인할 시 관리자 페이지 추가
<br>
<br>

- 유저
    - 상품 장바구니 담기
    - 장바구니 상품 구매
        - 구매 시 이용 약관에 동의 하지 않았거나, 결제 방식을 선택 하지 않았으면 각각의 선택 메시지 출력
        - 위의 이용 약관과 결제 방식을 선택 후 구매 버튼을 눌렀다면, 결제API 모듈 실행
    - 옥션 상품 입찰, 구매
        - 낙찰된 최종 입찰자만 구매 가능
    - 핫딜 상품 구매
    - 자유 게시판 이용
        - 등록, 수정, 삭제 가능(수정 과 삭제는 작성한 본인 만 가능)
    - QnA 게시판 이용
        - 등록, 수정, 삭제는 어드민만 가능
<br>
<br>

- 마이 페이지
    - 내 정보
        - 비밀번호 입력 후 수정 페이지로 이동
        - 아이디와 이름을 제외한 정보들을 수정 가능
        - 수정란에 NOT NULL인 비밀번호를 입력하지 않을 시 
        - “비밀번호를 입력해주세요” 메시지 출력
        - 비밀번호를 재차 확인하여 비밀번호 일치 여부 확인
        - 회원 탈퇴 가능
    - 주문 내역
        - 자신이 구매한 주문 번호, 주문 날짜, 상태 목록 출력
        - 주문 번호 클릭 시 구매한 해당 주문 번호의 상품과 가격 등의 상세 내역을 확인
    - 내가 쓴 글
        - 자유 게시판에 작성한 나의 글 확인 및 수정, 삭제 가능
<br>
<br>

- 어드민
    - 상품관리
        - 브랜드 관리
            - 등록 : 브랜드 로고 이미지와 브랜드 명
            - 등록된 브랜드 삭제
        - 상품 관리
            - 등록 : 브랜드, 상품 카테고리, 상품 이름, 사이즈, 가격, 성별, 이미지, 상품 설명, 재고량 등을 설정
            - 삭제 가능
    - 회원 관리
        - 회원 정보 수정
        - 가입 일자를 제외한 모든 정보 수정 가능
        - 회원 삭제
    - 게시판 관리
        - 자유 게시판 관리
            - 자유 게시판 등록, 수정, 삭제 가능
        - QnA 게시판 관리
            - QnA 게시판 등록, 수정, 삭제 가능
    - 옥션
        - 옥션 상품 등록
        - 등록할 상품을 선택 후 시작가, 제한시간 설정
    - 핫딜
        - 핫딜 할인률과 할인할 상품 종류의 갯수, 할인 시간을 설정
    - 매출 관리
        - 판매한 총 매출 가격과 구매자, 주문 번호 출력
<br>
<br>

- 상품
    - 브랜드 로고 클릭 시 해당 브랜드의 모든 상품 리스트 출력
    - 카테고리 클릭 시 해당 브랜드의 카테고리가 설정된 상품 리스트 출력
    - 상품 검색 시 해당 검색어가 들어가는 상품 리스트 출력
    - 검색한 상품이 존재 하지 않을 시 “검색 결과 없음” 페이지 출력
<br>
<br>

- Contact
    - Kakao지도 API를 활용하여 설정한 임의의 회사 위치를 지도로 확인 가능
    - 회사의 상세 정보를 확인 가능

- 디자인
    - Bootstrap을 이용하여 모바일과 pc에 따른 반응형 웹으로 제작

## 4. Database 설계
![DB](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/784d1efd-69d4-4f8c-9eff-6d0a56dcbc8e)

![user](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/147a5246-e514-49b4-8382-f56957c856ac)

![product](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/ae94bda8-171a-44a1-9c1f-2e17e0c69f2d)

![free](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/8d7010ac-cbbf-40d8-9895-48957451d812)

![qna](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/4a0aa1f9-8491-433d-9158-391c83f3fd90)

![auction](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/02775171-b8b8-4f98-8576-1efe07dd677c)

![cart](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/7d2e4a6c-d573-413d-9f64-3bdc05f22043)


## 5. 팀원 및 역할 분담 🤼‍♀️
### 정자윤
- 데이터베이스 Table 구성
- 장바구니 및 결제 페이지 구현
- 마이 페이지 : 주문 내역 구현
- PPT 작성

### 박권능
- 회원 가입 구현
    - 유효성 검사 스크립트 삽입
    - 패스워드 암호화 처리
- 결제API, 지도API 삽입
- 게시판의 페이징 처리
- Contact 페이지 구현

### 이규동
- 게시판 구현
    - 댓글API인 disqus 스크립트 삽입
- 핫딜(타임세일) 구현
- 상품(로고, 카테고리, 검색)리스트 구현
- 로그인 및 회원 탈퇴 구현
- 마이 페이지 구현
    - 내가 쓴 글
    - 내 정보 수정
- 팀원들의 파일 취합 후 테스트, 에러 발생 시 에러 부분 수정
- 전체적인 UI 디자인 통합 및 수정
- 관리자 페이지의 QnA게시판 관리, 매출 관리 구현
- 전체 기능에 로그인이 필요한 기능은 스크립트 처리
- PPT 발표

### 임종민
- 관리자 페이지 구현
    - 브랜드, 상품관리
    - 회원 관리
    - 옥션 관리
    - 자유 게시판 관리 구현
- 옥션(경매) 구현


## 6. 화면 설계 
### 1) 로그인 하지 않은 경우
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/dVmVzd8xPAc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 로그인을 하지 않은 상태에서는 게시판 등록, 상품 구매, 장바구니등 기능을 이용할 수 없다.

### 2) 회원가입과 로그인 (유효성 검사)
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/YSQo2rH0BPc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 회원 가입과 로그인은 유효성 검사를 거쳐 진행된다.
- 회원 가입시 입력한 패스워드는 아래처럼 암호화 되어 저장된다.

![sha256](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/07169f7f-e11b-4de0-b5e2-accc1ef8a3de)


### 3) 상품 검색
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/KpQt5QU5Pp0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 검색한 단어를 포함한 상품을 출력하며, 검색한 단어를 포함한 상품이 존재하지 않을 시 검색 결과 없음 페이지를 보여준다.

### 4) 마이페이지
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/POD1eDlQZaY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 본인이 구매한 상품들의 주문내역을 확인 할 수 있고, 내가 작성한 글의 목록을 볼 수 있다.
- 내 정보에 들어가면 내 정보를 수정 할 수 있고, 탈퇴가 가능하다.

### 5) 게시판 이용
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/riogK5uUMtA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 자유 게시판 모든유저가 열람 가능하지만 등록은 회원가입한 유저만 이용 할 수 있고, 본인이 작성한 글만 수정 및 삭제를 할 수 있다.
- QnA 게시판은 모든 유저가 열람 가능하지만 등록, 수정, 삭제는 어드민만 가능 하다.

### 6) 관리자 페이지
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/eEkre28C3iQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

- 회원 관리, 게시판 관리, 상품 관리, 및 옥션(경매) 상품을 등록 할 수 있다.

### 7) 게시판 이용
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/riogK5uUMtA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## 7. 개선 사항과 느낀 점
### 개선 사항
1. 로그인 부분
    - 카카오톡/ 구글 API를 이용하여 로그인 가능하게 구현
    - 이메일 인증 기능 구현
2. 유저
    - 찜하기 기능
    - 장바구니에 담은 상품 수를 장바구니 뱃지에 표현
    - 마이페이지 내역이 일부 꼬임 현상 있음
    - 결제 시 결제한 금액에 따른 포인트 지급 및 포인트에 따른 회원 등급 조정
    - 회원 등급에 따른 상품 할인 쿠폰 지급
3. 상품
    - 상품 구매 할 시 재고량의 변동을 구현
    - 할인 쿠폰 구현
    - 취소 및 반품 구현
    - 옥션의 낙찰자가 상품 구매 후 해당 상품의 구매하기 버튼을 사라지게 구현
    - 상품 페이지의 페이징 처리
    - 핫딜 기능을 코드로 직접 고쳐서 반영 시키는게 아닌 어드민 페이지에서 조정 할 수 있게 구현
4. 게시판
    - 게시판의 답글 달기 및 API를 이용한 댓글이 아닌 웹페이지 자체의 댓글 기능 구현

### 느낀 점
수업 시간에 MCV1, MVC2 패턴을 이용하여 각각 CRUD 기능을 구현해봤는데 첫 프로젝트 때와 비교하여도 MVC2 패턴이 팀원들과 협업 했을 때에 기능을 나누고 합치기 편리했다. 이번 프로젝트에서는 데이터베이스와 장바구니 및 결제 페이지 등을 맡아서 진행 했었는데 프로젝트 내에서 CRUD 기능을 충분히 연습하지 못한 것 같아서 아쉬움이 남는다. 다음 프로젝트는 Spring Framework를 이용하여 이번 프로젝트를 이어서 개선사항들을 보완하고 CRUD 기능을 숙지해야 겠다. 