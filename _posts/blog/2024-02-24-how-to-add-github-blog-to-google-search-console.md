---
title: "[Google | Search Console]GitHub 블로그를 구글 검색 엔진에 노출시키기"
categories: [블로그, GitHub 블로그 만들기]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, Google, 구글, Search Console, 노출, 색인]
---

# [Google | Search Console]GitHub 블로그를 구글 검색 엔진에 노출시키기

## Search Console이란?

Search Console은 구글에서 무료로 제공하는 서비스로, 웹사이트 소유자와 웹 마스터가 자신의 웹사이트를 구글 검색 결과에 최적화하고 성능을 모니터링할 수 있도록 도와주는 플랫폼입니다.  웹사이트를 Search Console에 등록하지 않아도 구글에 노출이 될 수는 있지만, SEO(검색 엔진 최적화)를 충분히 신경쓰지 않는다면 우선순위에서 밀릴 가능성이 높습니다. Search Console은 그러한 불확실성을 제거하고, 더 나은 노출을 위한 조치를 취할 수 있는 인사이트를 제공합니다.

## 구글 검색의 작동 방식

### 1. 크롤링(Crawling)

크롤러(구글봇)라는 자동화된 프로그램을 사용하여 인터넷에서 찾은 페이지로부터 텍스트, 이미지, 동영상을 다운로드합니다.

### 2. 색인(Index) 생성

페이지의 텍스트, 이미지, 동영상 파일을 분석하고 대규모 데이터베이스인 Google 색인에 이 정보를 저장합니다.

### 3. 검색결과 게재

사용자가 구글에서 검색하면 구글에서는 사용자의 검색어와 관련된 정보를 반환합니다.

## Search Console 시작하기

### 소유권 확인

![01-enter-url](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/01-enter-url.png)
*[Search Console 홈페이지](https://search.google.com/search-console/welcome?hl=ko){: target="_blank" } > URL 접두어 유형의 빈 칸에 블로그 주소 입력*

![02-download-html-file](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/02-download-html-file.png)
*HTML 파일 다운로드*

구글 Search Console에 등록할 웹사이트의 소유권을 확인하기 위한 작업입니다. 다른 방법들도 있지만, 구글에서는 이 방법을 권장하고 있습니다.

![03-move-html-file](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/03-move-html-file.png)
*다운로드한 파일을 루트 디렉토리로 이동*

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

구글이 파일을 확인할 수 있도록 원격 저장소에 push합니다.

![04-verify-ownership(1)](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/04-verify-ownership(1).png)
*`확인`*

![05-verify-ownership(2)](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/05-verify-ownership(2).png)
*`속성으로 이동`*

### 사이트맵 제출

> **사이트맵(Sitemap)이란?**
>
> *사이트맵은 사이트에 있는 페이지, 동영상 및 기타 파일과 각 관계에 관한 정보를 제공하는 파일입니다. Google과 같은 검색엔진은 이 파일을 읽고 사이트를 더 효율적으로 크롤링합니다. 사이트맵은 내가 사이트에서 중요하다고 생각하는 페이지와 파일을 검색엔진에 알리고 중요한 관련 정보를 제공합니다. 관련 정보의 예로는 페이지가 마지막으로 업데이트된 시간, 페이지의 대체 언어 버전이 있습니다.*
>
> ["사이트맵 알아보기", Google 검색 센터, 2024-02-22](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview?hl=ko){: target=" _blank" }

![06-add-sitemap](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/06-add-sitemap.png)
*`Sitemaps` > sitemap.xml 입력 > `제출`*

Chirpy 테마의 경우 `_site` 폴더에 `sitemap.xml` 파일이 자동으로 생성됩니다. 만약, `_site` 폴더 또는 루트 디렉토리에 `sitemap.xml` 파일이 존재하지 않는다면 [XML Sitemap Generator](https://www.xml-sitemaps.com/){: target="_blank" }와 같은 사이트를 이용하여 `sitemap.xml` 파일을 생성하고, 생성된 파일을 루트 디렉토리에 위치시킨 뒤 구글이 `sitemap.xml` 파일을 확인할 수 있도록 원격 저장소에 push합니다.

![07-submit-sitemap](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/07-submit-sitemap.png)
*닫기*

![08-succeed-in-submitting-sitemap](/assets/img/posts/blog/how-to-add-github-blog-to-google-search-console/08-succeed-in-submitting-sitemap.png)

위 사진처럼 제출한 사이트맵의 상태가 성공이라고 표시된다면 성공입니다. 이제부터 구글은 등록된 웹사이트를 수집하고 색인화하는 과정을 거친 후에 검색 결과에 표시합니다. 이 과정은 일반적으로 몇 일에서 몇 주까지 걸릴 수 있으며, 특정한 상황에 따라 더 오랜 시간이 소요될 수도 있습니다.

## 참고자료

- ["Google 검색의 작동 방식에 대한 상세 가이드", Google, 2024-02-23](https://developers.google.com/search/docs/fundamentals/how-search-works?hl=ko){: target="_blank" }