## 오늘의 개인 공부

### Q1. JVM, JRE, JDK의 차이?

```text
JVM은 Java Virtual Machine의 줄임말이다. OS에 종속받지 않고 Java를 실행되기 위해서는 OS 위에서 Java를 실행시킬 무언가가 필요한데 그것이 바로 JVM이다.

JRE는 Java Runtime Environment의 줄임말이다. JVM + Java 클래스 라이브러리 등으로 구성되어 있고, 컴파일된 Java 프로그램을 실행하는데 필요한 패키지다.

JDK는 Java Development Kit로, 자바 개발 키트이다. Java를 사용하기 위해 필요한 모든 기능을 갖춘 Java용 SDK(Software Development Kit)이고, JDK는 JRE와 javac, jdb, javadoc과 같은 도구를 포함하고 있다.
```

### Q2. WAS와 WS의 차이?

```text
Web Server, WS는 정적인 웹 리소스(HTML, CSS, JavaScript 등)를 서비스하는 데 특화된 서버 소프트웨어를 의미한다. 웹 서버는 클라이언트의 HTTP 요청을 받아 해당 요청에 맞는 정적 컨텐츠를 반환한다. 중요한 점은 🍀주로 웹 어플리케이션의 비즈니스 로직을 처리하지 않고 단순히 클라이언트에게 정적인 웹 페이지를 제공하는 데 사용된다는 점이다.🍀

대표적인 웹 서버로는 Nginx, Apache 등이 있다.

Web Application Server, WAS는 말 그대로 웹 어플리케이션을 실행하기 위한 서버 소프트웨어를 의미한다. 클라이언트의 요청에 따라 동적인 웹 페이지를 생성하고 데이터베이스와의 상호작용, 트랜잭션 처리, 보안, 세션 관리 등 웹 어플리케이션의 핵심 비즈니스 로직을 수행하는 역할을 담당한다.

대표적인 웹 어플리케이션 서버로는 Apache Tomcat, Red Hat JBoss 등이 있다.
```

### Q3. EJB가 도대체 뭘까?

```text
앞서, Java EE는 간편하고 확장 가능하며 안전한 서버측 자바 어플리케이션을 위한 산업 표준이다. Java EE에는 웹 어플리케이션 개발을 위한 Servlet, JSP 외에도 JMS(Java Message Service), JDBC(Java Database Connectivity), JTA(Java Transaction API), JNDI(Java Naming and Directory Interface), 메일 지원(Java Mail API)등 많은 기능들이 있다.

그 중에 EJB란 분산 어플리케이션을 지원하는 컴포넌트 기반의 객체다. JSP가 화면 로직을 처리한다면 EJB는 업무 로직을 처리하는 역할을 한다. EJB는 비즈니스 객체들을 관리하는 컨테이너 기술 등을 포함하고 있었고, 개념이 워낙 획기적이었기 떄문에 Java 진영에서 표준으로 인정한 기술이었다.

❌하지만, EJB의 다양한 기술들을 사용하기 위해서는 EJB 스펙을 사용해야 했고 그로 인해 비즈니스 로직보다 EJB 컨테이너 설정을 위해 더 많은 시간을 써야 했다.

개발자들은 오히려 평범한 자바 클래스(POJO, Plain Old Java Object)를 사용하려는 경향을 보였고, 복잡한 EJB의 컨테이너를 대체하기 위해서 등장한 것이 바로 Spring 컨테이너이다.
```
