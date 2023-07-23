---
title: Team Project) 생명사슬 ~ Life Chain ~ 05
date: 2023-07-18 14:00:00 +/- TTTT
categories: [PROJECT, Life Chain]
tags: [project] # TAG names should always be lowercase
---
## 관리자 회원목록(3)

<img width="541" alt="image" src="https://github.com/jeongjayun/life-chain/assets/116062065/cf3def3f-82ea-48ad-ba5e-d37ef56dc317">

프로필 이미지를 추가하면서 변경된 테이블의 구조는 이렇게 된다. 원래는 member_info 테이블에 profileImage column을 만들려고 했으나 파일명, uuid, 폴더경로 등 이미지 파일을 저장할 때 생각하게 되는 다른 변수들 때문에 테이블을 따로 두어 테이블 별로 용도 및 쓰임을 확실히 하기로 했다.

처음에는 attachImageVO를 BaseTimeEntity 처럼 만들어서 다른 이미지 테이블에서 상속받아서 쓰면 어떨까 생각을 하면서 만들었고 물리적 테이블이 아니라 논리적으로 이미지를 담아놓을 객체로만 정하려고 했었는데 JPA를 통해서 데이터베이스를 짜다보니 사용하지 않는 더미테이블...이 만들어졌다. member_info_image_list 테이블도 본래는 member_info에 imageList 라고 속성을 만들었었는데 테이블로 따로 분리가 되버렸다.

member_info 테이블에 member_img를 List<AttachImageVO> memberImg 처럼 리스트로 넣어서 만들려고 했으나 아래와 같은 에러가 지속되어 다른 방법을 찾던 중 효율없는 방향으로 구현이 되었다. 스프링부트와 JPA가 생소해서 다시 관련 부분 공부하면서 이 부분은 많은 수정이 필요하다.

> No primary or default constructor found for interface, java.util.List
{: .prompt-danger }


### 1. 프로필 이미지 변경
프로필 이미지를 등록할 때 이미지 저장도 중요하지만 변경 할 이미지를 회원정보 수정 페이지에서 바로 볼 수 있도록 Ajax/json 통해 구현했다.

> 코딩가게 구멍단 : 코드로 배우는 스프링 웹 프로젝트 참고
{: .prompt-info}

### 1-1. 프로필 이미지 올리기
#### setting.html 이미지 올릴 부분
```html
<div id="profileImage">
    <div id="noMemberImage" th:if="${memberImg == null}">
        <img
             th:src="@{/assets/img/logo/logo.png}"
             alt="user-avatar"
             class="d-block rounded"
             height="100"
             width="100"
        />
    </div>

    <div id="memberImage" th:if="${memberImg!= null}">
        <img
            th:src="@{Ajax/json 비동기 통신으로 서버에 올라간 이미지 주소 받아올 부분}"
            alt="user-avatar"
            class="d-block rounded"
            height="100"
            width="100"
        />
    </div>
</div>

<div id="uploadResult">
    <!--업로드 이미지-->
</div>

<div class="button-wrapper">
    <!-- label의 for, input의 id, name은 통일시켜줘야 한다 -->
    <label for="uploadfile" class="btn btn-primary me-2 mb-4" tabindex="0">
    <span class="d-none d-sm-block">사진 변경</span>
    <i class="bx bx-upload d-block d-sm-none"></i>
    <input
        type="file"
        id="uploadfile"
        name="uploadfile"
        hidden
        class="account-file-input"
    />
    <!--프로필 이미지만 올릴거니까 multiple X-->
    </label>
</div>
```

- th:if문을 사용해서 등록된 memberImg 가 없으면 생명사슬 로고이미지를, 등록된 memberImg 가 있으면 등록된 이미지를 프로필 이미지로 가져온다.
- 스크립트 코드가 길어져 JS 파일을 따로 분리했다.


#### ImageAjax.js
<details>
<summary>ImageAjax.js 전문 펼치기</summary>

<div markdown="1">

