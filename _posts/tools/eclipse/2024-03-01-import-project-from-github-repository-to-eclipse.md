---
title: "[Eclipse]GitHub Repository에서 이클립스로 프로젝트 가져오기"
categories: [tools, Eclipse]
tags: [Eclipse, 이클립스, import, GitHub]
---

# GitHub Repository에서 이클립스로 프로젝트 가져오기

![01-copy-url-to-clipboard](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/01-copy-url-to-clipboard.jpg)
*Repositry > `Code` > `Copy url to clipboard`*

![02-import(1)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/02-import(1).jpg)
*`File` > `Import..`*

![03-import(2)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/03-import(2).jpg)
*`Git/Projects from Git` 선택 > `Next >`*

![04-import(3)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/04-import(3).jpg)
*`Clone URI` 선택 > `Next >`*

![05-import(4)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/05-import(4).jpg)
*복사한 URI를 URI 칸에 붙여넣기 > `Next >`*

- URI
	+ URI 주소
- Host
	+ Git 서버의 호스트 주소
	+ GitHub의 경우 URI 칸에 주소를 입력하면 자동으로 값이 입력
- Repository path
	+ 원격 저장소의 경로를 입력
	+ GitHub의 경우 URI 칸에 주소를 입력하면 자동으로 값이 입력
- Protocol
	+ 원격 저장소를 가져오는 데 필요한 네트워크 프로토콜 선택
- Port
	+ 특정 포트에 접속해서만 원격 저장소를 가져올 수 있는 경우 해당 포트 번호를 입력
		* 아닌 경우 공백
- User
	+ Git 계정 ID
- Password
	+ Git 계정 비밀번호
- Store in Secure Store
	+ 로컬 보안 저장소에 원격 저장소를 저장해야 할 경우 체크

![06-import(5)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/06-import(5).jpg)
*가져올 브랜치 선택 > `Next >`*

![07-import(6)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/07-import(6).jpg)
*`Next >`*

- Directory
	+ 원격 저장소를 저장할 디렉토리 경로
- Initial branch
	+ 초기 브랜치 설정
- Clone submodules
	+ 현재 원격 저장소를 메인 프로젝트의 하위 프로젝트인 서브 모듈로 클론하려면 체크 표시
	+ 해당 원격 저장소가 프로젝트에 사용되는 라이브러리일 경우 유용
- Remote name
	+ 원격 저장소의 이름
	+ default origin

![08-import(7)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/08-import(7).jpg)
*`Import 방법 선택 > `Next >`*

- Import existing Eclipse projects
	+ 현재 존재하는 이클립스 프로젝트에 원격 저장소를 불러옴
- Import using the New Project wizard
	+ 새로운 프로젝트 마법사를 이용해 프로젝트를 만들고 원격 저장소를 불러옴
- Import as general project
	+ 이클립스에 생성된 프로젝트가 아닌 프로젝트를 불러옴

![09-import(8)](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/09-import(8).jpg)
*`Finish`*

![10-import-complete](/assets/img/posts/tools/eclipse/import-project-from-github-repository-to-eclipse/10-import-complete.jpg)
*Import 성공*

## 참고 자료

- [김현호, "[Git] 원격 저장소의 내용을 로컬 저장소로 가져오기", 혀노블로그, 2015-12-21](https://blog.naver.com/kimnx9006/220574706346){: target="_blank" }