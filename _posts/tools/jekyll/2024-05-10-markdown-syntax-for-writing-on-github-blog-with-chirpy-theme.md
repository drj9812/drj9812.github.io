---
title: "[Jekyll]Chirpy 테마를 적용한 GitHub 블로그의 글 작성을 위한 마크다운(Markdown) 문법"
categories: [Tools, Jekyll]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, Markdown, 마크다운]
math: true
image:
  path: /assets/img/posts/tools/jekyll/markdown-syntax-for-writing-on-github-blog-with-chirpy-theme/01-markdown-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Markdown
---

# Chirpy 테마를 적용한 GitHub 블로그의 글 작성을 위한 마크다운(Markdown) 문법

## 백틱(\`) 기호를 인라인 코드로 표현

```markdown
<code class="language-plaintext highlighter-rouge">`</code>
```

## 코드 블럭 이름 표시

```markdown
```language
코드 작성
\```
{: file="표시할 이름" }
```
{: file="이곳에 이름이 표시된다." }

- 위 코드에서 역슬래시(`\`) 제거

## 렌더링 제외

```markdown
```language
{\% raw %}
렌더링 제외시킬 코드
{\% endraw %}
\```
```

- 위 코드에서 역슬래시(`\`) 제거

## 파일 주소

```markdown
`browserconfig.xml`{: .filepath}
```

## 참고자료

- [Cotes Chung, "Writing a New Post", Chirpy, 2024-01-31](https://chirpy.cotes.page/posts/write-a-new-post/){: target="_blank" }
- [Cotes Chung, "Text and Typography", Chirpy, 2024-01-31](https://chirpy.cotes.page/posts/text-and-typography/){: target="_blank" }