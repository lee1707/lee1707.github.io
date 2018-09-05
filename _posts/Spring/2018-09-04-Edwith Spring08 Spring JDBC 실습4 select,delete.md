---
layout: post
title: Edwith Spring 08 - Spring JDBC 실습4 select, delete
category: Spring
tags: [spring, springjdbc, select, delete]
---

>[Edwith Spring - Spring JDBC 실습](https://www.edwith.org/boostcourse-web/lecture/20661/)을 보고 학습용으로 정리한 내용입니다.

# select, delete

### 1. RoleDaoSqls에 쿼리 추가

```
public static final String SELECT_BY_ROLE_ID = "SELECT role_id, description FROM role where role_id = :roleId";
public static final String DELETE_BY_ROLE_ID = "DELETE FROM role WHERE role_id = :roleId";
```

query문을 적을 때 `*`로 적는 것보다는 column명을 정확하게 나열하는것이 의미 전달이 명확하다.

### 2. RoleDao에 select와 update메서드를 추가

```
public int deleteById(Integer id) {
	Map<String, ?> params = Collections.singletonMap("roleId", id);
		
	//NamedParameterJdbcTemplate이 갖고 있는 update메서드 실행
	return jdbc.update(DELETE_BY_ROLE_ID, params);
}
```

delete경우 값이 하나만 들어오기 때문에 update문 처럼 SqlParameterSource(Map으로 바꿔주는 것)을 객체를 만들어서 사용하기 보다는

Collections.singletonMap같은 경우처럼 값이 여러개 들어가지 않고 딱 1건만 넣어서 사용할 때 쓰는 것을 사용하면 좋다
		

```	
public Role selectById(Integer id) {
	try {
		Map<String, ?> params = Collections.singletonMap("roleId", id);
		return jdbc.queryForObject(SELECT_BY_ROLE_ID, params, rowMapper);		
	}catch(EmptyResultDataAccessException e) {
		return null;
	}
}

```


한건 select 시에는 queryForObject라는 메서드를 사용한다.

첫번째 파라미터에는 query가 들어오고 두번째 파라미터로는 넣어둔 roleId값이 전달딘다.

select했을 때 해당 조건에 맞는 값이 없을 경우에는 값이 제대로 넘어오지 않을 수 있기 때문에 Exception처리를 적절하게 해야 한다.


### 3. test

JdbcTest에 추가

```
Role resultRole = roleDao.selectById(201);
System.out.println(resultRole);
		
int deleteCount = roleDao.deleteById(500);
System.out.println(deleteCount + "건 삭제하였습니다.");
	
Role resultRole2 = roleDao.selectById(500);
System.out.println(resultRole2);
```
