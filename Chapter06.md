# 웹 자바 프로그래밍 6장 – JSP와 MariaDB 연동

---

## 1. 데이터베이스 서버 접속

### [예제 6-1] try-with-resources 사용

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>

<%
Class.forName("org.mariadb.jdbc.Driver"); // MariaDB 커넥터 로드
try (
    Connection conn = DriverManager.getConnection(
        "jdbc:mariadb://localhost:3308/jspdb", "jsp", "1234")
) {
    out.println("DB 접속 성공 !");
} catch (Exception e) {
    e.printStackTrace();
}
%>
```

- `try-with-resources` 사용 → try 종료 시 자동으로 `close()` 호출

---

### [예제 6-2] try-with-resources 미사용

```jsp
Connection conn = null;
Class.forName("org.mariadb.jdbc.Driver");
try {
    conn = DriverManager.getConnection("jdbc:mariadb://localhost:3308/jspdb", "jsp", "1234");
    out.println("DB 접속 성공 !");
} catch(Exception e) {
    e.printStackTrace();
} finally {
    if (conn != null) conn.close();
}
```

---

## 2. 테이블 생성

### [예제 6-3] 성적 테이블 생성

```jsp
String sql =
"create table score (" +
"    num  int primary key," +
"    name varchar(20)," +
"    kor  int," +
"    eng  int," +
"    math int" +
")";
stmt.executeUpdate(sql);
out.println("성적 테이블 생성 성공 !");
```

---

## 3. 레코드 추가

### [예제 6-4] 샘플 데이터 삽입

```jsp
String[][] score = {
    { "1", "홍길동", "50", "60", "70" },
    { "2", "이순신", "65", "75", "85" },
    { "3", "강감찬", "60", "80", "70" }
};

for (int i = 0; i < score.length; i++) {
    String sql = String.format(
        "insert into score values (%s, '%s', %s, %s, %s)",
        score[i][0], score[i][1], score[i][2], score[i][3], score[i][4]
    );
    stmt.executeUpdate(sql);
    out.println("쿼리 실행 성공 : " + sql + "<br>");
}
```

---

## 4. 데이터 조회

```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("select * from score");

while (rs.next()) {
    int num = rs.getInt("num");
    String name = rs.getString("name");
    int kor = rs.getInt("kor");
    int eng = rs.getInt("eng");
    int math = rs.getInt("math");
}
```

- `executeQuery()` → SELECT 쿼리 실행  
- 반환값은 **ResultSet 객체**  
- `rs.next()` → 레코드 포인터 이동, 레코드 존재 시 `true`

---

### [예제 6-5] 데이터 출력

```jsp
<table>
<tr><th>번호</th><th>이름</th><th>국어</th><th>영어</th><th>수학</th><th>총점</th><th>평균</th></tr>

<%
while (rs.next()) {
    int sum = rs.getInt("kor") + rs.getInt("eng") + rs.getInt("math");
%>
<tr>
<td><%= rs.getInt("num") %></td>
<td><%= rs.getString("name") %></td>
<td><%= rs.getInt("kor") %></td>
<td><%= rs.getInt("eng") %></td>
<td><%= rs.getInt("math") %></td>
<td><%= sum %></td>
<td><%= String.format("%.2f", (float)sum / 3) %></td>
</tr>
<% } %>
</table>
```

---

## 5. 쿼리 종류별 접근 코드 요약

| 쿼리 종류 | 메서드 | 반환값 |
| --- | --- | --- |
| SELECT | `executeQuery()` | `ResultSet` |
| INSERT/UPDATE/DELETE/CREATE | `executeUpdate()` | 영향을 받은 레코드 수(int) |

---

## 6. 연습문제 예제

### 6-2) 주소록 테이블 생성

```jsp
String sql =
"create table addrbook (" +
"    num  int primary key," +
"    name varchar(20)," +
"    addr varchar(20)," +
"    tel  varchar(20)" +
")";
stmt.executeUpdate(sql);
out.println("주소록 테이블 생성 성공 !");
```

### 6-3) 주소록 데이터 추가

```jsp
stmt.executeUpdate("insert into addrbook values (1, '홍길동', '서울','123-4567')");
stmt.executeUpdate("insert into addrbook values (2, '신혜지', '서울','133-4567')");
stmt.executeUpdate("insert into addrbook values (3, '황인준', '서울','113-4567')");
```

### 6-4) 주소록 조회

```jsp
ResultSet rs = stmt.executeQuery("select * from addrbook");

while (rs.next()) {
%>
<tr>
<td><%= rs.getInt("num") %></td>
<td><%= rs.getString("name") %></td>
<td><%= rs.getString("addr") %></td>
<td><%= rs.getString("tel") %></td>
</tr>
<% } %>
```

---

## 핵심 요약

1. JSP에서 MariaDB 연동 → **DriverManager, Connection, Statement, ResultSet** 사용  
2. try-with-resources → 자원 자동 해제  
3. SELECT 쿼리 → `executeQuery()`, 나머지 쿼리 → `executeUpdate()`  
4. `ResultSet` → 레코드 반복 처리 (`rs.next()`)  
5. CRUD 예제 → 테이블 생성, 데이터 추가, 조회, 출력
