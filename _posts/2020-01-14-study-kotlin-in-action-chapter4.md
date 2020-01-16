##### Kotlin In Action Chapter4
# 클래스, 객체, 인터페이스 


## 1. 클래스와 인터페이스

### 1.1 클래스

코틀린에서 클래서는는 기본적으로 **final**이며 **public**이다.<br>
조슈아 블로그(Joshua Block)가 쓴 'Effective Java'에서 "*상속을 위한 설계와 문서를 갖추거나, 그럴 수 없다면 상속을 금지하라* "라는 철학을 따른다. 이는 특별히 하위 클래스에서 오버라이드하게 의도된 클래스와 메소드가 아니라면 모두 final로 만들라는 뜻이다. 클래스를 상속하는 방법에 대해 정확한 규칙을 제공하지 않는다면, 상속을 받는 클래스에서 기존 의도와 다른 방식으로 메소드를 오바라이드할 위험이 있기 때문이다.<br>

### 1.2 인터페이스
코틀린 인터페이스는 클래스 이름 뒤에 콜론(**:**)을 붙이고 인터페이스와 클래스 이름을 적는 것으로 클래스 확장과 인터페이스를 구현한다.<br>
코틀린 인터페이스 안에는 추상 메소드뿐 아니라 구현이 있는 메소드도 정의 할 수 있다. 다만 인터페이스에는 아무런 상태(필드)도 들어갈 수 없다.
<br>

ㆍ추상 메소드가 있는 인터페이스 선언

```kotlin
interface Clickable {
    fun click()
}
```
<br>

ㆍClickable 인터페이스의 click 추상메소드를 구현하는 클래스
```kotlin
class Button : Clickable {
    override fun click() = println("I was clicked")
}
```
<br>

ㆍ위의 코드 실행
```kotlin
fun main(args: Array<String>) {
    Button().click() //I was clicked
}
```
<br>

ㆍ상속한 다중 인터페이스 메소드 구현

```kotlin
interface Clickable {
    fun click()
    fun showOff() = println("I'm clickable!")
}
 
interface Focusable {
    fun setFocus(b: Boolean) = println("I ${if (b) "got" else "lost"} focus.")
 
    fun showOff() = println("I'm focusable!")
}

class Button : Clickable, Focusable {
    override fun click() = println("I was clicked")
    override fun showOff() {
        super<Clickable>.showOff()
        super<Focusable>.showOff()
    }
}

fun main(args: Array<String>) {
    var button = Button()
    button.click() // I was clicked
    button.setFocus(true) // I got focus.
    button.showOff() // I'm clickable!  I'm focusable!
}
```
<br>

## 2. 생성자와 프로퍼티

## 3. 데이터 클래스

## 4. 클래스 위임

## 5. object 키워드 사용
