## chpater3. 스프링 IoC와 DI

**3.1 IoC와 DI <br>
3.2 IoC의 종류 <br>
3.3 스프링의 제어 역전 <br>
3.4 스프링의 의존성 주입 <br>
3.5 애플리케이션 컨텍스트 구성하기 <br>
3.6 빈에 자동와이어링하기 <br>
3.7 빈 상속 설정하기** <br>
<br>

* 살펴보기
  * 스프링 깃허브 저장소 주소 : https://github.com/spring-projects/spring-framework
  * **스냅샷(snapshot) 빌드**란 아직 정식 버전이 공개되지 않은 개발 버전의 마지막 버전을 나타냄.<br>
    만약 1.0-SNAPSHOT과 같이 버전이 붙은 파일이 있다면 이는 1.0 정식 배포 전 해당 라이브러리를 개발 중임을 나타냅니다. 또한, 스냅샷은 받는 시점에 따라 파일에 담긴 내용이 바뀔 수 있음.
  * 스프링 5.1이 공식 지원하는 자바 버전은 자바 11이며, 최소 자바 8이 필요합니다.
  * 후보 컴포넌트 인덱스 기능은 스프링 5.0에 처음 도입된 기능으로, 이를 통해 클래스패스 기반 컴포넌ㄴ트 스캔을 대체할 수 있음. 이를 사용하면 무거운 애플리케이션의 기동 시간 개선에 큰 효과가 있음.
  * 전이 의존성(transitive dependency)이란 개발 중인 애플리케이션이 A 라이브버리를 필요로 하고 A 라이브러리가 내부적으로 B 라이브러리를 필요로 하는 경우, 애플리케이션이 B 라이브러로도 필요로 한다고 간주하는 것.
  * **빈(bean)은 스프링에서 클래스의 인스턴스를 의미**함.
  * 이클립스는 인텔리제이 IDEA와 다르게 의존성을 트리 형태로 보여주지 않음. 그러나 빌드 구성 파일에 지정하지 않은 aop, beans 등의 모듈이 있는 것을 보면 그레이들이 전이 의존성을 통해 필요한 모든 라이브러리를 받았음을 알 수 있음

* IoC와 DI의 주 목적 : **컴포넌트의 의존성을 제공**하고 **이러한 의존성을 라이프사이클 전반에 걸쳐 관리**하는 보다 간편한 메커니즘을 제공하는 것.

* IoC(Inversion of Control, 제어의 역행) 두 가지 하위분류로 나눌 수 있음
  * 의존성 주입(Dependency Injection, DI)
  * 의존성 룩업(Dependency Lookup, DL)
    * 이들은 다시 구체적인 IoC 구현체로 나늼.

* IoC 종류
  * **의존성 주입(DI)** : 이 방식의 IoC에서는 IoC **컨테이너가 컴포넌트에 의존성을 주입**합니다.<br>
    * 덧붙여 설명하자면..
    * Martin Fowler가 제어의 어떠한 부분이 반전되는가라는 질문에 '의존 관계 주입'이라는 용어를 사용하였다고 함.
    * 의존성 주입(DI)은 프로그래밍에서 구성요소간의 의존 관계가 소스코드 내부가 아닌 **외부의 설정파일 등을 통해 정의**되게 하는 디자인 패턴 중 하나.
      * 여기서 외부 설정파일은 스프링 설정파일을 의미하며, **스프링 설정파일을 통하여 객체간의 의존관계를 설정**
      * 스프링 컨테이너(Spring Container)가 제공하는 API를 이용해 객체를 사용.
    * 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것.
      * 생성자(constructor) 의존성 주입
      * 수정자(setter) 의존성 주입
      * 메소드(Method Injection) 의존성 주입(책에서는 없는 내용이지만 추가함.)
  * **의존성 룩업(DL)** : 이 방식의 IoC에서는 **컴포넌트 스스로 의존성의 참조를 가져와야** 합니다.
    * 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것.
      * 의존성 풀(Dependency Pull)
      * 의존성 룩업(CDL)

* **Spring DI 컨테이너** (책에 없는 내용)
  * **빈(Bean)** : Spring DI 컨테이너가 관리하는 객체
  * **빈 팩토리(Bean Factory)** : 빈(Bean)들을 관리하는 컨테이너. Bean(객체) 등록, 생성, 조회, 반환 관리. 빈 객체간의 의존관계 설정해주는 기능 제공.
  * **어플리케이션 컨텍스트(Application Context)** : 빈 팩토리(Bean Factory)에 여러가지 컨테이너 기능을 추가한 것(추가 기능: AOP, 메시지 지원, 국제화 지원 등). 보통은 BeanFactory를 바로 사용하지 않고, 이를 확장한 Application Context를 사용.
  * **스프링 컨테이너(Spring Container)** : 설정파일에 설정된 내용을 읽어 Application에서 필요한 기능들을 제공. 객체를 관리.
  * **스프링 설정파일** : Spring Container가 어떻게 일할지를 설정하는 파일. XML 기반으로 작성. RootTag는 <beans>임.
    * ex.) applicationContext.xml
```xml
        <?xml version="1.0" encoding="UTF-8"?>

        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans 
                http://www.springframework.org/schema/beans/spring-beans.xsd">

            <bean id="oracle" name="wiseworm" class="com.apress.prospring5.ch3.BookwormOracle"/>
        </beans>
```

    * 여기서 <bean> 태그는 스프링 컨테이너가 관리할 Bean객체를 설정하는 것임.
  
* 3.2.1 **의존성 풀(Dependency Pull)**
  * 의존성 풀에서는 필요에 따라 레지스트리에서 의존성을 가져오게 됩니다.<br>
    ex) JNDI API를 사용한 EJB 컴포넌트 룩업<br>
  * JNDI(Java Naming and Directory Interface)는
    * 디렉터리 서비스에서 제공하는 데이터 및 객체를 발견(discover)하고 참고(lookup)하기 위한 자바 API
    * 서버에서 관리하고 있는 리소스에 대한 정보를 알고 있고 특정 리소스를 찾아서 사용할 수있도록 객체를 반환
          
```java
         // lookup 소스..

         public class DatabaseManager
         {
            public static Connection getConnection() throws Exception
            {
               Context ctx = new InitialContext();
               DataSource ds = (DataSource) ctx.lookup("java:comp/env/jdbc/myoracle");
               return ds.getConnection();
            }
         }
```

  * 스프링 프레임워크도 자신이 관리하는 컴포넌트를 가져오는 메커니증 중 하나로서 의존성 풀을 제공
  * 의존성 풀 사용 예
 
```java

package com.apress.prospring5.ch3;

import com.apress.prospring5.ch2.decoupled.MessageRenderer;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class DependencyPull {
    public static void main(String... args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("spring/app-context.xml");

        MessageRenderer mr = ctx.getBean("renderer", MessageRenderer.class);
        mr.render();
    }
}
```


* 3.2.2 문맥에 따른 **의존성 룩업(CDL)**
  * 의존성 풀처럼 특정 중앙 레지스트리에서 의존성을 가져오는 것이 아니라 **자원을 관리하는 컨테이너**에서 의존성을 가져옴. <- 풀, 룩업 차이점!
  * 늘 수행되는 것이 아니라 몇 사기 정해진 시점에 수행
  * 다음은 문맥에 따른 의존성 룩업 메커니즘
    * 의존 객체 -> 룩업 -> 컨테이너 
  *  컨테이너에서 의존성을 가져오는 컴포넌트
  
```java
public class ContextualizedDependencyLookup implements ManagedComponent {

    private Dependency dependency;

    // 1)의존성 룩업을 수행하는 메소드
    
    @Override
    public void performLookup(Container container) {
        this.dependency = (Dependency) container.getDependency("myDependency");
    }

    @Override
    public String toString() {
        return dependency.toString();
    }
}
```

3.2.3 생성자 의존성 주입
* 컴포넌트의 생성자(또는 여러 생성자)를 이용해서 해당 컴포넌트가 필요로하는 의존성을 제공하는 방식.
* 어떤 컴포넌트가 의존성을 인수로 가져오도록 생성자 또는 여러 생성자를 선언한다면 IoC 컨테이너는 해당 컴포넌트를 초기화할 때 컴포넌트에 필요한 의존성을 전달함.
* 주의점 : 생성자 주입을 사용할 때 의존성 주입 없이는 빈을 생성할 수 없으므로 **반드시 의존성을 주입**해야함!
* 생성자 의존성 주입 예제 코드

```java
public class ConstructorInjection {
	private Dependency dependency;

	public ConstructorInjection(Dependency dependency) {
		this.dependency = dependency;
	}

	@Override
	public String toString() {
		return dependency.toString();
	}
}
```

3.2.4 수정자 의존성 주입
 * 수정자(setter) 의존성 주입 방식에서 IoC 컨테이너는 자바빈 방식의 **수정자 메서드**를 이용해 컴포넌트의 의존성을 주입.
 * 컴포넌트의 수정자는 **IoC 컨테이너가 관리할 수 있도록 의존성을 노출**함.
 * 수정자 주입을 사용할 때 명확한 것은 의존성 없이도 객체를 생성할 수 있으며 해당 수정자를 호출해 의존성을 나중에 제공할 수 있음.
 * 의존성 주입이 필요한 일반적인 컴포넌트 예제

```java
public class SetterInjection {

	private Dependency dependency;

	public void setDependency(Dependency dependency) {
		this.dependency = dependency;
	}

	@Override
	public String toString() {
		return dependency.toString();
	}
}
```

3.2.5 **의존성 주입** vs. 의존성 룩업
* 현재 사용하는 컨테이너에 따라 IoC의 방식이 정해짐
* ex) EJB 2.1 이하 버전을 사용할 때 EJB를 JEE 컨테이너에서 가져오려면 의존성 룩업(JNDI를 통한) 방식의 IoC를 사용해야 함.
      스프링에서는 초기 빈 룩업을 제외하면 컴포넌트와 의존성은 항상 의존성 주입 방식의 IoC를 이용해 연결됨.
