---
layout: post
title: Error04 No mapping found for HTTP request with URI
category: Errors 
tags: [nomappingfound, error]
---

> 출처: [티스토리 acua.tistory.com](http://acua.tistory.com/entry/No-mapping-found-for-HTTP-request-with-URI) 

# No mapping found for HTTP request with URI

세가지 원인이있다

1. web.xml에 맵핑을 안 해놓았을 경우

2. servlet.xml에 맵핑을 안 해놓았을 경우

3. 컨트롤러에 정의하지 않은 경우



나의 경우, 첫번째는 아래와 같은 RequestMapping의 value 뒤에 /를 붙이지않은것이 원인이었다.


```
@RequestMapping(value="/app/", method = RequestMethod.GET)
```


두번째는 servlet-context.xml에서

```
<resources mapping="/resources/**" location="/resources/admin">
```

이 앞 부분 mapping값을 잘못 지정해준게 원인이었다.