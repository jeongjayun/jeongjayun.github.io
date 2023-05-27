---
title: NomadCoders Flutter Challenges Day 01
date: 2023-05-28 18:40:08 +/- TTTT
categories: [STUDY, Flutter]
tags: [dart]     # TAG names should always be lowercase
---
> 05/29 Flutter Challenges Start <br>
> Flutter 챌린지는 저번에 신청했었는데 정처기 필기랑 겹치면서 떨어졌다... 😅 <br>
> 그 땐 공부하면서 제대로 메모해놓은 게 없어서 이번엔 열심히 수업필기 예정... <br>
> 떨어졌다? 졸업 할 때까지 재도전은 계속된다 🔥🔥🔥


> 진도표 <https://nomadcoders.co/faq/challenge/schedule-flutter>
{: .prompt-info}

## 0.1 Why Dart 🎯
왜 구글이 Dart & Flutter 를 채택했는가? <br>
Dart = UI에 최적화된 언어, 모든 플랫폼에서 빠르기 때문.

### Dart는 2개의 compiler를 가지고 있다.
내가 쓴 Dart 코드를 여러 CPU의 아키텍쳐에 맞게 변환해준다.
> **컴파일러 compiler**
> : 특정 프로그래밍 언어로 쓰여 있는 문서를 다른 프로그래밍 언어로 옮기는 언어 번역 프로그램
{: .prompt-tip }
<img width="833" alt="image" src="https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/b2cce3cf-a32c-4296-bb6d-40619994e194"><br>
1. Dart Web (JS)
1. Dart Native (ARM32, ARM64, x86_64 etc)
    * AOT : ahead-of-time <br>
    ex ) C++로 코딩을 완료했다고 하면  컴파일 할 때 아키텍쳐를 지정해줘야 함. 컴파일을 먼저하고 그 결과인 바이너리를 배포. 컴파일하려면 시간이 많이 걸림 (기계어를 바꿔줘야 하기 때문에). UI를 개발하고 있는 중에는 변경한 내용을 바로 확인해야 하는데 이런 경우는 그렇지 못함. 무언가 변경할 때마다 전체를 컴파일 해야하는 것은 효율이 떨어짐.

    * JIT : just-in-time <br>
    dartVM을 사용하여 쓴 **코드의 결과를 바로 바로 확인** 할 수 있음. 오직 **개발 중일 때만 사용**해야 함. 모든 걸 끝내고 앱을 배포하고 싶으면 dartVM이 아니라 AOT 컴파일러를 사용해야 함.
    
JIT 컴파일러를 사용하여 개발을 하고 배포 시에는 AOT 컴파일러를 사용해야 함.<br>
Dart는 이 2가지를 지원하기 때문에 개발 환경이 수월함. 이렇게 지원하는 언어가 많지 않음.

### null safety
안전한 프로그램을 빌드할 때 아주 중요함.

Java , C++ 등 많은 프로그래밍 언어에서 문제가 발생하는게 개발자가 null 값을 참조해버리면 모든 게 고장나기 때문. Dart 는 null safety를 도입하여 프로그램을 안전하게 만들어 준다.

### flutter는 왜 dart를 택했을까?
1. jit, aot를 지원하기 때문 = 모바일 개발에 아주 좋은 언어.
    - 빠른 피드백을 원하면서도 최종 앱은 컴파일되어 빨라야 함.
1. dart , flutter 를 구글이 만들었기 때문.
    - dart는 flutter 가 필요하면 언어를 할 수 있다.
    - ex) reactjs팀은 좀 더 빠르게 작동하려고 javascript를 수정할 수 없는데 flutter는 필요하면 dart를 수정하는 게 가능.

## 0.2 How To Learn
Dart & Flutter 는 진정한 객체 지향 언어라 class 배우는 데 비중이 크다. Dart, Flutter 는 훨씬 많은 내용을 내포하고 있는데 nico 가 수업시간에 필요한 내용만 간추린 내용을 수업들을 예정임. 필요하면 추가로 더 공부해야함을 잊어서 안됨. 

### dart.dev
사이트 <http://dart.dev> 에서 dart 언어를 익히고 테스트할 수 있다.

### dart & fltter 설치
- [x] homebrew 통해 설치

## 1.0 Hello World
### main 
```dart
void main(){
    print('hello world'); /* 반드시 세미콜론을 붙여줘야 함 */
    /* 실제로 무언가 행동하는 코드는 반드시 main 내부에 넣어줘야 함. */
}
```
main 함수는 모든 Dart 프로그램의 Entry point 이기 때문에 매우 중요.<br>
main 함수에서 내가 쓴 함수가 호출되기 때문에 반드시 main 함수를 작성해야 한다.