* 주입 방식을 선택하는 가장 큰 이유는 의존성 주입을 사용하면 그만큼 개발이 쉬워지기 때문. 작성해야 하는 코드량 줄이고 코드도 간결. IDE를 이용해 자동화도 가능.
* **주입 코드**는 **객체가 필드에만 저장**됨. 저장소나 컨테이너에서 의존성을 가져오는 코드가 전혀 필요하지 않음. 따라서 코드는 훨씬 단순해지고 에러도 줄어듬.

3.2.6 **수정자 주입** vs. **생성자 주입**
* **생성자 주입(constructor injection)**
  * **컴포넌트 사용 전에 해당 컴포넌트의 의존성을 반드시 갖고 있어야 할 때 매우 유용**. 의존성 점검 메커니즘을 제공하는지와 상관없이 의존성에 대한 요구사항 지정 가능.
  * 빈 객체를 불변 객체로 사용 가능.
* **수정자 주입(setter injection)**
  * 기존 의존성을 제공할 때 일반적으로 **수정자 주입이 의존성 주입에 가장 좋은 방법**
  * 인터페이스에서 모든 의존성을 선언할 수 있음
  
* 의존성 주입 메소드를 비즈니스 인터페이스에서 정의하지 않고, 비즈니스 인터페이스를 구현하는 클래스에서 이 메소드를 정의!

```java
public interface Oracle {
    String defineMeaningOfLife();
}
```

```java
public class BookwormOracle implements Oracle {
    private Encyclopedia encyclopedia;

    public void setEncyclopedia(Encyclopedia encyclopedia) {
        this.encyclopedia = encyclopedia;
    }

    @Override
    public String defineMeaningOfLife() {
        return "Encyclopedias are a waste of money -  go see the world instead";
    }
}
```

* BookwormOracle 클래스는 Oracle 인터페이스를 구현했을 뿐만 아니라 의존성 주입을 위한 수정자도 정의
* 스프링은 이와 같은 구조를 다루는데 훨씬 더 편리
* 여기서 비즈니스 인터페이스에서 의존성을 정의할 필요없음
* 의존성을 정의하는 인터페이스를 사용할 수 있다는 것이 수정자 주입의 잘 알려진 장점!
  * 하지만 실제로는 주입을 위한 수정자가 사용자 인터페이스의 외부에 존재하도록 노력해야함
  
**3.3 스프링 제어 역전**
* 제어 역전(Inversion of Control, IoC)은 스프링이 하는 일 중에 가장 큰 부분을 차지!
* 의존성 룩업 기능이 제공되지만, **스프링 구현의 핵심은 의존성 주입**임!
  * 스프링의 의존성 주입 메커니증
    [의존 객체] <------의존성 주입------- [스프링 BeanFactory 컨테이너]

3.4 **빈(Bean)과 빈 팩터리(Bean Factory)**
**스프링의 의존성 주입 컨테이너의 핵심**은 **빈 백터리 인터페이스** 입니다.
* **빈 팩터리(Bean Factory)**
  * 컴포넌트 관리
  * 컴포넌트의 라이프사이클뿐만 아니라 의존성까지 관리

3.4.2 **BeanFactory 구현체**
* 독립 실행형 자바 어플리케이션에서 원하는 처리를 하기 위해 스프링의 BeanFactory를 초기화하고 oracle 빈을 가져오는 방법 코드

```java
public class XmlConfigWithBeanFactory {

	public static void main(String... args) {
	        // BeanFactory 구현체 중 하나인 DefaultListableBeanFactory 사용
		DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
		
		// XmlBeanDefinitionReader를 사용해 XML 파일의 BeanDefinition 정보 읽음
		XmlBeanDefinitionReader rdr = new XmlBeanDefinitionReader(factory);
		
		rdr.loadBeanDefinitions(new ClassPathResource("spring/xml-bean-factory-config.xml"));
		
		// oracle 빈을 가져오기
		Oracle oracle = (Oracle) factory.getBean("oracle");
		
		System.out.println(oracle.defineMeaningOfLife());
	}
}
```

* xml-bean-factory-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
    
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="oracle" name="wiseworm" class="com.apress.prospring5.ch3.BookwormOracle"/>
</beans>
```

3.4.3 **애플리케이션 컨텍스트(ApplicationContext)**
* 스프링의 애플리케이션 컨텍스트 인터페이스는 **BeanFactory를 상속한 인터페이스**
* DI 서비스 외에도 트랜잭션 서비스, AOP 서비스, 국제화(il8n)를 위한 메시지 소스, 애플리케이션 이벤트 처리와 같은 여러 서비스 제공
* 스프링 기반 애플리케이션 개발 할 때 **ApplicationContext 인터페이스**를 이용해 스프링을 사용하는 것을 권장!

* 스프링은 ApplicationContext를 
  * **직접 코드**로 **부트스트랩**(직접 인스턴스를 생성하고 적절한 애플리케이션 구성을 불러오는 방식)하거나
  * 웹 컨테이너 환경에서 **ContextLoaderListener를 이용**해 부트스트랩 합니다.

<br>

* BeanFactory <<'interface>> <br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ↑ 
* ApplicationContext <<'interface>> <br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ↑ 
* WebApplicationContext <<'interface>>

<br>

* **빈(bean) 객체 주입 받기** (책에 없는 내용)
  * 설정파일 설정
    * <bean> : 스프링 컨테이너가 관리할 bean 객체를 설정
  
```xml
        <?xml version="1.0" encoding="UTF-8"?>

        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans 
                http://www.springframework.org/schema/beans/spring-beans.xsd">

            <bean id="dao" class="com.spring5.ch3.model.MemberDAO"/>
        </beans>
```

  * 설정 파일에 설정한 내용을 바탕으로 Spring API를 통해 객체를 주입 받는다.
    * 설정파일이 어디 있는지 설정
    * 객체를 만들어 주는 (Assembler) 객체 생성
    
```java
    public static void  main(String[] args) {
      //설정파일이 어디 있는지를 저장하는 객체
      Resource resource = new ClassPathResource("applicationContext.xml");

      //객체를 생성해주는 factory 객체
      BeanFactory factory = new XmlBeanFactory(resource);

      //설정파일에 설정한 <bean> 태그의 id/name을 통해 객체를 받아온다.
      MemberDAO dao = (MemberDAO)factory.getBean("dao");
    }
```

3.5.1 스프링 구성 옵션 설정하기
* 스프링에서 애플리케이션 구성을 정의할 수 있는 옵션
  * 빈(bean) 정의 : 프로퍼티 or XML 파일을 사용해 빈 정의할 수 있도록 지원
  * JDK 5가 출시되고, 스프링이 자바 애너테이션 지원하면서 스프링 2.5부터는 ApplicationContext를 구성하는데 자바 애너테이션을 지원하기 시작!
  * XML과 애너테이션 방식 중 어느것이 더 나을까? -> 각 장단점이 있기에 정답 없음.
    * XML 파일을 사용하면 모든 구성을 자바에서 분리해 외부에서 관리
    * 애너테이션을 사용하면 개발자가 코드 내에서 DI 구성을 정의하고 확인 가능
  * 일반적인 접근 중 하나는
    * XML 파일에 애플리케이션의 인프라(예를 들어 데이터 소스, 트랜잭션 관리자, JMS 연결 팩터리(Connection Factory), JMX 등) 정의하고
    * 애너테이션으로 DI 구성(주입 가능 빈과 빈의 의존성)을 정의하는 방식
  => 어떤 옵션을 선택하는지, 선택한 방법을 일관성 있게 준수하고 전체 개발팀이 분명히 이해할 수 있게 메시지를 명확히 전달
  
  
3.5.2 기본 구성의 개요
* **XML 구성을 사용**하려면 애플리케이션에서 필요한 **스프링 네임스페이스 베이스**를 선언해야함


* 스프링 빈 정의에 사용하는 네임스페이스 선언의 예 (app-context-xml.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="renderer"
        class="com.apress.prospring5.ch3.annotated.StandardOutMessageRenderer"
        p:messageProvider-ref="provider"/>

    <bean id="provider"
        class="com.apress.prospring5.ch3.annotated.HelloWorldMessageProvider"/>
</beans>
```

* **컴포넌트 스캔(component-scan)**
  * 지정한 패키지의 모든 하위 패키지에 있는 클래스에 애너테이션이 선언된 클래스를 찾아(스캔) 자동으로 bean을 생성

* **컴포넌트 스캐닝 설정**을 한 XML 구성 파일 (app-context-annotation.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context 
          http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan 
          base-package="com.apress.prospring5.ch3.annotated"/>
</beans>
```

* **<context:component-scan> 태그**는
  * **지정한 패키지의 모든 하위 패키지에 있는 클래스에 선언된 @Autowired, @Inject, @Resource 애너테이션 뿐만 아니라, **
  * **@Component, @Controller, @Repository, @Service 애너테이션이 선언된, 의존성 주입이 가능한 빈의 코드를 스캔하도록 스프링에게 지시**함!!
  * 여러 패키지 정의 가능 : <context:component-scan> 태그에서 쉼표, 세미콜론, 공백을 구분 기호로 사용해 여러 패키지를 정의할 수 있음
  * 스캔 범위 제어 가능 : 이 태그에서는 컴포넌트 스캔의 범위와 제외 범위를 지정해 스캔 범위 제어 가능

  
  * 스캔에서 제외할 대상(클래스 또는 인터페이스) 예

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context 
          http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan 
		base-package="com.apress.prospring5.ch3.annotated"/>
	  <context:exclude-filter type="assignable" expression="com.example.NotAService"/>
    </context:component-scan>

</beans>
```

<br>

* 컴포넌트 스캔(component-scan) 다른 예제
* 예제1) 설정파일로 bean 객체 주입하는 예제

