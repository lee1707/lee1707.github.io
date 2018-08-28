---
layout: post
title: Windows에서 Jekyll 블로그 만들기
category: Jekyll
tags: [blog, jekyll, github.io, windows]
---

### Windows에서 Jekyll 블로그 만드는 '간략한' 방법
1. Ruby+Devkit 다운로드, jekyll 설치
2. github에서 repository 만들고, clone으로 local에 repository 폴더 만들기 - 1차로 잘 되는지 [http://127.0.0.1:4000](http://127.0.0.1:4000) 으로 확인
3. 2의 폴더에 theme 압축 푼것 넣고 다시 jekyll 구동 
4. http://[github 사용자명.github.io]으로 2차 확인


-


### 자세한 순서(도움받은 사이트 목록) - 그대로 설치 하면 됨  
1. Jekyll 설치에 앞서서
[맨처음 해야할 것](https://pages.github.com/)

2. Jekyll 설치 - Ruby+DevKit 다운로드(Windows에 설치시)
- [설치]( https://blog.psangwoo.com/coding/2017/04/02/install-jekyll-on-windows.html)
-  [만약 설치에 대해 모르겠으면](http://tech.whatap.io/2015/09/11/install-jekyll-on-windows/) - 오래된 자료라 python, pip은 추가로 설치 필요 없음, 1번과 마지막만 참고하기

3. Theme적용하기
-  [Jekyll을 이용한 .github.io 블로그 만들기](https://gmlwjd9405.github.io/2017/10/06/Jekyll-github.io-blog-3.html)
-  [Jekyll 블로그에 테마 적용하기](http://my2kong.net/2016/07/07/jekyll-blogging-theme/)

4. 포스팅하기
- [지킬(jekyll)로 포스팅하기](https://jungtaejtkim.github.io/dev/2017/05/02/posting/)
- [Jekyll과 Lanyon을 이용한 블로깅 및 테마 편집 방법](https://minyoungjung.github.io/%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95/%EB%B8%94%EB%A1%9C%EA%B7%B8/2017/05/31/lanyon-theme-customize/)
- [깃허브블로그 jekyll, lanyon theme 적용](https://wkimdev.github.io/jekyll/2018/04/06/blog-theme-change/)


### github blog 만들때 헷갈릴 때 알아두어야 할 것
1. repository이름은 *꼭* [github사용자명.github.io]로 만들어야함
2. [http://127.0.0.1:4000](http://127.0.0.1:4000) 으로 확인
3. 다운받은 theme은 local의 [github사용자명.github.io] 폴더 아래에 압축 풀어서 두기
4. Start Command Prompt with Ruby로 [github사용자명.github.io] 폴더로 들어가서 jekyll serve --watch
5. theme적용한 후 http://[github 사용자명.github.io]으로 접속하려면 시간이 걸림(5분정도, 로딩시간) - 바로 제대로 뜨지 않더라도 에러가 아님.

