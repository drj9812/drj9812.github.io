---
title: "[Git]Reset, Revert"
categories: [Tools, Git]
tags: [Git, 깃, Reset, Revert]
image:
  path: /assets/img/posts/tools/git/git/01-git-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Git
---

# Reset, Revert

## Reset

### Reset이란?

![01-reset(1)](/assets/img/posts/tools/git/git/reset-and-revet/01-reset(1).jpg)
*`reset`*

```bash
$ bash git reset --hard 돌아갈_커밋_해시
```

- HEAD 포인터를 이동시켜 **이전 커밋**으로 되돌리는 작업
- <font color="red">커밋 이력을 변경하므로 이전 커밋 이후의 작업 내용이 모두 삭제될 수 있음</font>
- 주로 세 가지 모드로 사용됨
  + `--soft`: 이전 커밋으로 이동하지만 변경 사항은 staged 상태로 남아 있음
  + `--mixed`(기본값): 이전 커밋으로 이동하면서 변경 사항은 unstaged 상태로 남아 있음
  + `--hard`: 이전 커밋으로 이동하면서 변경 사항은 모두 삭제됨

> 공유된 커밋을 `reset`할 경우에는 공유된 커밋을 기반으로 작업한 다른 사람들의 코드와 충돌이 발생할 수 있다. 이는 `reset` 명령어를 사용하여 커밋 이력을 변경하면 해당 브랜치의 이력이 변경되기 때문이다. 이렇게 되면 다른 사람들이 해당 브랜치를 가져와서 작업을 할 때 이전과 다른 커밋 이력을 갖게 되어 충돌이 발생할 수 있으므로, 공유된 커밋을 되돌리는 경우에는 `reset` 대신에 `revert`를 사용하는 것이 좋다.
{: .prompt-warning }

### 예시

![02-ex-reset(1)](/assets/img/posts/tools/git/git/reset-and-revet/02-ex-reset(1).jpg)
*`reset` 전*

![03-ex-reset(2)](/assets/img/posts/tools/git/git/reset-and-revet/03-ex-reset(2).jpg)
*`git log` > 돌아갈 커밋 해시 복사*

![04-ex-reset(3)](/assets/img/posts/tools/git/git/reset-and-revet/04-ex-reset(3).jpg)
*`git reset --hard 돌아갈_커밋_해시`*

![05-ex-reset(4)](/assets/img/posts/tools/git/git/reset-and-revet/05-ex-reset(4).jpg)
*`reset` 후*

## Revert

### Revert란?

![06-revert(1)](/assets/img/posts/tools/git/git/reset-and-revet/06-revert(1).jpg)
*`revert`*

```bash
$ bash git revert 돌아갈_커밋_해시
```

- 특정 커밋의 변경 사항을 취소하고 **이전 상태**로 되돌리는 작업
  + 이전 커밋을 수정하는 것이 아니라, 새로운 커밋을 생성하여 이전 상태로 되돌림
    * 새로운 커밋은 이전 커밋의 역산

> `git revert --no-commit` 명령어를 사용하면 특정 커밋의 변경 사항을 되돌리되, 즉시 새로운 커밋을 생성하지 않고 워킹 디렉토리와 인덱스에 변경 사항을 적용할 수 있다. 이렇게 하면 사용자가 변경 사항을 검토하거나 다른 변경 사항과 병합한 후 수동으로 커밋할 수 있다.
{: .prompt-info }

![07-revert(2)](/assets/img/posts/tools/git/git/reset-and-revet/07-revert(2).jpg)
*`revert`*

- 이미 공유된 커밋을 수정하지 않고 이전 상태로 돌아갈 수 있음
  + 새로운 커밋을 생성하므로 이전 커밋 이후의 이력이 유지됨

### 예시

![08-ex-revert(1)](/assets/img/posts/tools/git/git/reset-and-revet/08-ex-revert(1).jpg)
*`revert` 전*

![09-ex-revert(2)](/assets/img/posts/tools/git/git/reset-and-revet/09-ex-revert(2).jpg)
*`git log` > 돌아갈 커밋 해시 복사*

![10-ex-revert(3)](/assets/img/posts/tools/git/git/reset-and-revet/10-ex-revert(3).jpg)
*`git revert 돌아갈_커밋_해시` > `:wq`*

![11-ex-revert(4)](/assets/img/posts/tools/git/git/reset-and-revet/11-ex-revert(4).jpg)
*`revert` 후*

## 참고자료

- [얄코, "제대로 파는 Git & GitHub - 깃 끝.장.내.기", 얄팍한 코딩사전, 2022-01-21](https://www.youtube.com/watch?v=1I3hMwQU6GU){: target="_blank" }