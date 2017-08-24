---
layout: post
title:  "SEO: noindex, nofollow and canonical url"
date:   2017-08-22 00:25:04
author: Jun Kim
categories: SEO
---


### NOINDEX


`noindex` directive는 search engine에게 현재 페이지를 인텍싱 하지 말라는 의미이다. 기본적으로 모든 페이지는 `index`로 지정되어 있다. 
이를 피하기 위해서는 head에 다음과 같은 메타 정보값을 넣어준다.
```html
<head>
    <meta name="robots" content="noindex" />
</head>
```
search engine은 이를 보고 현재 페이지를 crawl 하지 않을 것이다.


그렇다면 어떤 경우에 사용하는 것인가? 기본적으로 페이지가 검색결과에 노출되지 않기를 바랄때 사용할 수 있다. 
- 장바구니 페이지
- 회원가입 성공/실패 페이지
- Amdin 관련 숨겨진 페이지들




### NOFOLLOW


'nofollow` directive는 search engine에게 현재 페이지에 존재하는 어떤 링크에 link equity를 부여하지 말라는 지시자이다. 링크는 SEO에서 중요한 역할을 차지 하는데 이 이유는 이 링크들이 현재 문서와 검색 query와의 연관성을 계산하는데 사용되어 ranking과 직결되기 때문이다.

역시 기본값은 `follow`로 설정되어 있지만 특정 링크의 "rel"값을 `nofollow` 로 지정하여
```html
<a href="http://www.example.com/" rel="nofollow">Click here</a>
```
search engine으로 하여금 이 링크를 연관성 계산에서 제외하도록 알려줄수 있다.

```html
<head>
    <meta name="robots" content="nofollow">
</head>
```
또는 head에 넣어 현재 페이지의 모든 링크를 제외 시킬수도 있다.

어떤 경우에 사용하면 좋을까?
crawler가 페이지 안에 링크를 따라가지 않길 원할때 사용한다. 
- 링크가 포함된 댓글: `nofollow`가 설정되어 있지 않을 경우 갖가지 spam link로 도배될수 있다.
- paid 링크: 사용자에 의한 정확한 analytics 결과를 얻고자 할때(e.g. clicking event) 

### CANONOCAL

`canonical` tag는 여러 버전의 같은 페이지들이 존재할 때 메인 페이지의 url을 지정할 때 사용된다. 

예를 들어 `/page-foo`, `/page-boo` 그리고 /`page-best` 가 모두 같은 페이지를 가리킨다고 가정하자. 

```html
<head>
    <link rel="canonical" href="http://www.example.com/best-page" />
</head>
```

그 페이지의 canonical tag를 다음과 같이 설정하면 `/page-best` 가 모든 페이지의 대표 url이 되어 검색결과에 나타날 수 있게 된다.