```javascript
// 코드 시작
// CSRF 토큰 가져오기
function getCsrfToken() {
  return $("meta[name='_csrf']").attr("content");
}

// CSRF 헤더 이름 가져오기
function getCsrfHeader() {
  return $("meta[name='_csrf_header']").attr("content");
}

console.log("CSRF 토큰: ", getCsrfToken());
console.log("CSRF 헤더: ", getCsrfHeader());

// Ajax 요청 전에 CSRF 토큰을 설정
$.ajaxSetup({
  beforeSend: function(xhr) {
    xhr.setRequestHeader(getCsrfHeader(), getCsrfToken());
  }
});

/* 이미지 업로드 */
$("input[name='uploadfile']").on("change", function(e) {

  // 이미지 존재시 삭제
		if($(".imgDeleteBtn").length > 0){
			deleteFile();
		}

  // 파일 업로드를 위한 필요한 변수 및 객체 초기화
  let formData = new FormData();
  let fileInput = $(this);
  let fileList = fileInput[0].files;
  let fileObj = fileList[0];

  // 파일 정보 출력
  console.log("fileList: ", fileList);
  console.log("fileObj : ", fileObj);
  console.log("fileName : ", fileObj.name);
  console.log("fileSize : ", fileObj.size);
  console.log("fileType(MimeType) : ", fileObj.type);

  // 파일 유효성 검사
  if(!fileCheck(fileObj.name, fileObj.size)){
    return false;
  }

  alert("통과");

  // FormData에 파일 추가
  for(let i = 0; i < fileList.length; i++){
    formData.append("uploadfile", fileList[i]);
  }

  // 파일 업로드 Ajax 요청
  $.ajax({
    url:'/uploadAjaxAction', // 서버로 보낼 url
    processData:false, // 서버로 전송할 데이터를 queryString 형태로 변환할 지 여부
    contentType: false, // 서버로 전송되는 데이터의 content-type
    data: formData, // 서버로 전송할 데이터
    type:'POST', // 서버 요청 타입(Get Post),
    dataType:'json',
    success : function(result){
	    		console.log(result);
          showUploadImage(result);
	    	},
	  error : function(result){
	    		alert("이미지 파일이 아닙니다.");
	    	}
  });
});

// 파일 업로드 확장자 및 용량 제한
let regex = new RegExp("(.*?)\.(jpg|png)$");
let maxSize = 1048576; // 1MB

function fileCheck(fileName, fileSize){
  if(fileSize >= maxSize){
    alert("파일 사이즈 초과");
    return false;
  }
  if(!regex.test(fileName)){
    alert("해당 종류의 파일은 업로드 할 수 없습니다.");
    return false;
  }
  return true;
}

// 이미지 출력
function showUploadImage(uploadResultArr){
  //전달받은 데이터 검증
  if(!uploadResultArr||uploadResultArr.length==0){return}

  let uploadResult=$("#uploadResult");
  let profileImage = $("#profileImage");

  let obj=uploadResultArr[0];

  let str="";

  let fileCallPath = obj.uploadPath.replace(/\\/g, '/') + "/s_" + obj.uuid + "_" + obj.fileName;
    str += "<div id='result_card'>";
		str += "<img src='/display?fileName=" + fileCallPath +"'>";
    str += "<div class='imgDeleteBtn' data-file='" + fileCallPath + "'></div>";
    str += "<input type='hidden' name='imageList[0].fileName' value='"+ obj.fileName +"'>";
    str += "<input type='hidden' name='imageList[0].uuid' value='"+ obj.uuid +"'>";
    str += "<input type='hidden' name='imageList[0].uploadPath' value='"+ obj.uploadPath +"'>";
    str += "</div>";

  uploadResult.append(str);
  profileImage.hide();

}

// 이미지 삭제 버튼 동작
	$("#uploadResult").on("click", ".imgDeleteBtn", function(e){
    e.preventDefault(); // 이벤트 전파 중지
		deleteFile();
	});


// 파일 삭제 메서드
	function deleteFile(){
		let targetFile = $(".imgDeleteBtn").data("file");	
		let targetDiv = $("#result_card");
    let profileImage = $("#profileImage");

    console.log("삭제 할 targetFile : " + targetFile);

    $.ajax({
			url: '/deleteFile',
			data : {fileName : targetFile},
			dataType : 'text',
			type : 'POST',
			success : function(result){
				console.log(result);
			
				targetDiv.remove();
				$("input[type='file']").val("");

        profileImage.show();
				
			},
			error : function(result){
				console.log(result);
				
				alert("파일을 삭제하지 못하였습니다.")
			}
    });
		
	}

// 전체 코드 끝    
```

</div>

</details>


