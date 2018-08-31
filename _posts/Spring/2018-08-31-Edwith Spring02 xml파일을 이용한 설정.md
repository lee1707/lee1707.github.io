---
layout: post
title: Edwith Spring 02 - xml파일을 이용한 설정
category: Spring
tags: [spring, container, ioc, di, xml]
---

> [Edwith Spring02 - xml파일을 이용한 설정](https://www.edwith.org/boostcourse-web/lecture/20657/)

1. Maven project만들기
	QuickStart로 만든 다음 pom.xml 파일에 JDK를 사용하기 위한 플러그인 설정을 추가한다.
	- maven-compiler-plugin 추가함(Java 1.8로 만들기 위해)
	 `<build>
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
	  </build>	`


	- Maven update
	- project-properties-java compiler에서 버전 확인


2. Bean class 만들기
	src/main/java아래에 프로젝트 패키지 아래에 Bean을 만들어줌.
	예를 들면 이런 식임
	
	package kr.or.connect.diexam01;
	
	//빈클래스
	public class UserBean {
		
		//필드는 private한다.
		private String name;
		private int age;
		private boolean male;
		
		//기본생성자를 반드시 가지고 있어야 한다.
		public UserBean() {
		}
		
		public UserBean(String name, int age, boolean male) {
			this.name = name;
			this.age = age;
			this.male = male;
		}
	
		// setter, getter메소드는 프로퍼티라고 한다.
		public void setName(String name) {
			this.name = name;
		}
	
		public String getName() {
			return name;
		}
	
		public int getAge() {
			return age;
		}
	
		public void setAge(int age) {
			this.age = age;
		}
	
		public boolean isMale() {
			return male;
		}
	
		public void setMale(boolean male) {
			this.male = male;
		}
	
	}


- Bean class란?

예전에는 Visual 한 컴포넌트를 Bean이라고 불렀지만, 근래 들어서는 일반적인 Java클래스를 Bean클래스라고 보통 말한다.

Bean클래스의 3가지 특징은 다음과 같다.

	 1.기본생성자를 가지고 있다.
	 2. 필드는 private하게 선언한다.
	 3. getter, setter 메소드를 가진다.
getName() setName() 메소드를 name 프로퍼티(property)라고 한다. (용어 중요)


3. 이제 pom.xml 파일에 Spring공장을 만들어줌. Spring공장은 spring context임
	
	`<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	  <modelVersion>4.0.0</modelVersion>
	
	  <groupId>kr.or.connect</groupId>
	  <artifactId>diexam01</artifactId>
	  <version>0.0.1-SNAPSHOT</version>
	  <packaging>jar</packaging>
	
	  <name>diexam01</name>
	  <url>http://maven.apache.org</url>
	
	  <properties>
	    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	    <spring.version> 4.3.14.RELEASE</spring.version>
	  </properties>
	
	  <dependencies>
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
	  
	  
	   <dependency>
	      <groupId>junit</groupId>
	      <artifactId>junit</artifactId>
	      <version>3.8.1</version>
	      <scope>test</scope>
	    </dependency>
	  </dependencies>
	  
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
	</project>

Maven Dependencies아래에 spring관련 라이브러리가 추가된 것을 볼 수 있다

내가 직접하는 게 아니라 누군가 대신 뭔가를 해줘야 될 때는 반드시 규칙이 있어야함




4. Spring공장에 UserBean은 이렇게 생긴애다 라고 알려줘야함
src-main-java에 resources라는 폴더를 만들고(Spring에 정보를 줄 파일이 필요한데, source랑 파일이랑 섞여있으면 복잡하기 때문에) applicationContext.xml을 만들어줌


resources 폴더는 소스폴더임.그래서 resources폴더에서 생성한 xml파일은 자동으로 classpath로 지정이됨. JAVA 디렉토리에서 만들어진 클래스와 마찬가지로 bean디렉토리에 생성이 되어있을 것임. 그래서 ClassPathXmlApplicaionContext로 읽어서 사용할 수가 있다.


ClassPathXmlApplicationContext인스턴스가 생성될 때 생성자 파라미터로 지정된 설정파일을 읽어들인 후에(applicationContext.xml) 그 안에 선언된 bean의 정보를 다 읽어들여서 bean에 설정되어 있는 객체들을 전부 생성해서 모두 메모리에 올려줄 것임 


	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
		
	    <bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>
	</beans>


루트 element(가장 바깥쪽 태그)는 반드시 beans로 되어있어야하고, xml스키마에 대한 설정도 되어 있어야함
	`<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">`

지금 우리가 하고 싶은 것은 Spring 컨테이너한테 내가 사용할 객체를 대신 생성하게 하는 것. 이때, Spring 컨테이너가 아무런 정보를 갖고 있지 않으면 만들 수 없음. 이때 사용되는 element가 bean이라는 element임

`<bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>`


id는 userBean이라고 하고, 클래스는 내가 진짜 등록하고 싶은 클래스의 정보를 알려주면 됨(내가 만든 클래스)

이렇게 bean을 등록했다는 건, Spring이 나중에 이렇게 객체를 생성하는 것과 같은 의미임. 
`kr.or.connect.diexam01.UserBean userBean = new kr.or.connect.diexam01.UserBean();`

spring 컨테이너는 이런 객체를 하나만 생성해서 가지고 있음
이렇게 객체를 딱 하나만 가지고 있는 것을 싱글톤 패턴이라고 함.

5. 4의 설정파일을 읽어들이는 객체 생성
src/main/java의 패키지 아래에 ApplicationContextExam01 클래스를 생성함
이 클래스를 생성하는 이유는 어디선가 이 프로그램을 시작시킬 시작점이 필요하기 때문임.
Spring이 가진 공장을 만드는 코드를 쓰는 것. 

	package kr.or.whiuni.diexam01;
		
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;
		
	public class ApplicationContextExam01 {
		
	public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		System.out.println("초기화 완료!");
		
		UserBean userBean = (UserBean)ac.getBean("userBean");
		userBean.setName("kang");
		
		System.out.println(userBean.getName());
		
		UserBean userBean2 = (UserBean)ac.getBean("userBean");
		
		if(userBean == userBean2) {
			System.out.println("같은 인스턴스입니다");
		}
	}
}


