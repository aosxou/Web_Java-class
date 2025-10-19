# 웹 자바 프로그래밍 2장 정리

---

## JSP 프로그램 파일의 확장자

- JSP 파일의 확장자는 **`.jsp`**
- 파일 내용은 기본적으로 **HTML** 형태
- 필요한 부분에 **JSP 코드 삽입**
- JSP 코드 시작: `<%`, 끝: `%>`

---

## [예제 2-1] JSP 프로그램의 기본 형태 (`2-1.jsp`)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
           pageEncoding="UTF-8"%>
<%@ page import="java.time.*" %>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>테스트 페이지</title>
</head>
<body>
<%
    out.println("오늘 날짜 : " + LocalDate.now() + "<br>");
    out.println("현재 시간 : " + LocalTime.now());
%>
</body>
</html>
```

**실행 결과 예시:**

```
오늘 날짜 : 2025-10-16
현재 시간 : 20:05:19.1741112000
```

---

## Page 지시자 (Directive)

- `<%@` 로 시작하는 코드를 **지시자(Directive)** 라고 함
- 톰캣(서블릿 컨테이너)에게 JSP 페이지 정보 전달
- **page 지시자** 예:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
           pageEncoding="UTF-8"%>
<%@ page import="java.time.*" %>
```

| 속성 | 의미 |
|------|------|
| `language="java"` | 프로그래밍 언어로 **Java** 사용 |
| `contentType="text/html; charset=UTF-8"` | 실행 결과 문서는 UTF-8 인코딩 HTML |
| `pageEncoding="UTF-8"` | JSP 파일 자체 인코딩 UTF-8 |
| `import="java.time.*"` | 일반 Java 코드 import와 동일 |

---

## 스크립틀릿 (Scriptlet)

- `<% … %>` 사이에 **Java 코드 작성**
- 웹 브라우저에 출력: `out.print()` / `out.println()` 사용
- 여러 개 스크립틀릿 사용 가능

```jsp
<%
    out.println("오늘 날짜 : " + LocalDate.now() + "<br>");
    out.println("현재 시간 : " + LocalTime.now());
%>
```

---

## [예제 2-2] 2개의 스크립틀릿 사용 (`2-2.jsp`)

```jsp
오늘 날짜 :
<%
    out.println(LocalDate.now());
%>
<br>
현재 시간 :
<%
    out.println(LocalTime.now());
%>
```

---

## JSP 프로그램 저장 위치 및 실행

- JSP 파일은 **웹 브라우저에서 직접 실행 불가**
  - 톰캣이 **해석 후 실행**해야 결과 확인 가능
- **톰캣 Application Base 폴더** 아래에 저장해야 함
  - 예: `C:\jsp\apache-tomcat-10.1.28\webapps\`  
  - 폴더에 바로 넣지 말고, **웹 애플리케이션 폴더** 생성 후 저장

---

## 주석 (Comment)

- **스크립틀릿(Java 영역)**: `/*…*/` / `//…` 사용
- **HTML 영역**: `<!-- … -->` 사용
- **JSP 전용 주석**: `<%-- … --%>` (브라우저 소스 보기에서 숨김)

```jsp
<%
    /**
     여러 줄 주석
    */
    out.println("오늘 날짜 : " + LocalDate.now());
    // 한 줄 주석
%>

<!-- HTML 주석 -->
<%-- JSP 전용 주석 --%>
```

---

## 화면 출력

- **웹 브라우저 출력**: `out.print()` / `out.println()`
- 주의사항:
  - 연속된 공백/개행은 **한 개 공백**으로 출력
  - 줄 시작 공백 무시
- `<br>` 태그 사용으로 줄 바꿈 가능
- 의도적인 공백: `&nbsp;`

```jsp
<%
    out.println("줄을 넘기려면 br 태그 사용<br>");
    out.println("연속된 공백은             공백 한 개<br>");
    out.println("&nbsp;&nbsp;&nbsp;&nbsp;의도적인 공백");
%>
```

**웹 브라우저 화면:**

```
줄을 넘기려면 br 태그 사용
연속된 공백은 공백 한 개
    의도적인 공백
```

---

## 표현식 (Expression)

- `<%= … %>`: **값 하나만 출력** 시 사용
- **세미콜론(;) 필요 없음**

```jsp
오늘 날짜 : <%= LocalDate.now() %><br>
현재 시간 : <%= LocalTime.now() %>
```

---

## 요약

1. JSP 파일은 HTML + Java 코드를 혼합하여 작성
2. **Page 지시자**, **스크립틀릿**, **표현식** 사용 방법 숙지
3. **톰캣 환경**에서만 실행 가능
4. 웹 브라우저에서 출력 시 공백/개행 처리 주의
5. 주석 종류에 따라 소스 보기 여부 달라짐

---
