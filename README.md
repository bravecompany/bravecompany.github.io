
### 작성자정보 등록 (포스트 상단에 입력되는 author에 따라 작성자 정보를 자동으로 불러옴)
- _authors 디렉토리에 lastname.firstname.md 이름으로 필자 정보 파일 추가
- layout: author # 레이아웃(필수)
- name: lastname.firstname
- title: 한글이름
- image: /images/author/파일명

>예시
~~~
---
layout: author
name: judy.kim
title: 김혜림
image: /images/author/judy.png
---
~~~

### _posts 디렉토리 내 파일명 작성 규칙
파일 이름은 YYYY-MM-DD-name-of-post.md 형식으로 짓는다. ex) 2019-11-22-refactoring.md
마크다운 파일 상단에 'Front matter' 작성
- layout 고정
- category의 경우, category디렉토리 하위에 해당 category.html 파일이 있어야 함
>예시
~~~
---
layout: post
title: 'Refactoring'
author: judy.kim
date: 2019-10-28 13:11
categories:[techCamp]
tags: [refactoring]
published : false
---
~~~
