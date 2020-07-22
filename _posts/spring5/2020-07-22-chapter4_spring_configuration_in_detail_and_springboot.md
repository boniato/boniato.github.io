## chpater4. 스프링 구성 상세와 스프링부트

4.1 스프링이 애플리케이션 이식성에 미치는 영향 <br>
4.2 빈 라이프사이클 관리 <br>
4.3 빈 생성 시점에 통지 받기 <br>
4.4 빈 소멸 시점에 통지 받기 <br>
4.5 빈이 스프링을 알게(Spring Aware)하기 <br>
4.6 FactoryBean 사용하기 <br>
4.7 자바빈 PropertyEditor <br>
4.8 그 외의 스프링 ApplicationContext 구성 살표보기 <br>
4.9 리소스 접근하기 <br>
4.10 자바 클래스를 사용한 구성 <br>
4.11 프로파일 <br>
4.12 Environment와 PropertySource 추상화 <br>
4.13 JSR-330 애너테이션을 사용한 구성 <br>
4.14 그루비를 사용한 구성 <br>
4.15 스프링 부트 <br>
4.16 정리 <br>
<br>


4.3.1 빈 생성시 메서드 실행하기

* 초기화 콜백을 받는 방법 중 하나는 빈의 메서드 하나를 지정해 초기화 콜백으로 사용하겠다고 스프링에 설정하는 것
* 이런 콜백 메커니즘은 같은 타입의 빈이 몇 개 안 되거나 애플리케이션이 스프링과 결합되지 않게 할 때 유용
* 이 메커니즘을 사용하는 또 다른 이유로 스프링 기반 애플리케이션이 이전에 만들어진 빈이나 서드파티 벤더가 제공하는 빈을 사용해야 할 때를 들 수 있음
* 콜백 메서드를 지정하려면 빈을 정의하는 <bean> 태그에 init-method 애트리뷰트로 메서드 이름을 설정함.
  
* 다음은 의존성 두 개가 필요한 간단한 빈의 코드

* 예제 4-1 초기화 콜백 메서드가 정의된 Singer 클래스([[bean-init-method]] Singer.java)
```java
import org.springframework.beans.factory.BeanCreationException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class Singer {
    private static final String DEFAULT_NAME = "Eric Clapton";

    private String name;
    private int age = Integer.MIN_VALUE;

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    private void init() {
        System.out.println("빈 초기화");

        if (name == null) {
            System.out.println("기본 이름 사용");
            name = DEFAULT_NAME;
        }

        if (age == Integer.MIN_VALUE) {
            throw new IllegalArgumentException(
                    Singer.class +" 빈 타입에는 반드시 age 프로퍼티를 설정해야 합니다.");
        }
    }

    public String toString() {
        return "\t이름: " + name + "\n\t나이: " + age;
    }

    public static void main(String... args) {
        GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
        ctx.load("classpath:spring/app-context-xml.xml");
        ctx.refresh();

        getBean("singerOne", ctx);
        getBean("singerTwo", ctx);
        getBean("singerThree", ctx);

        ctx.close();
    }

    public static Singer getBean(String beanName, ApplicationContext ctx) {
        try {
            Singer bean = (Singer) ctx.getBean(beanName);
            System.out.println(bean);
            return bean;
        } catch (BeanCreationException ex) {
            System.out.println("빈 구성 도중 에러 발생: "
                    + ex.getMessage());
            return null;
        }
    }
}
```

* 이 예제에서 콜백 메서드로 작동할 init() 메서드를 정의한 점에 주목
* init() 메서드는 name 프로퍼티가 설정됐는지 확인하고 프로퍼티가 설정되지 않았으면 DEFAULT_NAME 상수에 선언된 값을 기본값으로 설정
* 또한, init() 메서드는 age 프로퍼티가 설정됐는지 점검해 설정되지 않았으면 IllegalArgumentException 예외를 던짐

<br>

* 예제 4-2 Singer 빈 및 초기화 콜백 메서드 구성([[bean-init-method]] app-context-xml.xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd"
    default-lazy-init="true">

    <bean id="singerOne"
        class="com.apress.prospring5.ch4.Singer"
        init-method="init" p:name="John Mayer" p:age="39"/>

    <bean id="singerTwo"
        class="com.apress.prospring5.ch4.Singer"
        init-method="init" p:age="72"/>

    <bean id="singerThree"
        class="com.apress.prospring5.ch4.Singer"
        init-method="init" p:name="John Butler"/>
</beans>
```

