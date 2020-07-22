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

<br>
<br>

**4.15 스프링 부트**

지금까지 스프링 애플리케이션을 구성하는 여러 가지 방법을 살펴봤습니다. 이제 스프링을 구성할 때 XML, 애너테이션, 자바 구성 클래스, 그루비 스크립트 또는 이 모든 것을 함께 사용하는 방법에 대한 기초 지식을 이해하고 있어야 합니다. 그런데 이들보다 훨씬 근사한 방법이 있다면 어떨까요?

스프링 부트(Spring Boot) 프로젝트는 처음 스프링을 사용해 애플리케이션을 만드는 작업을 간단하게 하려는 데 목적이 있습니다. 스프링 부트는 사용자가 필요한 의존성을 직접 추측해서 모으는 작업을 없애고 측정(metrics)5이나 상태 점검(health check)처럼 대부분 애플리케이션이 필요로 하는 몇 가지 공통 기능을 제공합니다.

스프링 부트는 다양한 유형의 애플리케이션에 맞는 시작 프로젝트를 제공하는데 이들 프로젝트에는 이미 적절한 의존성 및 버전이 포함돼 있습니다. 스프링 부트는 이와 같은 “완고한(opinionated)” 접근법으로 개발자의 노력을 단순화하며 개발 시작에 들어가는 시간을 줄여줍니다. 또한, 스프링 부트는 구성 XML을 완전히 없애려는 개발자를 타겟으로 해, XML 기반 구성이 전혀 필요하지 않습니다.

이번 예제에서는 고전적인 Hello World 웹 애플리케이션을 조금 다르게 만들어보겠습니다. 아마도 일반적인 웹 애플리케이션 초기 설정과 비교해 볼 때 정말 적은 양의 코드만 작성하면 되는 것에 놀라게 될 것입니다. 일반적인 스프링 예제는 먼저 프로젝트에 추가해야 하는 의존성을 정의하는 작업부터 시작했습니다. 스프링 부트가 제공하는 단순화 모델의 한 부분은 개발에 필요한 의존성을 사전에 준비하는 것입니다. 예를 들어 메이븐을 사용하면 개발자는 부모 POM을 사용해 필요한 의존성을 얻을 수 있습니다. 그레이들을 사용하면 이 작업이 더 간단해집니다. 그레이들 플러그인과 starter 의존성을 제외하면 부모 프로젝트도 필요하지 않습니다. 다음 예제에서는 컨텍스트에 있는 모든 빈 목록을 가져온 뒤 helloWorld 빈에 접근합니다. boot-simple 프로젝트의 그레이들 구성은 다음과 같습니다.

* 예제 4-100 스프링 부트 프로젝트 그레이들 빌드 파일([[boot-simple]] build.gradle)

```java
buildscript {
    repositories {
        mavenLocal()
        mavenCentral()
        maven { url "http://repo.spring.io/release" }
        maven { url "http://repo.spring.io/snapshot" }
        maven { url "https://repo.spring.io/libs-snapshot" }
        maven { url "http://repo.spring.io/milestone" }
        maven { url "https://repo.spring.io/libs-milestone" }
    }

    dependencies {
        classpath boot.springBootPlugin
    }
}

apply plugin: 'org.springframework.boot'

dependencies {
    compile boot.starter
}
```

boot.springBootPlugin 줄은 부모 프로젝트의 build.gradle 파일에 정의된 boot 배열의 springBootPlugin 프로퍼티를 참조합니다. 부모 프로젝트의 build.gradle 파일 내용(스프링 부트에만 관련 있는 구성)은 다음과 같습니다.

* 예제 4-101 부모 프로젝트의 build.gradle

```java
ext {
    bootVersion = '2.1.6.RELEASE'
    ...
    boot = [
        springBootPlugin: "org.springframework.boot:spring-boot-gradle-plugin:$bootVersion",
        starter: "org.springframework.boot:spring-boot-starter:$bootVersion",
        starterWeb: "org.springframework.boot:spring-boot-starter-web:$bootVersion
 ]
...
}
```

스프링 부트는 버전별로 지원하는 의존성 관리 목록을 갖고 있습니다. 필요한 라이브러리의 버전이 사전에 선택돼 있어 API가 완벽히 일치하게 되며, 이런 라이브러리와 버전은 스프링 부트가 관리합니다. 그러므로 수작업으로 의존성을 관리할 필요가 없습니다. 스프링 부트를 업그레이드하면 이런 의존성도 같이 업그레이드됩니다. 앞서 살펴본 구성을 사용하면 API 호환이 보장되는 적절한 버전의 의존성이 프로젝트에 추가될 것입니다. IntelliJ IDEA나 이클립스 같은 스마트 편집기에서는 프로젝트에 추가된 의존성 목록을 확인할 수 있습니다. IntelliJ를 사용할 때는 Gradle Projects 뷰에서 각 모듈을 펼쳐 사용 가능한 작업과 의존성 목록을 확인할 수 있습니다.



설정을 마쳤으니 클래스를 생성해 보겠습니다. HelloWorld 클래스는 다음과 같습니다.

* 예제 4-102 HelloWorld 클래스([[boot-simple]] HelloWorld.java)

```java
package com.apress.prospring5.ch4;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Component
public class HelloWorld {

    private static Logger logger = 
        LoggerFactory.getLogger(HelloWorld.class);

    public void sayHi() {
        logger.info("Hello World!");
    }
}
```

이 클래스는 특별하지도 복잡하지도 않으며, 메서드 하나가 정의돼 있고 빈 선언용 애너테이션이 적용돼 있을 뿐입니다. 스프링 부트를 사용해 어떻게 애플리케이션을 만들고 이 빈을 담는 ApplicationContext를 생성하는지 알아보겠습니다.
