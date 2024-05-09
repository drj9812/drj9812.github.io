---
title: "[GitHub | GitHub Pages]GitHub 블로그를 네이버 검색 엔진에 노출시키기"
categories: [Tool, GitHub]
tags: [GitHub, GitHub Pages, Jekyll, Chirpy, GitHub 블로그, Naver, 네이버, Search Advisor, 노출, 색인]
image:
  path: /assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/01-naver-search-advisor-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Naver Search Advisor
---

# GitHub 블로그를 네이버 검색 엔진에 노출시키기

## Search Advisor 시작하기

### 사이트 소유확인

![02-naver-search-advisor-homepage](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/02-naver-search-advisor-homepage.jpg)
*[Search Advisor 홈페이지](https://searchadvisor.naver.com/){: target="_blank" } > 로그인 > `웹마스터 도구`*

![03-enter-url](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/03-enter-url.jpg)
*URL 입력 > `Enter`*

![04-download-html-file](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/04-download-html-file.jpg)
*HTML 파일 다운로드*

![05-move-html-file](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/05-move-html-file.jpg)
*다운로드한 파일을 루트 디렉토리로 이동*

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

네이버가 HTML 파일을 확인할 수 있도록 변경사항을 원격 저장소에 업로드한다.

![06-verify-ownership(1)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/06-verify-ownership(1).jpg)
*소유확인*

![07-verify-ownership(2)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/07-verify-ownership(2).jpg)
*확인*

![08-click-url](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/08-click-url.jpg)
*등록된 사이트 클릭*

### RSS 제출

> *RSS(Rich Site Summary, Really Simple Syndication)*
>
> *RSS는 어떤 사이트에 새로운 콘텐츠가 올라왔을 때 해당 사이트에 방문하지 않고, RSS서비스를 통해 리더 한 곳에서 그 콘텐츠를 이용하기 위한 방법이다. 쉽게 생각하면, 여러 언론사 사이트를 모두 방문할 필요 없이 다양한 기사를 네이버뉴스 한 곳에서 볼 수 있는 것과 같다고 보면 된다.*
>
> [나무위키, "RSS", 2023-03-21](https://namu.wiki/w/RSS){: target=" _blank" }

![09-not-valid-rss](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/09-not-valid-rss.jpg)

Chirpy 테마의 경우 RSS 피드를 위한 Atom 형식으로 작성된 `feed.xml` 파일이 `assets` 폴더에 자동으로 생성되는데, 네이버의 서치어드바이저는 Atom 형식이 아닌 RSS 형식으로 작성된 RSS 피드 파일을 요구한다. 그래서 Atom 형식으로 자동으로 생성된 이 `feed.xml` 파일의 URL을 제출할 경우, 위 사진처럼 "올바른 RSS 가 아닙니다." 라는 창이 뜨면서 제출에 실패하게 된다.

![10-generate-rss(1)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/10-generate-rss(1).jpg)
*[https://jekyllcodex.org/without-plugin/rss-feed/](https://jekyllcodex.org/without-plugin/rss-feed/){: target="_blank" } > `feed.xml`*

다행히 Jekyll은 어드바이저가 요구하는 RSS 형식의 RSS 피드 파일을 작성하는 방법을 제공하고 있다.

```xml
{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:10 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% for tag in post.tags %}
        <category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}
        <category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>
{% endraw %}
```

![11-generate-rss(2)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/11-generate-rss(2).jpg)
*코드 복사*

![12-generate-rss(3)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/12-generate-rss(3).jpg)
*루트 디렉토리에 `rss.xml` 파일 생성 > 편집기를 사용하여 생성한 `rss.xml` 파일에 복사한 코드 붙여넣기*

> Atom 형식으로 작성된 기존의 `feed.xml` 파일은 나중에 쓰일 수도 있으니 굳이 삭제하지 않았다.
{: .prompt-tip }

![13-generate-rss(4)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/13-generate-rss(4).jpg)
*코드 복사*

![14-generate-rss(5)](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/14-generate-rss(5).jpg)
*편집기를 사용하여 `_includes` 폴더에 위치한 `head.html` 파일의 `<head>` 태그 안에 복사한 코드 붙여넣기*

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

네이버가 RSS 파일을 확인할 수 있도록 변경사항을 원격 저장소에 업로드한다.

![15-submit-rss](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/15-submit-rss.jpg)
*`요청` > `RSS 제출` > https://username.github.io/rss.xml 입력 > `확인`*

### 사이트맵 제출

![16-submit-sitemap](/assets/img/posts/tool/github/how-to-add-github-blog-to-naver-search-advisor/16-submit-sitemap.jpg)
*`요청` > `사이트맵 제출` > https://username.github.io/sitemap.xml 입력 > `확인`*

RSS와 사이트맵을 성공적으로 제출했다면, 이제부터 네이버는 등록된 웹사이트를 수집하고 색인화하는 과정을 거친 후에 검색 결과에 표시한다. 이 과정은 일반적으로 몇 일에서 몇 주까지 걸릴 수 있으며, 특정한 상황에 따라 더 오랜 시간이 소요될 수도 있다.

## 참고자료

- [Youngseok Choi, "Jekyll - 올바른 RSS가 아닙니다 (네이버 서치어드바이저)", Three Dash Two Four, 2020-12-03](https://3-24.github.io/scribbles/naver-search-atom/){: target="_blank" }