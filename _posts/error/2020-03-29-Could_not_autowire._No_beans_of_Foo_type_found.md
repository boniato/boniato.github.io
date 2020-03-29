### "Could not autowire. No beans of 'postsRepository' type found." ###
<img src="/assets/images/posts/error/2020-03-29-Could_not_autowire_01.png" width="100%" height="100%" alt="error">

유닛 테스트 코드를 작성하니 해당 bean을 찾을 수 없어 Could not autowire 에러나 났다.
왜 유닛 테스트 코드에서만 찾지 못할까 고민하며 구글링하니 아래와 같은 해결책을 찾을 수 있었다. 

* 올바른 패키지를 가져 왔는지 확인하기
* autowire할 대상이 interface(MyService)가 아닌 구체적인 class(MyServiceImpl)로 하기.

나 같은 경우에는 패키지가 잘못되어 이를 정정해줌으로써 해결할 수 있었다.

<img src="/assets/images/posts/error/2020-03-29-Could_not_autowire_02.png" width="100%" height="100%" alt="error">


참조) https://stackoverflow.com/questions/21878714/how-to-write-junit-test-with-spring-autowire