```javascript
// CSRF 토큰 가져오기
function getCsrfToken() {
  return $("meta[name='_csrf']").attr("content");
}

// CSRF 헤더 이름 가져오기
function getCsrfHeader() {
  return $("meta[name='_csrf_header']").attr("content");
}

console.log("CSRF 토큰: ", getCsrfToken());
console.log("CSRF 헤더: ", getCsrfHeader());

// Ajax 요청 전에 CSRF 토큰을 설정
$.ajaxSetup({
  beforeSend: function(xhr) {
    xhr.setRequestHeader(getCsrfHeader(), getCsrfToken());
  }
});
```

- Ajax, Json 통하여 이미지를 올리려는데 네트워크 403 에러가 나서 찾아보니 Security를 사용하는 경우 토큰 에러가 생길 수 있다고 한다. 우선 검색해서 밑에처럼 적용하여 문제해결 했으나 추가로 공부해야 됨.
    ```html
    // /admin/template/header.html 중 헤더부분에서
        <title>생명사슬 - 관리자페이지</title>

        <meta name="description" content=""/>
        <meta name="_csrf_header" th:content="${_csrf.headerName}">
        <meta name="_csrf" th:content="${_csrf.token}">
        <!--Ajax 403에러 이유 :  csrf(Cross-site request forgery)의 token이 누락-->
    ```
    - 스크립트에서 CSRF 토큰과 헤더를 확인하는 meta name 추가

    ```java
    // SecurityConfig.java
    package com.mysite.sbb.common.config;
     @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                // 기존코드 생략 //
                // 403 예외처리 핸들링
                .exceptionHandling().accessDeniedPage("/error")
                .and()
                .csrf()
                .ignoringAntMatchers("/uploadAjaxAction") // CSRF 보호 제외
    }
    ```
    - Security에서 uploadAjaxAction 403 예외처리 핸들링을 적용


```javascript
/* 이미지 업로드 */
$("input[name='uploadfile']").on("change", function(e) {

  // 파일 업로드를 위한 필요한 변수 및 객체 초기화
  let formData = new FormData();
  let fileInput = $(this);
  let fileList = fileInput[0].files;
  let fileObj = fileList[0];

  // 파일 정보 출력
  console.log("fileList: ", fileList);
  console.log("fileObj : ", fileObj);
  console.log("fileName : ", fileObj.name);
  console.log("fileSize : ", fileObj.size);
  console.log("fileType(MimeType) : ", fileObj.type);

  // 파일 유효성 검사
  if(!fileCheck(fileObj.name, fileObj.size)){
    return false;
  }

  alert("통과");

  // FormData에 파일 추가
  for(let i = 0; i < fileList.length; i++){
    formData.append("uploadfile", fileList[i]);
  }

  // 파일 업로드 확장자 및 용량 제한
let regex = new RegExp("(.*?)\.(jpg|png)$");
let maxSize = 1048576; // 1MB

function fileCheck(fileName, fileSize){
  if(fileSize >= maxSize){
    alert("파일 사이즈 초과");
    return false;
  }
  if(!regex.test(fileName)){
    alert("해당 종류의 파일은 업로드 할 수 없습니다.");
    return false;
  }
  return true;
}
```
- setting.html 에서 '사진변경' 버튼을 눌러 파일을 추가한다.
- 파일 확장자 및 유효성 검사를 진행하고 통과하면 Form Data에 파일을 추가한다.

```javascript
  // 파일 업로드 Ajax 요청
  $.ajax({
    url:'/uploadAjaxAction', // 서버로 보낼 url
    processData:false, // 서버로 전송할 데이터를 queryString 형태로 변환할 지 여부
    contentType: false, // 서버로 전송되는 데이터의 content-type
    data: formData, // 서버로 전송할 데이터
    type:'POST', // 서버 요청 타입(Get Post),
    dataType:'json',
    success : function(result){
	    		console.log(result);
          showUploadImage(result);
	    	},
	  error : function(result){
	    		alert("이미지 파일이 아닙니다.");
	    	}
  });
});

```
- ajax를 통해 /uploadAjaxAction (서버) 로 이동하여 아래 코드 중 PostMapping으로 /uploadAjaxAction 이 지정된 **uploadAjaxActionPOST** 메서드가 실행된다.

