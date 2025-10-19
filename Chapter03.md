# 웹 자바 프로그래밍 3장 정리

---

## JSP 프로그램 기본 틀

- JSP 프로그램의 기본 틀은 **이클립스가 자동 생성**
- 개발자는 필요한 부분만 **추가 입력**하면 됨

---

## [예제 2-5] 표현식 사용 (`2-5.jsp`)

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

오늘 날짜 : <%= LocalDate.now() %><br>
현재 시간 : <%= LocalTime.now() %>

</body>
</html>
```

---

## 설명

- `<%= … %>` : **표현식(Expression)**  
  - 값 하나만 화면에 출력할 때 사용
  - 스크립틀릿 `<% … %>` 과 달리 **세미콜론(;) 필요 없음**
- 위 예제에서는 **오늘 날짜와 현재 시간**을 브라우저에 출력

---

## 요약

1. JSP 기본 틀은 **이클립스가 자동 생성**
2. **표현식 `<%= … %>`** 는 값 출력 전용
3. **스크립틀릿 `<% … %>`** 는 Java 코드 블록 작성 가능
4. 브라우저에서 결과 확인 가능 (톰캣 필요)

---
