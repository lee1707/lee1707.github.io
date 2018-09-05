---
layout: post
title: Edwith Spring 07 - Spring JDBC 실습3 insert,update
category: Spring
tags: [spring, springjdbc, insert, update]
---

>[Edwith Spring - Spring JDBC 실습](https://www.edwith.org/boostcourse-web/lecture/20661/)을 보고 학습용으로 정리한 내용입니다.

# insert와 update 구문 

### 1. RoleDao에 가서 insert문을 넣어준다.

insert문을 실행하기 위해서는 SimpleJdbcInsert라는 객체가 필요하다.

SimpledbcInsert를 추가해준다.

```
@Repository
public class RoleDao {
	private NamedParameterJdbcTemplate jdbc;
	//SimpleJdbcInsert선언
	private SimpleJdbcInsert insertAction;
	private RowMapper<Role> rowMapper = BeanPropertyRowMapper.newInstance(Role.class);

	public RoleDao(DataSource dataSource) {
		this.jdbc = new NamedParameterJdbcTemplate(dataSource);
		//생성자에서 dataSource를 넣어서 생성 
		this.insertAction = new SimpleJdbcInsert(dataSource)
                .withTableName("role");

	}
```

실제 insert문을 구현한다.	

insert문의 경우, primary key를 자동으로 생성해야 되는 경우도 존재한다. 이럴때는 생성된 primary key 값을 다시 읽어와야 되는 등의 부분이 필요하다. **SimpleJdbcInsert**객체가 그 일을 해주는데 이번에는 그냥 값을 직접 넣어준다.

```
	public int insert(Role role) {
		SqlParameterSource params = new BeanPropertySqlParameterSource(role);
		return insertAction.execute(params);
	}
```


insert()메서드는 Role객체를 받아들여서 해당 Role객체에 있는 값을 맵으로 바꿔주는데 이때 우리가 선언한 roleId를 column명 role_id로 알아서 맵객체를 생성해준다. 

이렇게 생성한 맵 객체를 SimpleJdbcinsert가 가지고 있는 execute()라는 메서드의 파라미터로 전달을 할 경우에 값이 알아서 저장이 되게 된다.


### 2. 잘 실행되는지 테스트

main패키지 안에 만들어주기

```
public class JdbcTest {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

		RoleDao roleDao = ac.getBean(RoleDao.class);

		Role role = new Role();
		role.setRoleId(301);
		role.setDescription("PROGRAMMER");

		//입력한 후 출력하기
		int count = roleDao.insert(role);
		System.out.println(count+"건 입력하였습니다");
	}

}
```

-MySQL에서 `select * from role`하고 실행해서 잘 들어갔나 확인하기


### 3. 같은 방식으로 update 작성

- RoleDaoSqls에 query문 넣기

`public static final String UPDATE = "UPDATE role SET description = :description WHERE ROLE_ID = :roleId";`

여기서 값이 들어가는 부분은 :하고 나와있는 부분이다. 이 부분이 나중에 값으로 바인딩 될 값이라고 생각하면 된다. 


- RoleDao에 update문넣기

```
public int update(Role role) {
		SqlParameterSource params = new BeanPropertySqlParameterSource(role);
		// 첫번째 파라미터는 SQL, 두번째 파라미터는 맵객체인데 SQL에서 값을 넣어야 하는 부분에 맵 객체가 들어간다.
		return jdbc.update(UPDATE, params);
	}
	```

update문을 수행할때 인자로 받아온 DTO(role)가 가진 값을 가지고 맵으로 변환시켜줘야되는데, 그때 사용한 객체가 params이다. SqlParameterSource객체가 외부로 바꿔주는 일을 수행하게 된다.


윗 부분에 자동으로 SELECT_ALL로 바뀐 import문을 보자

`import static kr.or.whiuni.daoexam.dao.RoleDaoSqls.SELECT_ALL;`

update도 가져와야 하니 와일드카드로 바꿔준다

`import static kr.or.whiuni.daoexam.dao.RoleDaoSqls.*`


### 4. update문 테스트 

```
public class JdbcTest {

	public static void main(String[] args) {
		ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);

		RoleDao roleDao = ac.getBean(RoleDao.class);

		Role role = new Role();
		role.setRoleId(301);
		role.setDescription("CEO");

		int count = roleDao.update(role);
		System.out.println(count+"건 수정하였습니다.");
	}
}
```	
