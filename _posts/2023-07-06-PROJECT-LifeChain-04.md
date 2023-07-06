---
title: Team Project) 생명사슬 ~ Life Chain ~ 04
date: 2023-07-06 17:00:00 +/- TTTT
categories: [PROJECT, Life Chain]
tags: [project] # TAG names should always be lowercase
---

<img width="587" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/1e43d88e-d526-4f2b-86a3-5bdea46e8c16">

지난 번 글에서 회원 목록보기까지 구현 했었는데 추가한 부분이 있어서 회원 목록부터 다시 작성했다.

## 관리자 회원목록(2)

회원목록에 계정의 활동 상태도 함께 표현하기 위해 열거형 함수를 추가했다.

### 1. 관리자 목록에서 계정상태 확인하기

#### MemberStatus.class

```java
@Getter
public enum MemberStatus {
    ACTIVE("유효계정"),
    DORMANT("휴면계정"),
    DEACTIVE("정지계정"),
    WITHDRAWAL("탈퇴계정");

    private final String status;

    MemberStatus(String status) {
        this.status = status;
    }

    public boolean isActive() {
        return this == ACTIVE;
    }

    public boolean isDormant() {
        return this == DORMANT;
    }

    public boolean isDeactive() {
        return this == DEACTIVE;
    }

    public boolean isWithdrawal() {
        return this == WITHDRAWAL;
    }
}
```

#### Member.class

```java
@Getter
@Setter
@Entity
@NoArgsConstructor //기본 생성자를 생성
public class Member extends BaseEntity {

    // (기존내용)

    @Enumerated(EnumType.STRING)
    @Column(name = "member_status")
    private MemberStatus memberStatus;
    // 데이터베이스에도 Status 부분을 추가한다.

}
```

#### MemberDto.class

```java
@Getter
@Setter
@NoArgsConstructor
public class MemberDto {

    // (기존내용)
     private MemberStatus memberStatus; // 추가

     @Builder
    public MemberDto( ..., MemberStatus memberStatus, ...) {
        // (기존 내용)

        this.memberStatus = memberStatus; //추가

        // (기존내용)
    }

}
```

#### AccountService.java

```java
public List<MemberDto> getMemberList() { //전체회원 가져오기
        List<Member> members = memberRepository.findAll();
        List<MemberDto> memberDtoList = new ArrayList<>();

        for (Member member : members) {
            MemberDto memberDto = MemberDto.builder()
                    .id(member.getId())
                    .memberId(member.getMemberId())
                    .memberNick(member.getMemberNick())
                    .memberDate(member.getCreatedDate())
                    .memberStatus(member.getMemberStatus()) //추가된 부분
                    .lastLoginDate(member.getUpdatedDate())
                    .build();
            memberDtoList.add(memberDto);
        }
        return memberDtoList;
    }
```

- 회원 목록에서는 Member를 리스트로 담아오기 때문에 MemberStatus도 추가해준다.

#### list.html

```java
<td>
    <span class="badge bg-label-primary me-1">Active</span>
</td>
```

앞서 작성했던 list.html 의 위 부분을 아래와 같이 바꿔준다.

```java
<td>
    <span th:if="${memberList.memberStatus.isActive()}" class="badge bg-label-primary me-1">Active</span>
    <span th:if="${memberList.memberStatus.isDeactive()}" class="badge bg-label-primary me-2">Deactive</span>
    <span th:if="${memberList.memberStatus.isDormant()}" class="badge bg-label-primary me-3">Dormant</span>
    <span th:if="${memberList.memberStatus.isWithdrawal()}" class="badge bg-label-primary me-4">WithDrawal</span>
</td>
```

- thymeleaf의 th:if 문을 써서 member.memberStatus 에 맞는 값으로 표시한다.

### 2. 회원가입 시 계정상태 설정하기

모든 회원은 가입하자마자 ACTIVE 활동계정 상태를 받고,  
1년 이상 접속하지 않으면 DORMANT 휴면계정 상태로 변경된다.

회원가입 하자마자 ACTIVE 상태가 되도록 가입함수를 수정해준다.

#### MemberService.java

```java
public Member create(MemberRegisterForm memberRegisterForm) {
        Member member = Member.builder()
                .memberId(memberRegisterForm.getMemberId())
                .memberPw(passwordEncoder.encode(memberRegisterForm.getMemberPw1()))
                .memberNick(memberRegisterForm.getMemberNick())
                .memberStatus(MemberStatus.ACTIVE) //추가
                .build();

        // Member 저장
        this.memberRepository.save(member);
        return member;
    }
```

