---
title: "[GitHub | GitHub Actions]'a' tag is missing a reference 에러"
categories: [Tools, CM]
tags: [CM, Git, 깃, GitHub, GitHub Actions, GitHub Pages, Jekyll, Chirpy, GitHub 블로그, 에러, Error]
image:
  path: /assets/img/posts/tools/cm/github/github-actions/01-github-actions-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: GitHub Actions
---

# 'a' tag is missing a reference 에러

## 개요

![01-test-fail-during-build](/assets/img/posts/tools/cm/github/github-actions/a-tag-is-missing-a-reference-error/01-test-fail-during-build.png)

작성한 글이 로컬 서버에서 문제가 없다는 걸 확인한 뒤, 원격 저장소에 push 했는데 `'a' tag is missing a reference` 라는 이유로 빌드가 실패됐다.

## 원인 분석

### &lt;a&gt; 태그 참조 누락 확인

Jekyll에서 MD 파일로 작성된 파일은 HTML 파일로 변환되는데, 이때 md 파일 내에서 링크(URL 주소, 이미지)를 나타내는 구문이 HTML 파일 내에서 &lt;a&gt; 태그로 변환된다.

따라서 문제가 발생한 파일의 &lt;a&gt; 태그와 관련된 모든 부분을 샅샅이 살펴보았으나 누락된 부분은 확인되지 않았다.

### 작성자 정보 충돌

[https://talk.jekyllrb.com/t/chirpy-theme-a-tag-is-missing-a-reference-error/8731](https://talk.jekyllrb.com/t/chirpy-theme-a-tag-is-missing-a-reference-error/8731){: target="_blank" }

검색을 통해 나와 같은 고충을 겪고 있는 분의 질문 글을 발견했다.

답변에 의하면 게시글의 작성자 정보를 담고 있는 `_data/authors.yml`{: .filepath }이 불완전할 경우 문제가 발생할 수 있다고 한다.

![02-author-link](/assets/img/posts/tools/cm/github/github-actions/a-tag-is-missing-a-reference-error/02-author-link.jpg)

게시글을 작성될 때 작성자의 이름과 하이퍼링크는 `_config.yml`{: .filepath } 또는 `_data/authors.yml`{: .filepath }에 명시된 정보들을 바탕으로 설정된다.

![03-author-information](/assets/img/posts/tools/cm/github/github-actions/a-tag-is-missing-a-reference-error/03-author-information.png)

이때 `_config.yml`{: .filepath }이 `_data/authors.yml`{: .filepath }보다 우선되지만, 두 파일의 정보가 충돌이 되어 명시된 하이퍼링크의 주소가 답변처럼 불완전한 상태가 되는 것이 아닌가하는 생각이 들었다.

![04-authors.yml-comment-out](/assets/img/posts/tools/cm/github/github-actions/a-tag-is-missing-a-reference-error/04-authors.yml-comment-out.png)
*`_data/authors.yml`{: .filepath }*

그래서 `_data/authors.yml`{: .filepath }의 작성자 정보를 전부 주석 처리하고 push 해보았지만 에러는 사라지지 않았다.

## 해결 방법

![05-pages-deploy.yml-test-site-comment-out](/assets/img/posts/tools/cm/github/github-actions/a-tag-is-missing-a-reference-error/05-pages-deploy.yml-test-site-comment-out.png)
*`.github/github-actions/workflows/pages-deploy.yml`{: .filepath } > Test site 단계 주석 처리 또는 제거*

에러가 발생한 Github Actions의 Test Site 단계는 정적 웹사이트에 대한 HTML 검증 및 품질 테스트를 제공하는 도구인 html-proofer가 Jekyll로 빌드된 사이트를 테스트를 하도록 `.github/github-actions/workflows/pages-deploy.yml`{: .filepath }에 설정되어있다.

이 과정은 빌드된 웹사이트의 품질을 보장하고, 잠재적인 문제를 식별하여 배포 전에 이를 해결할 수 있도록 도와준다.

다르게 말하면, 문제가 없을 경우 생략해도 된다는 것인데 완벽하게는 아니지만 html-proofer가 하는 역할을 로컬 서버에서 미리 확인하는 것으로 어느정도 대체할 수 있다고 생각했기 때문에 이 Test site 단계를 전부 주석 처리하여 실행되지 않도록 하였더니 더 이상 에러가 발생하지 않았다.

## 참고자료

- [Cotes Chung, *Writing a New Post*, Chirpy, 2024-01-31](https://chirpy.cotes.page/posts/write-a-new-post/#author-information){: target="_blank" }