---
layout: post
title: Edwith Spring 10 - Spring MVC 구성요소
category: Spring
tags: [spring, springmvc]
---

>[Edwith Spring - Spring MVC 구성요소](https://www.edwith.org/boostcourse-web/lecture/16763/)을 보고 학습용으로 정리한 내용입니다.


# Spring MVC 기본 동작 흐름
![dispatcherservlet](../public/img/spring/component/dispatcherservlet.png)

- 파란색 : Spring MVC가 제공해주는 것들
- 보라색 : 개발자가 만들어야 하는 부분
- 녹색 : Spring이 제공하는 부분 + 개발자가 만들어야 하는 부분(공존)

1. 클라이언트가 요청을 보내면 보낸 모든 요청을 Dispatcher Servlet이라고 하는 서블릿 클래스가 받는다. 

2. Dispatcher Servlet은 요청을 처리해줄 컨트롤러와 메서드가 무엇인지 Handler Mapping에게 물어본다. 

3. Handler Mapping이 혼자 알아낼 수 없기 때문에, 개발자가 어떤 요청에 어떤 컨트롤러가 동작할지를 xml파일이나 java파일에 어노테이션으로 설정을 하게 된다.

이런 정보들을 Spring으로 만들어진 웹 애플리케이션이 실행할 때 handler Mapping객체들이 생성이 되면서 관리를 하게 된다. 

4. Dispatcher Servlet은 그렇게 Handler Mapping으로부터 지금 들어온 요청에 알맞은 컨트롤러가 무엇인지, 해당되는 메서드는 무엇인지에 대한 정보를 알아내게 된다.

5. 그림상 (3)번인데, Dispatcher Servlet은 Handler Adapter에게 실행을 요청한다. 그때 결정된 컨트롤러와 해당 메서드가 실행이 될 것이다. 그 결과를 Model에 받아서 Dispatcher Servlet에게 전달을 하게 된다.

이때 Dispacther Servlet은 컨트롤러가 리턴한 view name을 알아오게 된다. 

6. 컨트롤러가 리턴한 view name을 가지고 적절한 View Resolver를 통해서 view를 출력하게 된다.

View Resolver가 어떤 View이다는 정보를 정확하게 알려주게 되면 이 View로 응답을 하게 되는 과정을 거칠 것이다.

**Spring MVC를 이해한다는 것은 DispatcherServlet이 어떻게 동작하는지를 이해하는 것이다**




# DispatcherServlet

DispatcherServlet을 프론트 컨트롤러라고 한다. 프론트 컨트롤러는 회사의 대표전화번호라고 생각하면 된다. 

클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고, 핸들러가 처리한 결과를 받아 사용자에게 응답결과를 보여준다.

DispatcherServlet은 여러 컴포넌트를 이용해 작업을 처리한다.


----------------




# 1. DispatcherServlet 내부 동작 흐름

![dispatcherservletinside.png](../public/img/spring/component/dispatcherservletinside.png)

1. 요청이 들어오면 요청에 대한 선처리 작업을 한다.

2. 선처리 작업이 끝나면 HandlerExecutionChain을 결정한다.

3. 결정이 되면 HandlerExecutionChain을 실행한다.

4. 실행하는 과정에서 예외가 발생을 했다고 한다면 예외처리를 한다. 예외가 발생하지 않았다면 뷰를 렌더링 한다.

5. 요청처리를 종료한다.



## 2. DispatcherServlet 내부 동작흐름 상세 - 요청 선처리 작업

![sunchuri.png](../public/img/spring/component/sunchuri.png)


1. **Locale 결정**

Spring MVC는 지역화를 지원한다. 지역화는 똑같은 사이트에 들어가도 어떤 사용자한테는 영어로 된 화면이 보여지고 어떤 사용자한테는 독일어로 된 화면이 보여지게 처리할수 있다는 뜻이다.

실제 각 사용자의 브라우저에 언어세팅이 되어있다. 브라우저가 보내는 헤더 정보에서 언어 설정의 값을 이용해서 Locale을 결정할 수 있다. 

이 정보를 가지고 Locale에 설정한 값을 한국어, 일본어, 중국어, 영어 이렇게 다양하게 보내지면 그 부분에 따라 각각 다른 언어로 된 화면을 보여주게 하는 처리를 지역화라고 한다.


2. **RequestContextHolder에 요청 저장**

스레드 로컬 객체이다. 요청을 받아서 응답할 때까지 HttpServletRequest, HttpServletResponse등을 Spring이 관리하는 객체 안에서 사용할 수 있도록 해주는 것을 이야기한다.

Spring MVC를 사용하다 보면 컨트롤러가 가진 메서드 안에서 '나 Request객체 필요해'라고 하면 인자에다가 HttpServletRequest requset이렇게 선언만 하면 된다. 


3. **FlashMap 복원**

Spring3에서 추가된 기능이다. FlashMap은, redirect로 값을 전달할 때 우리가 ?, 파라미터 등을 이용하는데 이렇게 하다보면 URL이 복잡해진다.(+URL은 길이 제한이 있다)

FlashMap을 사용하면 redirect될 때 딱 한번 값을 유지시킬 수 있게 해주는 것을 이야기한다. 즉, FlashMap을 복원한다는 것은 현재 실행이 redirect 되었을 때만 실행이 되는 부분들, 그 부분을 위해서 이런것도 제공하더라 하고 보면 된다.


4. **멀티파트 요청**

사용자가 파일 업로드를 했을 때 파일 정보를 읽어들이는 특수한 형태의 Request객체가 필요하다. 이러한 멀티파트 요청이 들어오게 되면 MultipartResolver가 멀티파트를 결정하게 해준다.

그리고 나서 실제 요청을 처리하는 핸들러를 결정하고 실행을 하는 역할을 해준다.




### 요청 선처리 작업시 사용된 컴포넌트 

- `org.springframework.web.servlet.LocaleResolver`

지역 정보를 결정해주는 전략 오브젝트이다.
디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정해준다.

- `org.springframework.web.servlet.FlashMapManager`

FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스
RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
리다이렉트 후 조회를 하면 바로 정보는 삭제된다.

- `org.springframework.web.context.request.RequestContextHolder`

일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.

- `org.springframework.web.multipart.MultipartResolver`

멀티파트 파일 업로드를 처리하는 전략




## 3. DispatcherServlet 내부 동작흐름 상세 - 요청 전달

![d1.png](../public/img/spring/component/d1.png)

1. HandlerMapping으로 HandlerExecutionChain이 결정된다.
2. HandlerExecutionChain이 있다면 HandlerAdapter가 결정되는데, 결정된 Handler Adpater가 없다면 서버잘못으로 ServletException이 발생된다.



### 요청 전달시 사용된 컴포넌트

- `org.springframework.web.servlet.HandlerMapping`

HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.

디폴트로 설정되는 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.


- `org.springframework.web.servlet.HandlerExecutionChain`

HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.

즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.

- `org.springframework.web.servlet.HandlerAdapter`

실제 핸들러를 실행하는 역할을 담당한다.
핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변화하는 방법에 대해 알고 있다.


디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.


@RequestMapping과 @Controller 애노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고, 그에 대응되는 AnnotationMethodHandlerAdapter에 의해 호출이 일어난다.


------------------


## 4. DispatcherServlet 내부 동작흐름 상세 - 요청 처리

![d2.png](../public/img/spring/component/d2.png)

인터셉터 : 일종의 필터


### 요청 처리시 사용된 컴포넌트

`org.springframework.web.servlet.ModelAndView`

ModelAndView는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스이다.

전에 우리가 서블릿에서 모델을 얻고, 그 모델을 JSP로 넘길때 Request와 같은 객체를 이용했는데 Spring엣는 종속되지 않게 하지 위해 ModelandView같은 객체를 사용한다.

Requset에다 값을 넣어서 사용하는 것보다 이런 컴포넌트를 이용하는게 바람직하다.

RequestToViewNameTranslator는 ModelAndView가 뷰가 없을 때 작동한다.


## 5. DispatcherServlet 내부 동작흐름 상세 - 예외 처리

![d3.png](../public/img/spring/component/d3.png)


## 6. DispatcherServlet 내부 동작흐름 상세 - 뷰 렌더링

![d4.png](../public/img/spring/component/d4.png)


## 7. DispatcherServlet 내부 동작흐름 상세 - 요청 처리 종료 
![d5.png](../public/img/spring/component/d5.png)