```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans 
          http://www.springframework.org/schema/beans/spring-beans.xsd">

      <bean id="memberDAO" class="com.spring5.ch3.model.MemberDAO"/>
      <bean id="sellerDAO" class="com.spring5.ch3.model.SellerDAO"/>
  </beans>
```

* 예제2) 예제1)을 컴포넌트 스캔(component-scan)으로 변경

```xml
  <?xml version="1.0" encoding="UTF-8"?>

  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans 
          http://www.springframework.org/schema/beans/spring-beans.xsd"
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">

      <context:component-scan base-package="com.spring5.ch3.model"></context:component-scan>
  </beans>
```

* 위의 설정파일 java 예제

```java
  public static void  main(String[] args) {
    //설정파일이 어디 있는지를 저장하는 객체
    Resource resource = new ClassPathResource("applicationContext.xml");

    //객체를 생성해주는 factory 객체
    BeanFactory factory = new XmlBeanFactory(resource);

    //설정파일에 설정한 <bean> 태그의 id/name을 통해 객체를 받아온다.
    MemberDAO memberDAO = (MemberDAO)factory.getBean("memberDao");      
    SellerDAO sellerDAO = (SellerDAO)factory.getBean("sellerDAO");
  }
```

<br>

3.5.3 스프링 컴포넌트 선언하기
* 스프링에게 이 빈(개발한 클래스)이 다른 빈에 주입될 수 있다는 것을 알려주고, 스프링이 이 빈들을 관리하게 해야 함.

1) xml 설정 
* MessageRenderer가 렌더링할 메시지를 제공하는 MessageProvider에 의존

```java
public interface MessageRenderer {
	void render();
	void setMessageProvider(MessageProvider provider);
	MessageProvider getMessageProvider();
}

// renderered 구현체
public class StandardOutMessageRenderer implements MessageRenderer {

    private MessageProvider messageProvider = null;

    public void render() {
        if (messageProvider == null) {
            throw new RuntimeException(
                    "You must set the property messageProvider of class:"
                            + StandardOutMessageRenderer.class.getName());
        }

        System.out.println(messageProvider.getMessage());
    }

    public void setMessageProvider(MessageProvider provider) {
        this.messageProvider = provider;
    }

    public MessageProvider getMessageProvider() {
        return this.messageProvider;
    }

}

// provider 인터페이스
public interface MessageProvider {
    String getMessage();
}


// provider 구현체
public class HelloWorldMessageProvider implements MessageProvider {

@Override
    public String getMessage() {
        return "Hello World!";
    }
}
```


app-context-xml.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="htt
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="renderer"
        class="com.apress.prospring5.ch2.decoupled.StandardOutMessageRenderer"
        p:messageProvider-ref="provider"/>

    <bean id="provider"
        class="com.apress.prospring5.ch2.decoupled.HelloWorldMessageProvider"/>
</beans>
```


2) 애너테이션 설정 예
* **애너테이션을 사용해 빈 정의를 생성하는 클스** 예

```java
//간단한 빈
@Service("provider")
public class HelloWorldMessageProvider implements MessageProvider {

    @Override
    public String getMessage() {
        return "Hello World!";
    }
}
```


```java
//복잡한 서비스 빈
@Service("renderer")
public class StandardOutMessageRenderer implements MessageRenderer {

    private MessageProvider messageProvider = null;

    public void render() {
        if (messageProvider == null) {
            throw new RuntimeException(
                    "You must set the property messageProvider of class:"
                            + StandardOutMessageRenderer.class.getName());
        }

        System.out.println(messageProvider.getMessage());
    }

    public void setMessageProvider(MessageProvider provider) {
        this.messageProvider = provider;
    }

    public MessageProvider getMessageProvider() {
        return this.messageProvider;
    }

}
```

* **ApplicationContext로부터 빈을 얻는 방법**

```java
public class DeclareSpringComponents {

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();
		MessageRenderer messageRenderer = ctx.getBean("renderer",
				MessageRenderer.class);
		messageRenderer.render();
		ctx.close();
	}
}
```


3.5.3.1 자바 구성(Java Configuration) 사용하기
* 생성된 빈 타입을 드러내도록 클래스를 수정하지 않아도 app-context-xml.xml을 구성 클래스로 교체 가능.
* 이는 애플리케이션에게 필요한 빈 타입이 수정할 수 없는 서드파티 라이브러리의 일부인 경우에 유용
  * 이러한 구성 클래스에는 **@Configuration 애너테이션**을 적용!!
  * 구성 클래스 내에서는 스프링 IoC 컨테이너가 빈 인스턴스를 만들 때 직접 호출하는 @Bean 애너테이션이 적용된 메서드가 포함돼 있음 

* 생성되는 빈의 이름이 되는 메서드 이름을 표시하는 예. provider 메소드.
```java
@Configuration
public class HelloWorldConfiguration {

	@Bean
	public MessageProvider provider() {
		return new HelloWorldMessageProvider();
	}

	@Bean
	public MessageRenderer renderer(){
		MessageRenderer renderer = new StandardOutMessageRenderer();
		renderer.setMessageProvider(provider());
		return renderer;
	}
}
```

* **AnnotationConfigApplicationContext**를 사용해 빈 가져오기 (ApplicationContext 구현체 사용)

```java
public class HelloWorldSpringAnnotated {
    public static void main(String[] args) {
       ApplicationContext ctx = new AnnotationConfigApplicationContext(HelloWorldConfiguration.class)
       MessageRenderer messageRenderer = ctx.getBean("renderer", MessageRenderer.class);
       messageRenderer.render()
    }
}
```

* DefaultListableBeanFactory 대신 AnnotationConfigApplicationContext 인스턴스를 생성함
* AnnotationConfigApplicationContext 클래스는 ApplicationContext 인터페이스를 구현한 클래스로, 
* HelloWorldConfiguration클래스에 정의된 구성을 이용해 스프링의 ApplicationContext를 부트스트랩 할 수 있음
* 빈 정의 구성이 해당 빈 클래스의 일부이므로 구성 클래스가 더 이상 @Bean 애너테이션이 적용된 매소드를 갖고 있지 않아도 됨
* <context:component-scanning ... />과 동일한 역할을 하는 애너테이션을 구성 클래스에 추가하여 컴포넌트 스캐닝을 할 수 있음


3.5.3.2 **수정자 주입 사용**하기
* XML을 이용해 수정자 주입을 구성하려면 <bean> 태그 아래에 의존성을 주입할 **<property> 태그**를 지정

```xml
<beans>
    <bean id="renderer" class="com.apress.prospring5.ch2.decoupled.StandardOutMessageRenderer">
    <property name="messageProvider ref="provider" />
</beans>
```


* 이 코드에서 provider 빈이 messageProvider 프로퍼티에 지정되어 있음을 알 수 있음
* ref 애트리뷰트를 사용하여 프로퍼티에 빈 참조를 지정할 수 있음
* 스프링 2.5버전 이상을 사용한다면 XML 구성 파일에 **p네임 스페이스를 선언하여 의존성 주입**을 할 수 있음
* 네임스페이스는 XSD 파일에 정의되어 있지 않고 스프링 코어에만 존재함

```xml
<beans>
    <bean id="renderer" class="com.apress.prospring5.ch2.decoupled.StandardOutMessageRenderer" 
          p:messageProvider-ref="provider" />
</beans>
```

* 애너테이션을 사용한다면 수정자 메서드에 @Autowired 애너테이션만 추가하여 간단하게 사용할 수 있음

```xml
@Service("renderer")
public class StandardOutMessageRenderer implements MessgeRenderer [
    @Override
    @Autowired
    public void setMessageProvider(MessageProvider provider) {
        this.MessageProvider = provider;
    }
}
```


* @Autowired 대신 @Resource(name="messageProvider")를 사용해도 같은 결과를 얻을 수 있음
* @Resource는 @Autowired와 달리 더 세부적인 DI요구에 대응하려는 목적으로 name인자를 제공
* XML 구성 파일에서 <context:component-scan /> 태그를 선언했으므로 스프링은 ApplicationContext를 초기화하는 동안에 @Autowired를 발견하고 필요에 따라 의존성을 주입


3.5.3.3 **생성자 주입 사용**하기
* 이제까지는 HelloWorldMessageProvider의 getMessage()를 호출하면 하드코딩된 메시지를 동일하게 반영했음
* 메시지 값을 전달하지 않고서 ConfigurableMessageProvider의 인스턴스를 생성할 수 없기 때문에 이 클래스는 생성자 주입을 하는게 좋음
* 아래의 코드는 ConfigurableMessageProvider 인스턴스를 생성하려는 목적으로 provider 빈을 다시 정의하고 생성자 주입으로 메시지를 주입하는 방법을 보여줌

```xml
<bean id="messageProvider" class="com.apress.prospring5.ch3.xml.ConfigurableMessageProvider">
    <constructor-arg value="Hi! This is message!" />
</bean>
```

* <property> 태그 대신 <constructor-arg> 사용
* 다른 빈을 전달하지 않고 문자열만을 전달하기 때문에 ref대신 value 애트리뷰트를 사용하여 생성자 인수의 값을 지정
* 둘 이상의 생성자 인수가 있거나 클래스에 둘 이상의 생성자가 있는 경우에는 인수 인덱스를 지정하는 index애트리뷰트를 각 <constructor-arg>태그에 제공해야 함
* 인자 사이의 혼동을 피하고 스피링이 올바른 생성자를 선택하도록 하려면 다중 인수를 갖는 생성자를 다룰 때마가 항상 index애트리뷰트를 사용하는 것이 좋음
* <constructor-arg>태그는 c네임 스페이스를 사용하여 표현할 수도 있음

```xml
<bean id="messageProvider" class="com.apress.prospring5.ch3.xml.ConfigurableMessageProvider"
    c:message="Hi! This is message!" />
