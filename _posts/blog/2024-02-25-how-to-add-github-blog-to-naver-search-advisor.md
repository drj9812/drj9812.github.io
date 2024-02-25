---
title: "[Naver | Search Advisor]GitHub 블로그를 네이버 검색 엔진에 노출시키기"
categories: [블로그, GitHub 블로그 만들기]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, Naver, 네이버, Search Advisor, 노출, 색인]
---

# [Naver | Search Advisor]GitHub 블로그를 네이버 검색 엔진에 노출시키기

## Search Advisor 시작하기

### 사이트 소유확인

![01-naver-search-advisor-homepage](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/01-naver-search-advisor-homepage.jpg)
*[Search Advisor 홈페이지](https://searchadvisor.naver.com/){: target="_blank" } > 로그인 > `웹마스터 도구`*

![02-enter-url](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/02-enter-url.jpg)
*URL 입력 > `Enter`*

![03-download-html-file](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/03-download-html-file.jpg)
*HTML 파일 다운로드*

![04-move-html-file](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/04-move-html-file.jpg)
*다운로드한 파일을 루트 디렉토리로 이동*

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

네이버가 HTML 파일을 확인할 수 있도록 변경사항을 원격 저장소에 업로드합니다.

![05-verify-ownership(1)](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/05-verify-ownership(1).jpg)
*소유확인*

![06-verify-ownership(2)](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/06-verify-ownership(2).jpg)
*확인*

![07-click-url](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/07-click-url.jpg)
*등록된 사이트 클릭*

### RSS 제출

> **RSS(Rich Site Summary, Really Simple Syndication)란?**
>
> *RSS는 어떤 사이트에 새로운 콘텐츠가 올라왔을 때 해당 사이트에 방문하지 않고, RSS서비스를 통해 리더 한 곳에서 그 콘텐츠를 이용하기 위한 방법이다. 쉽게 생각하면, 여러 언론사 사이트를 모두 방문할 필요 없이 다양한 기사를 네이버뉴스 한 곳에서 볼 수 있는 것과 같다고 보면 된다.*
>
> ["RSS", 나무위키, 2023-03-21](https://namu.wiki/w/RSS){: target=" _blank" }

![08-not-valid-rss](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/08-not-valid-rss.jpg)

Chirpy 테마의 경우 RSS 피드를 위한 Atom 형식으로 작성된 `feed.xml` 파일이 `assets` 폴더에 자동으로 생성되는데, 네이버의 서치어드바이저는 Atom 형식이 아닌 RSS 형식으로 작성된 RSS 피드 파일을 요구합니다.

그래서 이 `feed.xml` 파일의 URL 제출할 경우, 위 사진처럼 `올바른 RSS 가 아닙니다.` 라는 문구가 뜨면서 RSS 제출에 실패하게 되는데, 다행히 [Jekyll은 RSS 형식으로 작성된 RSS 피드 파일을 작성하는 방법](https://jekyllcodex.org/without-plugin/rss-feed/){: target="_blank" }을 제공하고 있습니다.

서치어드바이저가 요구하는 RSS 파일을 생성하려면 [Jekyll은 RSS 형식으로 작성된 RSS 피드 파일을 작성하는 방법](https://jekyllcodex.org/without-plugin/rss-feed/){: target="_blank" }에 들어가서 따라하거나, 사이트의 루트 디렉토리 `rss.xml` 파일을 생성한 뒤, 편집기를 이용하여 이 [링크](https://raw.githubusercontent.com/jhvanderschee/jekyllcodex/gh-pages/feed.xml){: target="_blank" }의 코드를 복사해서 붙혀넣으면 됩니다. Atom 형식으로 작성된 기존의 `feed.xml` 파일은 나중에 쓰일 수도 있으니 그대로 둡니다.

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

네이버가 RSS 파일을 확인할 수 있도록 변경사항을 원격 저장소에 업로드합니다.

![09-submit-rss](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/09-submit-rss.jpg)
*`요청` > `사이트맵 제출` > https://username.github.io/rss.xml 입력 > `확인`*

### 사이트맵 제출

![10-submit-sitemap](/assets/img/posts/blog/how-to-add-github-blog-to-naver-search-advisor/10-submit-sitemap.jpg)
*`요청` > `사이트맵 제출` > https://username.github.io/sitemap.xml 입력 > `확인`*

RSS와 사이트맵을 성공적으로 제출했다면, 이제부터 네이버는 등록된 웹사이트를 수집하고 색인화하는 과정을 거친 후에 검색 결과에 표시합니다. 이 과정은 일반적으로 몇 일에서 몇 주까지 걸릴 수 있으며, 특정한 상황에 따라 더 오랜 시간이 소요될 수도 있습니다.

## 참고자료

- [Youngseok Choi, "Jekyll - 올바른 RSS가 아닙니다 (네이버 서치어드바이저)", Three Dash Two Four, 2020-12-03](https://3-24.github.io/scribbles/naver-search-atom/){: target="_blank" }