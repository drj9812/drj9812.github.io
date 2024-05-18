---
title: "[Jekyll | Chirpy]Chipry 테마를 적용한 GitHub 블로그 만들기(Windows)"
categories: [Tools, Jekyll]
tags: [Jekyll, Chirpy, GitHub, GitHub Pages, GitHub 블로그, Ruby 설치, Node.js 설치]
image:
  path: /assets/img/posts/tools/jekyll/01-jekyll-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Jekyll
---

# Chipry 테마를 적용한 GitHub 블로그 만들기(Windows)

기존에도 여러 글들을 참고하여 Jekyll의 Chirpy 테마를 적용한 블로그를 운영하고 있었는데, 빌드 및 배포 과정에서 여러 에러들이 발생하였다. 참고한 글마다 환경, 버전에 따른 소스 코드들이 전부 달랐음에도 불구하고 취합해서 구축하려다 보니 발생한 문제였다.

에러들을 해결하기 위해 노력했지만 여러 이유로 한계를 느꼈고, 이런 일이 더 이상 발생하지 않도록 기존의 블로그를 삭제하고 다시 구축하게 되었다.

![01-chirpy-getting-started(1)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/01-chirpy-getting-started(1).png)

위 [Chirpy 테마 시작 가이드](https://chirpy.cotes.page/posts/getting-started/){: target="_blank" }에 의하면 새 repository를 만드는 방법에는 두 가지가 있는데, 이 글에선 맞춤형 개발에 적합한 GitHub Fork 방식을 사용한다.

## 개발 환경
2024.01.19일 기준
- OS: Winodws 11 Home
- Ruby: 3.2.2
- Node.js: 20.11.0

## 개발 환경 구축

이 글은 기본적으로 git이 설치되어있다는 전제로 작성되었기 때문에 git 설치 과정은 생략한다.

### Ruby 설치

![02-jekyll-docs](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/02-jekyll-docs.png)

Ruby를 기반으로 만들어진 Jekyll을 구동시키기 위해서는 반드시 Ruby를 설치해야 한다. [Jekyll 공식 문서](https://jekyllrb.com/docs/installation/){: target="_blank" }에 의하면 **최소 Ruby 2.5.0 버전 또는 그 이상이 요구**된다.

![03-download-ruby](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/03-download-ruby.png)
*<https://rubyinstaller.org/downloads/>{: target="_blank" } > `Ruby+Devkit 3.2.2-1 (x86)` 다운로드*

나는 64bit(x64) 운영체제를 사용하고 있고 문서에서도 3.2.X (x64)를 권장하고있지만, Jekyll이 32bit(x86)라서 32bit(x86)를 설치했다.

![04-install-ruby(1)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/04-install-ruby(1).png)
*`rubyinstaller-devkit-3.2.2-1-x86.exe`{: .filepath } 실행 > `Install for me only (recommended)`*

![05-install-ruby(2)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/05-install-ruby(2).png)
*`I accept the Lincense` 체크 > `Next`*

![06-install-ruby(3)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/06-install-ruby(3).png)
*`Add Ruby executables to your PATH`, `Associate .rb and .rbw files with this Ruby installation` 체크 > `Install`*

- Add Ruby executables to your PATH: 환경 변수 PATH에 ruby 디렉토리 추가
- Associate .rb and .rbw files with this Ruby installation: 명령 프롬프트 또는 스크립트에서 `.rb` 및 `.rbw` 파일을 실행할 때 시스템이 해당 Ruby를 사용하도록 연동

![07-install-ruby(4)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/07-install-ruby(4).png)
*`Ruby RI and HTML documentation`, `MSYS2 development toolcahin 2023-04-01` 체크 > `Next`*

- Ruby RI and HTML documentation: Ruby의 도움말 시스템인 Ruby Interative Reference와 HTML 설명서
- MSYS2 development toolcahin 2023-04-01: Windows에서 유닉스 스타일의 개발 환경을 제공하기 위한 툴체인과 개발 환경을 제공하는 도구

![08-install-ruby(5)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/08-install-ruby(5).png)
*`Run 'ridk install' to setup MSYS2 and development toolchain. MSYS2 is required to install gems with C extensions.` 체크 > `Finish`*

![09-install-ruby(6)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/09-install-ruby(6).png)
*`Enter`*

![10-install-ruby(7)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/10-install-ruby(7).png)
*`Enter`*

### Node.js 설치

<a id=anchor1></a>
![11-chirpy-getting-started(2)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/11-chirpy-getting-started(2).png)

[Chirpy 테마 시작 가이드](https://chirpy.cotes.page/posts/getting-started/){: target="_blank" }에 의하면 Node.js를 설치하고 `bash tool/init` 명령어를 실행해야 한다. `tool`{: .filepath } 폴더에 있는 `init`{: .filepath }을 실행하는 명령어인데, 이 `init`{: .filepath } 내부에 `npm i && npm run build` 명령어를 실행하는 부분이 있다. 여기서 npm은 `npm` 명령어를 통해 Node.js의 패키지를 관리하는 Node Package Manager의 약자다. npm은 Node.js를 설치하면 함께 설치된다.

![12-node-homepage](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/12-node-homepage.png)
*<https://nodejs.org/en>{: target="_blank" } > `20.11.0 LTS` 다운로드*

![13-install-node(1).=](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/13-install-node(1).png)
*`node-v20.11.0-x64.msi`{: .filepath } 실행 > `Next`*

![14-install-node(2)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/14-install-node(2).png)
*`I accept the terms in the License Agreement` 체크 > `Next`*

![15-install-node(3)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/15-install-node(3).png)
*`Next`*

![16-install-node(4)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/16-install-node(4).png)
*`Next`*

![17-install-node(5)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/17-install-node(5).png)
*`Automatically install the necessary tool. Note that this will also install Chocolatey. The script will pop-up in a new window after the installation completes.` 체크 > `Next`*

![18-install-node(6)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/18-install-node(6).png)
*`Install`*

![19-install-node(7)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/19-install-node(7).png)
*`Finish`*

![20-install-node(8)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/20-install-node(8).png)
*`Enter`*

![21-install-node(9)](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/21-install-node(9).png)
*`Enter`*

### Jekyll, bundler 설치

```console
$ gem install jekyll
$ gem install bundler
```

Jekyll과 bundler를 설치한다.

> 일반적으로 Ruby를 설치하게되면 Ruby의 패키지 관리자인 RubyGems도 함께 설치되는데, 이때 RubyGems의 `gem`명령어를 통해 설치되고 관리되는 Ruby의 패키지(Jekyll, bundler)를 gem이라고 부른다. bundler는 Ruby에서 사용되는 종속성 관리 도구다.
{: .prompt-info }

<br>

```console
$ ruby -v
$ node -v
$ jekyll -v
$ bundler -v
```

설치가 잘 되었는지 확인한다.

![22-check-version](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/22-check-version.png)

2024년 01월 19일을 기준으로 나의 환경은 위와 같다.

## jekyll-theme-chirpy Repository Fork

![23-fork-repo](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/23-fork-repo.png)
*<https://github.com/cotes2020/jekyll-theme-chirpy>{: target="_blank" } > `Fork` > `Create a new Fork`*

> GitHub Fork 방식을 사용하면 커밋을 해도 GtiHub 잔디가 안 심어진다. 잔디가 필요하다면 Repository를 직접 생성한 다음 [Chirpy 테마 repository](https://github.com/cotes2020/jekyll-theme-chirpy){: target="_blank" }에서 소스 코드를 zip 파일 형식으로 다운받아 따라하면 된다.
{: .prompt-tip }

![24-create-fork](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/24-create-fork.png)
*`username.github.io` 형식으로 `Repository name` 설정 > `Create fork`*

![25-change-repo-name](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/25-change-repo-name.png)
*Fork한 Repository > `Settings` > `General` > `Default branch`를 `master`에서 `main`으로 변경*

![26-copy-repo-url](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/26-copy-repo-url.png)
*Fork한 Repository > `<> Code` > `Copy url to clipboard`*

```console
$ cd clone할 디렉토리
$ git clone 복사한 url
```

Fork한 Repository의 소스코드들을 로컬에 복제한다.

## 로컬 서버 실행

![27-bundle-install](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/27-bundle-install.png)

```console
$ cd clone한 디렉토리
$ bundle install
```

**복제한 위치로 디렉토리를 변경한 다음**, `Gemfile`{: .filepath }에 적힌 종속성 패키지들을 설치한다.

![28-npm-install-&&-npm-run-build](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/28-npm-install-&&-npm-run-build.png)

```console
$ npm install && npm run build
```

[Node.js를 설치하기 앞서](#anchor1) `bash tool/init` 명령어를 실행해야 한다고 했다. 하지만 리눅스 환경에서 동작하는 `bash tool/init` 명령어를 Windows에서 사용할 수 없기 때문에, `tool`{: .filepath } 폴더 내의 `init`{: .filepath }을 열어 내용을 직접 처리해야 한다. 아래는 처리해야할 목록이다.

1. **`.github/workflows`{: .filepath } 디렉토리 내의 `pages-deploy.yml.hook`{: .filepath }을 `pages-deploy.yml`{: .filepath }로 이름을 변경하고, `pages-deploy.yml`{: .filepath }을 제외한 모든 파일을 삭제**
2. `_post`{: .filepath } 폴더 내의 모든 파일 삭제
3. **`npm install && npm run build` 명령어 실행**
4. **`.gitignore`{: .filepath }의 `assets/js/dist` 주석 처리 또는 제거**

2번은 선택 사항이다. `_post`{: .filepath } 폴더 내부에는 블로그를 처음 시작하는 사람들을 위한 가이드 형식의 글들이 존재한다. `_post`{: .filepath }는 앞으로 포스팅할 글들의 디렉토리다.

> `npm install && npm run build` 명령어를 실행하면 `package.json`{: .filepath }의 `scripts` 섹션에 정의된 build 스크립트인 `NODE_ENV=production npx rollup -c --bundleConfigAsCjs` 명령어가 실행되면서 6개의 자바스크립트 파일이 생성되는데, 이 파일들이 없으면 `assets/js/dist/*.min.js Not Found` 에러를 발생하면서 블로그 기능이 정상적으로 작동하지 않게 된다.
{: .prompt-info }

![29-bundle-exec-jekyll-serve](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/29-bundle-exec-jekyll-serve.png)

```console
$ bundle exec jekyll serve
```

위 명령어로 생성된 블로그를 로컬 서버에 올리고 `Server address`에 나와있는 `http://127.0.0.1:4000/` 주소를 통해 블로그에 접속할 수 있다.  이 서버에서는 `_config.yml`{: .filepath }을 제외한 변경 사항들이 반영되기 때문에, 배포하기 전에 이상이 없는지 먼저 확인하는 용도로 사용된다. cmd 창을 닫거나,  `Ctrl` + `c` 조합을 사용해서 나가면 서버가 종료된다.

> `_config.yml`{: .filepath }의 수정사항은 서버를 닫고 다시 열어야 반영된다.
{: .prompt-tip }

![30-access-server-address](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/30-access-server-address.png)

`http://127.0.0.1:4000/` 주소에 접속했을 때, 위와 같은 화면이 나와야 한다.

## 빌드 및 배포

```console
$ git add -A
$ git commit -m "커밋 메시지"
$ git push
```

지금껏 구축한 환경들을 Repository에 Push한다. 

![31-check-workflows](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/31-check-workflows.png)
*Fork한 Repository > `All workflows` > `Actions`*

위 사진과 같은 초록색 체크 표시는 빌드, 배포에 성공했다는 뜻이다.

![32-check-page-deployment](/assets/img/posts/tools/jekyll/chirpy/make-github-blog-with-chirpy-theme-on-windows/32-check-page-deployment.png)

`username.github.io` 주소에 접속하여 실제로 페이지가 잘 배포됐는지 확인한다.

## 참고자료

- ["DOCS", Jekyll, Date unknown](https://jekyllrb.com/docs/){: target="_blank" }
- ["Tutorials", Jekyll, Date unknown](https://jekyllrb-ko.github.io/tutorials/home/){: target="_blank" }
- [Cotes Chung, "Getting Started", Chirpy, 2024-01-23](https://chirpy.cotes.page/posts/getting-started/){: target="_blank" }
- [하얀눈길, "Jekyll Chirpy 테마 사용하여 블로그 만들기", 하얀눈길 블로그, 2021-12-28](https://www.irgroup.org/posts/jekyll-chirpy/){: target="_blank" }
- [의사줌치, "gem 기반으로 jekyll 블로그 만들기(chirpy 테마, windows)", 의사줌치, 2021-07-23](https://a3magic3pocket.github.io/posts/jekyll-theme-chirpy-with-gem/){: target="_blank" }