</bean>
```

* 애너테이션을 이용해 생성자 주입을 할 때에도 마찬가지로 대상 빈의 생성자 매소드에 @Autowired 애너테이션을 사용
* 이 방식은 수정자 주입 방식을 대체할 수 있음

```java
@Service("provider")
public class ConfigurableMessageProvider implements MessageProvider {

    private String message;

    @Autowired
    public ConfigurableMessageProvider(@Value("Configurable message) String message) {
        this.message = message;
    }

    @Override
    public String getMessage() {
        return this.message;
    }
}
```

* @Value를 사용하여 생성자에 주입할 값을 정의함
* 이런 방식으로 스프링에서는 빈에 값을 주입함
* 간단한 문자열 외에 동적으로 값을 주입하고 싶다면 강력한 기능인 SpEL을 사용할 수 있음
* 그러나 예제와 같이 코드에 값을 하드코딩하는 것은 좋지 않음
* 애너테이션 방식의 DI를 사용하더라도 주입할 값은 외부에 두는 것이 좋음
* 메시지를 외부화 하기 위해서는 아래와 같이 애너테이션 구성파일에서 메시지를 스프링 빈으로 정의해야 함

```xml
<beans .... />
    <context:component-scan base-package="com.apress.prospring5.ch3.annotated" />
    <bean id="message" class="java.lang.String" c:_0="This is message" />
</beans>
```

* Id가 message이고 클래스타입이 java.lang.String인 빈을 정의
* 생성자 주입을 위해 c네임스페이스를 사용해 문자열 값을 설정
* _0는 생성자 인수에 대한 인덱스를 나타냄
* 이 빈을 사용하면 아래와 같이 @Value를 제거해도 됨

```java
@Service("provider")
public class ConfigurableMessageProvider implements MessageProvider {
    
    private String message;

    @Autowired
    public ConfigurableMessageProvider(String message) {
        this.message = message;
    }

    @Override
    public String getMessage() {
        return this.message;
    }
}
```

* message 빈의 ID가 ConfigurableMessageProvider 클래스의 생성자에 지정된 인자의 이름과 동일하게 선언되었기 때문에 스프링은 @Autowired 애너테이션을 감지하여 값을 생성자 메소드에 주입할 것임
* 인수의 개수도 같은 생성자가 있는 경우 스프링이 어떤 생성자를 사용하여 주입을 해야할지 판단하지 못할 수도 있음

<bean id="messageProvider" class="com.apress.prospring5.ch3.xml.ConfigurableMessageProvider" >
    <constructor-arg type="int">
        <value>90</value>
    </constructor-arg>
</bean>

* <constructor-arg> 태그에는 스프링이 찾아야 할 인수의 데이터 타입을 지정하는 type 애트리뷰트가 있음
* 애너테이션 방식의 생성자 주입을 사용할 때에는 애너테이션을 대신 생성자 메소드에 직접 적용해 이런 혼란을 피할 수 있음

```java
@Service("ConstructorConfusion")
public class ConstructorConfusion {

    private String someValue;

    public ConstructorConfusion(String message) {
        this.message = message;
    }


    @Autowired
    public ConstructorConfusion(@Value("90") int message) {
        this.message = message;
    }
        ...
}
```

* 원하는 생성자 매소드에 @Autowired 애너테이션을 적용하면 스프링은 이 메소드를 사용해 빈 인스턴스를 생성하고 지정한 값을 주입함
* @Autowired 애너테이션은 생성자 메소드 중에서 하나에만 적용할 수 있음
* 하나 이상의 생성자에 적용하면 스프링은 ApplicationContext를 부트스트랩하는 과정에서 오류를 일으킴

3.5.3.4 **필드 주입 사용**하기

* 의존성을 생성자나 수정자를 사용하지 않고 필드에 직접 주입함
* 클래스 멤버 변수에 @Autowired 애너테이션을 적용
* 의존성이 필요한 객체의 내부에서만 주입된 의존성을 사용할 경우 사용 하면 개발자가 빈 초기 생성 시 의존성 주입에 사용되는 코드를 작성하지 않아도 되므로 실용적임

* 예제 3-40) 필드 주입을 이용한 빈 클래스([filed-injection] Singer.java)

```java
@Service("singer")
public class singer {

    @Autowired
    private Inspiration inspirationBean;

    public void sing() {
        System.out.println("..." + inspirationBean.getLyric());
    }
}
```

* Inspiration 필드는 private이지만 스프링 IoC 컨테이너가 의존성을 주입하는 것에는 문제없음
* 스프링 컨테이너가 리플렉션을 이용해 필요한 의존성을 주입하기 때문

* 다음 예제에서 Inspiration 클래스 코드를 확인. 이 빈은 String 타입의 멤버 변수를 가진 단순한 빈.
* 예제 3.41) String 타입 멤버 변수를 가진 빈 클래스(filed-injection] Inspiration.java)

```java
package com.apress.prospring5.ch3.annotated;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Inspiration {

	private String lyric = "I can keep the door cracked open, to let light through";

	public Inspiration(@Value("For all my running, I can understand") String lyric) {
		this.lyric = lyric;
	}

	public String getLyric() {
		return lyric;
	}

	public void setLyric(String lyric) {
		this.lyric = lyric;
	}
}
```

* 아래 코드는 스프링 IoC컨테이너가 생성할 빈의 정의를 찾을 수 있도록 컴포넌트 스캔 설정을 활성화하는 구성
* 예제 3-42) 컴포넌트 스캐닝 설정을 한 구성 파일(filed-injection] app-context.xml)

```xml
<beans .... />
    <context:component-scan base-package="com.apress.prospring5.ch3.annotated" />
</beans>
```

* 스프링 IoC 컨테이너는 Inspiration타입의 빈을 발견하면 singer 빈의 inspirationBean 멤버에 해당 빈을 주입할 것
* 그래서 다음 코드에 나와 있는 예제를 실행하면 "For all my running, I can understand"이라는 메시지가 콘솔에 출력됨
* 예제 3-43) 필드 주입 테스트 실행을 위한 FieldInjection 클래스([filed-injection] FieldInjection.java)

```java
package com.apress.prospring5.ch3.annotated;

import org.springframework.context.support.GenericXmlApplicationContext;

public class FieldInjection {

	public static void main(String... args) {

		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context.xml");
		ctx.refresh();

		Singer singerBean = ctx.getBean(Singer.class);
		singerBean.sing();

		ctx.close();
	}
}
```

* 하지만 필드 주입은 여러 단점이 있어서 일반적으로 사용을 권장하지 안음!

* 필드 주입의 단점
  * 의존성을 추가하기 쉽지만 단일 책임 원칙을 위반하지 않도록 주의해야함
    * 더 많은 의존성이 생기면 클래스에 대한 책임이 커지므로 리팩토링 시에 관심사를 분리하기 어려움
    * 클래스가 비대해지는 상황은 생성자나 수정자 주입을 사용하면 쉽게 알 수 있지만 필드 주입은 알아채기 어려움
  * 의존성 주입의 책임은 스프링 컨테이너에게 있지만, 클래스는 public 인터페이스의 매소드나 생성자를 이용해 필요한 의존성 타입을 명확하게 전달해야함
    * 그러나 필드 주입을 사용하면 어떤 타입의 의존성이 실제 필요한지, 의존성이 필수인지 여부가 명확하지 않을 수도 있음
  * 필드 주입은 final 필드에 사용할 수 없음. 이 타입 필드는 오직 생성자 주입만을 사용하여 초기화 할 수 있음
  * 필드 주입은 의존성을 수동으로 주입해야하므로 테스트 코드를 작성하기 어려움

<br>

3.5.3.5 **주입 인자 사용**하기

* 스프링은 다른 컴포넌트나 단순 값 외에 자바 컬렉션, 외부에 정의된 프로퍼티, 다른 팩터의 빈을 주입할 수 있도록 많은 옵션의 주입인자를 지원함
* 수정자 주입이나 생성자 주입 시에 <property>, <constructor-arg> 태그의 관련 태그들을 사용해 다양한 타입의 주입 인자를 사용할 수 있음

<br>

3.5.3.7 **SpEL을 사용**해 값 주입하기

* 스프링 표현식 언어(Spring Expression Language)
* SpEL을 사용하면 표현식을 동적으로 평가하여 그 결과를 스프링의 ApplicationContext 내에서 사용할 수 있음
* 표현식 평가 결과는 스프링 빈에 주입될 수 있음
* 다른 빈의 프로퍼티를 참조할 때는 #{classname.참조할 변수}처럼 SpEL을 사용함

```xml
<baen id="injectionSimpleSpel"
     class="com.apress.prospring5.ch3.xml.InjectionSimpleSpel"
     p:name="#{injectSimpleConfig.name}"> 
</bean>
```

* injectionSimpleSpel 빈을 주입받을 클래스 , injectSimpleConfig 빈의 값을 정의한 클래스(빈을 주입한 클래스)
* injectSimpleConfig에서 정의된 값을 xml을 통해 injectionSimpleSpel에 주입함

* 애너테이션 방식으로 값을 주입하려면 value 애너테이션을 SpEL 표현식과 함께 사용하도록 변경

```java
@Service
public class InjectSimpleSpel {
    
