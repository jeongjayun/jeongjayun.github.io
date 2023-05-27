---
title: NomadCoders Flutter Challenges Day 03
date: 2023-05-31 19:15:08 +/- TTTT
categories: [STUDY, Flutter]
tags: [dart] # TAG names should always be lowercase
---

# FUNCTIONS

## 3.0 Defining a Function

```dart
void sayHello(String name){
//사람들에게 Hello 하고 인사하는 함수
//사람들의 이름을 받아야됨
    print("Hello $name nice to meet you.");
}
// void : 이 함수는 아무것도 return 하지 않는다.
// void 함수가 return 하게 되면 error 발생함.

String sayHello2(String name){
    return ("Hello $name nice to meet you.");
}
// String은 무조건 String 형태를 가지는 값을 return 해야함.

void main(_{
    print(sayHello('meongge'));
})
```

### fat arrow syntax

```dart
String sayHello2(String name) => return ("Hello $name nice to meet you.");
```

- 위의 sayHello2 처럼 메서드가 특이 내용없이 바로 return 값을 가질 때 fat arrow syntax 형태로 작성할 수 있다.

## 3.1 Named Parameters

- flutter 에서 자주 사용되는 개념

```dart
String sayHello(String name, int age, String country){
    return "Hello $name, you ar $age, and you come from $country";
}

void main(){
    print(sayHello('meongge',10,'korea'));
}
```
'meongge',10,'korea'는 바로 읽었을 때 이해가 가지 않고 잊어버릴 수 있어 좋은 방법은 아님.<br>
-> name argument 를 사용하면 됨.

### Name argument

```dart
String sayHello({String name, int age, String country}){
    return "Hello $name, you ar $age, and you come from $country";
}

void main(){
    print(sayHello(
        age: 10,          // 순서는 중요하지 않다.
        country: 'korea',
        name: 'meongge'
        ));
}
```

1. Name argument를 적용할 함수의 parameter 부분에 {} 를 추가해준다.
1. main에서 함수 호출할 때 순서와 상관 없이 parameter 이름과 값을 적어준다.

>이 때 사용자가 3개의 인수를 다 적지 않으면 중간에 null이 입력 될 수 있음.
{: .prompt-danger}

- 이같은 에러를 예방하는 방법 2가지

* named argument에 default value 정하기
   : 사용자가 인수 쓰는 것을 까먹었을 때 설정된 기본 값을 넣어준다.

```dart
String sayHello({
    String name='anon',
    int age =99,
    String country='wakanda',
}){
    return "Hello $name, you ar $age, and you come from $country";
}
```

* required modifier를 이용하여 필수값 설정하기.

```dart
String sayHello1({
    required String name,
    required int age,
    required String country,
    }) {
  return "Hello $name, you are  $age, and you come from $country";
}
```

## 3.2 Recap

### parameter 2가지 종류

#### positional parameter

```dart
String sayHello(String name, int age, String country){
    return "Hello $name, you ar $age, and you come from $country";
}
void main(){
    print(sayHello('meongge',10,'korea'));
}

```

- 순서가 중요해서 순서를 틀리면 오류가 발생한다. 각각 무엇이었는지 기억해야만 한다.

  > 클린코드(책)에 나중에 봤을 때 이해하기 힘든 코드들은 최소화하라고 적혀있음.

> NICO쌤 : parameter가 2 이하일 때는 괜찮으나 그 이상으로 많아질 경우에는 named argument 를 적용
{: .prompt-tip}

#### named parameter (3.1 다시보기)

## 3.3 Optional Positional Parameters

```dart
String sayHello(String name, int age, String country) => 'Hello $name, you are $age years and you come from $country';

String sayHello2(String name, int age, [String? country = 'cuba']) =>
    'Hello $name, you are $age years and you come from $country';
// []은 optional, positional parameter를 명시할 때 사용
// name, age는 필수값이고 []을 통해 country를 optional 값으로 지정해줄 수 있다.

void main() {
  var results = sayHello2('nico', 12);
  print(results);
}
// country를 입력안했는데 에러가 나지 않음. country 인수가 optional parameter 로 지정이 되었고 굳이 사용자가 입력하지 않아도 기본값 cuba가 입력 되었기 때문.
```

## 3.4 QQ Operator

### ??

- 물음표가 2개

```dart
String capitalizeName(String name)=>name.toUpperCase();
// name을 대문자로 바꾸는 함수

void main(){
    capitalizeName('nico');
}

// 위와 같은 함수가 있다고 하면
// 사용자가 name 대신에 null을 입력하게 되는 경우는 어떡하나.

String capitalizeName(String name){
    if(name !=null){
        return name.toUpperCase();
    }
    return 'ANON';
}
// 처럼 처리해도 괜찮고

String capitalizeName(String? name) =>
name.toUpperCase()??'ANON';
// 이렇게 ??를 사용해서 짧게 표현할 수도 있다.

```

- ?? 연산자를 이용하면 좌항이 null일 때 우항값을 반환,
- 좌항값이 null이 아니라면 좌항값을 반환한다.

### ??=

```dart
void main() {
  String? name;
  name ??= 'nico';
  name = null;
  name ??= 'another';
  print("name : " + name);
}
```

- ??= 연산자를 이용하면 변수 안의 값이 null일 때를 체크해서 값을 할당할 수 있다.

## 3.5 Typedef

```dart
typedef ListOfInts = List<int>;

List<int> reverseListOfNumbers(List<int> list) {
  var reversed = list.reversed;
  return reversed.toList();
}
// Typedef는 자료형에 alias를 붙일 수 있게 해준다.

void main() {
    print(reversedListOfNumbers([1,2,3]));
}
```
## 😂 수업 중 어려운 부분
- [ ] ...

## 🤓 Quiz
- [x] 5/31 Day 3 풀기
- [x] 오답 확인 