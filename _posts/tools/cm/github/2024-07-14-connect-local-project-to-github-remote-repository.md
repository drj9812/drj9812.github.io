---
title: "[GitHub]로컬 프로젝트를 원격 저장소에 연결하기"
categories: [Tools, CM]
tags: [CM, Git, 깃, GitHub]
image:
  path: /assets/img/posts/tools/cm/github//01-github-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: GitHub
---

# 로컬 프로젝트를 원격 저장소에 연결하기

![01-copy-the-clipboard](/assets/img/posts/tools/cm/github/connect-local-project-to-github-remote-repository/01-copy-the-clipboard.jpg)
*Repository > `HTTPS` 또는 `SSH` 선택 > `…or create a new repository on the command line` 또는 `…or push an existing repository from the command line`의 클립보드 복사*

SSH 키가 존재하는 경우 SSH 방식을 사용해도 되며, 로컬에 git으로 관리되고 있는 이미 프로젝트가 존재하는 경우 `…or create a new repository on the command line`의 클립보드의 명령어를 복사하여 실행한다.

```bash
$ bash git remote add origin https://github.com/username/repo_name.git
$ bash git branch -M main
$ bash git push -u origin main
```

- `git remote add origin https://github.com/username/repo_name.git`
  + 현재 로컬 Git 저장소에 `origin`이라는 새로운 원격 저장소 추가
- `git branch -M main`
  + 현재 로컬 Git 저장소의 브랜치 이름을 `main`으로 변경
- `git push -u origin main`
  + 로컬 저장소의 main 브랜치를 원격 저장소의 `main` 브랜치에 푸시
  + `-u`
    * 설정한 원격 브랜치(origin main)를 추적하도록 설정
    * 이후 `git push` 명령어를 사용할 때, 원격 저장소와 브랜치를 생략할 수 있게 해줌

![02-result-push](/assets/img/posts/tools/cm/github/connect-local-project-to-github-remote-repository/02-result-push.jpg)
*성공*

## 참고자료

- [얄코, *제대로 파는 Git & GitHub - 깃 끝.장.내.기*, 얄팍한 코딩사전, 2022-01-21](https://www.youtube.com/watch?v=1I3hMwQU6GU){: target="_blank" }