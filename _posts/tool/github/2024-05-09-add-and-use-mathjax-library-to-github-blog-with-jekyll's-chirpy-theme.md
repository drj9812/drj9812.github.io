---
title: "[GitHub | GitHub Pages]Jekyll의 Chirpy 테마를 사용한 GitHub 블로그에 MathJax 라이브러리 추가 및 사용하기"
categories: [Tool, GitHub]
tags: [GitHub, GitHub Pages, Jekyll, Chirpy, GitHub 블로그, MathJax]
math: true
image:
  path: /assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/01-mathjax-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: MathJax
---

# Jekyll의 Chirpy 테마를 사용한 GitHub 블로그에서 MathJax 라이브러리 추가 및 사용하기

![02-js-selector.html(1)](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/02-js-selector.html(1).jpg)
*`_includes/js-selector.html`*

```javascript
{% raw %}
{% if page.math %}
  <!-- MathJax -->
  <script>
    /* see: <https://docs.mathjax.org/en/latest/options/input/tex.html#tex-options> */
    MathJax = {
      tex: {
        /* start/end delimiter pairs for in-line math */
        inlineMath: [
          ['$', '$'],
          ['\\(', '\\)']
        ],
        /* start/end delimiter pairs for display math */
        displayMath: [
          ['$$', '$$'],
          ['\\[', '\\]']
        ]
      }
    };
  </script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <script id="MathJax-script" async src="{{ site.data.origin[type].mathjax.js | relative_url }}"></script>
{% endif %}
{% endraw %}
```

보통 Chirpy 테마를 GitHub Fork 방식으로 구축한 경우, 이미 `_includes/js-selector.html` 파일에 MathJax 라이브러리를 추가하는 위 코드가 포함되어 있다. 따라서 `_includes/js-selector.html` 파일에 위 코드만 명시되어 있다면 지금도 MathJax를 사용할 수 있지만, Chirpy 테마의 공식 Reposiotry에서 최근 [커밋 히스토리](https://github.com/cotes2020/jekyll-theme-chirpy/commit/44f552cbcee83d037de0e59496bf6bb19eea2691){: taregt="_blank" }를 확인해 보니, MathJax 라이브러리를 추가하는 코드를 더 이상 `_includes/js-selector.html` 파일에서 관리하지 않고 따로 자바스크립트 파일을 생성해서 관리하려는 것 같다. 

MathJax를 자바스크립트 파일을 생성해서 관리하는 방식이 아직 정식 릴리즈는 아니지만 직접 적용해 보니 사용하는 데 큰 문제는 없는 것 같다. 설명에 의하면 이 방식을 따를 경우 필요에 따라 옵션을 변경하거나 확장을 추가하는 것과 같은 작업을 할 수 있다고 한다.

## MathJax 자바스크립트 파일 생성

![03-js-selector.html(2)](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/03-js-selector.html(2).jpg)
*`_includes/js-selector.html` > MathJax 라이브러리 추가 코드 주석 또는 제거 > `<script src="{{ '/assets/js/data/mathjax.js' | relative_url }}"></script>` 추가*

![04-create-mathjax.js](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/04-create-mathjax.js.jpg)
*`assets/js/data/mathjax.js` 생성*

> 만약 블로그를 Chipry의 GtiHub Fork 방식이 아닌, Using the Chirpy Starter 방식으로 구축했다면 Chirpy 테마가 설치된 디렉토리(`bundle info --path jekyll-theme-chirpy` 명령어를 통해 나온 디렉토리)에 `mathjax.js` 파일을 생성하면 된다.
{: .prompt-info }

![05-mathjax.js](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/05-mathjax.js.jpg)
*`assets/js/data/mathjax.js`*

```javascript
{% raw %}
---
layout: compress
# WARNING: Don't use '//' to comment out code, use '{% comment %}' and '{% endcomment %}' instead.
---

{%- comment -%}
  See: <https://docs.mathjax.org/en/latest/options/input/tex.html#tex-options>
{%- endcomment -%}

MathJax = {
  tex: {
    {%- comment -%} start/end delimiter pairs for in-line math {%- endcomment -%}
    inlineMath: [
      ['$', '$'],
      ['\\(', '\\)']
    ],
    {%- comment -%} start/end delimiter pairs for display math {%- endcomment -%}
    displayMath: [
      ['$$', '$$'],
      ['\\[', '\\]']
    ],
    {%- comment -%} equation numbering {%- endcomment -%}
    tags: 'ams'
  }
};
{% endraw %}
```

생성한 `assets/js/data/mathjax.js` 파일에 위 코드를 추가한다.

위 코드에서 `tex:`는 MathJax가 TeX 형식의 수식을 웹 페이지에 렌더링하는 방식을 제어하는 설정인데, inlineMath 및 displayMath의 TeX 입력 옵션 중 구분자를 정의하고 있다. 여기서 inlineMath는 `$ 수식 $` 또는 `\\( 수식 \\)`을 명령어를 사용해서 $ a + b= c $ 와 같이 양 옆에 있는 텍스트와 동일한 줄 안에 표시하는 방식을 의미하고,

$$ a + b = c $$

displayMath는 `$$ 수식 $$` 또는 `\\[수식 \\]`을 명령어를 사용해서 위와 같이 주변 텍스트와 수직으로 중앙 정렬되어 표시되는 방식을 의미한다.

## MathJax 활성화

```yaml
---
math: true
---
```

웹 사이트 성능상의 이유로, 수학적 기능은 기본적으로 로드되지 않기 때문에 추가한 MathJax를 사용하려면 위와 같이 포스팅할 파일의 Front Matter를 통해 활성화해야 한다.

![06-test](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/06-test.jpg)
*테스트*

![07-result-test](/assets/img/posts/tool/github/add-and-use-mathjax-library-to-github-blog-with-jekyll's-chirpy-theme/07-result-test.jpg)
*테스트 결과*

## 참고자료

- [yoongyoonge, "github blog - jekyll 테마에서 수식을 표현하자(MathJax) + Markdown 주요 수식", YYE, 2023-11-26](https://yoongyoonge.github.io/blog-mathematical-expression/){: target="_blank" }
- [Park Kibum, "[BoostCamp AI Tech / 심화포스팅] Jekyll Blog와 MathJax", Coding Gallery, 2022-02-18](https://cow-coding.github.io/posts/githubblog/){: target="_blank" }