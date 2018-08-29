---
layout: post
title: Jekyll에서 Github post가 update되지 않는 경우 해결방법
category: Jekyll 
tags: [jekyll]
---

### 문제

**내 Github page에서 post가 업데이트 되지 않는다**

## 먼저 확인해야 할것 
1. Post를 알맞게 적었는지(=1. 시간 설정을 잘 알고 있고 2. 제목을 알맞게 적었다) 
	
	1. 만약 따로 시간 설정을 하지 않았을 경우 UTC 시간이 포스팅의 기준이 된다. 현재 시간을 기준으로 미래 시간 타이틀인 포스트는 업데이트 되지 않는다. 
	ex) 오늘이 1월 1일이고 2040-01-01-title.md로 포스팅을 한 경우,  UTC Time이 12월 31일 저녁 10시면, 포스팅은 2시간 후에 올라오게 된다.

	2. 제목은 위와 같이 YYYY-MM-DD로 적어야 하며 포스팅 페이지 위쪽의 title : title... 등도 콜론(:)을 알맞게 적어야 한다. 

	- [만약 이게 확실하지 않을 경우+이 외에도 체크해야할 것](https://stackoverflow.com/questions/30625044/jekyll-post-not-generated)

	-[UTC 현재시간 확인](https://time.is/UTC)


2. Github으로 push를 성공적으로 했으며 이를 내 Github에서 확인 가능하다. 

3. 그러나 내 Github blog는 post가 update되지 않는다

4. branch 오류도 아니다 - [만약 master branch 이외의 branch를 사용한다면 오류가 의심된다면](https://stackoverflow.com/questions/24713112/my-github-page-wont-update-its-content)
여기서 user6929107의 답변을 보기

### 해결방법

**브라우저 캐시 지우기(방문기록 등의 캐시 전체 지우기)**

*나의 경우*

Specifically my issue was the following:

- I had created a github.pages site with a custom domain.
- I was pushing commits to the correct GitHub branch but not seeing the updates on the github.pages site.

* Solution: The issue turned out to be my browser caching the page (despite my having page caching disabled). To fix it I just cleared my cached data from the past hour and that worked instantly.

To clear the cache data in Chrome go to the Chrome menu then More Tools > Clear Browsing Data.

즉, 단순히 브라우저의 캐시를 지우는 것 만으로(크롬:방문기록 지우기 클릭) post update가 이루어진 것을 확인 할 수 있었다


## 도움받은 곳
[My GitHub page won't update its content](https://stackoverflow.com/questions/24713112/my-github-page-wont-update-its-content) 
Josh의 답변
