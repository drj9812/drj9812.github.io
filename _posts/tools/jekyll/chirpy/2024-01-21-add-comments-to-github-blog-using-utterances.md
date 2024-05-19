---
title: "[Jekyll | Chirpy]utterances를 사용해서 GitHub 블로그에 댓글 기능 추가하기"
categories: [Tools, Jekyll]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, 댓글 기능, utterances]
image:
  path: /assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/01-utterances-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: utterances
---

# utterances를 사용해서 GitHub 블로그에 댓글 기능 추가하기

## utterances 설치

![02-utterances-homepage](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/02-utterances-homepage.png)
*<https://utteranc.es/>{: target="_blank" } > [utterances app](https://github.com/apps/utterances){: target="_blank" }*

![03-install-utterances(1)](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/03-install-utterances(1).png)
*`Install`*

![04-install-utterances(2)](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/04-install-utterances(2).png)
*`Only select repositories` > `Select repositories` > 적용할 Repsository 선택*

![05-install-utterances(3)](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/05-install-utterances(3).png)
*`Install`*

`Install` 버튼을 클릭하면 다시 [utterances 홈페이지](https://utteranc.es/){: target="_blank" }로 이동한다.

## utterances 설정

### Repository

![06-input-repo](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/06-input-repo.png)

이동된 [utterances 홈페이지](https://utteranc.es/){: target="_blank" }에서 스크롤을 내리면 `configuration` 섹션이 있다. `Repository` 항목의 `repo:`에 `username/username.github.io` 형식으로 입력한다.

### Mapping

![07-choose-mapping](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/07-choose-mapping.png)

uttreances는 블로그의 각 게시물에 대해 하나의 `Issues`를 사용하는데, 이때 각 게시물과 `Issues`를 어떻게 mapping할지 설정해야 한다.

나는 pathname을 선택했지만, 사실 무엇을 선택해도 상관없는 것 같다.

### Label

![08-choose-label](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/08-choose-label.png)

댓글로 발생한 Issue를 구분하기 위해서 label을 지정할 수 있다.

### Theme

![09-choose-theme](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/09-choose-theme.png)

원하는 Theme를 선택한다. 댓글의 UI가 바뀐다.

## 설정값 적용

![10-copy-script](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/10-copy-script.png)
*`Enable Utterances` 섹션 > 방금 입력한 username/username.github.io 형식의 값이 repo 속성에 들어가 있는 걸 확인 > `Copy`*

![11-paste-script](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/11-paste-script.png)
*`_layouts/post.html`{: .filepath } > 복사한 코드 붙여넣기*

<br>

```console
$ git add -A
$ git commit -m "커밋_메시지"
$ git push
```

> push하기 전, `bundle exec jekyll serve` 명령어를 통해 로컬에서 먼저 확인해도 된다.
{: .prompt-tip }

## 확인

![12-activate-issues](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/12-activate-issues.png)
*Repository > `Settings` > `General` > `Features` 섹션에서 `Issues` 체크*

> **repository를 fork하여 생성한 경우 `Issue` 탭을 활성화시켜줘야 댓글이 저장된다.**
{: .prompt-info }

![13-check-comment](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/13-check-comment.png)
*댓글 테스트*

![14-check-issues](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/14-check-issues.png)
*Blog Repopsitory > `Issues`*

댓글은 활성화시킨 `Issues` 탭에서도 확인할 수 있다.

![15-possible-delete.](/assets/img/posts/tools/jekyll/chirpy/add-comments-to-github-blog-using-utterances/15-possible-delete.png)
*댓글 삭제*

필요에 따라 위와 같이 댓글을 삭제할 수도 있다.

## 참고자료

- [하얀눈길, "Jekyll 테마에 utterances 댓글 연동하기", 하얀눈길 블로그, 2021-12-28](https://www.irgroup.org/posts/utternace-comments-system/){: target="_blank" }