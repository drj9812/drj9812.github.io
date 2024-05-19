---
title: "[Jekyll | Chirpy]GitHub 블로그 글에 조회수 추가하기"
categories: [Tools, Jekyll]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, GoatCounter]
math: true
image:
  path: /assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/01-goatcounter-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: GoatCounter
---

# GitHub 블로그 글에 조회수 추가하기

## GoatCounter 회원가입

![02-goatcounter-homepage](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/02-goatcounter-homepage.jpg)
*<https://blog.ju-ing.com/posts/jekyll-theme-chirpy-goatcounter/>{: taget="_blank" } > `☞ Sign up`*

![03-sign-up-goatcounter](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/03-sign-up-goatcounter.jpg)
*정보 기입 > `Sign up`*

- Code: 생성될 GoatCounter 계정의 서브 도메인 주소
  + https://your_code.goatcounter.com/
- Site domain: 조회수가 추가될 도메인 주소
- Email address: 이메일 주소
- Password: 비밀번호
- Fill in 9 here: 9 입력

![04-verify-email-and-click-settings](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/04-verify-email-and-click-settings.jpg)
*Email 인증 > `Settings`*

![05-check-and-save](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/05-check-and-save.jpg)
*`Allow adding visitor counts on your website` 체크 > `Save`*

## _config.yml 수정

![06-modify-_config.yml](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/06-modify-_config.yml.jpg)
*`_config.yml`{: .filepath } 수정*

- `analyanalytics.goatcounter`: 회원가입할 때 입력한 Code(서브 도메인 주소)
- `pageviews.Provider`: GoatCounter

> Chirpy 테마의 버전이 낮거나 업데이트하지 않아 GoatCounter와 관련된 파일이 없는 경우, [feat(analytics)!: add post pageviews for GoatCounter (#1543)](https://github.com/cotes2020/jekyll-theme-chirpy/commit/90693ff95e72ca4b5135a7b454a6ab521b995b3e){: target="_blank" }, [feat: add analytics support for GoatCounter (#1526)](https://github.com/cotes2020/jekyll-theme-chirpy/commit/b641b3f1f2e54bcfe96d8dff46d4f94186492d98){: target="_blank" }를 참조해서 파일을 생성한다.
{: .prompt-info }

## 확인

![07-check-views](/assets/img/posts/tools/jekyll/chirpy/add-views-to-posts-in-github-blog-using-goatcounter/07-check-views.jpg)
*성공*

## 참고자료

- [Ju-ing, "goatcounter 페이지 조회수 가져오기 (jekyll-theme-chirpy)", Ju-ing's blog, 2024-03-19](https://blog.ju-ing.com/posts/jekyll-theme-chirpy-goatcounter/){: target="_blank" }