#### 세미콜론 
JS나 typesript 같은 다른 프로그래밍 언어에서는 formatter가 자동적으로 세미콜론을 붙여주지만 dart 에서는 그런 기능이 없다. **why? dart에서는 일부러 세미콜론을 안 쓸 때가 있기 때문. (dart의 기능 중 하나)**

세미콜론을 제대로 붙여주지 않으면 프로그램이 컴파일도 되지 않기 때문에 신경써야 한다.

dart가 가진 _**cascade operator**_ 기능 때문에 세미콜론을 꼭 붙여줘야 한다.(세미콜론이 그 기능을 끝내는 역할을 함)

## 1.1 The Var Keyword
### Dart에서 변수를 선언하는 방법
1. 기초적인 방법
    ```dart
    void main(){
        var name = '멍게';
        name = 1; /* 작동안함 */
        name = 'meong-ge'; /* 작동함 */
    }
    ```
    var 라는 키워드와 변수의 이름을 적어주고 그 안에 저장하고 싶은 데이터 넣기.<br>
    변수의 타입에 대해서 지정하지 않았는데 dart의 컴파일러는 name이 String이라는 것을 알고 있다.<br>
    var 는 값을 업데이트 할 수 있는데 값을 업데이트 할 땐 변수의 본래 타입과 일치해야 함.
1. var 키워드를 사용하지 않고 명시적으로 변수의 타입을 지정
    ```dart
    void main(){
        String name = '멍게';
        name = 'meong-ge'; /* 작동함 */
    }
    ```
    명시적으로 변수의 타입을 지정한 경우에도 값을 업데이트 가능하다.<br>
    var와 마찬가지로 본래 타입과 일치해야 함.

#### 언제 var? 언제 변수의 타입을 지정할까?
* 관습적으로 함수나 메서드 내부에 지역 변수를 선언할 때에 var 를 사용하는 것이 dart 스타일가이드의 권장방식임.
* class에서 변수나 property를 선언할 때에는 타입을 지정.

### 💡 변수는 업데이트 가능

## 1.2 Dynamic Type
* dynamic
     : 여러가지 타입을 가질 수 있는 변수에 쓰는 키워드.<br>
     **사용하는 게 추천되지 않지만 때때로 유용하다.**

```dart
 void main() {
    var name; /* var대신 dynamic name; 으로 쓸 수 있음. */
    /* 아무것도 지정해주지 않으면 dynamic 속성을 가진다. */
    name = '우렁이';
    name = 'jeongjayun';
    name = 12;
    name = true;
    print(name);
}
```
이렇게 작성하면 name은 String, int, boolean 등 여러가지 속성을 지닐 수 있게 됨. 

### dynamic이 필요한 이유
1. 변수가 어떤 타입일지 알기 어려운 경우가 있음<br>
    (특히 flutter나 json과 함께 작업하다보면 그렇다.)
1. 가끔은 dynamic 이 유용한 경우가 있음.

```dart
void main(){
    dynamic name;
    name.(적은 메서드)
    /* name이 무슨 타입인지 모르기 때문 */

    if(name is String){
        name.(다양한 메서드)
        /* 이 if절 블럭 내에서는 name이 String이라고 인식하여 다양한 메서드를 사용할 수 있음. */
    }

//// if 블럭 밖에서 name은 dynamic이라 타입을 체크해야 함.

    if(name is int){
        name.(다양한 메서드)
        /* 이 if절 블럭 내에서는 name이 int라고 인식하여 다양한 메서드를 사용할 수 있음. */
    }
}
```

## 1.3 Nullable Variables
### null safety
* 개발자가 null 값을 참조할 수 없도록 하는 기능. (만약 내가 쓴 코드가 null 값을 참조하게 되면 런타임 에러가 발생함. 컴파일 전에 이 에러를 잡아내는 게 좋은데 이를 도와주는 기능임.)

```dart
//without null safety :
bool isEmpty(String string) => string.length == 0;

main(){
    isEmpty(null); /* 이전 버전에서는 에러가 났었음 */
}
```
* null은 부재를 의미 = 아무것도 없음.<br>
    이런 상태도 있어야 함.
* dart에서는 null safety 기능이 있어서 어떤 변수가 null이 될 수 있음을 정확히 표시해야 함.

```dart
void main(){
    String meongge = '멍게';
    meongge = null;
    /* meongge는 string이어야 하기 때문에 이런 건 불가능 */
}
```
* 언어에서 null이란 값을 삭제하는 것은 정답이 아니다.

