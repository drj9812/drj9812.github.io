---
title: "[Jekyll | Chirpy]문법 강조(Syntax Highlight) 바꾸기"
categories: [Tools, Jekyll]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, Rouge]
math: true
image:
  path: /assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/01-rouge-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Rouge
---

# 문법 강조(Syntax Highlight) 바꾸기

## 문법 강조 스타일 선택하기

![02-rouge-theme-preview-site](/assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/02-rouge-theme-preview-site.jpg)
*<https://spsarolkar.github.io/rouge-theme-preview/>{: target="_blank" } > `Select Theme ▼`*

<https://spsarolkar.github.io/rouge-theme-preview/>{: target="_blank" }에 들어가서 변경할 문법 강조 스타일을 확인한다.

```bash
$ bash cd 루트 디렉토리
$ bash rougify help style
```

위 명령어를 입력하면 현재 사용할 수 있는 Rouge의 문법 강조 스타일 목록이 나온다. 변경하고자 하는 스타일이 목록에 있는지 확인한다.

## 스타일 파일 설치

```bash
$ bash rougify style 설치할 스타일 이름 > 설치할 디렉토리/파일명.scss
```

위 명령어를 통해 확인한 스타일을 설치한다. 설치할 디렉토리와 파일명은 자유롭게 설정해도 되지만, Chirpy 테마의 경우 `_sass`{: .filepath } 폴더에서 `_sass/colors`{: .filepath } 폴더를 참조하는 구조로 문법 강조를 설정하고 있기 때문에 `_sass/colors` 폴더에 설치하는 것을 권장한다.

> Chirpy 테마는 두 개(base16.dark, base16.light)의 스타일을 기반으로 문법 강조를 설정하고 있는데, 이는 화면 모드(Dark/Light 모드)가 바뀔 때 문법 강조 스타일도 함께 바뀌도록 설정하기 위함이다. 따라서 이를 구현하기 위해서는 각기 다른 2개의 스타일 파일을 설치해야 한다. 만약, 화면 모드와 상관없이 일관된 문법 강조 스타일을 유지하고 싶다면 하나의 파일만 설치하면 된다.
{: .prompt-tip }

## 설치한 스타일 파일 수정

```scss
/*
 * The syntax dark mode styles.
 */

@mixin dark-syntax {
  --language-border-color: #2d2d2d;
  --highlight-bg-color: #151515;
  --highlighter-rouge-color: #c9def1;
  --highlight-lineno-color: #808080;
  --inline-code-bg: #323238;
  --code-color: #b0b0b0;
  --code-header-text-color: #6a6a6a;
  --code-header-muted-color: #353535;
  --code-header-icon-color: #565656;
  --clipboard-checked-color: #2bcc2b;
  --filepath-text-color: #cacaca;

  .highlight .gp {
    color: #87939d;
  }

  /* --- Syntax highlight theme from `rougify style base16.dark` --- */
  //...
}
```
{: file="_sass/colors/syntax_dark.scss" }

```scss
/*
 * The syntax light mode code snippet colors.
 */

@mixin light-syntax {
  /* --- custom light colors --- */
  --language-border-color: #ececec;
  --highlight-bg-color: #f6f8fa;
  --highlighter-rouge-color: #3f596f;
  --highlight-lineno-color: #9e9e9e;
  --inline-code-bg: #f6f6f7;
  --code-color: #3a3a3a;
  --code-header-text-color: #a3a3a3;
  --code-header-muted-color: #e5e5e5;
  --code-header-icon-color: #c9c8c8;
  --clipboard-checked-color: #43c743;

  [class^='prompt-'] {
    --inline-code-bg: #fbfafa;
  }

  /* --- Syntax highlight theme from `rougify style github` --- */
  // ...
} 
```
{: file="_sass/colors/syntax_light.scss" }

위의 두 코드는 각각 Chirpy 테마에서 base16.dark, base16.light를 기반으로 사용하고 있는 문법 강조 스타일 파일(`_sass/colors/syntax_dark.scss`{: .filepath }, `_sass/colors/syntax_light.scss`{: .filepath })의 일부로서 화면 모드가 바뀔 때 적용된다.

