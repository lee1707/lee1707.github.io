---
layout: post
title: Edwith Spring 03 - Java Config를 이용한 설정
category: Spring
tags: [spring, container, ioc, di, javaconfig]
---

> 출처 : [Edwith 부스트코스 Full-Stack Web Developer - Spring Java Config를 이용한 설정](https://www.edwith.org/boostcourse-web/lecture/20658/)

### Java Config를 이용한 설정 

src/main/java에 ApplicationConfig클래스를 만든다.

가장 먼저 해야할 일을 클래스 위에 나는 conifg파일이라고 알리는 것

@Configuration이라고 쓴다<br/> 
이제 bean을 등록한다<br/>
@Bean이라고 쓰면 된다


	@Configuration
	public class ApplicaionConfig {
		@Bean
		public Car car(Engine e) {
			Car c = new Car();
			c.setEngine(e);
			return c;
		}
	}

어노테이션은 jdk5부터 지원이 되기시작한다.

@Configuration이라는 어노테이션은 Spring설정 클래스라는 의미를 가진다 AnnotationConfigApplicationContext는 나중에 이런 자바 config클래스를 읽어들여서 IoC와 DI를 적용하게 된다.

설정 파일 중에 bean이라는 어노테이션이 붙어있는 메서드들을 AnnotationConfigApplicationcontext는 자동으로 실행해서 그 결과로 리턴하는 객체들을 기본적으로 싱글톤으로 관리하게 해준다.

이 설정파일을 읽어서 실제 실행을 시켜주는 자바 파일을 하나만들어보자.

	ApplicationContextExam03
		
	package kr.or.whiuni.diexam01;
		
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.annotation.AnnotationConfigApplicationContext;
		
	public class ApplicationContextExam03 {
		
		public static void main(String[] args) {
			ApplicationContext ac= new AnnotationConfigApplicationContext(ApplicaionConfig.class);
			//getBean에 Car.class를 해도 된다
			Car car = (Car) ac.getBean("car");
			car.run();
		}
	}


bean을 만들어내는 공장인 AnnotationConfigApplicationContext가 생성될 때 
ApplicationConfig한테 있는 설정을 읽어들여서 공장을 세웠을 것이다.

그 말은, bean들을 이미 다 만들어서 갖고 있다는 것이다.
그리고 사용자가 요청했을 때 그에 알맞은 결과를 줄 것이다.



ac.getBean에 Car.class라는 클래스 타입을 넣어주면 찾는 메소드의이름과 상관없이
리턴타입이 Car인 애를 찾아간다.

public `Car` car(Engine e)
 

ApplicationContext가 관리하는 객체 중에서 Car라는 게 있으면 무조건 알아서 반환을 해주는 부분이다.


ApplicationConfig2를 만든다.

	package kr.or.whiuni.diexam01;
		
	import org.springframework.context.annotation.ComponentScan;
	import org.springframework.context.annotation.Configuration;
		
	@Configuration
	@ComponentScan
	public class ApplicationConfig2 {
		
	}



@ComponentScan은 알아서 네가 여기에 있는 것들 읽어들여서 지정하라고 해주는 것이다.


사용자가 일일이 다 하나하나 알려주지 않아도 네가 어노테이션 붙어있는 것들을 잘 찾아내서 등록해줬으면 좋겠어 하는 것이다.


@ComponentScan을 할 때는 반드시패키지 명을알려줘야 한다.
`@ComponentScan(“kr.or.whiuni.diexam01”)`

이 패키지 안에 있는 것들 중에 스캔해와 라고 하는 것이다.


이때 @ComponentScan에서 읽어들이는 어노테이션은 컨트롤러,서비스, 리포지토리, 컴포넌트이런 어노테이션이 붙어있는 클래스를 찾아서
그 어노테이션이 붙어있는 것들을 다bean으로 등록을 시켜주는 거라고 생각하면된다.


Engine클래스에 @Component붙여준다.
    
    package kr.or.whiuni.diexam01;
										
	import org.springframework.stereotype.Component;
			
	@Component
	public class Engine {
		public Engine() {
			System.out.println("Engine 생성자");
		}
		public void exec() {
			System.out.println("엔진이 동작합니다");
		}
	}



Car클래스에 @Component를 붙여준다.

	package kr.or.whiuni.diexam01;
		
	import org.springframework.stereotype.Component;
		
	@Component
	public class Car {
		@Autowired
		private Engine v8;
		
		public Car() {
			System.out.println("Car 생성자");
		}
		
		//public void setEngine(Engine e) {
		//	this.v8 = e;
		//}
		
		public void run() {
			System.out.println("엔진을 이용해서 달립니다");
			v8.exec();
		}
		
	}

setEngine() 메서드는없애고 위의 Engine v8에 @Autowired라는 어노테이션을 붙인다.
이건 Engine타입의 객체가 생성된게 있으면 여기 주입해줘 라는 뜻이다.


@Autowired가 알아서 해주기 때문에 굳이 setter메소드는 없어도 괜찮다.


수정된 어노테이션을 읽어들이는 ApplicationContext객체를 똑같이 하나 만들자.


	package kr.or.whiuni.diexam01;
		
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.annotation.AnnotationConfigApplicationContext;
		
	public class ApplicationContextExam04 {
		
		public static void main(String[] args) {
			ApplicationContext ac= new AnnotationConfigApplicationContext(ApplicationConfig2.class);
			
			//getBean에 Car.class를 해도 된다
			Car car = (Car) ac.getBean("car");
			car.run();
		}
	}


Spring에서 사용하기 알맞은 컨트롤러, 서비스, 리포지토리, 컴포넌트 이런 어노테이션이 붙어있는 객체들을 ComponentScan을 이용하면 해당 객체들을 읽어내서 메모리에 올리고 DI를 주입하도록 한다.

이런 어노테이션이 붙어있지 않은 객체들은 bean이라는 어노테이션을 이용해서 직접 생성해주는 방식으로 클래스를 관리할 수 있다. 


### 이렇게 componentScan만 하면 편한데 일부러 bean을 등록하는 이유
componentScan은 약속된 어노테이션이 붙어있는 것만 읽어온다.


나중에 Spring JDBC를 쓰거나 다른 라이브러리가 가지고 있는 객체를 사용하면 우리가 그 라이브러리를 열어서 어노테이션을 붙일 수 없다. 

그때는 이렇게 bean을 등록하는 방법을 가지고 해당 bean을 등록하면 편리하게 쓸 수 있다.