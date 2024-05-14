---
title: "[GitHub | GitHub Pages]구글 서치 콘솔 색인이 생성되지 않은 페이지"
categories: [Tools, GitHub]
tags: [GitHub, GitHub Pages, Jekyll, Chirpy, GitHub 블로그, Google, 구글, Search Console, 노출, 색인, SEO]
image:
  path: /assets/img/posts/tools/github/add-github-blog-to-google-search-console/01-google-search-console-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Google Search Console
---

# 구글 서치 콘솔 색인이 생성되지 않은 페이지

## 개요

![01-no-seach-result](/assets/img/posts/tools/github/google-search-console-not-indexing/01-no-seach-result.jpg)

정확히 작년 9월 13일에 서치 콘솔에 블로그를 등록했으니 대략 5개월이라는 시간이 지났는데, 아직도 구글에 내 블로그가 노출이 안되고 있다.

<div style="display: flex;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/02-not-indexing.jpg" style="flex: 1; margin-left: 10px;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/03-reason-not-indexing(1).jpg" style="flex: 1; margin-right: 10px;">
</div>

서치 콘솔에 들어가서 확인해보니 "발견됨 - 현재 색인이 생성되지 않음"이라는 이유로 색인이 생성되지 않고 있었다.

## 원인 분석

### 발견됨 - 현재 색인이 생성되지 않음

![04-no-seach-result(2)](/assets/img/posts/tools/github/google-search-console-not-indexing/04-reason-not-indexing(2).jpg)

[서치 콘솔 고객센터의 페이지 색인 생성 보고서](https://support.google.com/webmasters/answer/7440203#discovered__unclear_status){: target="_blank" }에 의하면 "발견됨 - 현재 색인이 생성되지 않음"이란, 크롤러인 구글봇이 나의 블로그는 발견했지만 크롤링을 하게 될 경우 어떠한 이유로 사이트가 과부하될 수 있기 때문에 크롤링 우선 순위를 뒤로 미루는 것이다.

## 해결 방법

### URL 검사 및 실제 URL 테스트

#### URL 검사

![05-url-inspection](/assets/img/posts/tools/github/google-search-console-not-indexing/05-url-inspection.jpg)
*<https://search.google.com/search-console/welcome?hl=ko>{: target="_blank" } > `URL 검사` > URL 입력 > `Enter`*

![06-request-indexing](/assets/img/posts/tools/github/google-search-console-not-indexing/06-request-indexing(1).jpg)
*`색인 생성 요청`*

페이지의 색인 생성을 요청한다. 하루에 검사를 요청할 수 있는 횟수에는 소유한 속성별로 제한이 있다.

<div style="display: flex;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/07-request-indexing(2).jpg" style="flex: 1; margin-left: 10px;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/08-request-indexing(3).jpg" style="flex: 1; margin-right: 10px;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/09-request-indexing(4).jpg" style="flex: 1; margin-right: 10px;">
</div>

색인 생성이 요청되었다. 사진에 나와있듯이, 요청을 여러 번 제출해도 크롤링의 우선순위가 변경되지 않는다.

#### 실제 URL 테스트

![10-test-url](/assets/img/posts/tools/github/google-search-console-not-indexing/10-test-url(1).jpg)
*`실제 URL 테스트`*

<div style="display: flex;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/11-test-url(2).jpg" style="flex: 1; margin-left: 10px;">
	<img src="/assets/img/posts/tools/github/google-search-console-not-indexing/12-test-url(3).jpg" style="flex: 1; margin-right: 10px;">
</div>

URL 검사를 통해서 색인 생성을 요청한 사이트의 페이지가 색인이 생성될 수 있는 페이지인지 테스트한다. `테스트된 페이지 보기` 버튼을 클릭하면 테스트된 페이지의 HTML 코드, 스크린샷, 추가 정보를 확인할 수도 있다.

테스트에서 큰 문제가 보이지 않는다면, 구글에 색인 생성이 정상적으로 요청되었다는 것이다. 이 과정을 노출하고자 하는 사이트의 모든 페이지에 반복한다.

## 확인

### 2024년 4월 28일

![13-2024-04-28](/assets/img/posts/tools/github/google-search-console-not-indexing/13-2024-04-28.jpg)

- 전과 다르게 화면의 문구가 "데이터를 처리하는 중이므로 며칠 후에 다시 확인해 보세요"로 바뀐 것을 확인하였다.

## 참고자료

- ["페이지 색인 생성 보고서", Search Console 고객센터, Date unknown](https://support.google.com/webmasters/answer/7440203#discovered__unclear_status){: target="_blank" }
- ["URL 검사 도구", Search Console 고객센터, Date unknown](https://support.google.com/webmasters/answer/9012289#not_tested){: target="_blank" }