#### AjaxController.java
```java
package com.mysite.sbb.common.controller;

@Controller
public class AjaxController {

    private static final Logger logger = LoggerFactory.getLogger(AjaxController.class);

    @PostMapping(value = "/uploadAjaxAction", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    public ResponseEntity<List<AttachImageVO>> uploadAjaxActionPOST(@RequestParam("uploadfile") MultipartFile[] uploadFile) {
        logger.info("uploadAjaxActionPOST .......");

        /* 이미지 파일 체크 */
        for (MultipartFile multipartFile : uploadFile) {
            File checkfile = new File(multipartFile.getOriginalFilename());
            String type = null;

            try {
                type = Files.probeContentType(checkfile.toPath());
                logger.info("MIME TYPE : " + type);
            } catch (IOException e) {
                e.printStackTrace();
            }

            if (!type.startsWith("image")) {
                List<AttachImageVO> list = null;
                return new ResponseEntity<>(list, HttpStatus.BAD_REQUEST);
            }
        }

        String osName = System.getProperty("os.name").toLowerCase();
        String uploadFolder;

        logger.info("접속한 운영체제 : " + osName);

        if (osName.contains("win")) { // Windows
            uploadFolder = "C:\\upload";
        } else if (osName.contains("nix") || osName.contains("nux") || osName.contains("mac")) { // Linux, macOS
            uploadFolder = System.getProperty("user.home") + "/workspace/upload/lifechain/";
        } else {
            throw new UnsupportedOperationException("지원되지 않는 운영체제입니다.");
        }

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date date = new Date();
        String str = sdf.format(date);
        String datePath = str.replace("-", File.separator);

        /* 폴더 생성 */
        File uploadPath = new File(uploadFolder, datePath);

        if (!uploadPath.exists()) {
            uploadPath.mkdirs();
        }

        List<AttachImageVO> list = new ArrayList<>(); //이미지 정보를 담는 객체

        for (MultipartFile multipartFile : uploadFile) {
            logger.info("-----------------------------------------------");
            logger.info("파일 이름 : " + multipartFile.getOriginalFilename());
            logger.info("파일 타입 : " + multipartFile.getContentType());
            logger.info("파일 크기 : " + multipartFile.getSize());

            //이미지 정보 객체
            AttachImageVO vo = new AttachImageVO();

            /* 파일 이름 */
            String uploadFileName = multipartFile.getOriginalFilename();

            vo.setFileName(uploadFileName);
            vo.setUploadPath(datePath);

            /* uuid 적용 파일 이름 */
            String uuid = UUID.randomUUID().toString();
            vo.setUuid(uuid);

            uploadFileName = uuid + "_" + uploadFileName;

            /* 파일 위치, 파일 이름을 합친 File 객체 */
            File saveFile = new File(uploadPath, uploadFileName);

            /* 파일 저장 */
            try {
                multipartFile.transferTo(saveFile);

                File thumbnailFile = new File(uploadPath, "s_" + uploadFileName);
                BufferedImage bo_image = ImageIO.read(saveFile);

                //비율
                double ratio = 3;
                //넓이 높이
                int width = (int) (bo_image.getWidth() / ratio);
                int height = (int) (bo_image.getHeight() / ratio);

                Thumbnails.of(saveFile)
                        .size(width, height)
                        .toFile(thumbnailFile);

            } catch (Exception e) {
                e.printStackTrace();
            }
            list.add(vo);
        } //for문 끝

        ResponseEntity<List<AttachImageVO>> result = new ResponseEntity<List<AttachImageVO>>(list, HttpStatus.OK);
        logger.info("result : " + result);

        return result;
    }
```
- 서버에서도 Ajax 통해 받은 파일이 이미지 파일인지 한번 더 체크한다.
- 아래의 이 부분은 운영체제에 따라서 파일을 저장하는 폴더 위치를 지정한다. 이번 프로젝트도 함께 하는 친구가 윈도우여서 이전 프로젝트에서 작성했던 코드를 그대로 가져와서 재사용했다. 기능적으로 자주 쓰게 되는 이런 코드들은 따로 정리하고 보완 해야겠다.
    ```java
    String osName = System.getProperty("os.name").toLowerCase();
        String uploadFolder;

        logger.info("접속한 운영체제 : " + osName);

        if (osName.contains("win")) { // Windows
            uploadFolder = "C:\\upload";
        } else if (osName.contains("nix") || osName.contains("nux") || osName.contains("mac")) { // Linux, macOS
            uploadFolder = System.getProperty("user.home") + "/workspace/upload/lifechain/";
        } else {
            throw new UnsupportedOperationException("지원되지 않는 운영체제입니다.");
        }
    ```