<img width="981" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/c742084c-33a3-4109-88d6-75ae27c46645">

- 회원가입 후에 데이터베이스를 확인하면 member_status가 ACTIVE 로 가입되는 것을 확인할 수 있다.

## 회원정보 조회

<img width="359" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/d8221f32-b53b-423d-8aa0-2d72ee35bab2">

모든 회원은 아이디, 비밀번호, 닉네임 외에 개인정보는 가입 후에 원하는 사람만 선택적으로 입력 할 수 있도록 설정해두었다. 그래서 member와 member_info 로 테이블을 따로 작성하였다.

한명의 회원은 하나의 회원정보만 가져야 하기에 1:1 관계로 맺었다. member 테이블에 회원이 생성되면 member 테이블의 id 값을 member_info 의 member_id 값으로 저장(FK)하여 같은 번호로 생성되도록 했다.

### 1. Entity

#### Member.java

```java
@Getter
@Setter
@Entity
@NoArgsConstructor //기본 생성자를 생성
public class Member extends BaseEntity {
    /* 기본키(primary key) */
    @Id
    /* 자동증가(Auto Increment) */
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne(mappedBy = "member", cascade = CascadeType.ALL, orphanRemoval = true)
    private MemberInfo memberInfo;

    // (기존내용)
}
```

#### MemberInfo.java

```java
@Getter
@Setter
@NoArgsConstructor
@Entity
public class MemberInfo extends BaseEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;

    @Column(name = "info_member_id")
    private String memberId; // memberId 필드 이름 변경

    private String zipcode;
    private String address1;
    private String address2;
    private String memberImg; //이미지 주소
    private String memberIntroduce; //자기소개, 한마디

    @Column(unique = true) //전화번호 중복 방지
    private String memberTel;

    @Builder
    public MemberInfo(Long id, String zipcode, String address1, String address2, String memberImg, String memberIntroduce, String memberTel) {
        this.id = id;
        this.zipcode = zipcode;
        this.address1 = address1;
        this.address2 = address2;
        this.memberImg = memberImg;
        this.memberIntroduce = memberIntroduce;
        this.memberTel = memberTel;
    }

}
```

먼저 Member 엔티티에 memberInfo를 연관관계로 설정해야 한다. Member 엔티티에 @OneToOne 어노테이션을 사용하여 MemberInfo를 참조하도록 한다.

MemberInfo 엔티티에서도 member를 연관관계로 설정해야 한다. MemberInfo 엔티티에 @OneToOne 어노테이션을 사용하여 Member를 참조한다.

```java
//Member.java
    @OneToOne(mappedBy = "member", cascade = CascadeType.ALL, orphanRemoval = true)
    private MemberInfo memberInfo;

//MemberInfo.java
    @OneToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "member_id")
    private Member member;
```

### 2. 목록에서 조회 페이지로 이동하기

#### AccountController.java

```java
@GetMapping("/{id}")
    public String accountDetail(@PathVariable("id") Long id, Model model) {
        Member member = accountService.getMember(id);
        MemberInfo memberInfo = accountService.getMemberInfoByMember(member);

        model.addAttribute("member", member);
        model.addAttribute("memberInfo", memberInfo);

        return "/admin/templates/pages/account/detail";
    }
```

#### AccountService.java
```java
public Member getMember(Long id) { //id로 멤버 찾기
        Optional<Member> member = this.accountRepository.findById(id);
        if (member.isPresent()) {
            return member.get();
        } else {
            throw new DataNotFoundException("member not found");
        }
    }

public MemberInfo getMemberInfoByMember(Member member) { //member로 memberInfo 찾기
        if (member != null) {
            MemberInfo memberInfo = member.getMemberInfo();
            if (memberInfo != null) {
                return memberInfo;
            }
        }
        return null;
    }
```

- 데이터베이스에서 해당 id에 해당하는 Member 튜플과 MemberInfo 튜플을 찾아 model로 detail.html 에 반환한다.

#### list.html
```html
 <tr th:each="memberList: ${memberList}">
    // (기존내용)

    <td>
        <a th:href="@{'/admin/account/'+${memberList.id}}"
        th:text="${memberList.memberId}"></a>
    </td>

    // (기존내용)
</tr>
```