    @Value("#{injectSimpleConfig.name}")
    private String name;
}
```

* @Servicedhk @Component는 동일한 효과의 애너테이션
* @Service는 @Component의 특수한 형태이며, @Service가 사용된 클래스는 애플리케이션 내의 다른 레이어에게 비즈니스 서비스를 제공하고 있음


3.5.3.8 **같은 XML구성 내에세 빈 주입** 받기

* ref 태그를 사용하면 특정 빈을 다른빈에 주입할 수 있름
* 어떤 빈을 다른 빈에 주입하도록 스프링 구성을 하려면, 먼저 두 개의 빈에 대해 각각 구성을 해야함
* 하나의 빈은 주입이 되는 빈이고, 다른 하나의 빈은 주입을 받는 대상 빈

```xml
<beans>
    <baen id="oracle" name="wisdom" class="com.apress.prospring5.ch3.BookwormOracle"/> 
    <bean id="injectRef" class="com.apress.prospring5.ch3.xml.InjectRef">
        <property name="oracle">
            <ref bean="oracle" />
        </property>
    </bean>
</beans>
```

* 주입되는 빈의 타입이 대상 빈에 정의된 타입과 정확히 같을 필요는 없음. 타입은 서로 호환되기만 하면 됨
* 호환된다는 것은 대상 빈에 선언된 타입이 인터페이스라면 주입된 타입이 이 인터페이스의 구현체여야함
* 선언된 타입이 클래스라면 주입된 타입은 동일한 타입이거나 해당 타입을 상속한 타입이어야 함
* 주입할 빈의 ID는 <ref>태그의 local애트리뷰트를 사용해 지정할 수 있음
* local애트리뷰트를 사용하면 <ref> 태그는 빈의 별칭을 찾지 않고 ID만 보며, 빈정의는 같은 XML 구성 차일에 존재해야함
* 다른 이름으로 빈을 주입하거나 다른 XML 구성 파일에 선언된 빈을 가져오려면 local애트리뷰트 대신 <ref>태그의 bean 애트리뷰트를 사용해야함
* 위의 예제를 보면 name 애트리뷰트를 사용해 oracle 빈에 별칭을 부여하고, 이 별칭을 <ref> 태그의 bean 애트리뷰트에 지정해 oracle 빈을 injectRef 빈에 주입

<br>

3.5.3.9 **주입과 ApplicationContext의 중첩**

* 지금까지 본 예시는 우리가 주입한 빈은 주입 대상 빈과 동일한 ApplicationContext(즉, 동인한 BeanFactory)에 있었음
* 스프링은 어떤 컨텍스트가 다른 컨텍스트의 부모가 될 수 있도록 ApplicationContext의 계층 구조도 지원함
* ApplicationContext를 중첩할 수 있는 기능을 통해 스프링은 구성파일을 서로 다른 여러 파일로 나눌 수 있게 함
* 스프링은 ApplicationContext 인스턴스를 중첩 시킬 때, 부모 컨텍스트에 있는 참조 빈을 자식 컨텍스트에 있는 빈처럼 사용할 수 있게함
* GenericXmlApplicationContext를 사용하면 간단하게 중첩시킬 수 있음
* 특정 GenericXmlApplicationContext를 다른 GenericXmlApplicationContext 클래스 안에 중첩시키려면 자식 ApplicationContext에서 setParent() 메소드를 호출하면 됨
* 자식 ApplicaationContext의 구성 파일에서 부모 ApplicationContext의 빈을 참조하는 일은 자식 ApplicationContext에 동일한 이름을 가진 빈이 없는 한 동일함
* 이 경우 ref요소의 bean애트리뷰트를 parent로 바꾸기만 하면 됨

```java
public class HierarchicalAppContextUsage {

	public static void main(String... args) {
		GenericXmlApplicationContext parent = new GenericXmlApplicationContext();
		parent.load("classpath:spring/parent-context.xml");
		parent.refresh();

		GenericXmlApplicationContext child = new GenericXmlApplicationContext();
		child.load("classpath:spring/child-context.xml");
		child.setParent(parent);
		child.refresh();

		Song song1 = (Song) child.getBean("song1");
		Song song2 = (Song) child.getBean("song2");
		Song song3 = (Song) child.getBean("song3");

		System.out.println("parent 컨텍스트로부터: " + song1.getTitle());
		System.out.println("child 컨텍스트로부터: " + song2.getTitle());
		System.out.println("parent 컨텍스트로부터: " + song3.getTitle());

		child.close();
		parent.close();
	}
}
```

```java
public class Song {
    private String title;
    
    public void setTitle(String title) {
        this.title = title;
    }
    
    public String getTitle() {
        return title;
    }
}
```

child-context.xml
```xml
<beans>
    <bean id="song1" class="com.apress.prospring5.ch3.Song" p:title-ref="parentTitle" />
    <bean id="song2" class="com.apress.prospring5.ch3.Song" p:title-ref="childTitle" />
    <bean id="song3" class="com.apress.prospring5.ch3.Song" >
        <property name="title">
            <ref parent="childTitle">
        </property>
    </bean>
    <bean id="childTitle" class="com.apress.prospring5.ch3.Song" c:_0="해당 값이 없습니다." />
</beans>
```

* song1 빈은 ref 애트리뷰트를 사용해 parentTitle이라는 빈을 참조함
  * parentTitle 빈은 부모 BeanFactory에만 존재하므로 Song1은 parentTitle 빈에 대한 참조를 받음
  * 빈 구성시 애트리뷰트로 자식 ApplicationContext와 부모 AC의 빈을 참조할 수 있음
      * 때문에 빈을 투명하게 탐조할 수 있어서 애플리케이션이 커지면 구성 파일간에 빈을 이동할 수 있음
  * local 애트리뷰트를 사용해 부모 AC의 빈을 참조할 수 없음
      * XML 파서는 local애트리뷰트의 값이 동일한 파일에 유효한 요소로 존재하는지 확인하므오 부모 컨텍스트의     빈을 참조하지 않도록 함
* song2 빈은 ref 애트리뷰트를 사용해 childTitle을 참조함
  * song2빈이 참조하는 childTitle빈이 양쪽 AC에 정의되어 있으므로 song2빈은 자신의 AC에서 childTitle에 대한 참조를 받음
* song3 빈은 <ref>태그를 사용해 부모 AC로부터 직접 childTitle을 참조힘
  * <ref> 태그의 parent 애트리뷰트를 사용하기 때문에 자식 AC에 선언된 childTitle 인스턴스는 완전히 무시됨
* p네임스페이스는 편리한 단축 표현을 제공하지만 부모 빈을 참조하려고 property태그를 사용하는 것과 같은 모든 기능을 제공하지는 않음
* 꼭 필요한 경우가 아니라면 예시처럼 섞어서 사용하지 않는 것이 좋음

<br>

3.5.3.10 **컬렉션 주입**

* List, Map, Set, Properties 인스턴스를 나타내는 <list>, <map>, <set>, <props>태그 중 하나를 선택하고 다른 주입 방식 처럼 개별 항목을 전달
* Properties 클래스는 String 프로퍼티 값만을 허용하기 때문에 <props>태그는 String만 값으로 전달할 수 있음
* <list>, <map>, <set>을 사용해 프로퍼티에 주입할 때에는 원하는 태그를 사용할 수 있으며, 다른 컬렉션 태그도 함께 사용할 수 있음

```xml
<property name="map">
    <map>
        <entry key="someValue">
            <value>HelloWorld</value>
        </entry>
        <entry key="someBean">
            <ref bean="lyricHolder"/>
        </entry>
    <map>
</property>
```

* map의 각 항목은 <entry>태그를 사용해 지정하는데, 이 항목은 문자열 키와 엔트리 값을 가짐
* 이 엔트리 값은 프로퍼티에 별도로 주입할 수 있는값이 될 수 있음
* <map> 요소에는 <value> 및 <ref> 요소 대신 value와 value-ref 애트리뷰트를 사용하여 보다 간결하게 사용 가능

<br>

```xml
<property name="set">
    <set>
        <value>HelloWorld</value>
        <ref bean="lyricHolder">
    </set>
</property>

<property name="list">
    <list>
        <value>HelloWorld</value>
        <ref bean="lyricHolder">
    </list>
</property>
```

* <list>와 <set> 태그는 동일한 방식으로 동작함
* 값 하나를 프로퍼티에 주입하는데 사용되는 <value>나 <ref>와 같은 태그 중 하나를 사용해 각각의 요소를 지정
* <list>, <map>, <set> 태그에 컬렉션 엔트리 값 하나를 지정할 때, 컬렉션이 아닌 프로퍼티 값을 설정하는 태그를 사용할 수 있음
* 이는 원시값 컬렉션만을 빈에 주입할 수 있는 것이 아닌 빈으로 이워진 컬렉션이나 다른 컬렉션으로 이뤄진 컬렉션도 필요한 곳에 주입할 수 있음
* 이 기능을 사용하면 애플리케이션을 모듈화하기가 쉽고 서로 다른 애플리케이션 로직의 주요 부분을 사용자가 선택한 구현체로 제공할 수 있음
* XML 구성 외에 애노테이션을 사용해 컬렉션을 주입할 수 있지만 유지보수를 쉽게 하려면 컬렉션 값을 외부 구성 파일로 관리하는 것이 좋음
* 컬렉션 타입을 주입할 때에는 빈 이름을 지정할 수 있도록 지원하는 @Resource 애너테이션을 사용해 빈 이름을 지정하고, 스프링에 명시적으로 의존성을 알맞게 주입하도록 알려줘야함

<br>

3.5.4 **메소드 주입 사용**하기

* 자주 사용하지는 않음
* loosely related 형태인 룩업 메소드 주입과 메소드 대체 방식 제공
* 메소드 대체를 사용하면 원래 소스 코드를 변경하지 않고 임의로 메소드 구현을 변경할 수 있음
* 이 두가지 기능을 제공하기 위해 스프링은 CGLIB의 동적 바이트 코드 확장 기능을 사용

<br>

3.5.4.1 **룩업 메소드 주입**

* 싱글턴 빈이 비싱글턴 빈에 의존하는 상황과 같이 어떤 빈이 다른 라이츠 사이클을 가진 빈에 의존할 때 발생하는 문제를 극복하기 위해 스프링 1.1버전에 추가
* ApplicationContextAware 인터페이스를 구현하면 싱글턴 빈은 ApplicationContext 인스턴스를 사용해서 필요할 때마다 비싱글턴 의존성의 새 인스턴스를 룩업할 수 있음
* 룩업 메소드 주입을 이용하면 별도의 스프링 인터페이스를 구현하지 않아도 싱글턴 빈이 비싱글턴 빈 의존성을 필요로 할 때마다  선언하여 비싱글턴 빈의 새로운 인스턴스를 얻을 수 있게 해줌
* 비싱글턴 빈의 인스턴스를 반환하는 룩업 메소드를 싱글턴 빈에 선언함으로써 동작
* 애플리케이션에서 싱글턴에 대한 참조를 얻을 때, 스프링이 구현해 둔 룩업 메소드를 사용해서 동적으로 생성된 서브 클래스에 대한 참조를 받음
* 일반적으로는 룩업 메소드를 구현없이 정의만하므로 구현클래스를 abstract으로 선언함
* abstract로 선언하면 메소드 주입 구성을 잊어버렸을 때 스프링이 수정한 서브 클래스의 메소드가 아닌 텅 빈 메소드에서 가져온 빈 클래스를 직접 사용해 발생하는 문제를 방지함
* abstract LookupBean은 <lookup-method> 태그를 사용해 룩업 메소드를 구성 해야 함
* <lookup-method> 태그의 name 애트리뷰트는 빈이 오버라이드해야 하는 메소드 이름을 스프링에 알려줌
* 이 메소드는 인수를 받지 말아야 하며, 반환 타입은 메소드가 반환하는 빈의 타입이어야 함
* bean 애트리뷰트는 룩업 메소드가 반환해야 하는 빈이 무엇인지 스프링에 알려줌

```java
public class LookupDemo {