- 정해진 경로에 파일을 올리는 오늘 날짜를 가져와서 년/월/일로 나누어서 폴더를 생성하고 이름이 중복되지 않도록 uuid를 파일명에 붙여줬다.
- 이미지 저장할 때 섬네일도 필요할까 싶어서 우선 만들었는데 추후에 작업하면서 필요없는 코드는 지워야겠다.
- ResponseEntity로 서버에 올린 이미지를 list로 담아 httpstatus.ok 와 함께 다시 view 로 보내준다.

```java
    @GetMapping("/display")
    public ResponseEntity<byte[]> getImage(String fileName) {
        logger.info("getImage() ......... " + fileName);

        String osName = System.getProperty("os.name").toLowerCase();
        String uploadFolder;

        logger.info("접속한 운영체제 : " + osName);

        if (osName.contains("win")) { // Windows
            uploadFolder = "C:\\upload";
        } else if (osName.contains("nix") || osName.contains("nux") || osName.contains("mac")) { // Linux, macOS
            uploadFolder = System.getProperty("user.home") + "/workspace/upload/lifechain/";
        } else {
            throw new UnsupportedOperationException("지원되지 않는 운영체제입니다.");
        }

        File file = new File(uploadFolder + fileName);

        ResponseEntity<byte[]> result = null;
        try {
            HttpHeaders header = new HttpHeaders();
            header.add("Content-type", URLConnection.guessContentTypeFromName(file.getName()));
            result = new ResponseEntity<>(FileCopyUtils.copyToByteArray(file), header, HttpStatus.OK);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return result;
    }
```
- 절대경로를 노출시키지 않고 상대경로로 이미지를 가져온다.
- local 폴더에 저장한 이미지를 byte[] 형태로 담아서 result 결과값으로 json 통해 ImageAjax.js로 보낸다.

#### ImageAjax.js
```javascript
// 이미지 출력
function showUploadImage(uploadResultArr){
  //전달받은 데이터 검증
  if(!uploadResultArr||uploadResultArr.length==0){return}

  let uploadResult=$("#uploadResult");
  let profileImage = $("#profileImage");

  let obj=uploadResultArr[0];

  let str="";

  let fileCallPath = obj.uploadPath.replace(/\\/g, '/') + "/s_" + obj.uuid + "_" + obj.fileName;
    str += "<div id='result_card'>";
		str += "<img src='/display?fileName=" + fileCallPath +"'>";
    str += "<div class='imgDeleteBtn' data-file='" + fileCallPath + "'></div>";
    str += "<input type='hidden' name='imageList[0].fileName' value='"+ obj.fileName +"'>";
    str += "<input type='hidden' name='imageList[0].uuid' value='"+ obj.uuid +"'>";
    str += "<input type='hidden' name='imageList[0].uploadPath' value='"+ obj.uploadPath +"'>";
    str += "</div>";

  uploadResult.append(str);
  profileImage.hide();

}

```
- uploadResult에 result_card div가 생성하고 기존의 profileImage div를 숨겨 수정 될 이미지만 보이도록 했다.

#### setting.html
```html
<div id="memberImage" th:if="${memberImg!= null}">
    <img
        th:src="@{/display(fileName=${memberImg.uploadPath + '/' + memberImg.uuid + '_' + memberImg.fileName})}"
        alt="user-avatar"
        class="d-block rounded"
        height="100"
        width="100"
     />
</div>
```
- 그리고 스크립트로 만들었던 fileCallPath는 테이블에 저장한 uploadPath, uuid, fileName으로 이미지를 가져왔다.


### 1-2. 프로필 이미지 삭제
프로필 수정 시 이미지를 업로드 했으나 다른 이미지로 변경하게 될 경우, 선택했던 이미지도 local에 저장이 된다. 프로필 이미지를 올렸는데 다른 이미지로 또 바꾸려는 경우 이전의 이미지를 삭제하는 메서드를 작성했다.

#### ImageAjax.js
```javascript
// 이미지 삭제 버튼 동작
	$("#uploadResult").on("click", ".imgDeleteBtn", function(e){
    e.preventDefault(); // 이벤트 전파 중지
		deleteFile();
	});
```
- .imgDeleteBtn 을 누르면 uploadResult의 내용을 삭제한다.

