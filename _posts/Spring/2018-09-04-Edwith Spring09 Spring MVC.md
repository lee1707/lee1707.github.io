---
layout: post
title: Edwith Spring 09 - Spring MVC Model1 Model2
category: Spring
tags: [spring, springmvc]
---

>[Edwith Spring - Spring MVC](https://www.edwith.org/boostcourse-web/lecture/16762/)을 보고 학습용으로 정리한 내용입니다.

# MVC란?
MVC는 Model-View-Controller<br/>

원래는 제록스 연구소에서 일하던 트뤼그베 린즈커그가 처음으로 소개한 개념으로, 데스트톱 어플리케이션용으로 고안되었다.


# MVC Model 1 아키텍처

![MVC_model1.png](../public/img/spring/MVC_model1.png)

브라우저가 요청을 하면 해당 요청을 JSP가 받는다. 따라서, 요청만큼 JSP페이지가 존재해야 한다. 이런 JSP는 Java로 만들어진 클래스인 Java bean을 이용해서 데이터 베이스를 사용하고 이 결과를 화면에 출력한다.

Java bean은 앞에서 실습한 JDBC로 작성했던 RoleDao 클래스를 생각하면 된다.

- 문제점

JSP자체에 Java코드와 HTML코드가 섞이고 유지보수가 어렵다.

# MVC Model 2 아키텍처

![MVC_model2.png](../public/img/spring/MVC_model2.png)

모델 2 아키텍처는 요청을 서블릿이 받게 하고, 서블릿이 Java bean을 이용해서 DB에서 데이터를 꺼내오고 이런 결과를 JSP를 통해서 화면에 보여주게 한다.

서블릿은 요청과 데이터를 처리하는 Controller의 역할을 수행하고<br/>
JSP는 모델의 결과를 보여주는 View의 역할을 한다.<br/>
-> 이렇게 함으로써 로직과 뷰를 분리할 수 있게 된다.


# Model 2의 발전된 형태 


![model2_upgrade.png](../public/img/spring/model2_upgrade.png)

클라이언트가 보내는 모든 요청을 프론트 컨트롤러라고 하는 서블릿 클래스가 다 받는다. 이 서블릿은 딱 하나만 존재한다.

프론트 컨트롤러는 요청만 받고 실제 일은 컨트롤러 클래스(=핸들러 클래스)에 위임한다. 서블릿은 관련된 요청을 처리하기에 조금 불편한 구조를 가지고 있기 때문이다. 이렇게 위임함으로써 관련된 URL을 하나의 클래스에서 다 처리할 수 있도록 하게 된다.

이런 컨트롤러는 Java bean을 이용해서 결과를 만들어내고 만들어진 결과를 모델에 담고 프론트 컨트롤러에 보내면,

프론트 컨트롤러는 알맞은 뷰에게 모델을 전달해서 그 결과를 출력하게 된다.



# Spring MVC(Spring Web Module)

![spring_web_module.png](../public/img/spring/spring_web_module.png)


이러한 형태가 Spring프레임워크 모듈중에 하나인 Web모듈에 구현되어 있다.(즉, Model2 MVC패턴을 지원한다) 이것을 Spring MVC라고 한다.




