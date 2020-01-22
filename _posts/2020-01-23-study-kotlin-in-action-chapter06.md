##### Kotlin In Action Chapter5
# 람다로 프로그래밍

## 1. 람다 식

* **람다 식(lambda expression)** 또는 **람다**는 기본적으로 다른 함수에 넘길 수 있는 작은 코드 조각을 뜻한다.
* 람다는 값처럼 여기저기 전달할 수 있는 동작의 모음이다.
<br>

### 1.1 람다 소개: 코드 블록을 함수 인자로 넘기기
람다 식을 사용하면 함수를 선언할 필요가 없고 코드 블록을 직접 함수의 인자로 전달할 수 있다.
<br>

ㆍ익명 클래스로 리스너 구현하기
```java
/* 자바 */
button.setOnClickListener(new onClickListener() {
    @Override
    public void onClick(View view) {
        /* 클릭 시 수행할 동작 */
    }
})
```
<br>

ㆍ**람다**로 리스너 구현하기
```kotlin
button.setOnClickListener { /* 클릭 시 수행할 동작 */ }
```

### 1.2 람다와 컬렉션
코틀린에서는 컬렉션 라이브러리 함수를 제공하기 때문에 필요한 컬렉션 기능을 직접 작성해야되는 불편함을 덜어준다.

ㆍ람다를 사용해 컬렉션 검색하기

```kotlin
val people = listOf(Person("Alice", 29), Person("Bob", 31))
println(people.maxBy { it.age }) /* 나이 프로퍼티를 비교해서 값이 가장 큰 원소 찾기 */
// Person(name=Bob, age=31)
```

ㆍ위 코드를 **멤버 참조**를 사용해 컬렉션을 검색할 수 있다.
```kotlin
people.maxBy(Person::age)
```

### 1.3 람다 식의 문법
이미 말했지만 람다는 값처럼 여기저기 전달할 수 있는 동작의 모음이다. 함수에 인자로 넘기면서 람다를 정의하는 경우가 대부분이다.
<br>
ㆍ람다 식을 선언하기 위한 문법
```kotlin
 { x: Int, y: Int  ->  x + y }
   --------------      -----
      파라미터          본문
```
* 람다는 항상 중괄호 사이에 위치한다.
* 인자 목록 주변에 괄호가 없다.
* 화살표(->)가 인자 목록과 람다 본문을 구분해준다.
* 람다 식을 변수에 저장할 수 있다.
* 람다가 저장된 변수를 다른 일반 함수와 마찬가지로 다룰 수 있다.
```kotlin
val sum = { x: Int, y: Int -> x + y }
print(sun(1, 2))
// result : 3
```