- 설명 

Spring공장 중에서 ApplicationContext공장을 이용할 것임
ApplicationContext는 인터페이스이고, ApplicationContext를 구현하는 객체가 여러개 있는데 그 중 ClassPathApplicationContext라는 객체를 생성해서 사용하려고 함.

`ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");`
		

공장에게, 여기에 내가 빈 정보를 넣어놓았으니까 이 정보를 읽어들여서 공장을 세우세요 라고 알려주는 것임.

이 공장한테 얻어올 객체가 UserBean이라는 객체임. UserBean하고 선언하고 실제로 생성은 내가 안함. 공장한테 getBean()이라는 메서드를이용해서 얻어낼 것임. 그런데 이때 getBean()은 무조건 오브젝트 타입으로 리턴을 하니까 형변환이 필요함. 꺼내온 애를 UserBean으로 바꿔줌

`UserBean userBean = (UserBean)ac.getBean("userBean");`

공장은 해당 xml에서 getBean()에 “userBean”이라는 String을보냄, 그러면 이 xml파일 중에서 id가 이것과 일치하는 애가 있는 지를 찾고, 찾으면 그것과 같이 등록되어 있는 클래스 이름을 알아내고 클래스를 생성해줌. 

그 후 생성한 클래스를 리턴함. 

bean을 하나 얻어왔을 거고 얻어온 bean에다가 userBean.setName()이라는 메서드를 실행시키고 getName을 해보기

		userBean.setName("kang");
		
		System.out.println(userBean.getName());

6. getBean으로 하나 더 받아와서 아까 받아온 getBean과 같은 객체인지 확인하기
UserBean userBean2 = (UserBean)ac.getBean("userBean");
		
		if(userBean == userBean2) {
			System.out.println("같은 인스턴스입니다");
		}
	}

