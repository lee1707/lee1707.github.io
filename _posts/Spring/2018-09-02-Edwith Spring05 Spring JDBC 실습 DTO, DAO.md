---
layout: post
title: Edwith Spring 05 - Spring JDBC 실습 DTO, DAO
category: Spring
tags: [spring, springjdbc, dao, dto]
---

>[Edwith Spring - Spring JDBC 실습](https://www.edwith.org/boostcourse-web/lecture/20661/)을 보고 학습용으로 정리한 내용입니다.


# DTO란? 데이터를 갖고 다니는 하나의 가방

DTO란 Data Transfer Object의 약자.
계층간 데이터 교환을 위한 자바빈즈이다.

여기서의 계층이란 컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계층을 의미.

일반적으로 DTO는 로직을 가지고 있지 않고, 순수한 데이터 객체이다.

필드와 getter, setter를 가진다. 추가적으로 toString(), equals(), hashCode()등의 Object 메소드를 오버라이딩 할 수 있다.


# DAO란?

DAO란 Data Access Object의 약자로 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체이다.

보통 데이터베이스를 조작하는 기능을 전담하는 목적으로 만들어진다.


# ConnectionPool 이란?

DB연결은 비용이 많이 든다.
커넥션 풀은 미리 커넥션을 여러 개 맺어 둔다.
커넥션이 필요하면 커넥션 풀에게 빌려서 사용한 후 반납한다.


![Spring_JDBC__DAO_.png](../public/img/spring/Spring_JDBC__DAO_.png)


# 실제 실행되는 순서

Spring 컨테이너인 Application Context는 설정파일로 ApplicationConfig라는 클래스를 읽어들인다

ApplicationConfig에는 componentScanAnnotation이 DAO클래스를 찾도록 설정한다.

찾은 모든 DAO클래스는 Spring 컨테이너가 관리하게 된다.


Application Context는 DBConfig 클래스를 import하게 된다.
DBConfig클래스에서는 데이터 소스와 트랜잭션 매니저 객체를 생성한다.

DAO는 필드로 NamedParameterJdbcTemplate과 SimpleJdbcInsert를 가지게 된다.

두개의 객체는 모두 Data Source를 필요로 한다.
두개의 객체 모두 SQL의 실행을 편리하게 하도록 Spring JDBC에서 제공하는 객체이기 때문에
DB연결을 위해서 내부적으로 DataSource를 사용하기 때문이다.


이 두개의 객체는 RoleDao 생성자에서 초기화를 하게된다.
RoleDao 생성자에서 초기화된 두개의 객체를 이용해서 RoleDao의 메서드를 구현하게 된다.

Spring JDBC를 사용하는 사용자는 파라미터와 SQL을 가장 신경써야 한다.

SQL은 RoleDao SQL의 상수로 정의를 해놓음으로써,
나중에 SQL이 변경될 경우에 좀더 편하게 수정할 수 있도록 했다.
한건의 Role정보를 저장하고 전달하기 위한 목적으로 RoleDTO가 사용되고 있다


# 실습

## maven에 추가해야할것
*어떤 것을 추가해야되지?*라고 생각하면서 추가하기

### 1. Java1.8을 사용하기 위해.(dependencies아래에)

```
<build>
  	<plugins>
  		<plugin>
  		<groupId>org.apache.maven.plugins</groupId>  			
  			<artifactId>maven-compiler-plugin</artifactId>
  			<version>3.6.1</version>
  			<configuration>
  				<source>1.8</source>
  				<target>1.8</target>
  			</configuration>
  		</plugin>
  	</plugins>
  </build>
```


### 2. Spring을 사용할 테니 spring-context가 필요함

```
<!-- Spring -->
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
```


### 3. 현재 spring jdbc를 사용하기 때문에 spring-jdbc와 spring-tx부분을 추가해야함

```
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>

```


### 4. spring버전은 각각 다를수 있기 때문에 properties에 spring version을 추가함,
인코딩도 properties에 추가함(properties는 dependencies위에)

```
<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<spring.version>4.3.5.RELEASE</spring.version>
	</properties>
```


### 5. MySQL 데이터베이스를 사용할것이기 때문에 MySQL에서 제공하는 드라이버가 필요

```
<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.45</version>
		</dependency>
```

### 6. DataSource를 사용해야 해야 되는데 Apache에서 제공하는 commons-dbcp2를 사용할 것임

```
<!-- basic data source -->
<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-dbcp2</artifactId>
			<version>2.1.1</version>
		</dependency>
```

### 7. 라이브러리들이 추가가 되었으면 Maven-Update Project를 수행하기

### 8. Spring JDBC를 사용할 준비 완료



## 이후 설정
ApplicationContext는 Spring이 제공해주는클래스.
ApplicationContext한테 어떤 설정들을 읽어들여서 공장을 만들어줘요 하는 부분이 ApplicationConfig이다.

### 1. 이러한 ApplicationConfig 클래스를 만들자. 


`@Configuration`과 `@Import`추가

`@Import` - 설정파일을 여러개로 나눠서 작성할 수 있음. 
	하나의 클래스가 모든 정보를 가지고 있으면 유지보수하기 힘듦.<br/>
	DB관련 설정은 DBConfig라는 파일에다가 따로 작성할 생각으로
	이렇게 Import라는 어노테이션을 이용해서 설정을 해줌

### 2. DBConfig에는
`@Configuration`과
`@EnableTransactionManagement`


Spring JDBC를 이용할 때 DataSource를 통해서 DB에 접속하는 부분을 얻어내기로 함. 
어떤 클래스가 필요하냐면 DataSource를 생성할 수 있는 클래스가 필요함

`@Bean`에다가 Bean등록하는 방법은 메서드 쓰는 방법과 유사하다
메서드의 이름은 Bean등록할 때 id로 지정하는 이름과 같다
DataSource객체는 커넥션을 관리할 것이기 때문에 JDBC 드라이버, url, username, password를 알아야만
생성할 수 있다


여기까지하면 최소한의 설정이 끝남
여기까지 했을 때 이런 정보를 잘 읽어서 데이터베이스에 접속이 되는지 확인해봐야함


## 데이터베이스 접속 확인하기 

### 1.DataSourceTest클래스를 만듦(다른 패키지에)
Bean들을 여러개 등록했으니까 **Spring 컨테이너**가 Bean들을 생성하고 Bean을 관리해야함

**ApplicationContext공장을 하나 새로 생성해야함**

ApplicationConfig에서 설정을 읽어들임(얘가 DBConfig도 Import하기에 얘만 읽어들이면됨)

```
ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
```

공장을 통해서 얻어내야 하는건 DataSource객체임

`DataSource ds = ac.getBean(DataSource.class);`
이렇게 해서 DataSource를 구현하고 있는 실제 객체를 얻어낼 수 있음

Connection을 잘 얻어오느냐를 해보고 싶기 때문에
Connection객체를 DataSource를통해서 얻어오면됨

```
Connection conn = null;
		try {
			conn = ds.getConnection();
			if (conn != null) {
				System.out.println("접속성공");
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			if(conn!=null) {
				try {
					conn.close();
				}catch(Exception e) {
					e.printStackTrace();
				}
			}
		}
```

ApplicationConfig.class에 들어있는 설정파일을 읽어서 ApplicationContext를 생성,
**ApplicationContext가 IoC/DI 컨테이너임**

이 컨테이너가 가지고 있는 getBean()을 이용해서 DataSource라는 클래스를 요청,
DataSource를 구현하고 있는 객체를 나한테 리턴해줌
DataSource한테 getConnection이라는 메서드를 이용해서 Connection을 얻어옴


### 2.DTO 클래스를 만들고,
RoleDaoSqls라는 클래스를 만듦.

query를 만들어서 사용할 건데 상수로 지정해줄것임
상수는 모든 글자를 대문자로 쓰는 것이 관례임

```
public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";
```

### 3.DAO 클래스를 만드는데, DAO 객체에는 저장소의 역할을 한다는 의미에서 @Repository라는 어노테이션을 붙임

DAO를 실행할 때 NamedParameterJdbcTemplate, SimpleJdbcInsert객체를 이용함

이 객체들은 Spring JDBC가 실제 JDBC를 편하게하기 위해서 이미 구현해 놓은 객체라고 생각하면됨
