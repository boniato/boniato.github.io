## Microservice란?

소프트웨어 아키텍쳐

* Antifragile
  * 시스템 변화에 바로 적용할 수 있고 비용 또한 저렴하다. ex) dev ops

  * auto scaling
    * 특징 : 자동 확장성을 갖는다.
    * 시스템을 구성하고 있는 인스턴스를 하나의 오토 스케일링 그룹으로 묶은 다음 그룹에서 유지되어야 하는 최소 인스턴스를 지정할 수 있고, 사용량에 따라 자동으로 인스턴스의 양을 증가시킬 수 있는 환경을 말한다.
    * ex.) 온라인 쇼핑몰에서 특수한 이벤트가 있는 달에 서버의 운영 개수를 늘리고, 비수기에는 서버의 운영 개수를 다시 줄이는 작업.
    * 이러한 작업은 관리자나 운영자에 의해 수작업 되는 것이 아니라 CPU, 메모리, 네트워크, 데이터베이스 사용량이나 접근에 따라서 자동으로 처리할 수 있는 것이 오토 스케일링.
  * Microservies
    * 클라우드 네이티브 아케텍쳐, 어플리케이션의 핵심. 기존의 시스템들이 하나의 거대한 형태로 구축되어 서비스 되었다고 하면, 마이크로서비스라는 것은 전체 서비스를 구축하고 있는 개별적인 모듈이나 기능을 동적으로 개발하고 배포하고 운영할 수 있도록 세분화된 서비스
    * ex.) 넷플릭스, 아마존  
  * Chaos engineering
    * 시스템이 급격하고 예측하지 못한 상황이라도 견딜 수 있고 신뢰성을 쌓기 위해 운영중인 소프트웨어 시스템을 실행하는 방법이나 규칙.
    * 시스템이 어떤 변동이나 예견된 불확실성이나 예견되지 않은 불확실성에 대해서도 안정적인 서비스를 제공할 수 있도록 구축되어야 한다.
  * Continuous deployments
    * 지속적인 통합, 지속적인 배포. 클라우드 네이티브 어플리케이션은 수십개~수백개 이상의  마이크로 서비스로 도메인이 분리되어 개발되어진다. 하나의 어플리케이션을 구성하는 수백개의 (각각의) 서비스들을 일일히 빌드하고 테스트하고 배포함에 있어서 자동화된 시스템을 구축하고 하나의 작업에서 다른 작업으로 연계되는 과정을 파이프라인으로 연결시켜 놓으면 작은 변화 뿐만 아니라  전체적인 시스템의 업그레이 등을 빠르게 적용시킬 수 있다.

* Cloud Native Architecture
  * 확장 가능한 아키텍처
    * 확장된 서버로 시스템의 부하 분산, 가용성 보장
    * 시스템 또는 서비스 애플리케이션 단위의 패키지(컨테이너 기반 패키지)
  * 탄력적 아키텍처
    * 어플리케이션을 구성하는 각 기능을 하나의 분리된 서비스로 개발하고 이렇게 분리된 서비스들을 통합하고 배포하기 까지 걸리는 시간을 CI, CD라는 자동화 파이프라인을 통해서 처리함으로써 시스템 환경에 적용하는 시간을 단축시킴.
    * 서비스 생성-통합-배포, 비즈니스 환경 변화에 대응 시간 단축 
  * 장애 격리
    * 특정 서비스에 오류가 발생해도 다른 서비스에 영향 주지 않음
  
* Cloud Native Application
  * 지속적인 통합, CI(Continouos Integration)
    * 통합 서버, 소스관리(SCM), 빌드 도구, 테스트 도구
    * ex.)Jenkins, Team CI, Travis CI, 소스 푸시 후 자동 빌드
  * 지속적 배포(CD)
    * 지속적인 전달과 지속적인 배포 
    * Continuous Delivery
    * Continuous Deployment
    * Pipe line


* Spring Cloud
  * Overview
    * Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems (e.g. configuration management, service discovery, circuit breakers, intelligent routing, micro-proxy, control bus, one-time tokens, global locks, leadership election, distributed sessions, cluster state). Coordination of distributed systems leads to boiler plate patterns, and using Spring Cloud developers can quickly stand up services and applications that implement those patterns. They will work well in any distributed environment, including the developer’s own laptop, bare metal data centres, and managed platforms such as Cloud Foundry.
  * https://spring.io/projects/spring-cloud
  * Spring Boot + Spring Cloud
  

