### "java.sql.SQLNonTransientConnectionException: Could not connect to address=(host=springboot2-webservice.ctwad1edhzet.ap-northeast-2.rds.amazonaws.com)(port=3306)(type=master) : connect timed out" ###

<img src="/assets/images/posts/error/2020.05.06-aws_rds_connect_fail_application.png" width="100%" height="100%" alt="error">

EC2 서버에 접속하여 애플리케이션을 배포하려니 위와 같은 에러가 났다.

결론부터 말하자면 보안그룹(Security Groups)에 Inbound rules에 EC2 서버 IP를 추가하지 않아서 났던 것이였다.

### 1. EC2 서버에서 RDS DB 인스턴스에 연결되었는지 확인하기

먼저 EC2 서버에서 RDS DB 인스턴스에 연결되었는지 다음 명령어중 하나를 실행하여 확인해보자.

```java
telnet <RDS endpoint> <port number>

nc <RDS endpoint> <port number>
```

간단히 nc 명령어를 통해 RDS DB 인스턴스에 연결이 실패된 것을 다음과 같이 확인 할 수 있다.

<img src="/assets/images/posts/error/2020.05.06-aws_rds_connect_fail.png" width="100%" height="100%" alt="error">

### 2. 보안그룹(Security Groups)에 Inbound rules에 EC2 서버 IP 추가하기

EC2 서버에서 **ip addr show**나 **ifconfig** 명령어로 IP 주소를 확인해보자.<br>
eth0에서 inet IP주소를 보면 된다.

AWS에서 보안그룹의 Inbound rules에서 EC2 서버 IP로 연결할 수 있는 rule을 추가한다. 위치는 다음과 같다.

* AWS - RDS - Databases - DB identifier - VPC security groups - Security Groups - Inbound rules

Type은 MYSQL/Aurora, Source는 Custom으로 두고 EC2 서버 IP를 입력한 후 저장한다.

<br>

다시 nc 명령어를 이용하여 RDS DB 인스턴스가 연결되는지 확인한다.

<img src="/assets/images/posts/error/2020.05.06-aws_rds_connect_success.png" width="100%" height="100%" alt="error">

연결이 성공하였다면 succeeded! 메시지를 볼 수 있다.

<br>

아래는 Amazon RDS DB 인스턴스에 연결할 때 발생하는 문제를 해결하는 방법을 가이드한 링크이다.

https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html
