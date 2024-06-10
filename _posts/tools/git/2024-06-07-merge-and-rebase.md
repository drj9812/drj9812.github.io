---
title: "[Git]Merge, Rebase"
categories: [Tools, Git]
tags: [Git, 깃, Merge, Rebase]
image:
  path: /assets/img/posts/tools/git/01-git-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Git
---

# Merge, Rebase

## Merge

### Merge란?

![git-merge(1)](/assets/img/posts/tools/git/merge-and-rebase/git-merge(1).jpg)
![git-merge(2)](/assets/img/posts/tools/git/merge-and-rebase/git-merge(2).jpg)

```bash
$ bash git merge 병합할_브랜치명
```

- 두 브랜치를 **통합**하여 하나의 브랜치로 만드는 명령어
- 병합한 브랜치의 흔적이 남음
    + Reset이 가능함
- `git merge --abort` 명령어를 통해 병합 중단 가능

### 충돌 예시

![01-ex-conflict(1)](/assets/img/posts/tools/git/merge-and-rebase/01-ex-conflict(1).jpg)
*`main` 브랜치와 `conflict-1` 브랜치가 서로 충돌되는 상황*

![02-ex-conflict(2)](/assets/img/posts/tools/git/merge-and-rebase/02-ex-conflict(2).jpg)
*`git merge` > 충돌 파일*

![03-ex-conflict(3)](/assets/img/posts/tools/git/merge-and-rebase/03-ex-conflict(3).jpg)
*충돌 해결(`수신 변경 사항 수락`) > `git add .` > `git commit` > `:wq`*

> `git commit -am "커밋 메시지"` 명령어는 수정된 파일을 스테이징하고 커밋하지만, `-a` 옵션은 충돌이 해결된 파일을 자동으로 스테이징하지 않기 때문에 충돌이 발생하면 먼저 충돌을 해결하고 수동으로 파일을 스테이징한 후 커밋해야 한다.
{: .prompt-info }

![04-ex-conflict(4)](/assets/img/posts/tools/git/merge-and-rebase/04-ex-conflict(4).jpg)
*`git merge` 결과*

## Rebase

### Rebase란?

![git-rebase(1)](/assets/img/posts/tools/git/merge-and-rebase/git-rebase(1).jpg)
![git-rebase(2)](/assets/img/posts/tools/git/merge-and-rebase/git-rebase(2).jpg)

```bash
$ bash git rebase 병합할_브랜치명
```

- 한 브랜치의 커밋들을 다른 브랜치의 **최상단으로 재배치**하는 작업
- 병합한 브랜치의 흔적이 남지 않음
- **공유된 커밋에 대해서는 사용하지 않는 것이 좋음**
- `git rebase --abort` 명령어를 통해 병합 중단 가능

### 충돌 예시

![05-ex-conflict(5)](/assets/img/posts/tools/git/merge-and-rebase/05-ex-conflict(5).jpg)
*`main` 브랜치와 `conflict-2` 브랜치가 서로 충돌되는 상황*

![06-ex-conflict(6)](/assets/img/posts/tools/git/merge-and-rebase/06-ex-conflict(6).jpg)
*`git rebase` > 첫 번째 충돌 파일*

![07-ex-conflict(7)](/assets/img/posts/tools/git/merge-and-rebase/07-ex-conflict(7).jpg)
*충돌 해결(`수신 변경 사항 수락`) > `git add .` > `git rebase --continue` > `:wq`*

![08-ex-conflict(8)](/assets/img/posts/tools/git/merge-and-rebase/08-ex-conflict(8).jpg)
*두 번째 충돌 파일*

![09-ex-conflict(9)](/assets/img/posts/tools/git/merge-and-rebase/09-ex-conflict(9).jpg)
*충돌 해결(`현재 변경 사항 수락`) > `git add .` > `git rebase --continue`*

![10-ex-conflict(10)](/assets/img/posts/tools/git/merge-and-rebase/10-ex-conflict(10).jpg)
*Rebase 결과*

두 개의 이력을 Rebase했지만, 두 번째 Rebase에서는 현재의 변경 사항을 그대로 유지함으로써 커밋을 추가할 필요가 없게 되어 하나의 이력만 생겨났다.

![11-ex-conflict(11)](/assets/img/posts/tools/git/merge-and-rebase/11-ex-conflict(11).jpg)
*브랜치 정리(Fast-forward)*

`main` 브랜치의 Head와 `conflict-2` 브랜치의 Head가 일치하지 않기 때문에 Fast-forward 방식으로 병합하여 브랜치를 정리한다.

> Fast-forward 병합은 두 브랜치가 직선적인 관계에 있을 때 사용되며, 병합 과정에서 새로운 커밋을 생성하지 않고 단순히 브랜치 포인터를 앞으로 이동시키는 방식이다.
{: .prompt-info }

![12-ex-conflict(12)](/assets/img/posts/tools/git/merge-and-rebase/12-ex-conflict(12).jpg)
*브랜치 정리 결과*

## 참고자료

- [얄코, "제대로 파는 Git & GitHub - 깃 끝.장.내.기", 얄팍한 코딩사전, 2022-01-21](https://www.youtube.com/watch?v=1I3hMwQU6GU){: target="_blank" }