    public static void main(String... args) {
        GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
        ctx.load("classpath:spring/app-context-xml.xml");
        ctx.refresh();
        DemoBean abstractBean = ctx.getBean("abstractLookupBean", DemoBean.class);
        DemoBean standardBean = ctx.getBean("standardLookupBean", DemoBean.class);
        displayInfo("abstractLookupBean", abstractBean);
        displayInfo("standardLookupBean", standardBean);
        ctx.close();
    }

    public static void displayInfo(String beanName, DemoBean bean) {
        Singer singer1 = bean.getMySinger();
        Singer singer2 = bean.getMySinger();
        // singer 타입의 로컬 변수 두 개 생성하고 전달된 빈에서 getMySinger()를 호출해 각 값을 할당

        System.out.println("[" + beanName + "]: Singer 인스턴스는 같은가? " + (singer1 == singer2));
        // 두 참조 객체가 같은 객체를 가리키는지 확인

        StopWatch stopWatch = new StopWatch();
        stopWatch.start("lookupDemo");

        for (int x = 0; x < 100000; x++) {
            Singer singer = bean.getMySinger();
            singer.sing();
        }

        stopWatch.stop();
        System.out.println("100000번을 얻어오는데 걸리는 시간 : " + stopWatch.getTotalTimeMillis() + " ms");
    }
}
```

* GenericXmlApplicationContext로 부터 abstractLookupBean과 standardLookupBean이 룩업되고, 각 참조가 displayInfo() 메소드에 전달됨
* 추상 클래스의 인스턴스 생성은 룩업 메소드 주입을 이용할 때만 지원됨
* 메소드 룩업을 사용하면 스프링이 CGLIB를 사용해 메소드를 동적으로 재정의 하는 AbstractLookupDemoBean클래스의 하위 클래스를 생성
* abstractLookupBean 빈은 getMySinger()를 호출할 때마다 Singer의 새 인스턴스를 검색해야 하므로 참조가 동일하지 X
* standardLookupBean의 경우 Singer의 단일 인스턴스가 수정자 주입을 통해 빈으로 전달되어 저장되며 getMySinger()를 호출할 때마다 해당 인스턴스가 반환되므로 두 참조가 동일해야함
* stopWatch 클래스는 스프링에서 사용할 수 있는 유틸리티 클래스로 간단한 성능을 테스트해야할 때와 애플리케이션 테스트를 할때 유용
* standardLookupBean이 속도가 더 빠름
* 애너테이션으로 할 경우 getMySinger() 메소드에 는 구현 내용을 비워두고 Singer 빈의 이름을 인수로 받는 @Lookup 애너테이션을 적용함
* 메소드의 내용은 동적으로 생성된하위 클래스에서 재정의됨


3.5.4.2 룩업 메소드 주입 시 고려사항
* 룩업 메소드 주입은 다른 라이프 사이클을 가진 두 빈을 사용해 작업을 하는 경우에 사용
* 하나의 공통 의존성을 공유하는 세 개의 싱글턴을 생각해 보면 비싱글턴으로 의존성을 생성할 수도 있지만, 동일한 인스턴스를 사용하더라도 문제가 없을 경우 수정자 주입이 이상적이며, 룩업 메소드 주입은 불필요한 오버헤드를 발생시킬 뿐임
* 일반적으로 IoC 목적으로만 사용되는 불필요한 정의로 비즈니스 인터페이스를 오염시키지 말아야 함

<br>

3.5.4.3 메소드 대체
* 지금까지는 협력 객체를 빈에 제공하는데 온전히 주입만을 사용
* 메소드 대체를 사용하면 수정 중인 빈의 소스를 변경하지 않고 임의로 모든 메소드 구현체를 바꿀 수 있음
* 내부적으로 빈 클래스의 서브 클래스를 동적으로 생성하면 메소드를 대체할 수 있음
* CGLIB를 사용해 원래 메소드의 호출을 MethodReplacer 인터페이스를 구현한 다른 빈에 대한 호출로 리디렉션할 수 있음

<br>

3.5.4.4 메소드 대체를 사용할 때
* 동일 타입의 모든 빈이 아닌 단일 빈에 대한 특정 메소드만 대체하려는 경우에 유용
* 런타임 바이트 코드 향상에 의존하기 보다는 표준 자바 메커니즘을 사용해 메소드를 대체하는 것이 더 바람직함
* 애플리케이션 일부에서 메소드 대체를 사용하려는 경우에는 메소드나 오버로드된 메소드 그룹마다 MethodReplacer를 하나만 사용하는 것을 권장함
* 사용시 성능이 걱정된다면 Boolean타입 애트리뷰트를 MethodReplacer에 추가하면 의존성 주입을 이용해 검사 여부를 켜고 끌 수 있음

<br>

3.5.5 **빈 명명 규칙** 이해하기
* 모든 빈은 ApplicationContext 내에서 고유한 하나 이상의 이름을 가져야 함
* <bean> 태그에 id 애트리뷰트의 값을 지정하면 이 값이 이름으로 사용됨
* id 애트리뷰트에 값이 지정되지 않으면 스프링은 name 애트리뷰트를 찾는데, name 애트리뷰트 값이 여러 개 정의되어 있다면 그 중 첫 번째 이름을 사용
* id와 name 애트리뷰트가 모두 지정되지 않으면 스프링은 빈의 클래스 이름을 빈 이름으로 사용(피하는게 좋음)
    * 동일한 타입의 여러 빈을 정의할 수 없으므로 유연하지 않음
    * 고유한 이름을 지정하면 사용하던 스프링의 동작 방식이 나중에 달라지더라고 애플리케이션은 계속 잘 동작할 것임
* id나 name이 없는 같은 타입의 빈이 여러 개 선언되면 스프링은 ApplicationContext를 초기화 하는 과정에서 해당 타입의 빈을 주입할 때 예외를 던짐

<br>

3.5.5.1 **빈 이름 별칭** 짓기
* 스프링에서는 빈이 여러 개의 이름을 가질 수 있음
* 빈에 여러 이름 지정하기 : name 애트리뷰트에 공백, 쉼표, 세미콜론 등으로 구분해 이름의 목록을 지정하면 됨
* name 애트리뷰트는 id 애트리뷰트 개신에 사용될 수도 있고, id 애트리뷰트와 함께 사용될 수도 있음
* name들에 대한 별칭을 정의할 때 <alias>태그를 사용할 수 있음
* ApplicationContext.getAliases(String)을 호출하며 빈의 이름이나 id를 전달해 빈의 별칭 목록을 검색할 수 있음
   * 이렇게 하면 지정한 이름 외에 별칭 목록이 String 배열로 반환됨
* name과 id 애트리뷰트 값은 스프링 IoC에 의해 다르게 다루어짐

<bean name="jon " class="java.lang.String"/>
<bane id="jon johnny,jonathan;jim" class="java.lang.String"/>

* name은 지정한 별칭을 제외하고 나머지 별칭이 문자열 배열로 반환됨
    * id : jon / 별칭: [johnny,jonathan,jim]
* id는 전체 문자열은 빈에 대한 고유한 식별자가 됨
    * id : jon,johnny,jonathan,jim / 별칭 : []
* 어떤 빈에 주입할 다른 빈이 많을 경우에는 같은 이름을 사용해 해당 빈에 접근하는 것이 더 나음
* 하지만 애플리케이션이 제품으로써 완성된 후 유지보수로 인해 수정하다 보면 빈 이름 별칭을 만드는 것이 유용

<br>

3.5.5.2 **애너테이션 구성을 이용**한 빈 명명 규칙
* 애너테이션으로 빈을 선언하는 것은 XML을 사용할 때와는 다름

```java
@Component
public class Singer {
    private String lyric = "hey~~~~!";

    public void setLyric(@Value("ggggggggg") String lyric) {
        this.lyric = lyric;
    }

    public void sing() {
        System.out.println(lyric);
    }
}
```


* 이 클래스에는 @Component 애너테이션을 적용한 Singer 타입의 싱글턴 빈 선언이 포함돼 있음
* @Component 애너테이션에는 인수가 없기 때문에 스프링 IoC 컨테이너가 빈에 대한 고유한 식별자를 결정
* 이 경우에는 클래스 자체로 빈을 명명하고 첫 번째 문자를 내림차순으로 처리하는 것 -> Singer이름이 빈 이름이 됨

```java
@Component("johnMayer")
public class Singer {