```dart
/* meongge가 null도, string도 될 수 있다고 하려면  */
void main(){
    String? meongge = '멍게'; /* 변수 뒤에 '?' 붙여주면 됨 */

    if (meongge != null) {
        meongge.isNotEmpty;
    }

    /* 위의 if문으로 쓸 수도 있고 아래처럼 쓸 수도 있음.  */
    meongge?.isNotEmpty;
}
```
**Dart에서 null safety란 어떤 변수, 혹은 데이터가 null이 될 수 있음을 명시하는 기능** <br>
기본적으로 모든 변수는 non-nullable.(null이 될 수 없음.)

### 💡 dart의 변수는 기본적으로 nullable이 아니다.
### 💡 nullable로 만들고 싶다면 꼭 ？를 넣어주기.

## 1.4 Final Variables
* 한 번 정의된 변수를 수정할 수 없게 만들려면 var 대신 _**final**_로 변수를 만들면 된다.
* JS, typescript의 const와  같음.

```dart
void main(){
    final name = 'meongge';

    final String name = 'meongge';
    /* 타입을 추가해서 쓸 수도 있지만 필수가 아님. 컴파일러가 알아서 타입을 추측해서 똑같이 동작해주기 때문에 위에처럼 짧게 써도 됨. */
}
```

## 1.5 Late Variables
* final 이나 var 앞에 붙여줄 수 있는 수식어

```dart
void main(){
    late final String name;
    // do something, go to api
    name = 'meongge';
}
```
* late : 초기값 없이 변수를 선언할 수 있게 해줌.
    : 후에 API 실행하고 나서 API 에서 값을 받아서 name에 값을 지정해줄 수 있음.<br>
    또 final 변수이기 때문에 값을 업데이트 할 수 없음.<br>

```dart
void main(){
    late final String name;
    // do something, go to api
    print(name); /* late 변수로 아직 값이 없어 값을 넣기 전에는 접근하지 않도록 알려줌. */
}
```

=> 변수를 먼저 만들고 값을 나중에 넣기 때문에 실수를 줄여준다.<br>
flutter로 data fetching 할 때 매우 유용함.

> Fetch : 브라우저가 가지고 있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로고침하지 않고 페이지의 일부만을 위한 데이터를 가져오는 기법. (<https://velog.io/@sham/Fetch란-무엇인가>)
{: .prompt-tip}

## 1.6 Constant Variables
### const, 상수
* dart의 const는 javascript, typescript와 다름.
    : 오히려 javascript, typescript의 const는 dart의 final 과 비슷함.

```dart
void main(){
    const name = 'meongge';
    name = '12'; /* 안됨 */
}
```

* dart에서 const : **compile-time constant**를 만듬.

> Complie-time constant : 컴파일 시간에 초기화 값을 확인할 수 있는 상수. <https://heroine-day.tistory.com/60>
{: .prompt-tip}


```dart
void main(){
    const API = fetchApi();
    /* compile-time constant 가 아님 */
    /* 컴파일러는 API 변수의 값을 모름. API에서 받아와야 하는 값임. */
}
```
* const 는 컴파일 할 때 알고 있는 값에 사용하는 것.
    : 앱스토어에 앱을 올리기 전에 알고 있는 값.

* ‼️ 어떤 값인지 모르고, 그 값이 API 에서 온다거나 사용자가 화면에서 입력해야 하는 값은 final or var 가되어야 함.

## 1.7 Recap
### final
* 값을 재할당하지 못하는 변수를 만듦.

### dynamic type
* 어떤 타입의 데이터가 들어올 지 모를 때 사용.

### const와 final 차이
* const : 수정할 수 없음. 코드를 컴파일 하기 전에 값을 갖고 있어야 함. 
* final : 런타임 중에 만들어질 수 있음. 사용자가 앱을 사용하면서 변수를 만들 수 있다는 점.

### null safety
* 기본적으로 dart의 언어는 nullable 이 아니다.

### late
* final, var, String 같은 것들 앞에 써줄 수 있는 수식어.
* 어떤 데이터가 올 지 모를 때 사용.

## 😂 수업 중 어려운 부분
- [ ] dart 에서의 const 의미를 잘 모르겠음 ...

## 🤓 Quiz
- [x] 5/29 Day 1 풀기
- [x] 오답 확인 <br>
    * Dart can be compiled into ARM64 and JavaScript.<br>
        : 다트는 ARM64와 JS로 컴파일할 수 있다 ... 있지 ... <br>
    * 영어로 문제를 보니까 또 헷갈린다 😱 <br>