#### detail.html
```html
<!-- Content -->
<div layout:fragment="content">
    <div class="container-xxl flex-grow-1 container-p-y">
        <h4 class="fw-bold py-3 mb-4"><span class="text-muted fw-light">Account/</span> 회원정보</h4>

        <div class="row">
            <div class="col-md-12">
                <ul class="nav nav-pills flex-column flex-md-row mb-3">
                    <li class="nav-item">
                        <a class="nav-link active" href="javascript:void(0);"><i class="bx bx-user me-1"></i>
                            Account</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="pages-account-settings-notifications.html"
                        ><i class="bx bx-bell me-1"></i> Notifications</a
                        >
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="pages-account-settings-connections.html"
                        ><i class="bx bx-link-alt me-1"></i> Connections</a
                        >
                    </li>
                </ul>
                <div class="card mb-4">
                    <h5 class="card-header">회원정보 상세</h5>
                    <h5 class="card-header">안녕하세요 <span th:text="${member.memberNick}">회원 닉네임</span>님</h5>
                    <!-- Account -->

                    <div class="card-body">
                        <div class="d-flex align-items-start align-items-sm-center gap-4">
                            <img
                                    th:src="@{/assets/img/avatars/1.png}"
                                    alt="user-avatar"
                                    class="d-block rounded"
                                    height="100"
                                    width="100"
                                    id="uploadedAvatar"
                            />

                            <div class="button-wrapper">
                                <span th:if="${memberInfo != null and memberInfo.memberIntroduce != null}"
                                      th:text="${memberInfo.name}">회원 자기소개 / 한마디</span>
                                <span th:if="${memberInfo == null || memberInfo.memberIntroduce == null}">입력된 자기소개가 없습니다.</span>
                            </div>
                        </div>
                    </div>

                    <hr class="my-0"/>
                    <div class="card-body">
                        <div class="row">
                            <div class="mb-3 col-md-6">
                                <label for="email" class="form-label">E-mail</label>
                                <div id="email"
                                     name="email"
                                     class="form-control"
                                     th:text="${member.memberId}">회원 이메일
                                </div>
                            </div>
                            <div class="mb-3 col-md-6">
                                <label for="memberNick" class="form-label">닉네임</label>
                                <span
                                        class="form-control"
                                        id="memberNick"
                                        name="memberNick"
                                        th:text="${member.memberNick}"> 회원 닉네임</span>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label">연락처</label>
                                <div class="input-group input-group-merge">

                                    <span th:if="${memberInfo != null and memberInfo.memberTel != null}"
                                          th:text="${memberInfo.memberTel}"
                                          class="form-control">연락처</span>
                                    <span th:if="${memberInfo == null || memberInfo.memberTel == null}"
                                          class="form-control">입력된 연락처가 없습니다.</span>
                                </div>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label">Zip Code</label>
                                <span th:if="${memberInfo != null and memberInfo.zipcode != null}"
                                      th:text="${memberInfo.memberTel}" maxlength="6"
                                      class="form-control">우편번호</span>
                                <span th:if="${memberInfo == null || memberInfo.zipcode == null}" maxlength="6"
                                      class="form-control">입력된 우편번호가 없습니다.</span>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label">Address</label>
                                <span th:if="${memberInfo != null and memberInfo.address1 != null}"
                                      th:text="${memberInfo.address1}"
                                      class="form-control">주소</span>
                                <span th:if="${memberInfo == null || memberInfo.address1 == null}"
                                      class="form-control">입력된 주소가 없습니다.</span>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label">Address 2</label>
                                <span th:if="${memberInfo != null and memberInfo.address2 != null}"
                                      th:text="${memberInfo.address2}"
                                      class="form-control">주소</span>
                                <span th:if="${memberInfo == null || memberInfo.address2 == null}"
                                      class="form-control">입력된 주소가 없습니다.</span>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label" for="createdDate">가입일</label>
                                <span id="createdDate" class="form-control" th:text="${member.createdDate}">
                                </span>
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label" for="updatedDate">마지막 접속일</label>
                                <span id="updatedDate" class="form-control" th:text="${member.updatedDate}">
                                </span>
                            </div>

                            <div class="mb-3 col-md-6">
                            </div>

                            <div class="mb-3 col-md-6">
                                <label class="form-label" for="memberStatus">계정상태</label>
                                <span id="memberStatus" class="form-control" th:text="${member.memberStatus}">
                                </span>
                            </div>
                        </div>
                        <div class="mt-2">
                            <a th:href="@{'/admin/account/update/'+${member.id}}" th:value="${member.id}">
                                <button type="submit" class="btn btn-primary me-2">회원정보 수정</button>
                            </a>
                            <a th:href="@{/admin/account/list}">
                                <button type="reset" class="btn btn-outline-secondary">
                                    회원 목록
                                </button>
                            </a>
                        </div>
                    </div>
                    <!-- /Account -->
                </div>
            </div>
        </div>
    </div>
</div>
    <!-- / Content -->

```