    private String lyric = "hey~~~~!";

    public void setLyric(@Value("ggggggggg") String lyric) {
        this.lyric = lyric;
    }

    public void sing() {
        System.out.println(lyric);
    }
}
```

* 위 방법으로 별칭을 선언하고자 할 때 @Component 애너테이션의 인수가 빈의 고유 식별자가 되므로 불가능

```java
public class AliasConfigDemo {

    @Configuration
    public static class AliasBeanConfig {

        @Bean
        public Singer singer() {
            return new Singer();
        }
    }
    ...
}
```

* @Baen 애너테이션에 인수가 제공되지 않으면 빈의 고유 식별자인 id로 메소드 이름이 사용됨
* 별칭을 선언하려면 @Bean 애너테이션의 name 어트리뷰트를 사용
* 이 애트리뷰트의 값이 별칭 지정 시에 사용할 수 있는 구분 기호(공백, 쉼표, 세미콜론)을 포함하는 문자열이라면 이 문자열이 빈의 id가 됨
* 값이 문자열 배열이면 첫 번쨰 값은 id가 되고, 다른 값은 별칭이 됨

```java
public class AliasConfigDemo {

    @Configuration
    public static class AliasBeanConfig {

        @Bean(name={"johnMayer", "john", "jonathan", "jonny"})
        public Singer singer() {
           return new Singer();
        }
    }
    ...
}
```

* 스프링 4.2dptj @AliasFor 애너테이션이 도입됨
* 이 애너테이션은 애너테이션 애트리뷰트에 대한 별칭을 선언하는 데 사용됨
* 대부분의 스프링 애너테이션은 이 애너테이션을 사용함
* 메타 애너테이션 애트리뷰트에 대한 별칭을 선언할 수 있음
* 그러나 스테에오 타입 애너테이션에 사용할 수 없음
    * @AliasFor가 생겨나기 수년 전부터 스테레오 타입 애너테이션의 value 애트리뷰트를 특수하게 처리했기 때문
    * 그래서 스테에오 타입 애너테이션에서 value 애트리뷰트에 별팅을 지정하는 코드를 작성해도 컴파일 오류는 없을 수 있지만 별칭에 제공된 인수는 무너짐
    * @Qualifier 애너테이션에도 동일한 제약이 적용됨
    * 스테레오 타입 애너테이션 : @Component과 @Service, @Repository, @Controller처럼 @Component 애너테이션의 특수한 형태인 애너테이션
    
<br>

3.5.6 **빈 생성 방식** 이해하기
* **기본적으로 스프링의 모든 빈은 싱글톤**임!!
* 스프링은 빈의 단일 인스턴스를 유지하고 관리하며, 모든 의존객체는 동일한 인스턴스를 사용하고 ApplicationContext.getBean()에 대한 모든 호출은 동일한 인스턴스를 반환
* 싱글톤은 애플리케이션 내에 단일 인스턴스를 갖는 객체(싱글톤)와 싱글톤 디자인 패턴을 나타내려는 목적(싱글톤패턴)으로 상호 교환적으로 사용

* NonSingleton 예제 코드

```java
public class NonSingletonDemo {
    public static void main(String... args) {
          GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
          //ctx.load("classpath:spring/app-context-xml.xml");
          ctx.load("classpath:spring/app-context-annotated.xml");
          ctx.refresh();
        
          Singer singer1 = ctx.getBean("nonSingleton", Singer.class);
          Singer singer2 =  ctx.getBean("nonSingleton", Singer.class);
        
          System.out.println("식별자가 동일한가?: " + (singer1 ==singer2));
          System.out.println("값이 동일한가?: " + singer1.equals(singer2));
          System.out.println(singer1);
          System.out.println(singer2);

          ctx.close();
    }
}
```

* Singleton 예제 코드

```java
public class Singleton {
    
    private static Singleton instance;

    static {
        instance = new Singleton();
    }

    public static Singleton getInstance() {
        return instance;
    }

    private Singleton(){
        // needed so developers cannot instantiate this class directly
    }
}
```

* 위의 코드는 애플리케이션 전체에서 클래스의 단일 인스턴스를 유지 관리하고 접근하도록하는 목표는 달성했지만, 결합도가 증가
* **싱글톤 패턴**
  * 두 개의 패턴이 하나로 합쳐진 것
  * 클래스가 단일 인스턴스로 동작하도록 관리하는 것
  * 인터페이스를 완전히 사용할 수 없는 객체 룩업을 위한 패턴
  * cf.) 어떤 클래스에서 호출해도 하나의 특정 인스턴스만 호출됨. 공통된 오브젝트를 사용해야 하는 상황에서 특정한 하나의 오브젝트만 사용하게 해줌.(ex.) DBConnectionPool)
    * 구현방법
      * 1) 생성자를 private으로으로 선언: 외부에서 클래스의 오브젝트를 생성할 수 없게됨.
      * 2) 참조는 static으로 정의: 어느 영역에서든 접근이 가능하도록 함.
      * 이렇게 하면 클래스가 classloader에 의해 단 한번만 인스턴스화 되는 것을 이용하여 구현.
      
* 싱글톤 패턴의 단점 
  * 싱글톤 패턴을 사용하면 싱글톤 인스턴스를 필요로 하는 대부분의 객체가 싱글톤 객체에 직접 접근하기 때문에 임의로 구현을 변경하기 어려움
  * 다행히도 스프링을 사용하면 싱글톤 디자인 패턴을 사용하지 않고도 싱글톤 인스턴스 생성 모델을 활용할 수 있음
* **스프링의 모든 빈은 기본적으로 싱글톤 인스턴스로 생성**
* 스프링은 해당 빈에 대한 모든 요청을 동일한 인스턴스를 사용하여 수행함
* 스프링의 인스턴스 생성방식은 애플리케이션 코드에는 아무런 영향을 주지 않으므로 스프링에서는 싱글톤 뿐만 아니라 모든 인스턴스 생성 방식을 사용할 수 있음
* 싱글톤이 멀티 스레드 접근에 적합하지 않다고 생각된다면, 애플리케이션 코드에 영향을 주지 않으면서 싱글톤이 아닌 타입으로 변경할 수 있음

* 아래 코드는 싱글톤에서 비싱글톤으로 바꾸는 예제

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c" xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="nonSingleton" class="com.apress.prospring5.ch3.annotated.Singer" scope="prototype" c:_0="John Mayer"/>
</beans>
```

```java
@Component("nonSingleton")
@Scope("prototype")
public class Singer {

    private String name = "unknown";

    public Singer(@Value("John Mayer") String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return name;
    }
}
```

* 컴포넌트 스캔이 활성화되어 있지 않으면 클래스의 애너테이션은 무시
* 스프링은 스코프의 기본값을 싱글톤으로 설정
* 프로토타입 스코프를 지정하면 스프링은 애플리케이션이 빈 인스턴스를 요청할 때마다 새 인스턴스를 생성

<br>

3.5.6.1 **인스턴스 생성 방식 선택**하기

* 싱글톤이 유리한 경우

* 상태가 없는 공유 객체
    * 상태를 유지하지 않으며, 많은 의존 객체를 가지는 경우
    * 상태가 없으면 동기화할 필요가 없어 의존 객체가 로직 처리를 위해 빈을 사용할 때마다 새 인스턴스를 생성할 필요가 없음

* 읽기 전용 상태를 갖는 공유 객체
    * 여전히 동기화가 필요하지 않으므로 빈을 요청할 때마다 인스턴스를 생성하면 불필요한 오버헤드만 늘어남

* 공유상태를 갖는 공유 객체
    * 공유되어야 하는 상태를 가진 빈이 있다면 싱글턴이 이상적인 선택임
    * 이 때에는 쓰기에 대한 동기화가 가능한 한 최소한으로 이루어져야 함

* 쓰기 가능 상태를 갖는 대용량 처리(High-throughput) 객체
    * 애플리케이션에서 많이 사용되는 빈이 있다면 싱글톤을 유지하고, 모든 쓰기 접근을 빈 상태에 동기화하면 수백 개의 인스턴스를 끊임없이 생성하는 것보다 나은 성능을 얻을 수 있음
    * 빈에 이런 방식을 사용할 때에는 일관성을 희생시키지 않으면서 가능한 한 동기화를 최소 규모로 유지해야함
    * 애플리케이션이 오랜 시간에 걸쳐 많은 인스턴스를 생성하거나, 공유 객체에 값을 쓰는 일이 적거나, 새 인스턴스를 생성하면서 컴퓨팅 자원을 많이 소모할 때 특히 유용

* 비싱글톤 고려
  * 쓰기 가능한 상태를 갖는 객체
     * 빈이 쓰기 가능한 상태를 많이 가지고 있다면, 의존 객체의 각 요청을 처리하는 데 새로운 인스턴스를 생성하는 비용보다 동기화하는 비용이 더 많이 들 때

  * private 상태를 갖는 오브젝트
    * 일부 의존 객체는 private 상태를 가진 빈을 사용해 빈에 의존하는 다른 객체와는 별도로 자신만의 처리 작업을 할 수 있음
    * 이런 경우 싱글톤은 명백하게 적합하지 않음

<br>

* cf.) **자바 싱글톤** Vs **스프링 싱글톤 패턴**의 차이점
  * 자바 싱글톤은 **클래스로더**에 의해 구현되고, 스프링의 싱글톤은 **스프링 컨테이너**에 의해 구현된다.
  * 자바 싱글톤의 scope는 코드 전체이고, 스프링 싱글톤의 scope는 해당 컨테이너 내부이다.
  * 스프링에 의해 구현되는 싱글톤패턴은 Thread safety를 자동으로 보장한다. 자바로 구현하는 싱글톤패턴은 개발자의 로직에 따라 thread safety를 보장할수도, 보장하지 않을수도 있다.

