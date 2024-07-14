---
title: "[HTML]&lt;script&gt;의 위치와 속성"
categories: [Language, HTML]
tags: [HTML, "&lt;script&gt;", Async, Defer]
image:
  path: /assets/img/posts/language/html/01-html5-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: HTML5
---

# &lt;script&gt;의 위치와 속성

## &lt;script&gt;의 위치

### <head>

![01-script-in-head](/assets/img/posts/language/html/script-tag-position-and-attributes/01-script-in-head.jpg)

- 브라우저가 스크립트를 발견하면 다운로드(Fetching) 및 실행(Executing)이 완료될 때까지 파싱이 일시 중단됨
- 스크립트의 용량이 크거나 개수가 많다면 사용자가 페이지를 보기까지 많은 시간이 소요될 수 있음

### <body>의 끝 부분

![01-script-in-body](/assets/img/posts/language/html/script-tag-position-and-attributes/02-script-in-body.jpg)

- 스크립트가 페이지의 모든 콘텐츠를 파싱한 후에 실행되기 때문에 사용자는 페이지를 더 빨리 볼 수 있음
- 스크립트에 의존적인 페이지의 경우 사용자는 스크립트의 다운로드 및 실행이 완료될 때까지 완성되지 않은 페이지를 볼 수 있음

### <head> + async 속성

![03-script-with-async-in-head](/assets/img/posts/language/html/script-tag-position-and-attributes/03-script-with-async-in-head.jpg)

- 파싱과 스크립트 다운로드가 병렬로 진행되며 다운로드가 완료되면 실행이 완료될 때까지 파싱이 일시 중단됨
- 다운로드 받는 시간을 절약할 수 있지만 파싱이 끝나기도 전에 스크립트가 실행되기 때문에 스크립트 내에서 HTML 요소를 조작하거나 사용하려는 경우 문제가 발생할 수 있음
- 스크립트를 실행하기 위해 파싱이 일시 중단되기 때문에 여전히 사용자가 페이지를 보기까지 시간이 소요될 수 있음

> 스크립트가 여러 개일 경우 먼저 다운로드된 순서대로 스크립트가 실행된다.
{: .prompt-info }

### <head> + defer 속성

![04-script-with-defer-in-head](/assets/img/posts/language/html/script-tag-position-and-attributes/04-script-with-defer-in-head.jpg)

- 파싱과 스크립트 다운로드가 병렬로 진행되며 파싱이 완료된 후 스크립트가 실행됨
- 가장 권장되는 방식

> 다운로드한 순서와 상관없이 HTML 파일 내에 명시된 순서대로 스크립트가 실행됨
{: .prompt-info }

# 참고자료

- [엘리, *자바스크립트 2. 콘솔에 출력, script async 와 defer의 차이점 및 앞으로 자바스크립트 공부 방향 \| 프론트엔드 개발자 입문편 (JavaScript ES5+)*, 드림코딩, 2020-04-07](https://www.youtube.com/watch?v=tJieVCgGzhs&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=2){: target="_blank" }