> model.addAttribute("member", member);   
> model.addAttribute("memberInfo", memberInfo);

- 컨트롤러에서 작성했던 것처럼 member와 memberInfo 및 thymeleaf문으로 데이터베이스에 있는 내용을 가져온다.


## 회원정보 수정하기

### 1. 회원정보 수정페이지로 Mapping

#### AccountController.java
```java
    @GetMapping("/update/{id}")
    public String updateAccountForm(@PathVariable("id") @Valid Long id, Model model) {
        Member member = accountService.getMember(id);
        MemberInfo memberInfo = accountService.getMemberInfoByMember(member);

        model.addAttribute("member", member);
        model.addAttribute("memberInfo", memberInfo);

        return "/admin/templates/pages/account/setting";
    }
```

- 조회페이지에서 버튼을 누르면 수정 페이지로 이동하도록 GetMapping을 한다.   
- Member, MemberInfo 테이블에 내용이 있으면 수정 페이지 input칸에 Placeholder 로 이미 입력 되어있는 내용을 가져올 수 있도록 addAttribute 한다.

#### 수정 페이지
```html
<!-- Content -->
<div layout:fragment="content">
    <div class="container-xxl flex-grow-1 container-p-y">
        <h4 class="fw-bold py-3 mb-4"><span class="text-muted fw-light">Account/</span> 회원정보 수정</h4>

        <div class="row">
            <div class="col-md-12">
                <ul class="nav nav-pills flex-column flex-md-row mb-3">
                    <li class="nav-item">
                        <a class="nav-link active" href="javascript:void(0);"><i class="bx bx-user me-1"></i>
                            Account</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="pages-account-settings-notifications.html"
                        ><i class="bx bx-bell me-1"></i> Notifications</a
                        >
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="pages-account-settings-connections.html"
                        ><i class="bx bx-link-alt me-1"></i> Connections</a
                        >
                    </li>
                </ul>
                <div class="card mb-4">
                    <h5 class="card-header">회원정보 수정</h5>
                    <h5 class="card-header">안녕하세요 <span th:text="${member.memberNick}"></span>님</h5>
                    <!-- Account -->
                    <div class="card-body">
                        <div class="d-flex align-items-start align-items-sm-center gap-4">
                            <img
                                    th:src="@{/assets/img/logo/logo.png}"
                                    alt="user-avatar"
                                    class="d-block rounded"
                                    height="100"
                                    width="100"
                                    id="noProfileImage"
                            />
                            <div class="button-wrapper">
                                <label for="upload" class="btn btn-primary me-2 mb-4" tabindex="0">
                                    <span class="d-none d-sm-block">사진 변경</span>
                                    <i class="bx bx-upload d-block d-sm-none"></i>
                                    <input
                                            type="file"
                                            id="upload"
                                            class="account-file-input"
                                            hidden
                                            accept="image/png, image/jpeg"
                                    />
                                </label>
                                <button type="button" class="btn btn-outline-secondary account-image-reset mb-4">
                                    <i class="bx bx-reset d-block d-sm-none"></i>
                                    <span class="d-none d-sm-block">Reset</span>
                                </button>

                                <p class="text-muted mb-0">Allowed JPG, GIF or PNG. Max size of 800K</p>
                            </div>
                        </div>
                    </div>
                    <hr class="my-0"/>
                    <div class="card-body">
                        <form id="formAccountSetting" method="post"
                              th:action="@{'/admin/account/update/' + ${member.id}}">
<!--                            <input type="hidden" name="_method" value="PUT"/>-->
                            <input type="hidden" th:value="${member.id}"/>

                            <div class="row">
                                <div class="mb-3 col-md-6">
                                    <label for="memberId" class="form-label">E-mail</label>
                                    <input
                                            class="form-control"
                                            type="text"
                                            id="memberId"
                                            name="memberId"
                                            th:value="${member.memberId}"
                                            readonly
                                    />
                                </div>
                                <div class="mb-3 col-md-6">
                                    <label for="memberNick" class="form-label">닉네임</label>
                                    <input
                                            type="text"
                                            class="form-control"
                                            id="memberNick"
                                            name="memberNick"
                                            th:value="${member.memberNick}"
                                    />
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">Phone Number</label>
                                    <div class="input-group input-group-merge">
                                        <span class="input-group-text">KR (+82)</span>
                                        <input type="text"
                                               id="memberTelWithValue"
                                               name="memberTel"
                                               class="form-control"
                                               th:if="${memberInfo != null and memberInfo.memberTel != null}"
                                               th:value="${memberInfo.memberTel}"/>

                                        <input th:if="${memberInfo == null || memberInfo.memberTel == null}"
                                               id="memberTelPlaceholder"
                                               name="memberTel"
                                               placeholder="01012345678, -빼고 숫자만 입력해주세요."
                                               class="form-control"/>
                                    </div>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">연락처 인증</label>
                                    <div class="input-group input-group-merge">
                                        <input class="form-control" placeholder="핸드폰 인증번호를 입력해주세요.">
                                        <button class="btn btn-primary me-2">인증번호 요청</button>
                                    </div>

                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">Zip Code</label>
                                    <input type="text"
                                           id="zipcodeWithValue"
                                           name="zipcode"
                                           class="form-control"
                                           th:if="${memberInfo != null and memberInfo.zipcode != null}"
                                           th:value="${memberInfo.zipcode}"/>

                                    <div class="input-group input-group-merge">
                                        <input th:if="${memberInfo == null || memberInfo.memberTel == null}"
                                               id="zipcodeWithPlaceholder"
                                               name="zipcode"
                                               placeholder="00000"
                                               class="form-control"/>
                                        <button id="kakao_address_button" class="btn btn-primary me-2">주소 찾기</button>
                                    </div>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <div class="input-group input-group-merge">

                                    </div>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">Address 1</label>

                                    <input type="text"
                                           id="address1WithValue"
                                           name="address1"
                                           class="form-control"
                                           th:if="${memberInfo != null and memberInfo.address1 != null}"
                                           th:value="${memberInfo.address1}"/>

                                    <input th:if="${memberInfo == null || memberInfo.address1 == null}"
                                           id="address1Placeholder"
                                           name="address1"
                                           placeholder="서울 특별시 OO구 OOO로 OO길"
                                           class="form-control"/>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">Address 2</label>

                                    <input type="text"
                                           id="address2WithValue"
                                           name="address2"
                                           class="form-control"
                                           th:if="${memberInfo != null and memberInfo.address2 != null}"
                                           th:value="${memberInfo.address2}"/>

                                    <input th:if="${memberInfo == null || memberInfo.address2 == null}"
                                           id="address2Placeholder"
                                           name="address2"
                                           placeholder="OO동 OO호"
                                           class="form-control"/>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label">한마디</label>

                                    <input type="text"
                                           id="memberIntroduceWithValue"
                                           name="memberIntroduce"
                                           class="form-control"
                                           th:if="${memberInfo != null and memberInfo.memberIntroduce != null}"
                                           th:value="${memberInfo.memberIntroduce}"/>

                                    <input th:if="${memberInfo == null || memberInfo.address2 == null}"
                                           id="memberIntroducePlaceholder"
                                           name="memberIntroduce"
                                           placeholder="자기소개를 적어주세요."
                                           class="form-control"/>
                                </div>

                                <div class="mb-3 col-md-6">
                                    <label class="form-label" for="memberStatus">계정상태</label>
                                    <select id="memberStatus" class="select2 form-select" th:field="${member.memberStatus}">
                                        <option th:value="ACTIVE" th:selected="${member.memberStatus == 'ACTIVE'}">Active</option>
                                        <option th:value="DEACTIVE" th:selected="${member.memberStatus == 'DEACTIVE'}">Deactive</option>
                                        <option th:value="DORMANT" th:selected="${member.memberStatus == 'DORMANT'}">Dormant</option>
                                        <option th:value="WITHDRAWAL" th:selected="${member.memberStatus == 'WITHDRAWAL'}">Withdrawal</option>
                                    </select>
                                </div>

                            </div>

                            <div class="mt-2">
                                <button type="submit" class="btn btn-primary me-2">수정</button>
                                <button type="reset" class="btn btn-outline-secondary">취소</button>
                            </div>

                        </form>
                    </div>
                    <!-- /Account -->
                </div>
            </div>
        </div>
    </div>
</div>
<!-- / Content -->

```
 - 회원 정보 조회 페이지를 수정해서 수정 페이지로 만든다. 

 Member 테이블에 새로운 계정이 생길 때 개인정보(MemberInfo)는 선택입력이기 때문에 FK(member_id, info_memberId) 외의 값은 null 값으로 저장되도록 설정했었다. 그래서 MemberInfo 테이블에 내용이 있으면 내용을 출력하고 내용이 없으면 null값 대신 Place holder로 설정된 값이 표시되도록 항목마다 th:if 문을 사용했다.

 ```html
 // detail.html 일부 

 <div class="mb-3 col-md-6">
    <label class="form-label">Address 1</label>

    <input type="text"
           id="address1WithValue"
           name="address1"
           class="form-control"
           th:if="${memberInfo != null and memberInfo.address1 != null}"
           th:value="${memberInfo.address1}"/>

    <input th:if="${memberInfo == null || memberInfo.address1 == null}"
           id="address1Placeholder"
           name="address1"
           placeholder="서울 특별시 OO구 OOO로 OO길"
           class="form-control"/>
</div>

 ```