3.5.6.2 **빈 스코프 구현**하기(스프링4 버전에서 지원하는 빈 스코프)

* **싱글톤(Singleton)** : 기본 싱글톤 스코프. 스프링 IoC 컨테이너당 하나의 객체만 생성됨
* **프로토타입(Prototype)** : 애플리케이션에서 요청할 때마다 스프링이 새 인스턴스를 생성

* 요청(Request)
    * 웹 애플리케이션에서 사용
    * 웹 애플리케이션에서 스프링 MVC를 사용할 때 요청 스코프를 가진 빈의 인스턴스는 모든 HTTP 요청이 있을 때마다 생성되고 요청 처리가 완료되면 소멸됨

* 세션(Session)
    * 웹 애플리케이션에서 사용
    * 웹 애플리케이션에서 스프링 MVC를 사용할 때 세션 스코프를 가진 빈의 인스턴스는 모든 HTTP 세션이 시작되면 생성되고 세션이 끝나면 소멸됨

* 글로벌 세션(Global Session)
    * 포틀릿 기반 웹 애플리케이션에서 사용
    * 글로벌 세션 스코프빈은 동일한 스프링 MVC 기반 포털 애플리케이션 내의 모든 포틀릿간에 공유될 수 있음

* 스레드(Thread)
    * 새로운 스레드에 의해 요청될 때 스프링에 의해 새 빈 인스턴스가 생성되고, 동일한 스레드에 대해서는 같은 빈 인스턴스 반환
    * 이 스코프는 기본적으로 등록되지 않음

* 사용자 정의(Custom)
    * org.springframework.beans.factory.config.Scope 인터페이스를 구현하고 스프링 구성에 사용자 정의 스코프를 등록해 생성할 수 있는 사용자 정의 빈 스코프

<br>

3.5.7 **의존성 해석**하기
* 사용자 구성 파일이나 구성 클래스에 적용된 애너테이션을 보고 의존성을 해석할 수 있음
* 스프링은 이런 방법으로 각각의 빈들이 올바른 순서로 구성될 수 있도록 해서 각각의 빈들의 의존성이 정확하게 주입되게 함
* 스프링은 구성을 통해서 명시하지 않으면 사용자 빈 사이에 어떤 의존성도 인식하지 못함

<bean id="johnMayer" class="com.apress.prospring5.ch3.annotated.Singer" depends-on="gopher"/>
<bean id="gopher" class="com.apress.prospring5.ch3.annotated.Guitar"/>

* 위의 구성을 통해 johnMayer 빈이 gopher 빈에 의존한다는 것을 스프링에 알림
* 스프링은 빈 인스턴스를 생성할 때 이를 고려해야하며, johnMayer보다 gopher를 먼저 생성해야함
* 하지만 이렇게 하려면 johnMayer가 ApplicationContext에 접근해야함
* 따라서 스프링에게 이 참조를 주입하도록 지시해서 johnMayer.sing()매소드가 호출될 때 gopher 빈을 얻어야함
* 이는 Singer 빈이 ApplicationContextAware 인터페이스를 구현한다면 가능함
* ApplicationContextAware를 구현하기 위해서는 이 인터페이스를 가져오는 수정자를 구현해야함
* ApplicationContextAware를 구현한 빈은 스프링 IoC 컨테이너에 의해 자동으로 감지되고 해당 빈 내부에 생성된 ApplicationContext가 주입됨
* 이러한 과정은 빈 생성자가 호풀된 이후에 수행되므로 생성자에서 ApplicationContext를 사용하면 NullPointException이 발생
* XML 구성 대신에 에너테이션을 이용해서 동일하게 구성할 수 있음

<beans>
    <context:component-scan base-package="com.apress.prospring5.ch3.annotated" />
</beans>

```java
@Component("johnMayer")
@DependsOn("gopher")
public class Singer implements ApplicationContextAware{

    ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException{
        this.applicationContext = applicationContext;
    }

    private Guitar guitar;

    public Singer(){}

    public void sing() {
        guitar = applicationContext.getBean("gopher", Guitar.class);
        guitar.sing();
    }
}
```

* @DependsOn 애너테이션은 XML 구성에서 depends-on 애트리뷰트와 같음
* 애플리케이션을 개발할 때 이 기능을 사용할 수 있게 설계하면 안됨
* 대신 수정자 주입이나 생성자 주입을 이용해 의존성을 주입할 수 있게 정의해야함



3.6 **빈에 자동 와이어링**하기

* byName
    * 이름에 의한 자동와이어링을 이용하면 스프링은 각 프로퍼티와 이름이 같은 빈을 찾아서 연결 시도
    
* byType
    * 타입에 의한 자동와이어링을 이용하면 스프링은 자동으로 ApplicationContext에서 동일한 타입의 빈을 대상 빈의 각 프로퍼티에 연결하려고 시도함
    * 빈 타입들이 서로 연관될 때 일이 복잡해짐
    * 같은 인터페이스를 구현한 클래스가 여러개이고 자동와이어링을 원하는 프로퍼티가 타입으로 인터페이스를 지정할 때 예외o

- Constructor(생성자)
    * 주입이 수정자가 아닌 생성자를 이용해 이루어진다는 점을 제외하면 타입에 의한 와이어링과 같음
    * 스프링은 생성자에서 가능한 많은 개수의 인수들을 일치시키려고 노력함

- default
    * 스프링은 Constructor방식과 byType방식을 자동으로 선택함
    * 빈에 기본 생성자(인자가 없음)가 있다면 스프링은 byType방식을 이용하고 그렇지 않으면 Constructor방식을 이용

- no
    * 이것이 기본 값임

<bean id="target" autowire="byName" class="com.apress.prospring5.ch3.xml.Target" lazy-init="true" />

* lazy-init="true" : 최초시작 시점이 아닌 처음 요청되었을 때만 빈 인스턴스를 생성
* 스프링이 자동와이어링을 해야 할 빈을 알지 못하면 명시적메시지와 함께 UnsatisfiedDependencyException을 발생
* 해당 예외는 빈을 발견했지만, 어떤 빈을 어디에 써야 할 지 모를 때 발생
* 해결 방법 1 : 빈 정의에서 primary 애트리뷰트 값을 true로 설정해 스프링이 자동와이어링을 할 때 해당 빈을 먼저 사용

<bean id="target" class="com.apress.prospring5.ch3.xml.Target" primary="true" />
 
* 그러나 primary 애트리뷰트 이용방식은 오직 두 개의 빈 타입만 존 재하는 경우에 사용 가능
* 애너테이션일 경우 두 개 이상 빈이 존재한다면 @Qualifier("빈이름") 애너테이션을 사용하는 것이 좋음
* 해결 방법 2 : 빈 이름을 지정하고 XML로 주입할 위치를 구성하는 방법

* **@Lazy 애너테이션** 사용
* @Lazy는 처음 접근이 일어날 때 인스턴스가 생성될 빈을 정의하려고 클래스에 적용하는 애너테이션
* 스테레오 타입 애너테이션을 사용하면 하나의 빈에 하나의 구성만 만들 수 있어 각 타입의 빈이 하나씩만 있어서 빈의 이름이 중요하지 않음
* 때문에 애너테이션을 이용해 구성할 때 기본 자동와이어링 방식은 byType임
* 빈 타입이 여러 개 있을 경우 이름을 기반으로 자동와이어링하게 지정할 수 있다는 것은 매우 유용함

```java
@Component("gigi")
@Lazy
public class TrickyTarget {
    Foo fooOne;
    Foo fooTwo;
    Bar bar;

    public TrickyTarget() {
        System.out.println("Target.constructor()");
    }
...
}
```

<br>

3.6.1 **자동 와이어링을 사용**하는 경우
* 자동와이어링은 소규모 애플리케이션에서 시간을 절약할 수 있지만 많은 나쁜 사례로 이어질 수 있음
* 대규모 애플리케이션에서는 유연성이 떨어짐
* byName을 사용하는 것이 좋은 것처럼 보일 수 있지만, 이 방법은 사용자가 자동와이어링 기능의 장점을 얻으려고 클래스에 인위적인 프로퍼티 이름을 지정하는 결과로 이어짐
* 중요한 애플리케이션이라면 자동와이어링을 사용하지 말아야함

<br>

3.7 **빈 상속 설정**하기
* 때로는 같은 타입의 빈이나 공유 인터페이스를 구현하는 빈을 여러 개 정의할 필요가 있음
* 스프링은 ApplicationContext 인스턴스에 존재하는 다른 빈의 프로퍼티 설정을 상속받을 수 있도록 <bean>정의를 제공함

<bean id="parent" class="com.apress.prospring5.ch3.xml.Singer" p:name="John Mayer" p:age="39" />
<bean id="child" class="com.apress.prospring5.ch3.xml.Singer" parent="parent" p:age="0"/>

* 이 코드에서 child 빈을 위한 <bean>태그에서 parent라는 별도의 애트리뷰트를 볼 수 있는데, 이는 스프링이 parent 빈을 child 빈의 부모로 간주해 구성을 상속해야 함을 나타냄
* ApplicationContext에서 부모 빈 정의를 룩업할 수 없게 하려면 parent 빈을 선언할 때 <bean>태그에 abstract="true" 애트이뷰트를 추가할 수 있음
* 자식 빈은 부모 빈에서 생성자 인수와 프로퍼티 값을 모두 상속하므로 빈 상속과 함께 두 가지 주입 방식을 모두 사용할 수 있음
* 공유 프로퍼티 값을 사용해 동일한 값의 빈을 여러 개 선언해야 할 때, 복사와 붙여넣기로 값을 공유하는 방식은 사용하지 말아야함
* 대신 구성에서 상속 계층 구조로 설정해야함
* 빈 구성을 상속할 때에는 자바 상속 계층 구조와 일치시킬 필요가 없다는 사실을 기억해야 함


