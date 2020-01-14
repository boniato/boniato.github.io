##### Kotlin In Action Chapter4
# 클래스, 객체, 인터페이스 


## 1. 클래스와 인터페이스

### 1.1 코틀린 인터페이스

코틀린 인터페이스 안에는 추상 메소드뿐 아니라 구현이 있는 메소드도 정의 할 수 있습니다.


추상 메소드가 있는 인터페이스 선언
```kotlin
interface Clickable {
    fun click()
}
```

Clickable 인터페이스의 click 추상메소드를 구현하는 클래스
```kotlin
class Button : Clickable {
    override fun click() = println("I was clicked")
}
```

위에 코드를 실행해 봅니다.
```kotlin
fun main(args: Array<String>) {
    Button().click() //I was clicked
}
```

상속한 다중 인터페이스 메소드 구현

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

## 2. 생성자와 프로퍼티

## 3. 데이터 클래스

## 4. 클래스 위임

## 5. object 키워드 사용
