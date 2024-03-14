---
title: "[GitHub | GitHub Pages]GitHub 블로그에 댓글 기능 적용하기"
categories: [Tool, GitHub]
tags: [GitHub, GitHub Pages, Jekyll, Chirpy, GitHub 블로그, 댓글 기능, utterances]
---
# GitHub 블로그에 댓글 기능 적용하기

## 참조

- [[Jekyll \| Chirpy]GitHub 블로그 만들기(Windows)](https://drj9812.github.io/posts/make-github-blog-with-jekyll-chirpy-on-windows/){: target="_blank" }

## utterances 설치

![01-utterances.homepage](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/01-utterances-homepage.png)
*[utterances 홈페이지](https://utteranc.es/){: target="_blank" } > [utterances app](https://github.com/apps/utterances){: target="_blank" }*

![02-install-utterances(1)](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/02-install-utterances(1).png)
*`Install`*

![03-install-utterances(2)](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/03-install-utterances(2).png)
*`Only select repositories` > `Select repositories` > 적용할 repsository 선택*

![04-install-utterances(3)](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/04-install-utterances(3).png)
*`Install`*

`Install` 버튼을 클릭하면 다시 [utterances 홈페이지](https://utteranc.es/){: target="_blank" }로 이동합니다.

## utterances 설정

### Repository

![05-input-repo](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/05-input-repo.png)

이동된 [utterances 홈페이지](https://utteranc.es/){: target="_blank" }에서 스크롤을 내리면 `configuration` 섹션이 있습니다. `Repository` 항목의 `repo:`에 username/username.github.io 형식으로 입력합니다.

### Mapping

![06-choose-mapping](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/06-choose-mapping.png)

uttreances는 블로그의 각 게시물에 대해 하나의 `Issues`를 사용하는데, 이때 각 게시물과 `Issues`를 어떻게 mapping할지 설정해야 합니다.

저는 pathname을 선택했지만, 사실 무엇을 선택해도 상관없는 것 같습니다. 

### Label

![07-choose-label](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/07-choose-label.png)

댓글로 발생한 issue를 구분하기 위해서 label을 지정할 수 있습니다. 저는 아무값도 지정하지 않았습니다.

### Theme

![08-choose-theme](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/08-choose-theme.png)

원하는 theme를 선택합니다. 댓글의 UI가 바뀝니다.

## 설정값 적용

![09-copy-script](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/09-copy-script.png)

스크롤을 내리면 `Enable Utterances` 섹션이 있는데, 위와 같이 방금 입력한 username/username.github.io 형식의 값이 repo 속성에 들어가 있는 걸 확인 후 `Copy` 버튼 클릭

![10-paste-script](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/10-paste-script.png)
*_layouts/post.html*

`_layouts/post.html` 파일에 들어가서 끝부분에 복사한 코드 붙혀넣기

<br>

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

> push하기 전, `bundle exec jekyll serve` 명령어를 통해 로컬에서 먼저 확인해도 됩니다.
{: .prompt-tip }

## 확인

![11-activate-issues](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/11-activate-issues.png)
*repository > `Settings` > `General` > `Features` 섹션에서 `Issues` 체크*

> **repository를 fork하여 생성한 경우 `Issue` 탭을 활성화시켜줘야 댓글이 저장됩니다.**
{: .prompt-info }

![12-check-comment](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/12-check-comment.png)

빌드 및 배포에 성공했다면 댓글 기능이 실제로 잘 작동되는지 댓글을 달아서 확인합니다.

![13-check-issues](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/13-check-issues.png)

댓글은 활성화시킨 `Issues` 탭에서도 확인할 수 있습니다.

![14-possible-delete.](/assets/img/posts/tool/github/add-comments-to-github-blog-using-utterances/14-possible-delete.png)

필요에 따라 위와 같이 댓글을 삭제할 수도 있습니다.

## 참고자료

- [하얀눈길, "Jekyll 테마에 utterances 댓글 연동하기", 하얀눈길 블로그, 2021-12-28](https://www.irgroup.org/posts/utternace-comments-system/){: target="_blank" }