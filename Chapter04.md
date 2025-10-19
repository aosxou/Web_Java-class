# 웹 자바 프로그래밍 4장 정리

---

## 1. 입력 폼 작성과 값 처리

### 입력 폼 기본 구조

```html
<form action="처리할 JSP 파일" method="get|post">
    입력 태그 …
</form>
```

- **action**: 입력 값을 전달할 JSP 프로그램  
- **method**: 데이터 전달 방식 (`get` / `post`)  
    - GET: 주소창에 값이 노출  
    - POST: 주소창에 값이 노출되지 않음

---

### 주요 입력 태그

| 입력 태그 | 설명 |
| --- | --- |
| `<input>` | 텍스트, 비밀번호, 라디오, 체크박스, submit, reset, 파일업로드 등 |
| `<select>` | 드롭다운 리스트, 일반 리스트 |
| `<textarea>` | 여러 줄 텍스트 입력 |

- 예: `<input type="text" name="abc">` → 한 줄 텍스트 입력란, 이름 "abc"
- 예: `<input type="submit" value="확인">` → 전송 버튼

---

## 2. GET 방식 입력 폼 예제 (4-1.html)

```html
<form action="4-2.jsp" method="get">
    국어: <input type="text" name="kor"><br>
    영어: <input type="text" name="eng"><br>
    수학: <input type="text" name="math"><br>
    <input type="submit" value="확인">
</form>
```

- 전달된 값 읽기: `request.getParameter("입력 태그 name")`

### GET 방식 값 출력 (4-2.jsp)

```jsp
국어: <%=request.getParameter("kor")%><br>
영어: <%=request.getParameter("eng")%><br>
수학: <%=request.getParameter("math")%><br>
```

---

## 3. POST 방식 입력 폼 예제 (4-3.html)

```html
<form action="4-2.jsp" method="post">
    국어: <input type="text" name="kor"><br>
    영어: <input type="text" name="eng"><br>
    수학: <input type="text" name="math"><br>
    <input type="submit" value="확인">
</form>
```

- 값 사용 방법은 GET 방식과 동일  
- POST 방식은 주소창에 값이 보이지 않음

---

## 4. 단일 값 입력 + 라디오, 드롭다운, textarea (4-4.html)

```html
<form action="4-5.jsp" method="post">
    <table>
        <tr>
            <td>아이디</td>
            <td><input type="text" name="id"></td>
        </tr>
        <tr>
            <td>비밀번호</td>
            <td><input type="password" name="pw"></td>
        </tr>
        <tr>
            <td>성별</td>
            <td>
                <input type="radio" name="gender" value="남" checked>남
                <input type="radio" name="gender" value="여">여
            </td>
        </tr>
        <tr>
            <td>가입경로</td>
            <td>
                <select name="intro">
                    <option value="웹검색" selected>웹검색</option>
                    <option value="지인소개">지인소개</option>
                    <option value="기타">기타</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>주소지</td>
            <td>
                <select name="addr" size="4">
                    <option value="서울" selected>서울</option>
                    <option value="경기">경기</option>
                    <option value="인천">인천</option>
                    <option value="기타">기타</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>메모</td>
            <td><textarea name="memo" rows="4"></textarea></td>
        </tr>
    </table>
    <input type="submit" value="가입">
</form>
```

### 출력 JSP (4-5.jsp)

```jsp
<%
request.setCharacterEncoding("utf-8"); // 한글 깨짐 방지
%>
아이디: <%=request.getParameter("id")%><br>
비밀번호: <%=request.getParameter("pw")%><br>
성별: <%=request.getParameter("gender")%><br>
가입경로: <%=request.getParameter("intro")%><br>
주소: <%=request.getParameter("addr")%><br>
메모: <%=request.getParameter("memo")%><br>
```

---

## 5. 다중 선택 입력 (체크박스, 다중 선택 리스트)

### 입력 폼 예제 (4-6.html)

```html
관심언어: 
<input type="checkbox" name="lang" value="PHP">PHP
<input type="checkbox" name="lang" value="JSP">JSP
<input type="checkbox" name="lang" value="ASP.NET">ASP.NET

취미:
<select name="hobby" size="4" multiple>
    <option value="영화">영화</option>
    <option value="운동">운동</option>
    <option value="독서">독서</option>
    <option value="기타">기타</option>
</select>
```

### 출력 JSP (4-7.jsp)

```jsp
<%
request.setCharacterEncoding("utf-8");
String[] lang  = request.getParameterValues("lang");
String[] hobby = request.getParameterValues("hobby");
%>

관심언어:
<% for(String s : lang) { out.println(s + " "); } %>
<br>

취미:
<% for(String s : hobby) { out.println(s + " "); } %>
<br>
```

- **getParameterValues()**: 다중 입력 값 처리 (배열 반환)  
- **for-each** 문으로 간단히 출력 가능

---

## 6. 연습문제 예제

### 6-1. 학생 점수 총점 및 평균 계산

**폼 (ex4-1a.jsp)**: 4-1.html과 동일, POST 방식

**출력 (ex4-1b.jsp)**

```jsp
<%
int kor  = Integer.parseInt(request.getParameter("kor"));
int eng  = Integer.parseInt(request.getParameter("eng"));
int math = Integer.parseInt(request.getParameter("math"));
int sum  = kor + eng + math;
double avg = sum / 3.0;
%>

국어: <%=kor%><br>
영어: <%=eng%><br>
수학: <%=math%><br>
총점: <%=sum%><br>
평균: <%=String.format("%.2f", avg)%>
```

---

### 6-2. 원의 둘레와 면적 계산 (GET 방식)

**폼 (ex4-2a.jsp)**

```html
<form action="ex4-2b.jsp" method="get">
    반지름: <input type="text" name="ban">
    <input type="submit" value="확인">
</form>
```

**출력 (ex4-2b.jsp)**

```jsp
<%
Double ban = Double.parseDouble(request.getParameter("ban"));
%>

원의 반지름: <%=ban%><br>
원의 둘레: <%=2 * Math.PI * ban %><br>
원의 면적: <%=Math.PI * ban * ban %>
```

---

### 6-3. 단일 + 다중 선택 입력을 활용한 회원가입

**폼 (ex4-3a.jsp)**: 단일 입력 + 체크박스 + 다중 선택 리스트 포함

**출력 (ex4-3b.jsp)**

```jsp
아이디: <%=request.getParameter("id")%><br>
비밀번호: <%=request.getParameter("pw")%><br>
성별: <%=request.getParameter("gender")%><br>
가입경로: <%=request.getParameter("intro")%><br>
주소: <%=request.getParameter("addr")%><br>
메모: <%=request.getParameter("memo")%><br>

<%
request.setCharacterEncoding("utf-8");
String[] lang  = request.getParameterValues("lang");
String[] hobby = request.getParameterValues("hobby");
%>

관심언어:
<% for(String s : lang) { out.println(s + " "); } %>
<br>

취미:
<% for(String s : hobby) { out.println(s + " "); } %>
<br>
```

---

## 핵심 요약

1. **폼 작성**: `<form>` + action + method  
2. **값 전달 방식**: GET(주소창 노출) / POST(비노출)  
3. **단일 값 처리**: `request.getParameter("name")`  
4. **다중 값 처리**: `request.getParameterValues("name")` → 배열 반환  
5. **한글 처리**: `request.setCharacterEncoding("utf-8")`  
6. **숫자 계산 시**: String → int/Double 변환 필요  
7. **평균/계산 결과 출력**: `String.format("%.2f", value)`로 소수점 처리  

