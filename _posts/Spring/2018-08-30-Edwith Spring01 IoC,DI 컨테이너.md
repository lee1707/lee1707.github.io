---
layout: post
title: Edwith Spring 01 - IoC/DI 컨테이너
category: Spring
tags: [spring, container, ioc, di]
---

> 출처 : [Edwith 부스트코스 Full-Stack Web Developer - Spring IoC/DI컨테이너](https://www.edwith.org/boostcourse-web/lecture/20656/)

### Framework 
반제품. 이미 중요하고 어려운 부분은 다 구현이 되어 있는 것. 
이걸 이용해서 원하는 것을 만들어내면 됨 

- 원하는 부분만 가져다 사용할 수 있도록 모듈화가 잘 되어 있다.
- IoC 컨테이너이다.
- 선언적으로 트랜잭션을 관리할 수 있다.
- 완전한 기능을 갖춘 MVC Framework를 제공한다.
- AOP 지원한다
- 스프링은 도메인 논리 코드와 쉽게 분리될 수 있는 구조로 되어 있다.

spring-core를 익히고, 필요할 때 필요한 것을 하나씩 익히면 된다. 


### 컨테이너(Container)
컨테이너는 보통 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하는 것을 말한다.

예시)

	Servlet을 실행해주는 WAS는 Servlet 컨테이너를 가지고 있다고 말한다.
		
	WAS는 웹 브라우저로부터 서블릿 URL에 해당하는 요청을 받으면, 서블릿을 메모리에 올린 후 실행한다.
			
	개발자가 서블릿 클래스를 작성했지만, 실제로 메모리에 올리고 실행하는 것은 WAS가 가지고 있는 Servlet컨테이너이다.

	Servlet컨테이너는 동일한 서블릿에 해당하는 요청을 받으면, 또 메모리에 올리지 않고 기존에 메모리에 올라간 서블릿을 실행하여 그 결과를 웹 브라우저에게 전달한다


### IoC 
- Inversion of Control. 제어의 역전.

- 개발자는 프로그램의 흐름을 제어하는 코드를 작성한다. 이 흐름의 제어를 개발자가 하는게 아니라 다른 프로그램이 그 흐름을 제어하는 것을 IoC라고 한다. 

예시)

	예를 들어 서블릿 클래스는 개발자가 만들지만, 그 서블릿의 메소드를 알맞게 호출하는 것은 WAS이다.
		
	이렇게 개발자가 만든 어떤 클래스나 메소드를 다른 프로그램이 대신 실행해주는 것을 제어의 역전이라고 한다.


TV에 비유하자면 공장을 짓는 일은 Spring이 해주고 공장에서 TV를 만들어서 필요한 TV를 주는 것을 IoC가 하게된다. 

(L-TV를 사용하거나 S-TV를 사용해도 우리가 TV를 사용하는 코드가 달라지는게 아니다. Servlet컨테이너가 어노테이션이나 xml에 따라 해당하는 Servlet클래스를 만들어서 우리한테 줬듯이.)


### DI
DI는 Dependency Injection의 약자로, 의존성 주입이란 뜻을 가지고 있다.

DI는 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.

예를 들면, 공장이 만든 인스턴스를 내가 사용하는 프로그램에서 가져오는 방법 중 DI가 있다. 

- DI가 적용되지 않은 예 (개발자가 직접 인스턴스 생성)

	class 엔진 {
	
	}
	
	class 자동차 {
		엔진 v5 = new 엔진();
	}	

- DI가 적용된 예 (엔진 type의 v5변수에 아직 인스턴스가 할당되지 않았다. 컨테이너가 v5변수에 인스턴스를 할당해주게 된다.)

	@Component
	class 엔진 {
	}
	
	@Component
	class 자동차 {
    	 @Autowired
     	엔진 v5;
	}


### Spring에서 제공하는 IoC/DI 컨테이너(Spring이 가진 공장)

- BeanFactory: IoC/DI에 대한 기본 기능을 가지고 있다(아주 간단한 기능만 있다)

- ApplicationContext : BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천된다. 트랜잭션처리, AOP등에 대한 처리를 할 수 있다. </br>BeanPostProcessor, BeanFactoryPostProcessor등을 자동으로 등록하고, 국제화 처리, 어플리케이션 이벤트 등을 처리할 수 있다.

- BeanPostProcessor : 컨테이너의 기본로직을 오버라이딩하여 인스턴스화와 의존성 처리 로직 등을 개발자가 원하는 대로 구현 할 수 있도록 한다.
- BeanFactoryPostProcessor : 설정된 메타 데이터를 커스터마이징 할 수 있다.