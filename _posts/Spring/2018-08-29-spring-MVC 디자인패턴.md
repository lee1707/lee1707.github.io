---
layout: post
title: MVC 디자인 패턴
category: Spring
tags: [MVC디자인패턴]
---


> 출처 : [생활코딩 MVC 디자인 패턴](https://opentutorials.org/course/697/3828) <br/>
> [Swift로 iOS11 앱 개발하기](https://www.edwith.org/swiftapp/lecture/26620)

### MVC 디자인 패턴(Model-View-Controller)
**디자인 패턴**은 건축으로치면 공법에 해당하는 것으로 소프트웨어의 개발 방법을 공식화 한 것이다. 

소수의 뛰어난 엔지니어가 해결한 문제를 다수의 엔지니어들이 처리 할 수 있도록 한 규칙이면서, 구현자들 간의 커뮤니케이션의 효율성을 높이는 기법이다.

![MVC_Design_pattern](../public/img/spring/MVC_Design_pattern.png)

- User는 시스템을 사용한다=명령을 내린다.

시스템은 내부적으로 명령을 처리해서 결과를 사용자에게 보여준다.

- controller는 사용자의 의중을 파악해서 의중에 해당하는 작업을 한다.

- Model은 데이터 핸들링, View는 사용자에게 시각적으로 보여주는것을 핸들링,

- Controller는 사용자 요구에 따라 모델과 뷰를 적당히 사용해서 최종적으로 시스템이 가지고 있는 데이터를 View를 통해 사용자에게 보여주게된다.

### Web과 MVC
위의 개념을 웹에 적용해보자. 

- 사용자가 웹사이트에 접속한다. (Uses)
- Controller는 사용자가 요청한 웹페이지를 서비스 하기 위해서 모델을 호출한다. (Manipulates)
- Moel은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후에 그 결과를 리턴한다.
- Controller는 Model이 리턴한 결과를 View에 반영한다. (Updates)

데이터가 반영된 VIew는 사용자에게 보여진다. (Sees)

### Controller 
- 사용자가 접근한 url에 따라 url에 해당되는 로직을 실행될 수 있도록 한다. (url을 해석하는 것) 

즉, Controller는 class, url에 해당되는 메소드가 정의된 곳이 컨트롤러이다.

컨트롤러의 메소드에서는 모델과 뷰를 호출해서 사용자 요구사항에 맞는 데이터를 뽑아와서
뷰에 적용해서 최종적으로 사용자에게 보여진다.

**프레임워크**의 핵심을 규칙.
프레임워크의 규칙에 따라 코드를 작성해야한다.

### Framework
프래임워크란 에플리케이션을 구현 할 때 **공통되는 부분**과 에플리케이션 특화된 부분을 구분해서 공통되는 부분은 미리 만들어진 체계를 이용하고, 에플리케이션 특화된 부분은 직접 구현함으로서 생산성을 향상시키는 수단이다. 

잘 만들어진 프래임워크을 이용하면 높은 퀄리티로 프로젝트를 유지 할 수 있는 장점이 생긴다. 


### 프래임워크 도입시 주의할 점
하지만 프래임워크을 통해서 생산성을 높이기 위해서는 프래임워크을 잘 이해하는 것이 필수다. 

프래임워크 도입 초기에는 학습 부담으로 인해서 생산성이 오히려 저해된다. 프래임워크 도입에 대한 기대감으로 조급하게 성과를 기대하다가는 아무것도 이루지 못할 수 있다. 

프래임워크 도입을 위해서 충분한 학습시간을 갖는 것이 매우 중요하다. 


### MVC 디자인 패턴 간단히 정리(iOS App 관점의 설명) 
![MVC_Design_pattern2](../public/img/spring/MVC_Design_pattern2.png)

1. Model
앱이**무엇**인지에 대해 관심을 가진다. UI와 독립되어 있다.

2. Controller
**어떻게** 화면에 표시할 것인지에 대해 관심을 가진다.

3. View
UIButton, UIViewController, UILabel와 같은 UI와 관련된 것이고 Controller의 통제를 받게 됩니다.


## 서로의 관계

- Model과 Controller

Controller는 모델에 직접적으로 접근할 수 있지만, Model은 Controller에 Notification & KVO 방식을 통해 모델의 변화를 알린다.

- Model과 View

Model은 UI에 독립적이며 View와 소통할 수 없으며, View 또한 불가능하다.


- View와 Controller

Controller는 View에 대해 outlet을 이용해 View에게 직접적으로 접근할 수 있다. =

View는 Controller에게 구조적으로 미리 정해진 방식으로 Controller에게 행위에 대한 요청(delegate)과 데이터에 대한 요청(data source)을 할 수 있다. 

뿐만 아니라, action(View) - target(controller)의 구조로 사용자의 행위에 따라 필요한 함수를 호출할 수도 있다.

아래와 같은 MVC 패턴이 여러개 모여 하나의 앱을 만들게 된다
