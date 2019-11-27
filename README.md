## 포스트 작성방법
### 작성자정보 등록 
각 포스트 상단 Front matter의 author에 따라 등록된 작성자 정보를 노출시키기 위함

1. 위치  
_authors 디렉토리에 lastname.firstname.md 이름으로 작성자 정보 파일 추가

2. 작성방법
- 이미지는 /assets/images/author
- layout의 경우 author로 고정

```YAML
---
layout: author
name: lastname.firstname
title: 한글이름
image: /assets/images/author/파일명
---
```

### 포스트 작성

1. 파일명 
- jekyll이 _posts 디렉토리 내 생성된 파일을 포스트로 인식하게 하려면 아래와 같은 형식을 지켜야 한다.
- 파일 이름은 YYYY-MM-DD-name-of-post.md 형식으로 짓는다. ex) 2019-11-22-refactoring.md

2. Front matter 작성   
파일 내 최상단에 YAML형식으로 'Front matter'를 작성하며 해당 영역에 작성된 내용은 Jekyll에 의해 해당 포스트의 정보를 나타내는 변수로 정의된다.
- Front matter가 작성되지 않은 파일은 jekyll이 변환하지 않는다. 때문에 작성할 내용이 없어도 기본 시작과 끝(--- ---)은 작성해주어야 한다. 
- 포스트의 경우 환경설정 파일(_config.yml) 내 디폴트로 기본값이 지정되어있음 (현재 layout만 지정)
- layout: test (기본적으로 _layouts 디렉토리 내 post.html 사용하나 해당 포스트를 다른 레이아웃으로 사용하고자 할 경우 정의)
- date는 포스트 정렬을 위해 사용되며 파일명에 작성된 날짜보다 우선시 되나 아직 도래하지 않은 미래시각을 입력할 경우 해당 포스트는 생성되지 않는다. (site.future 값이 true인 경우에만 가능)
- author의 경우 _authors에 등록된 작성자로 입력
- categories : 지정한 카테고리에 포스트들을 묶기 위해 사용
  (지정한 category는 category 디렉토리 하위에 해당 카테고리명.html 파일이 있어야 그룹화 된 포스트 리스트가 정상 노출됨)
- published : 기본적으로 true이며 사이트에 비노출시키고 싶을 때 false로 사용
- permalink: 고유주소 지정. 기본적으로 포스트는 /posts/2019/11/27/test(페이지 타이틀)로 구성되지만, 다른 url을 지정할 수 있다.   
다른 포스트에 작성한 permalink와 중복될 경우 나중에 작성된 link가 우선시 됨
- 해당 영역에는 사용자변수도 정의할 수 있음 (전역변수인 page변수에 들어가며 page.사용자변수명 으로 호출가능)

> Front matter 작성예시
```YAML
---
layout: post
title: Refactoring
author: judy.kim
date: 2019-10-28 13:11
categories:[techCamp]
tags: [refactoring]
published : true
permalink: /blog/:title
---
```

3. 본문작성
- 이미지 경로 : {{'/assets/images/logo_1812.png'| absolute_url}}
- _posts 콜렉션 디렉토리 내에서는 해당 형식을 따르지 않는 파일들은 jekyll에 의해서 변환 및 생성되지 않는 것 같음 (posts)  
   (최종적으로 _site 디렉토리에 생성된 파일들이 쌓이고 이를 호출하는 url에 따라 보여주는 식인데 _site에 생성되지 않아 해당 이미지를 호출할 수 없다.)
```HTML
html
<img src="{{'/assets/images/logo_1812.png'| absolute_url}}" width="70%" height="70%" title="" alt=""/>
md
![이미지]({{'/assets/images/logo_1812.png'| absolute_url}})
```
