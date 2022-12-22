![7](https://user-images.githubusercontent.com/59721896/179404482-e8218ca7-997a-4969-b632-7f06673a988a.png)

<div align=center>
<h2>개발자를 위한 레시피 📓</h2>
<br>
<a href="https://hits.seeyoufarm.com"><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Flcomment%2Fdevelopment-recipes&count_bg=%23CA9ACC&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false"/></a>
</div>
<h1></h1>

### **👨‍👦‍👦 Collaborators**

| <img src="https://avatars.githubusercontent.com/u/56003992?v=4" width="150"> | <img src="https://avatars.githubusercontent.com/u/86272688?v=4" width="150"> | <img src="https://avatars.githubusercontent.com/u/86949394?v=4" width=150> |
| :--------------------------------------------------------------------------: | :--------------------------------------------------------------------------: | :------------------------------------------------------------------------: |
|                   [@lcomment](https://github.com/lcomment)                   |                   [@YongsHub](https://github.com/YongsHub)                   |                  [@himJJong](https://github.com/himJJong)                  |

</br>

### **🤝 Contributors**

<a href="https://github.com/lcomment/development-recipes/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=lcomment/development-recipes" />
</a>

</br>

### **✍️ Commit Convention**

→ [**`tag`**]: [**`content`**]  
ex) 💡create: 자바스크립트의 async/await 이론 작성

| 태그 |   `💡create`   |  `👆add`  | `🛠revise` |    `🧹delete`     |  `🎀style`  |
| :--: | :------------: | :-------: | :-------: | :---------------: | :---------: |
| 의미 | 새로운 글 작성 | 내용 추가 | 내용 수정 | 내용 및 파일 삭제 | 스타일 변경 |

<br>

### ❗️원하는 자료나 잘못된 내용은 `Issue`와 `Pull Request`로 남겨주세요❗️

---

</br>  
  
## **Ch01. Computer Science**
- ### Operating System
    - [운영체제란?](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/operatingSystem.md)
    - [프로세스와 스레드](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/process_thread.md)
    - [프로세스 주소 공간과 PCB](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/pas_pcb.md)
    - [프로세스의 문맥 교환 (Context Switching)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/contextSwitching.md)
    - [CPU 스케줄링](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/cpuScheduling.md)
    - [교착상태 (DeadLock)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/deadlock.md)
    - [경쟁상태 (Race Condition)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/raceCondition.md)
    - [세마포어(Semaphore)와 뮤텍스(Mutex)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Operating%20System/semaphore_mutex.md)
- ### Network
    - [OSI 7 Layer](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/osi.md)
    - [TCP와 UDP](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/tcpNudp.md)
    - [3-way Handshacking과 4-way Handshacking](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/3wayN4way.md)
    - TCP/IP
      - [흐름제어 (Flow Control)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/flowControl.md)
      - [혼잡제어 (Congestion Control)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/congestionControl.md)
    - [Blocking/Non-Blocking과 Synchronous/Asynchronous](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/blockNnonNsyncNasync.md)
    - [HTTP와 HTTPS](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/httpNhttps.md)
    - [로드 벨런싱 (Load-Balancing)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/loadBalancing.md)
    - [포트와 포트포워딩](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/port-forwarding.md)
    - [프록시 서버](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Network/proxy.md)
- ### Database
    - [데이터베이스 설계](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/dbDesign.md)
    - [관계형 데이터베이스 (RDB)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/rdb.md)
    - [RDB에서 꼭 지켜야 하는 7가지 네이밍 규칙](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/rdbNaming.md)
    - [키 (Key)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/key.md)
    - [식별 관계와 비식별 관계](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/identifyRelationship.md)
    - [데이터베이스 정규화 (Normalization)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/normalization.md)
    - [트랜잭션(Transaction)과 ACID](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/transaction.md)
    - [트랜잭션의 격리 수준 (Transaction Isolation Level)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/transactionIsolationLevel.md)
    - [인덱스와 인덱스를 사용하면 안되는 경우](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/index.md)
    - SQL
      - [DDL, DML, DCL, TCL](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/SQL/ddl_dml_dcl_tcl.md)
      - [SQL 기본 문법](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/SQL/sqlGrammar.md)
      - [조인 (Join)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/SQL/join.md)
    - [RDBMS와 NoSQL](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Database/sqlAndNoSql.md)
- ### Software Engineering
    - [SOLID](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/SW%20Engineering/solid.md)
    - [팩토리 패턴 (Factory Pattern)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/SW%20Engineering/factoryPattern.md)
    - [MVC 패턴 (MVC Pattern)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/SW%20Engineering/mvc.md)
- ### Data Structure
    - [ArrayList와 LinkedList](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/list.md)
    - [해시(Hash)와 해시충돌(Hash Collision)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/hash.md)
    - [트리(Tree)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/tree.md)
      - [이진 트리(Binary Tree)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/binaryTree.md)
      - [B-Tree, B*Tree, B+Tree](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/bTree.md)
    - [힙 (Heap)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Data%20Structure/heap.md)
- ### Algorithm
    - [그리디 알고리즘 (Greedy)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/greedy.md)
    - [완전탐색 (Brute-Force)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/bruteForce.md)
      - [순열 (Permutation)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/permutation.md)
      - [조합 (Combination)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/combination.md)
    - 그래프 탐색
      - [깊이 우선 탐색 (DFS)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/dfs.md)
      - [너비 우선 탐색 (BFS)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/bfs.md)
    - [단일 출발지 최단 경로 구하기 (Single-Source Shortest Path)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/shortestPath.md)
      - [다익스트라 알고리즘 (Dijkstra)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/dijkstra.md)
      - [벨만-포드 알고리즘 (Bellman-Ford)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/bellmanFord.md)
    - [최장 증가 수열 (LIS)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/lis.md)
    - [카탈란 수 (Catalan Number)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/catalan.md)
    - [문자열 탐색](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/substring.md)
      - [KMP 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/kmp.md)
      - [Rabin-karp 알고리즘](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/rabinKarp.md)
    - [최소 신장 트리 (MST)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/mst.md)
      - [크루스칼 알고리즘 (Kruskal)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/kruskal.md)
    - 기하 알고리즘
      - [CCW 알고리즘 (Counter Clockwise)](https://github.com/lcomment/development-recipes/blob/main/Computer%20Science/Algorithm/ccw.md)
</br>

## **Ch02. Language**

- ### Java
  - [Java의 역사와 특징](https://github.com/lcomment/development-recipes/blob/main/Language/Java/aboutJava.md)
  - [Java의 자료형](https://github.com/lcomment/development-recipes/blob/main/Language/Java/typeOfJava.md)
  - [Java의 작동 원리](https://github.com/lcomment/development-recipes/blob/main/Language/Java/executionJava.md)
  - [JVM의 메모리 영역](https://github.com/lcomment/development-recipes/blob/main/Language/Java/jvm.md)
    - [Garbace Collection (GC)](https://github.com/lcomment/development-recipes/blob/main/Language/Java/gc.md)
  - [Scanner와 BufferedReader](https://github.com/lcomment/development-recipes/blob/main/Language/Java/input.md)
  - [String 클래스](https://github.com/lcomment/development-recipes/blob/main/Language/Java/string.md)
  - [StringBuilder와 StringBuffer](https://github.com/lcomment/development-recipes/blob/main/Language/Java/stringBuilder.md)
  - [컬렉션 프레임워크 (Collection Framework)](https://github.com/lcomment/development-recipes/blob/main/Language/Java/collectionFramework.md)
    - [ArrayList와 LinkedList](https://github.com/lcomment/development-recipes/blob/main/Language/Java/list.md)
    - [HashSet과 TreeSet](https://github.com/lcomment/development-recipes/blob/main/Language/Java/set.md)
    - [우선순위 큐(PriorityQueue)](https://github.com/lcomment/development-recipes/blob/main/Language/Java/priorityQueue.md)
    - [Comparator와 Comparable](https://github.com/lcomment/development-recipes/blob/main/Language/Java/compare.md)
    - [Iterator와 ListIterator](https://github.com/lcomment/development-recipes/blob/main/Language/Java/iteratorNlistiterator.md)
  - Java 8
    - [Java의 Lambda](https://github.com/lcomment/development-recipes/blob/main/Language/Java/lambda.md)
    - [Java의 Stream](https://github.com/lcomment/development-recipes/blob/main/Language/Java/stream.md)
      - [왜 Stream Api를 사용해야 하는가?](https://github.com/lcomment/development-recipes/blob/main/Language/Java/aboutStream.md)
  - [Java의 버전별 차이](https://github.com/lcomment/development-recipes/blob/main/Language/Java/javaVersion.md)
- ### Python
  - 라이브러리
    - [selenium](https://github.com/lcomment/development-recipes/blob/main/Language/Python/selenium.md)
- ### Javascript
  - [ES2015의 등장](https://github.com/lcomment/development-recipes/blob/main/Language/JavaScript/ES2015%2B.md)
  - [async/await](https://github.com/lcomment/development-recipes/blob/main/Language/JavaScript/asyncAwait.md)
- ### Typescript
  - [타입스크립트 클린코드 작성법](https://github.com/lcomment/development-recipes/blob/main/Language/Typescript/typescript_cleancode.md)

</br>

## **Ch03. Programming**

- ### Test
  - [테스트와 테스트 코드](https://github.com/lcomment/development-recipes/blob/main/Programming/Test/test.md)
  - [Given-When-Then 패턴과 FIRST 전략](https://github.com/lcomment/development-recipes/blob/main/Programming/Test/testcodePattern.md)
  - [테스트 주도 개발방법론 (Test-Driven Development)](https://github.com/lcomment/development-recipes/blob/main/Programming/Test/tdd.md)
- ### Object-Orientation
  - [객체 지향 프로그래밍 (Object-Oriented Programming)](https://github.com/lcomment/development-recipes/blob/main/Programming/Object-Orientation/oop.md)
  - [객체 지향 개발방법론 (Object-Oriented Design)](https://github.com/lcomment/development-recipes/blob/main/Programming/Object-Orientation/ood.md)

</br>

## **Ch04. Cloud와 DevOps**

- [CI/CD란?](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/cicd.md)
- ### Git
  - [Github란?](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/git/github.md)
  - [깃 플로우란? (Git-Flow)](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/git/gitflow.md)
- ### AWS
  - [Cloud와 AWS](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/aws/cloudComputing.md)
  - [AWS RDS](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/aws/rds.md)
  - [AWS Auto Scaling](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/aws/autoScaling.md)
  - [AWS ECS, ECR](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/aws/ecs_ecr.md)
- ### Docker
  - [Docker란?](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/docker/docker.md)
- ### Terraform
  - [Terraform이란?](https://github.com/lcomment/development-recipes/blob/main/Cloud%20and%20DevOps/terraform/terraform.md)

</br>

## **Ch05. Web**

- [웹의 동작원리](https://github.com/lcomment/development-recipes/blob/main/Web/web.md)
- [CORS란?](https://github.com/lcomment/development-recipes/blob/main/Web/cors.md)
- [Param, Query, Body의 차이](https://github.com/lcomment/development-recipes/blob/main/Web/requestObject.md)
- [쿠키(Cookie)와 세션(Session)](https://github.com/lcomment/development-recipes/blob/main/Web/cookieNsession.md)
- [JWT (Json Web Token)](https://github.com/lcomment/development-recipes/blob/main/Web/jwt.md)
- [OAuth 2.0](https://github.com/lcomment/development-recipes/blob/main/Web/OAuth2.0.md)
- [REST API](https://github.com/lcomment/development-recipes/blob/main/Web/restApi.md)
- [PUT과 PATCH의 차이](https://github.com/lcomment/development-recipes/blob/main/Web/putNpatch.md)
- [Forward 방식과 Redirect 방식](https://github.com/lcomment/development-recipes/blob/main/Web/redirectNforward.md)
- [웹 크롤링 (Web Crawling)](https://github.com/lcomment/development-recipes/blob/main/Web/crawling.md)
- [ORM (Object-Relational Mapping)](https://github.com/lcomment/development-recipes/blob/main/Web/orm.md)

</br>

## **Ch06. Web - Spring**

- ### Spring & Spring Boot
  - [스프링 프레임워크란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/aboutSpring.md)
  - [스프링과 스프링부트](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/springNboot.md)
  - [스프링 컨테이너와 핵심원리](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/springContainer.md)
  - [BeanFactory와 BeanDefinition](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/aboutBeanFactory.md)
- ### Srping MVC
  - [Spring MVC Framework란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/Spring%20MVC/springMvc.md)
  - [@Controller와 @RestController](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/Spring%20MVC/controller.md)
  - [DTO와 DAO, VO](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/Spring%20MVC/dao_dto_vo.md)
- ### Spring Security
  - [Spring Security란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/springSecurity.md)
  - [Google 간편 로그인 연동하기](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/googleLogin.md)
- ### JPA
  - [JPA란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/jpa.md)
  - [JPQL (Java Persistence Query Language))](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/jpql.md)
  - [더티 체킹 (Dirty Checking)](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/dirtyChecking.md)
- ### Spring REST Docs
  - [Spring REST Docs란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/Spring%20REST%20Docs/springRestDocs.md)
- ### Test
  - [JUnit이란?](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/testCode.md)
- ### 추가
  - [mavenCentral과 jcenter](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/mavenCentral.md)
  - [Java 개발의 필수, Lombok](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/lombok.md)
  - [Spring Boot의 devtools의 개념과 기능](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/devtools.md)
  - [Swagger로 API 명세하기](https://github.com/lcomment/development-recipes/blob/main/Web-Spring/swagger.md)

</br>

## **Ch07. Web - Node js**

- ### Node js
  - [Node란?](https://github.com/lcomment/development-recipes/blob/main/Web-Node/Node%20js/nodejs.md)
- ### Express js
  - [express란](https://github.com/lcomment/development-recipes/blob/main/Web-Node/Express%20js/express.md)
  - middleware
    - [body-parser](https://github.com/lcomment/development-recipes/blob/main/Web-Node/Express%20js/middleware/body-parser.md)
- ### Nest js
  - Overview
    - [MiddleWare](https://github.com/lcomment/development-recipes/blob/main/Web-Node/Nest%20js/Overview/middleware.md)
    - [Exception Filters](https://github.com/lcomment/development-recipes/blob/main/Web-Node/Nest%20js/Overview/exceptionfilters.md)