각각의 파일은 base16.dark, base16.light 스타일에 Chipry 테마에 의해 코드가 추가된 형태(<code class="language-plaintext highlighter-rouge">/* --- Syntax highlight theme from `...` --- */</code> 위쪽이 추가된 코드)이기 때문에 추가된 부분은 제외하고, base16.dark, base16.light 스타일의 코드(<code class="language-plaintext highlighter-rouge">/* --- Syntax highlight theme from `...` --- */</code> 아래쪽 코드)를 설치한 스타일 파일의 코드로 대체해야 한다.

> 이 과정은 설치한 스타일 파일을 그대로 적용하기엔 부족한 부분이 많기 때문에 Chirpy 테마에 맞게 커스터마이징된 코드를 활용하는 것으로 CSS를 잘 아는 경우 생략해도 된다.
{: .prompt-tip }

![03-modify-installed-file](/assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/03-modify-installed-file.jpg)
*`설치한 스타일 파일명.scss`{: .filepath }*

즉, 위 사진처럼 설치한 스타일 파일에 그때 활용할 기존 스타일 파일의 코드(Chirpy 테마에 의해 추가된 코드)를 복사해서 붙여넣고, 마찬가지로 설치한 2개의 스타일 파일 중 남은 1개의 파일에도 이 과정을 반복해야 화면 모드가 바뀔 때 문법 강조 스타일도 함께 바뀔 수 있는 것이다.

> mixin의 이름은 문자, 숫자, 밑줄(`_`), 하이픈(`-`)으로 구성될 수 있지만 숫자로는 시작될 수 없다.
{: .prompt-warning }

## systax.scss 파일 수정하기

![04-modify-syntax.scss](/assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/04-modify-syntax.scss.jpg)
*`_sass/addon/syntax.scss`{: .filepath }*

`_sass/addon`{: .filepath } 폴더의 `systax.scss`{: .filepath }이 설치한 스타일 파일을 불러올(`@import`) 수 있도록 수정해야 한다.

**화면 모드에 따라 문법 강조 스타일이 바뀌도록 2개의 스타일 파일을 설치한 경우,** 기존 `@import` 문의 파일명을 설치한 파일명으로 모두 수정함으로써 불러올 수 있도록 한다. 이때 불러온(설치한) 파일의 mixin은 prefers-color-scheme 미디어 쿼리가 감지하고 반환한 값(`light`, `dark`)에 따라 사용(`@include`)되므로, `@include` 문을 사용하여 이러한 mixin을 적절하게 배치하려면 각 미디어 쿼리에 맞게 불러온 파일에서 정의한 mixin 이름으로 수정해야 한다.

**화면 모드와 상관없이 문법 강조 스타일이 유지되도록 1개의 스타일 파일을 설치한 경우,** 2개의 `@import` 문 중 1개를 제거(또는 주석 처리)하고 남은 `@import` 문의 파일명을 설치한 파일명으로 수정한다. 화면 모드와 상관없이 적용되야 하는 스타일이므로 미디어 쿼리의 반환 값과 상관없이 모든 `@include` 문의 mixin 이름을 설치한 파일명의 mixin 이름으로 수정한다.

## 확인

![05-test-light-mode](/assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/05-test-light-mode.jpg)
*Light 모드 테스트*

![06-test-dark-mode](/assets/img/posts/tools/jekyll/chirpy/change-syntax-highlight/06-test-dark-mode.jpg)
*Dark 모드 테스트*

미흡한 부분이 있다면 기존의 스타일 파일과 개발자 모드(<kbd>F12</kbd>)를 통해 수정한다.

## 참고자료

- [Carry J, "Jekyll 블로그 : Syntax highlighter 테마 설정으로 코드블럭 꾸미기", 디지털 마케팅
튜토리얼, 2021-05-20](https://hard-carry.com/how-to-change-syntax-highlighter-in-jekyll/){: target="_blank" }