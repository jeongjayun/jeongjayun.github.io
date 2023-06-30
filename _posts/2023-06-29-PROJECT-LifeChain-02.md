---
title: Team Project) 생명사슬 ~ Life Chain ~ 02
date: 2023-06-29 17:00:00 +/- TTTT
categories: [PROJECT, Life Chain]
tags: [project] # TAG names should always be lowercase
---
# 관리자 로그인 및 페이지 
<img width="428" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/9af62f10-785d-4d68-8e7c-d65c99844a87">

## 템플릿 사용
부트스트랩5를 이용한 무료 템플릿을 사용하기로 했다.
> 출처 : <https://themeselection.com/item/sneat-free-bootstrap-html-admin-template/>
{: .prompt-info }

### 레이아웃
Spring Boot에서는 Thymeleaf를 이용한 Layout을 사용해서 템플릿에서 head, header, footer, content 를 나누어 큰 틀을 만들 수 있다.

폴더 구조는 이렇게 작성했다.   
<img width="249" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/21f3c65e-5288-4c2e-844b-d789d5ce214b">

- 레이아웃을 사용하기 위해 build.gradle 에 의존성을 주입한다.

```gradle
 implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect'
```

- head, header, footer 에 맞춰서 html 파일을 나눈다.

```html
//head.html

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">

<th:block th:fragment="headFragment"> //Fragment 명시

    // head 부분에 들어갈 내용 작성 
    // html 및 meta 태그, title, css, script 등
    // 단, 타임리프의 경우 타임리프에 맞는 문법을 써주지 않으면 페이지 이동 시에 css 및 스크립트가 적용되지 않는 상황이 발생한다.

</th:block>

</html>
```

```html
//header.html

<html lagn="ko" xmlns:th="http://www.thymeleaf.org">
<!--headerFragment 선언-->
<div th:fragment="headerFragment"> //Fragment 명시
    <!-- Header Section -->

    // header 부분에 들어갈 내용 작성
    // 메뉴와 navbar, searchbar 내용을 함께 넣었다.

</html>
```

```html
//footer.html

<html lagn="ko" xmlns:th="http://www.thymeleaf.org">
<!--footerFragment 선언-->
<div th:fragment="footerFragment"> //Fragment 명시

<!-- Footer Section -->
    // footer 부분에 들어갈 내용 작성
<!-- / Footer -->
</div>
</html>
```

- 위의 fragment 들을 담을 default_layout 페이지를 만든다.

```html
//default_layout.html

<!DOCTYPE html>
<html lang="ko"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">

<!-- head fragment 사용 -->
<th:block th:replace="/admin/templates/fragments/head :: headFragment"></th:block> //head.html 로 대치

<body>

<!-- header fragment 사용 -->
<th:block th:replace="/admin/templates/fragments/header :: headerFragment"></th:block> //header.html 로 대치

<!-- Content wrapper -->
<div class="content-wrapper">
    <!-- content fragment 사용 -->
    <th:block layout:fragment="content"></th:block> //content.html 로 대치
</div>

<!-- footer fragment 사용 -->
<th:block th:replace="/admin/templates/fragments/footer :: footerFragment"></th:block> //footer.html 로 대치


<!-- 전페이지 공통에서 사용할 JS들 -->
    //Thymeleaf 문법에 맞춰서 @{} 식으로 링크걸어주지 않으면
    //깨지니까 타임리프 문법에 맞춰 주소들 모두 바꿔주기!


</body>
</html>
```

- 관리자 로그인하면 보이는 관리자 페이지 index.html을 완성. default_layout 형식에 맞춰서 페이지가 만들어 진다.

```html
//index.html

<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{/admin/templates/layouts/default_layout}">

<!-- Content -->
<div layout:fragment="content" class="layout-page">
    <h1>Content</h1> 
</div>

</html>
```

## 관리자 로그인
### Spring Security 사용한 로그인

- Spring Security 사용을 하기 위해 build.gradle 의존성을 주입하였다.

```gradle
implementation 'org.springframework.boot:spring-boot-starter-security'
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity5:3.0.4.RELEASE'
```

이전 Spring, JSP로 프로젝트 진행할 당시에는 스크립트와 코드로 유효성 검사를 진행하였으나 Srping Boot에서는 Thymeleaf & Security 를 사용할 수 있다는 점이 재미있었다.

처음에 프로젝트 생성 당시에 진희와 버전에 대해서 이야기 하지 않아서 나는 jdk 11, spring boot 2.7.12 버전에서 작업하고 있었고 진희는 jdk 17, spring boot 3.1 버전에서 작업하고 있어서 그 안의 플러그인이나 security 적용도 맞지 않아서 조금 곤혹스러웠다.

