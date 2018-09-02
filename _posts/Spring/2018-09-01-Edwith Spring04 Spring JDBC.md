---
layout: post
title: Edwith Spring 04 - Spring JDBC
category: Spring
tags: [spring, springjdbc]
---

>[Edwith Spring - Spring JDBC](https://www.edwith.org/boostcourse-web/lecture/20660/)를 보고 학습용으로 정리한 내용입니다.

# Spring JDBC

- JDBC 프로그래밍을 보면 반복되는 개발 요소가 있다.

개발하기 지루한 JDBC의 모든 저수준 세부사항을 스프링 프레임워크가 처리해준다. 개발자는 필요한 부분만 개발하면 된다.

# Spring JDBC 패키지

1. `org.springframework.jdbc.core`
	Spring JDBC에서 가장 중요한 패키지이다.
2. `org.springframework.jdbc.datasource`<br/>
3. `org.springframework.jdbc.object`<br/>
4. `org.springframework.jdbc.support`<br/>


# JDBC Template

- org.springframework.jdbc.core에서 가장 중요한 클래스이다.
- 리소스 생성, 해지를 처리해서 연결을 닫는 것을 잊어 발생하는 문제 등을 피할 수 있도록 한다.
- 스테이먼트(Statement)의 생성과 실행을 처리한다.
- SQL 조회, 업데이트, 저장 프로시저 호출, ResultSet 반복호출 등을 실행한다.
- JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환시킨다.


### JDBC Template select 예제)

```
Actor actor = this.jdbcTemplate.queryForObject(

  "select first_name, last_name from t_actor where id = ?",

  new Object[]{1212L},

  new RowMapper<Actor>() {

    public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {

      Actor actor = new Actor();

      actor.setFirstName(rs.getString("first_name"));

      actor.setLastName(rs.getString("last_name"));

      return actor;

    }

  });
```

### JDBC 프로그래밍이 불편해서 이를 해결하기 위한 기술에는 Spring JDBC 프로그래밍 이외에도 JPA, MyBatis가 있다. 

- Spring JPA

Spring JPA의 경우에는 객체 단위로 CUD가 가능. 일반적으로 CUD는 단일 테이블에 대하여 작업을 하는 경우가 많은데 Mybatis에서 하게 되면 해당 쿼리를 직접 짜고, 들어오는 Parameter에 대하여 모두 확인하여 처리해야 하는 경우가 많다.

- MyBatis

그러나 MyBatis의 장점을 무시할 순 없다. ER-DB를 사용하는 경우 자기가 원하는 Query를 짜서 직접 데이터를 손쉽게 전달하고 처리할 수 있다는 장점이 있다. 

[Spring JPA와 Mybatis의 차이점 출처](http://blog.naver.com/PostView.nhn?blogId=admass&logNo=220870636605)
