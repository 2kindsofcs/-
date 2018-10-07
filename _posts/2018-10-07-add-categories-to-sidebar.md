---
layout: posts
title: "사이드바에 카테고리 메뉴 추가하기"
slug: "add-categories-to-sidebar"
categories: general
---

카테고리별 글을 볼 수 있도록 사이드바에 카테고리별 링크를 추가했다. 
링크를 누르면 해당 카테고리에 포함된 모든 글들을 보여주는 페이지로 연결된다. 

이를 위해 `_includes` 폴더 밑에 있는 sidebar.html 파일을 수정하였다.
추가한 코드는 아래와 같다.

{% raw %}
```html
  <ul>
    {% for category in site.categories %}
    <li>
        <a href="{{site.baseurl}}/category/{{category[0]|slugize}}">{{category[0]|capitalize}}</a>
    </li>
    {% endfor %}
  </ul>
 ``` 
{% endraw %}

`categories.html` 파일을 살펴보았을 때, `site.categories`를 통해 카테고리와 관련된 정보를 가져올 수 있음을 알았다.
이를 통해 정보를 받아올 경우 `category[0]`이 카테고리명을 의미하는 것 같아보였다.  
[지킬 공식 문서]를 확인해보니, `site.categories.CATEGORY`라고 써놔서 바로 알아보기가 힘들었지만
 CATEGORY에 속한 모든 포스트 목록이라는 설명을 보니 `site.categories`는 딕셔너리의 키, 밸류값을 묶어서 튜플로 만든 것의 집합으로 보인다. 
 각 튜플의 형태는 **카테고리명, 해당 카테고리에 속한 포스트들**일 것이므로 추측했던 대로 category[0]을 이용해서 링크를 걸어주면 되었다.  

처음에는 category[0]이 아닌 category라고만 적었다가 사이드바에 전체 블로그 페이지가 출력되어버리는 괴상한 현상을 보았다. 
`posts.html`에서는 아래와 같이 카테고리 링크를 출력하기 때문에, 똑같이 작성하면 그대로 작동할 것이라고 생각했던 것이 문제였다.

{% raw %}
```html
{% for category in page.categories %}
  <a href="{{site.baseurl}}/category/{{category|slugize}}">{{category|capitalize}}</a>
{% unless forloop.last %}, {% endunless %}
{% endfor %}
```
{% endraw %}

`post.html`에서는 `page.categories`를 참조한다는 점을 간과했던 것이다. 
지킬에서 사용하는 다른 변수들에 대해서는 [지킬 공식 문서]를 참조하면 좋다.

[지킬 공식 문서]: https://jekyllrb-ko.github.io/docs/variables/ "Jekyll 변수 목록"