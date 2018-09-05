---
layout: post
title: Edwith Spring 06 - Spring JDBC 실습2 selectAll
category: Spring
tags: [spring, springjdbc, selectall]
---

>[Edwith Spring - Spring JDBC 실습](https://www.edwith.org/boostcourse-web/lecture/20661/)을 보고 학습용으로 정리한 내용입니다.

# Select

### Select해오기 위해서 어떤 클래스가 필요할까?
1. DTO(Data Transfer Object) : 데이터를 전달할 때 사용할 목적
2. Query문 : RoleDaoSqls
3. DAO(Data Access Object) : 여기에 selectAll 메서드도 추가해봄

- NamedParameterJdbcTemplate, SimpleJdbcInsert같은 객체들을 Spring JDBC가 제공해주는 객체이다.


# 실습

### 1.기본 패키지에 DTO패키지 추가

DTO패키지에 클래스 만들기

```
package kr.or.whiuni.daoexam.dto;

public class Role {
	private int roleId;
	private String description;

	public int getRoleId() {
		return roleId;
	}
	public void setRoleId(int roleId) {
		this.roleId = roleId;
	}
	public String getDescription() {
		return description;
	}
	public void setDescription(String description) {
		this.description = description;
	}
	@Override
	public String toString() {
		return "Role [roleId=" + roleId + ", description=" + description + "]";
	}
}
```

### 2. query문을 담고 있어야 되는 객체를 만듦. 

RoleDaoSqls라는 클래스를 만들기

query를 상수(public static final) 형태로 넣어주면 된다.<br/>

상수는 모든 글자를 대문자로 쓰는 것이 관례이다. 두단어 이상은 `_`를 사용한다.


```
package kr.or.whiuni.daoexam.dao;

public class RoleDaoSqls {
	public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";
}
```


### 3. 2를 가지고 실제로 데이터를 액세스 할 수 있는 DAO를 만듦

RoleDao 클래스를 만들기

이 클래스는 실제 Spring 컨테이너가 읽어들여서 사용해야 한다. Bean을 등록할 수 있는 여러 방법이 있는데 그 중 어노테이션을 붙여서 읽어내게 하는 방법을 써보자.

- DAO객체에는 저장소의 역할을 한다는 의미로 @Repository라는 어노테이션을 붙인다

- DAO가 실제 실행할 때 NamedParameterJdbcTemplate, SimpleJdbcInsert 객체들을 이용하는데, 이 객체들은 Spring JDBC가 실제 JDBC를 조금 편하게 하기 위해 이미 구현해 놓은 객체들이다.

- 현재 우리는 selectAll()을 수행할 것인데 select해올때에는 NamedParameterJdbcTemplate 안의 메서드인 query(), queryForObject(), update() 등을 사용하게 된다. 

```
package kr.or.connect.daoexam.dao;

import static kr.or.connect.daoexam.dao.RoleDaoSqls.*;

import java.util.Collections;
import java.util.List;
import java.util.Map;

import javax.sql.DataSource;

import org.springframework.dao.EmptyResultDataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.BeanPropertySqlParameterSource;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.core.namedparam.SqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;
import org.springframework.stereotype.Repository;

import kr.or.connect.daoexam.dto.Role;
import static kr.or.whiuni.daoexam.dao.RoleDaoSqls.*;
@Repository
public class RoleDao {
	private NamedParameterJdbcTemplate jdbc;
	private SimpleJdbcInsert insertAction;
	private RowMapper<Role> rowMapper = BeanPropertyRowMapper.newInstance(Role.class);

	public RoleDao(DataSource dataSource) {
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
		this.insertAction = new SimpleJdbcInsert(dataSource)
                .withTableName("role");
	}
	
	public List<Role> selectAll(){
		return jdbc.query(SELECT_ALL, Collections.emptyMap(), rowMapper);
	}

}
```

- NamedParameterJdbcTemplate

가장 중요한 것이 필드로 선언된 NamedParameterJdbcTemplate이다.<br/>
Spring JDBC에서 가장 중요한 객체가 JdbcTemplate인데, 이 객체는 바인딩할 때 ?를 사용했다.

?를 사용하면 sql문자열만 봤을 때는 어떤 값이 매핑되는지 알아보기 힘든 문제가 있어서 NamedParameterJdbcTemplate이 활용 되었다.

NamedParameterJdbcTemplate은 이름을 이용해서 바인딩하거나 결과값을 가져올 수 있다.


- 생성자

DataSource를 파라미터로 받아들이고 있다. Spring 버전 4.3부터는 ComponentScan으로 객체를 찾았을 때 기본 생성자가 없으면 자동으로 객체를 주입해준다.

DBConfig에서 Bean으로 등록했던 DataSource가 파라미터로 전달이 된다. 이 DataSource를 받아들여서 NamedParameterJdbcTemplate객체를 생성하게 된다.


- selectAll()

1. 첫번째 파라미터<br/>
jdbc.query()에서 첫번째 나오는 파라미터는 실제 query문인데 RoleDaoSqls객체에 선언된 변수를 사용하기 위해

`import static kr.or.whiuni.daoexam.dao.RoleDaoSqls.*;`
를 추가한다.

static import를 사용하면 RoleDaoSqls 객체에 선언된 변수를 클래스 이름 없이 바로 가져다가 쓸 수 있다. 


2. 두번째 파라미터<br/>
비어있는 맵 객체를 하나 선언하면된다.<br/>
`CollectionsEmptyMap()`

sql문에 바인딩할 값이 있을 경우에 바인딩할 값을 전달할 목적으로 사용하는 객체이다. 

3. 세번째 파라미터<br/>
위에서 만들어 놓은 rowMapper라는 객체를 전달한다. 

rowMapper는 select 한 건, 한 건의 결과를 DTO에 저장하는 목적으로 사용하게 된다. BeanPropertyRowMapper 객체를 이용해서 column의 값을 자동으로 DTO에 담아주게 된다.

query() 메서드는 결과가 여러 건이었을 때 내부적으로 반복하면서 DTO를 생성한다. 생성한 DTO를 List에다 담아주고 해당 List를 반환해준다.


### 4. JAVA와 DBMS의 표기법

JAVA - camel표기법<br/>
DBMS - column명인 단어와 단어를 구분할 때 `_`를 사용한다. 대소문자를 거의 구분하지 않기 때문에 camel표기법이 효과가 없기 때문이다.

그런데 BeanPropertyRowMapper는 DBMS와 JAVA의 이름 규칙을 맞춰주는 기능을 가지고 있다.

### 5. ApplicationConfig에 ComponentScan으로 읽어낼 것을 알리는 설정추가

이걸해야지 설정 파일을 읽어낼 때 약속된 어노테이션이 붙어있는 객체들을 찾아내서 일을 진행한다.

```
@ComponentScan(basePackages = {"kr.or.whiuni.daoexam.dao"})
```

basePackages해서 중괄호를 이용하면 패키지를 여러개 나열해서도 사용할 수 있다. 위의 부분을 추가시켰기 때문에 자동으로 RoleDao에 Repository가 붙어있는 클래스를 Bean으로 등록해준거랑 같은 역할을 해줄 것이다.


### 6. 잘 동작하는지 테스트

main 패키지에 SelectAllTest라는 클래스를 만들자

```
package kr.or.whiuni.daoexam.main;

import java.util.List;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import kr.or.whiuni.daoexam.config.ApplicationConfig;
import kr.or.whiuni.daoexam.dao.RoleDao;
import kr.or.whiuni.daoexam.dto.Role;


public class SelectAllTest {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class); 
		
		RoleDao roleDao =ac.getBean(RoleDao.class);

		List<Role> list = roleDao.selectAll();
		
		for(Role role: list) {
			System.out.println(role);
		}

	}

}
```

만약 값이 이상하다면, 
실제로 MySQL에 접속해서 값을 확인해볼 수 있다.






