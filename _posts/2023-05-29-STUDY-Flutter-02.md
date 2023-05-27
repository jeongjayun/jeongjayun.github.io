---
title: NomadCoders Flutter Challenges Day 02
date: 2023-05-29 11:30:08 +/- TTTT
categories: [STUDY, Flutter]
tags: [dart]     # TAG names should always be lowercase
---
> 진도표 <https://nomadcoders.co/faq/challenge/schedule-flutter>
{: .prompt-info}

# DATA TYPES
## 2.0 Basic Data Types
### String
```dart
void main(){
    String name = 'meongge';
    String name = "멍게";
    /* '', "" 둘 다 가능. */
}
```

### Bool
```dart
void main(){
    bool alive = true;
    /* bool : ture or false 를 값으로 가짐. */
}
```

### Integer
```dart
void main(){
    int age = 12;
}
```

### Double
```dart
void main(){
    double money = 69.99;
}
```

### Num
* int 와 double 은 num 을 상속받은 클래스다. (int & double의 부모 class)

```dart
void main(){
    num x = 12;
    x = 1.1; /* num은 int일 수도 double일 수도 있다. */
}
```

* x가 int 인지 num 인지 모를 때 num을 쓰면 됨.

### 💡 Dart의 전부가 object로 이루어져 있다.
* 모든 자료형과 function이 _**object**_ 로 이루어져 있음.
* dart가 진정한 객체 지향 언어로 불리는 까닭임.

## 2.1 Lists
* Flutter 에서 자주 쓰임 

### 리스트 만드는 방법 2가지
```dart
void main(){
    List<int> numbers = [1,2,3,4];
    /* List<자료형> 변수명 = [ 리스트에 들어갈 내용들 ]; */
    numbers.add("lalala"); /* 이렇게 안됨!! 오직 int만 삽입가능 */
}
```

```dart
void main(){
    var numbers = [1,2,3,4];
    /* var 변수명 = [ 리스트에 들어갈 내용들 ]; */
    numbers.add("lalalala"); /* 가능 */
    /* 이런 경우에 var 형태의 list를 사용 */
}
```
> VS Code or Dart Pad 사용 시 List를 만들고 끝을 쉼표로 마무리 해주면 자동으로 줄바꿈해줌.<br>
>리스트 구조를 보기 편하다.
{: .prompt-tip}

### collection if, collection for 지원
#### collection if
```dart
void main(){
    var giveMeFive = ture;

    var numbers = [
        1,
        2,
        3,
        4,
        if(giveMeFive) 5,
        /*
        if(giveMeFive){
            numbers.add(5);
        } 와 위의 한줄이 같은 기능
        */
    ];
}
```
위 처럼 giveMeFive 라는 함수가 참이면 리스트에 5를 추가하는 기능.
이 기능을 지원하는 언어가 잘 없다.

## 2.2 String interpolation
* String interpolation = text 에 변수를 추가하는 방법

```dart
void main(){
    var name = 'meongge';
    var greeting = 'Hello everyone, my name is $name. nice to meet you!';

    print(greeting);
}
```

사용할 때 '', "" 둘 다 써도 괜찮다. 상관 없음. $ 달러기호 뒤에 반드시 변수를 사용해주면 된다. <br>
이와 같은 문법은 **변수가 이미 존재 할 때 사용**함.


계산을 실행 할 떄의 문법은 조금 다르다.
```dart
void main() {
  var name = 'meongge';
  var age = 10;
  var greeting =
      "hello everyone, my name is $name, nice to meet you and I\'m ${age + 2}.";
  print(greeting);
```

> 📖 **정리** <br>
> 단순히 변수 값을 담고 싶은 거라면 **$** 달러기호 뒤에 **변수명**. <br>
> 무언가 계산하고 싶다면 **${ /*계산할 내용*/ }**. dart 가 계산해줌.

## 2.3 Collection for
```dart
void main() {
  var oldFriends = ['nico', 'lynn'];
  var newFriends = [
    'lewis',
    'ralph',
    'darren',
    for (var friend in oldFriends) "❤️‍🔥 $friend",
  ];
  print(newFriends);
```
기존에 있던 oldFriends 라는 list 를 friend로 형태를 약간 바꾸어 newFreinds 안의 list에 값을 넣은 것.

UI 인터페이스 만들 때 굉장히 유용한 기능이다 (특히 collection if)

## 2.4 Maps
일반적으로 맵은 key와 value를 연결하는 객체.
키와 값 모두 모든 유형의 객체가 될 수 있음.
각 키는 한 번만 발생하지만 동일한 값을 여러 번 사용할 수 있음.

### 맵을 정의하는 방법 2가지
#### 1) var를 이용한 방법 
```dart
void main() {
  var player = {
    'name': 'meongge',
    'xp': 19.99,
    'superpower': false,
  };
```
* var를 사용할 때에는 자료형을 따로 명시해 줄 필요 없음.
* 컴파일러가 파일형을 정하기 때문.

위의 player는 Type이 Map<String, Object> 이다.<br>
Dart의 모든 것은 object 로부터 생기기 때문에 기본적으로 **어떤 자료형이든지** 될 수 있다. _typescript의  any와 같다._

#### 2) var대신 map과 자료형을 명시하는 방법
dart는 모든 것이 class이므로 map 또한 method, property 를 가진다.
```dart
void main(){
  Map<int, bool> player1 = {
    //key 에는 int값만, values에는 'true or false'만 올 수 있음.
    1: true,
    2: false,
    3: true,
  };

  Map<List<int>, bool> player2 = {
    [1, 2, 3, 4, 5]: true,
  }; //복잡한 형식으로도 쓸 수 있다.

  print(player2);
}
```
굉장히 복잡한 것도 만들 수 있으나 굳이 이렇게 만들 필요는 없음. 

```dart
void main() {
  List<Map<String, Object>> players = [
    {'name': 'nico', 'xp': 199993.999},
    {'name': 'nico', 'xp': 199993.999},
    {'name': 'nico', 'xp': 199993.999},
    {'name': 'nico', 'xp': 199993.999},
  ];
}
```
이런 식으로 Map을 많이 사용하지 않는 것이 좋다.
key와 value로 이루어진 무언가를 정의할 때 class를 사용하는 것을 추천.

## 2.5 Sets
```dart
void mian(){
    Set<int> numbers = {1,2,3,4};
    numbers.add(1);
    numbers.add(1);
    numbers.add(1);
    numbers.add(1);
    numbers.add(1);
    numbers.add(1);

    print(numbers); /* 결과는 {1,2,3,4} 출력됨 */
}
```
* Set에 속한 모든 아이템들은 유니크하다.
* Set = sequence (순서가 있음)

```dart
void main() {
  List<int> numbers = [
    1,
    2,
    3,
    4,
  ];
  numbers.add(1);
  numbers.add(1);

  print(numbers); /* 결과는 [1,2,3,4,1,1] 출력됨*/
}
```
> 📖 **정리** <br>
> Set 일때는 add(1) 하면 원래 있던 값이라 추가되지 않지만 List 일때는 add(1) 하면 add 한 만큼의 1이 추가됨.
> * 요소가 항상 하나씩만 있어야 하면 Set을 사용
> * unique할 필요가 없다면 List 사용

## 😂 수업 중 어려운 부분
- [ ] ...

## 🤓 Quiz
- [x] 5/30 Day 2 풀기
- [x] 오답 확인 