### 2. 회원정보 수정 구현하기
#### AccountController.java
```java
    @PostMapping("/update/{id}")
    public String updateAccount(@PathVariable("id") @Valid Long id, MemberDto memberDto, MemberInfoDto memberInfoDto) {
        accountService.updateAccountByAdmin(memberDto);
        accountService.updateAccountByAdmin2(memberDto, memberInfoDto);
        return "redirect:/admin/account/list";
    }
```
- 글 수정을 완료하면 회원목록으로 redirect 하였다.   

#### AccountService.java
```java
@Transactional
    public void updateAccountByAdmin(MemberDto memberDto) { //Member 정보 업데이트 가능
        Member member = memberRepository.findById(memberDto.getId())
                .orElseThrow(() -> new DataNotFoundException("Member not found"));
        //member Id에 있는 계정 정보 확인

        member.setMemberNick(memberDto.getMemberNick());
        member.setMemberStatus(memberDto.getMemberStatus());

        System.out.println("member 수정 후 : " + member);

        accountRepository.save(member);
    }

@Transactional
    public void updateAccountByAdmin2(MemberDto memberDto, MemberInfoDto memberInfoDto) {
        Member member = memberRepository.findBymemberId(memberDto.getMemberId())
                .orElseThrow(() -> new DataNotFoundException("Member not found"));

        MemberInfo memberInfo = member.getMemberInfo();
        if (memberInfo == null) {
            memberInfo = new MemberInfo();
            memberInfo.setMember(member);
        }

        memberInfo.setZipcode(memberInfoDto.getZipcode());
        memberInfo.setAddress1(memberInfoDto.getAddress1());
        memberInfo.setAddress2(memberInfoDto.getAddress2());
        memberInfo.setMemberImg(memberInfoDto.getMemberImg());
        memberInfo.setMemberIntroduce(memberInfoDto.getMemberIntroduce());
        memberInfo.setMemberTel(memberInfoDto.getMemberTel());

        member.setMemberInfo(memberInfo);

        System.out.println("memberInfo 수정 후: " + memberInfo);

        memberInfoRepository.save(memberInfo); // memberInfo 저장
        System.out.println("회원 정보 수정 완료");
    }
```

회원 정보 수정 시 메서드가 길어져 2개로 나누었다. 위의 updateAccountByAdmin 메서드는 Member 테이블의 닉네임, 계정상태를 변경할 수 있고 아래의 updateAccountByAdmin2 메서드는 MemberInfo 테이블의 주소, 이미지, 연락처, 자기소개 등을 변경할 수 있도록 했다.


## 결과 화면
### - 회원 목록
<img width="1552" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/2f409756-3938-4f63-858c-52cca7b51354">

### - 조회 화면
<img width="1552" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/d1a0e3b8-6020-4a5c-9263-0d61adb3d2b3">

### - 수정 화면
<img width="1552" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/f89ead28-4aad-47e6-b4fe-7c8d8ba03d17">