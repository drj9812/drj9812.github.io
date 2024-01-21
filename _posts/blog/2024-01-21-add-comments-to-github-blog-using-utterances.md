---
title: "utterances를 사용하여 GitHub 블로그에 댓글 기능 적용하기"
categories: [블로그, GitHub 블로그 만들기]
tags: [Jekyll, Chirpy, GitHub 블로그, 댓글 기능, utterances]
---
# utterances를 사용하여 GitHub 블로그에 댓글 기능 적용하기

## utterances 설치

![1](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/1.png)

[utterances 홈페이지](https://utteranc.es/){: target="_blank" } > [utterances app](https://github.com/apps/utterances){: target="_blank" }

![2](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/2.png)

`Install`

![3](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/3.png)

`Only select repositories` > `Select repositories` > 적용할 repsository 선택

![4](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/install-click.png)

`Install` 버튼을 클릭하면 다시 [utterances 홈페이지](https://utteranc.es/){: target="_blank" }로 이동합니다.

## utterances 설정

![4](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/4.png)

`configuration` 섹션의 `Repository`에서 `repo:`에 username/username.github.io 형식으로 입력 

![5](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/5.png)

스크롤을 내리면 `Enable Utterances` 섹션이 있는데, 위와 같이 방금 입력한 username/username.github.io 형식의 값이 repo 속성에 들어가 있는 걸 확인 후 `Copy` 버튼 클릭

![6](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/6.png)
*_layouts/post.html*

_layouts/post.html 파일에 들어가서 끝부분에 복사한 코드 붙혀넣기

<br>

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

변경사항 push

> push하기 전, `bundle exec jekyll serve` 명령어를 통해 로컬에서 먼저 확인해도 됩니다.
{: .prompt-tip }

![7](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/7.png)

**repository를 fork하여 생성한 경우 `Issue` 탭을 활성화시켜줘야 댓글이 저장됩니다.**

repository > `Settings` > `General` > `Features` 섹션에서 `Issues` 체크

![8](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/8.png)

빌드 및 배포에 성공했다면 댓글 기능이 실제로 잘 작동되는지 댓글을 달아서 확인합니다.

![9](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/9.png)

활성화시킨 `Issues` 탭에서 댓글을 확인할 수 있습니다.

![10](/assets/img/posts/blog/add-comments-to-github-blog-using-utterances/10.png)

필요에 따라 위와 같이 댓글을 삭제할 수도 있습니다.

## 참고

- [Jekyll 테마에 utterances 댓글 연동하기](https://www.irgroup.org/posts/utternace-comments-system/){: target="_blank" }