이런 Spring이 제공하는 공장이 만들어 내는 객체들은 한가지만 있지는 않고 다양한객체를 생성할 수 있다.

이렇게 객체를 대신생성해주고 싱클톤으로 관리해주는기능을 IoC제어의 역전이라고 한다.



### DI를 알아보자

1. src/main/java에 Engine과 Car를 만든다 

	Engine.java
	package kr.or.connect.diexam01;
		
	public class Engine {
		public Engine() {
			System.out.println("Engine 생성자");
		}
		
		public void exec() {
			System.out.println("엔진이 동작합니다.");
		}
	}

Car.java

	package kr.or.connect.diexam01;
		
	public class Car {
		Engine v8;
		
		public Car() {
			System.out.println("Car 생성자");
		}
		
		public void setEngine(Engine e) {
			this.v8 = e;
		}
		
		public void run() {
			System.out.println("엔진을 이용하여 달립니다.");
			v8.exec();
		}
		public static void main(String[] args){
			Engine e = new Engine();
	Car c = new Car();
	c.setEngine( e );
	c.run();
		}
	}

그런데 main문 안에 첫 번째,두번째 줄을 Spring을 이용해서 DI받을 수 있다.

2. applicationContext.xml에 이렇게 입력한다

`<bean id="e" class="kr.or.connect.diexam01.Engine"></bean>
<bean id="car" class="kr.or.connect.diexam01.Car">
	<property name="engine" ref="e"></property>
</bean>`

property는 getter와 setter를 의미한다. bean태그 안에서는 모두 값을 **설정**하는 것이다. 그래서 setEngine()을 의미하게 된다. 

setEngine()은 파라미터로 Engine타입을 받게 된다. 그래서 e가 들어가는 것이다. = setEngine메서드에 파라미터로 전달해라.

property의 name은 setEngine의 set뒤의 이름을 소문자로 바꾼 것이 들어간다.(Car클래스 보면 이렇게 되어있다)
`public void setEngine(Engine e){
this.v8=3;`



위의 xml설정은 아래와 같은 뜻이다
`Engine e = new Engine();
Car c = new Car();
c.setEngine( e );`


3. 이런 설정들을 읽어들여서 실행하는 ApplicationContextExam02를 만든다.


	package kr.or.whiuni.diexam01;
		
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;
		
	public class ApplicationContextExam02 {
		
		public static void main(String[] args) {
			ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
			Car car =(Car)ac.getBean("c");
			car.run();
		}
	
	}


일단은 Spring컨테이너가 필요하다.
Spring 컨테이너 중 ApplicationContext를 이용할 거고,
ApplicationContext를 구현하고 있는 ClassPathXmlApplicationContext를 이용한다.


ClassPathXmlApplicationContext는 applicationContext.xml에 설정해놓은 정보를 가지고 
bean을 생성해야한다. 그래서 클래스의 생성자에다 해당 xml파일에 대한 정보를 넘겨준다.

`ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");`
	
자동차 객체를 생성하는데
자동차 객체를 생성하는 것이 이제 주체가 내가 아니라 Spring컨테이너가 해주는 것이 된다.


Spring 컨테이너인 ApplicatonContext한테 부탁을 하게 된다.
ApplicationContext가 갖고 있는 getBean을 이용하게 되고 이때 applicationContext에 등록했던 Bean id를 이용하면 된다.

`Car car =(Car)ac.getBean("c");`


이렇게 하면 엔진을 탑재한 자동차 객체가 만들어진다.
이 자동차가 가진 run()이라는 메소드를 가지고 실행시켜보면 엔진이 동작하는 것을 볼 수 있다.

### DI의 장점
- 사용자는 사용할 Car라는 클래스만 알면 된다. 
Car를 상속한 Bus라는 클래스가 생성되도록 xml파일만 바꿔주고

Bus가 상속하는 Engine도 Electric Engine으로 주입받도록 바꿔준다면

실행 클래스의 코드는 하나도 바뀌지 않고 전기 버스가 동작하는 코드가 될수도 있다.
