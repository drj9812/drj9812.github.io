---
title: "[Design Pattern | MVC]왜 VIEW는 WEB-INF 폴더에 위치해야 하는가"
categories: [Software Architecture, Design Patterns]
tags: [Software Architecture, Design Pattern, 디자인 패턴, MVC]
---

# 왜 VIEW는 WEB-INF 폴더에 위치해야 하는가

## 보안 강화

`/WEB-INF/`{: .filepath } 내의 JSP 파일은 URL로 직접 접근할 수 없다. 예를 들어, `http://example.com/WEB-INF/view.jsp`와 같은 요청은 서버에 의해 차단된다. 이렇게 함으로써 JSP 파일이 외부에 노출되지 않도록 하여 보안을 강화할 수 있다.

## MVC 패턴 준수

MVC 패턴(Model 2)에서는 Servlet이 Controller 역할을 하고, JSP는 View 역할을 한다. Controller가 요청을 처리하고, 필요한 데이터를 준비한 후 JSP로 포워딩한다. JSP는 단순히 데이터를 출력하는 역할만 한다. 이 패턴을 유지하려면 **JSP 파일이 Servlet을 통해서만 접근**될 수 있어야 한다. 이렇게 함으로써 모든 요청이 **컨트롤러를 통해 처리**되도록 강제할 수 있다.

## 비즈니스 로직과의 분리

JSP 파일이 외부에 노출되면 비즈니스 로직이 혼재될 수 있는 위험이 있다. JSP 파일이 `/WEB-INF/`{: .filepath }에 위치하면 **오직 Controller에서만 접근**할 수 있으므로 비즈니스 로직이 JSP 파일 내에 포함되는 것을 방지할 수 있다.

## 의도된 경로로의 접근 강제

JSP 파일이 `/WEB-INF/`{: .filepath }에 위치하면 클라이언트는 반드시 Servlet을 통해 요청해야 한다. 이는 애플리케이션의 흐름을 예측 가능하게 만들고, 요청의 처리 방식을 일관되게 유지하는 데 도움이 된다.