가장 큰 원인은 ***jakarta가 jdk17에서만 사용 가능***하여 그의 하위 버전인 javax로 교체하고 [공식문서](https://docs.spring.io/spring-boot/docs/2.7.13/reference/html/dependency-versions.html#appendix.dependency-versions)에서 호환되는 타임리프 security를 적용하였다.

- Security 를 설정하면 로그인창이 뜨고 로그인해야 우리의 기존 페이지로  접근할 수 있다.   
로그인 인증에 대한 설정은 SecurityConfig 파일을 작성하여 변경한다.

```java
//SecurityConfig.java 전체코드 

@RequiredArgsConstructor
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final MemberSecurityService memberSecurityService;

    public void configure(WebSecurity web) throws Exception {
        // static 디렉터리의 하위 파일 목록은 인증 무시 ( = 항상통과 )
        web.ignoring().antMatchers("/css/**", "/js/**", "/img/**", "/lib/**");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                // 페이지 권한 설정
                .antMatchers("/admin/**").hasRole("ADMIN") //admin 폴더 이하 관리자만
                .antMatchers("/member/**").hasRole("MEMBER") //member 폴더 이하 멤버만
                .antMatchers("/**").permitAll() //그외에 모든 페이지 권한 자유
                .and() // 로그인 설정
                .formLogin()
                .loginPage("/login")
                .successHandler((request, response, authentication) -> {
                    for (GrantedAuthority auth : authentication.getAuthorities()) {
                        if (auth.getAuthority().equals("ROLE_ADMIN")) { //관리자 로그인 시 관리자 페이지로
                            response.sendRedirect("/admin/");
                        } else if (auth.getAuthority().equals("ROLE_MEMBER")) { //멤버 로그인 시 메인화면으로
                            response.sendRedirect("/");
                        }
                    }
                })
                .permitAll()
                .and() // 로그아웃 설정
                .logout()
                .logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
                .logoutSuccessUrl("/")
                .invalidateHttpSession(true)
                .and()
                // 403 예외처리 핸들링
                .exceptionHandling().accessDeniedPage("/error");
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(memberSecurityService).passwordEncoder(passwordEncoder());
    }
}
```


1. WebSecurityConfigurerAdapter 상속받는다.
```java
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final MemberSecurityService memberSecurityService;

    public void configure(WebSecurity web) throws Exception {
        // static 디렉터리의 하위 파일 목록은 인증 무시 ( = 항상통과 )
        web.ignoring().antMatchers("/css/**", "/js/**", "/img/**", "/lib/**");
    }
```
1. configure 메서드를 override 하여 로그인 인증을 나눈다.
```java
    http.authorizeRequests()
                // 페이지 권한 설정
                .antMatchers("/admin/**").hasRole("ADMIN") //admin 폴더 이하 관리자만
                .antMatchers("/member/**").hasRole("MEMBER") //member 폴더 이하 멤버만
                .antMatchers("/**").permitAll() //그외에 모든 페이지 권한 자유
                .and() // 로그인 설정
                .formLogin()
                .loginPage("/login")
```
1. 로그인했을 때 role에 따라 로그인 후 페이지를 설정한다.
```java
.successHandler((request, response, authentication) -> {
                    for (GrantedAuthority auth : authentication.getAuthorities()) {
                        if (auth.getAuthority().equals("ROLE_ADMIN")) { //관리자 로그인 시 관리자 페이지로
                            response.sendRedirect("/admin/");
                        } else if (auth.getAuthority().equals("ROLE_MEMBER")) { //멤버 로그인 시 메인화면으로
                            response.sendRedirect("/");
                        }
                    }
                })
```

## 결과화면
### 처음 화면
<img width="1494" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/54dccb78-7b17-4cc0-9fa9-65dd14be3dbd">

### 로그인 화면
<img width="1494" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/7c21a2a2-02b1-4a78-9968-e581498029cc">

### 회원 화면
- 회원 로그인 하면 보이는 페이지   
바로 메인화면으로 가도록 리다이렉트 된다.

<img width="1494" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/94e88df5-fa1d-4f00-ab3d-0a439ec23a4a">


### 관리자 화면
- 관리자 로그인 하면 보이는 페이지   
관리자 메인화면으로 가도록 리다이렉트 된다.

<img width="1494" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/8dc8a008-6bdc-475b-895a-9569d9669541">
