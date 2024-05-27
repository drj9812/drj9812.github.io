---
title: "[VS Code]단축키 및 설정과 플러그인"
categories: [Tools, VS Code]
tags: [설치, VS Code, Visual Studio Code, 설정, 명령어]
image:
  path: /assets/img/posts/tools/vs-code/01-vs-code-logo.jpg
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: VS Code
---

# 단축키 및 설정과 플러그인

## 단축키

- 터미널 열기
	+ <kbd>Ctrl</kbd> + <kbd>`</kbd>
- 새 터미널 창
	+ <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>`</kbd>
- 커맨드 팔레트(Command Palette)
	+ <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>
- 익스텐션(Extensions) 창
	+ <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>
- 동일한 문자열 바꾸기
	+ <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>L</kbd>
	+ <kbd>Ctrl</kbd> + <kbd>F2</kbd>
- 선택한 코드 자동 들여쓰기
	+ <kbd>Ctrl</kbd> + <kbd>K</kbd> 이후 <kbd>Ctrl</kbd> + <kbd>F</kbd>
- 전체 코드 자동 들여쓰기
	+ <kbd>Shift</kbd> + <kbd>ALT</kbd> + <kbd>F</kbd>
- 설정 창
	+ <kbd>Ctrl</kbd> + <kbd>,</kbd>

## 설정

### 한국어 설정

![set-in-korean(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/set-in-korean(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'korean' 검색 > Korean Lanuage Pack for Visual Studio Code `Install`*

![set-in-korean(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/set-in-korean(2).jpg)
*`Change Language and Restart`*

![set-in-korean(3)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/set-in-korean(3).jpg)
*적용*

### 기본 터미널 변경

![change-default-terminal(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/change-default-terminal(1).jpg)
*커맨드 팔레트(<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>) > 'select default profile' 검색 > `터미널: 기본 프로필 선택`*

![change-default-terminal(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/change-default-terminal(2).jpg)
*기본으로 설정할 터미널 선택*

![change-default-terminal(3)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/change-default-terminal(3).jpg)
*적용*

### 괄호 색 구별

![bracket-pair-colorization(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/bracket-pair-colorization(1).jpg)
*설정 창(<kbd>Ctrl</kbd> + <kbd>,</kbd>) > `bracket pair colorization` 검색 > `Editor > Bracket Pair Colorization: Enabled` 체크*

![bracket-pair-colorization(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/bracket-pair-colorization(2).jpg)
*적용*

### 마크다운 미리보기

![markdown-open-preview(1).jpg](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/markdown-open-preview(1).jpg)
*마크다운 파일 > 커맨드 팔레트(<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>) > 'markdown open preview' 검색 > `Markdown: 미리 보기 열기`*

![markdown-open-preview(2).jpg](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/markdown-open-preview(2).jpg)
*적용*

### 코드 조각

![code-snippets(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(1).jpg)
*커맨드 팔레트(<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>) > 'configure user snippets' 검색 > `코드 조각: 사용자 코드 조각 구성`*

![code-snippets(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(2).jpg)
*코드 조각이 적용될 범위 선택*

![code-snippets(3)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(3).jpg)
*코드 조각 파일 이름 설정*

![code-snippets(4)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(4).jpg)
*코드 조각 설명*

- `scope`: 코드 조각이 사용되는 파일
- `prefix`: 코드 조각 사용 단축키
- `body`: 코드 조각 내용
- `description`: 설명

![code-snippets(5)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(5).jpg)
*예시*

![code-snippets(6)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(6).jpg)
*prefix + <kbd>Ctrl</kbd> + <kbd>Space</kbd> > `생성한 코드 조각`*

![code-snippets(7)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-snippets(7).jpg)
*예시 적용*

## 플러그인

### Code Runner

![code-runner(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-runner(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'code runner' 검색 > Code Runner `설치`*

![code-runner(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/code-runner(2).jpg)
*<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>N</kbd>)*

### Live Server

![live-server(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/live-server(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'live server' 검색 > Live Sever `설치`*

![live-server(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/live-server(2).jpg)
*커맨드 팔레트(<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>) > 'open with live server' 검색 > `Live Server: Open with Live Server`*

- Live Server에서는 페이지를 새로고침하지 않아도 파일의 변경사항이 자동으로 반영됨

### indent-rainbow

![indent-rainbow(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/indent-rainbow(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'indent-rainbow' 검색 > indent-rainbow `설치`*

![indent-rainbow(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/indent-rainbow(2).jpg
)
*적용*

### Material Icon Theme

![material-icon-theme(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/material-icon-theme(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'material icon theme' 검색 > Material Icon Theme `설치`*

![material-icon-theme(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/material-icon-theme(2).jpg)
*적용*

### Auto Rename Tag

![auto-rename-tag(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/auto-rename-tag(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'auto rename tag' 검색 > Auto Rename Tag `설치`*

![auto-rename-tag(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/auto-rename-tag(2).jpg)
*적용*

- 시작 태그 변경 시 종료 태그도 함께 변경

### CSS Peak

![css-peak(1)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/css-peak(1).jpg)
*익스텐션 창(왼쪽의 아이콘 클릭 또는 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>X</kbd>) > 'css peak' 검색 > CSS Peak `설치`*

![css-peak(2)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/css-peak(2).jpg)
*<kbd>Ctrl</kbd> + CSS 클릭*

![css-peak(3)](/assets/img/posts/tools/vs-code/shortcuts-and-settings-and-plug-ins/css-peak(3).jpg)
*적용*

- 해당 CSS 정의로 이동