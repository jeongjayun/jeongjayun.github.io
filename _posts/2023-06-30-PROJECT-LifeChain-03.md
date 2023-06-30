---
title: Team Project) 생명사슬 ~ Life Chain ~ 03
date: 2023-06-30 17:00:00 +/- TTTT
categories: [PROJECT, Life Chain]
tags: [project] # TAG names should always be lowercase
---
## 관리자 회원목록

MemberAccount(회원가입 등) 가 기존에 따로 있으나   
관리자 페이지에서만 사용할 기능들을 따로 추려 친구와 합치기 쉽도록   
AccountController, AccountService, AccountRepository를 새로 작성했다.
<br>
<br>

**Account Controller 🎮**
```java
@Controller
@RequestMapping("/admin/account")
public class AccountController {

    // 이하 내용들 

}
```

앞서 Spring Security로 접속하는 주소에 따라 권한을 설정했기 때문에 주소를 /admin/accout/ 로 mapping 한다. 이하 내용들에 들어가는 Mapping 들은 주소가 /admin/account/ *** 로 접속된다. 

- 목록을 불러오는 GetMapping 작성.   
list.html로 GetMapping 잘 되었는지 확인한다.

```java
    @GetMapping("/list")
    public String accountList(Model model) {
        return "/admin/templates/pages/account/list";
    }
```
- 타임리프 레이아웃을 적용한 list.html 만들어둔다.   
기존의 템플릿 페이지를 수정했음.

```html
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{/admin/templates/layouts/default_layout}">
      //default_layout 형식으로 꾸밈

<!-- Content -->
<div layout:fragment="content">

    //content 이하 내용을 채워준다.
    //decorate 로 지정한 default_layout처럼
    //mapping 시 head, header, footer 적용한 페이지로 보인다.
    
</div>
</html>
```
- MemberDto를 List 형태로 반환하여 Controller에서 model로 보내준다.

```java
//AccountService.java

 public List<MemberDto> getMemberList() { //전체회원 가져오기
        List<Member> members = memberRepository.findAll();
        //findAll()은 Member에서도 쓰고 있어서
        //AccountRepository에 따로 생성 없이
        //import하여 사용하였다.
        List<MemberDto> memberDtoList = new ArrayList<>();

        for (Member member : members) {
            MemberDto memberDto = MemberDto.builder()
                    .id(member.getId())
                    .memberId(member.getMemberId())
                    .memberNick(member.getMemberNick())
                    .memberDate(member.getCreatedDate())
                    .lastLoginDate(member.getUpdatedDate())
                    .build();
            memberDtoList.add(memberDto);
        }
        return memberDtoList;
    }
```

```java
//AccountController.java

 @GetMapping("/list")
    public String accountList(Model model) {
        List<MemberDto> memberDtoList = accountService.getMemberList();
        model.addAttribute("memberList", memberDtoList);
        return "/admin/templates/pages/account/list";
    }
```

- list.html의 thymeleaf 문법에 따라 Member 테이블의 정보들을 each문으로 가져온다.

```html
<tr th:each="memberList: ${memberList}">
    <td><img th:src="@{/assets/img/avatars/5.png}" alt="Avatar" class="w-px-40 h-auto rounded-circle"/></td> //memberInfo 구현 시 프로필 이미지 링크예정
        
    <td><a th:href="@{'/admin/account/'+${memberList.id}}"th:text="${memberList.memberId}"></a></td>
    //detail.html 로 memberId 통하여 접근예정
                                
    <td th:text="${memberList.memberNick}"></td>
    //멤버 닉네임

    <td th:text="${memberList.memberDate}"></td>
    //멤버 가입일

    <td th:text="${memberList.lastLoginDate}"></td>
    //멤버 마지막 접속일

    <td><span class="badge bg-label-primary me-1">Active</span></td>
    //계정 활성화 상태

    <td>
        <div class="dropdown">
            <button type="button" class="btn p-0    dropdown-toggle hide-arrow" data-bs-toggle="dropdown">
              <i class="bx bx-dots-vertical-rounded"></i>
           </button>

        <div class="dropdown-menu">
            <a class="dropdown-item" href="javascript:void(0);">
            <i class="bx bx-edit-alt me-1"></i> Edit</a >
            
            <a class="dropdown-item" href="javascript:void(0);">
            <i class="bx bx-trash me-1"></i> Delete</a>
        </div>
        //구현 예정
        </div>
    </td>
</tr>
```

## 결과화면
<img width="1494" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/8a12e504-b889-4037-baba-477b81dbc913">

### 수정점 ⚒️
- [ ] 가입한 회원들의 최종 접속일 업데이트가 되지 않는 부분