```javascript
// 파일 삭제 메서드
	function deleteFile(){
		let targetFile = $(".imgDeleteBtn").data("file");	
		let targetDiv = $("#result_card");
    let profileImage = $("#profileImage");

    console.log("삭제 할 targetFile : " + targetFile);

    $.ajax({
			url: '/deleteFile',
			data : {fileName : targetFile},
			dataType : 'text',
			type : 'POST',
			success : function(result){
				console.log(result);

				targetDiv.remove();
				$("input[type='file']").val("");

        profileImage.show();
				
			},
			error : function(result){
				console.log(result);
				
				alert("파일을 삭제하지 못하였습니다.")
			}
    });
		
	}
```
- ajax 통해서 Mapping된 deleteFile 로 넘어가 파일 삭제 후 결과를 반환받는다. 이미지 삭제 후 테이블에 저장되어 있는 / 이미지 변경 전의 이미지를 profileImage 가져온다.

#### AjaxController.java
```java
    @PostMapping("/deleteFile")
    public ResponseEntity<String> deleteFile(@RequestParam("fileName") String fileName) {
        logger.info("deleteFile()........" + fileName);
        File file = null;

        try {
            String osName = System.getProperty("os.name").toLowerCase();
            String uploadFolder;

            logger.info("접속한 운영체제 : " + osName);

            if (osName.contains("win")) { // Windows
                uploadFolder = "C:\\upload";
            } else if (osName.contains("nix") || osName.contains("nux") || osName.contains("mac")) { // Linux, macOS
                uploadFolder = System.getProperty("user.home") + "/workspace/upload/lifechain/";
            } else {
                throw new UnsupportedOperationException("지원되지 않는 운영체제입니다.");
            }

            //썸네일 파일 삭제
            file = new File(uploadFolder + URLDecoder.decode(fileName, "UTF-8"));
            file.delete();

            //원본 파일 삭제
            String originFileName = file.getAbsolutePath().replaceFirst("s_", "");
            logger.info("삭제 할 originFileName : " + originFileName);
            file = new File(originFileName);
            file.delete();

        } catch (Exception e) {
            e.printStackTrace();
            return new ResponseEntity<String>("fail", HttpStatus.NOT_IMPLEMENTED);
        }

        return new ResponseEntity<String>("success", HttpStatus.OK);
    }
}

```
- fileName 을 매개변수로 받아서 해당되는 파일을 삭제한다.

### 1-3. DB에 저장

#### AccountService.java
```java
@Transactional
    public void updateMemberInfoEntityByAdmin(MemberDto memberDto, MemberInfoDto memberInfoDto) { //MemberInfo 변경
        Member member = memberRepository.findBymemberId(memberDto.getMemberId())
                .orElseThrow(() -> new DataNotFoundException("Member not found"));
        MemberInfo memberInfo = getMemberInfoByMember(member);

        // AttachImageVO 객체를 MemberImg 엔티티로 변환하여 저장
        if (!memberInfoDto.getImageList().isEmpty()) {
            memberImgRepository.deleteAllByMemberInfo(memberInfo);

            for (AttachImageVO attachImageVO : memberInfoDto.getImageList()) {
                MemberImg memberImg = new MemberImg();
                memberImg.setUploadPath(attachImageVO.getUploadPath());
                memberImg.setUuid(attachImageVO.getUuid());
                memberImg.setFileName(attachImageVO.getFileName());
                memberImg.setMemberInfo(memberInfo);
                memberImgRepository.save(memberImg);
            }
        }

        memberInfo.setZipcode(memberInfoDto.getZipcode());
        memberInfo.setAddress1(memberInfoDto.getAddress1());
        memberInfo.setAddress2(memberInfoDto.getAddress2());
        memberInfo.setMemberIntroduce(memberInfoDto.getMemberIntroduce());
        memberInfo.setMemberTel(memberInfoDto.getMemberTel());

        logger.info("수정한 memberInfo : " + memberInfo);
        member.setMemberInfo(memberInfo);
        memberInfoRepository.save(memberInfo); // memberInfo 저장
    }
```
- AttachImageVO에 담아뒀던 이미지를 memberImg 테이블로 저장하고 그 외의 정보들도 같이 저장한다.


### 2. 최종 로그인 시간 구현

```java

/* Member.java */
private LocalDateTime lastLoginTime; //최종로그인

/* MemberDto.java */
private Date lastLoginDate; //마지막 접속일

```

Member.java 및 MemberDto.java 에 컬럼을 추가한다.

로그인 할 때마다 lastLogin