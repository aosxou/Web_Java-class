# 웹 자바 프로그래밍 7장 – JSP 파일 업로드

---

## 1. 파일 업로드 폼

### [예제 7-1] HTML 폼

```html
<form action="7-3.jsp" method="post" enctype="multipart/form-data">
업로드할 파일을 선택하세요.<br>
<input type="file" name="upload"><br>
<input type="submit" value="업로드">
</form>
```

- `method="post"` : GET 방식 사용 시 파일 내용이 아니라 **파일명만 전송**  
- `enctype="multipart/form-data"` : 파일 전송 시 반드시 지정  
- `<input type="file">` : 사용자가 업로드할 파일 선택

---

## 2. 업로드 파일 처리

### [예제 7-2] JSP에서 파일 업로드

```jsp
<%@ page import="java.io.*" %>

<%
Part part = request.getPart("upload"); // 업로드된 파일 객체

if (part.getSize() > 0) {
    String path = application.getRealPath("/files/"); // 서버 저장 경로
    String fileName = part.getSubmittedFileName();   // 업로드 파일명
    part.write(path + fileName);                     // 파일 저장

    out.print("업로드된 파일명 : " + fileName + "<br>");
    out.print("저장된 경로 : " + path + "<br>");
    out.print("파일 크기 : " + part.getSize() + "<br>");
} else {
    out.print("업로드할 파일이 지정되지 않았습니다.");
}
%>
```

---

## 3. 같은 파일명 처리

- 동일 이름 파일 업로드 시 덮어쓰지 않고 `(1), (2)` 붙여 저장
- JSP에서 메소드 정의 시 `<%! %>` 사용

### 파일명 중복 처리 메소드

```jsp
<%!
String getFileNameToSave(String path, String fileName) {
    int extIndex = fileName.lastIndexOf(".");
    String base, ext;

    if (extIndex > 0) {
        base = fileName.substring(0, extIndex);
        ext = fileName.substring(extIndex);
    } else {
        base = fileName;
        ext = "";
    }

    for (int i = 1; new File(path + fileName).exists(); i++) {
        fileName = base + " (" + i + ")" + ext;
    }

    return fileName;
}
%>
```

### 업로드 처리 코드 예제 (중복 파일명 고려)

```jsp
Part part = request.getPart("upload");

if (part.getSize() > 0) {
    String path = application.getRealPath("/files/");
    String fileName = part.getSubmittedFileName();
    String fileNameToSave = getFileNameToSave(path, fileName); // 중복 처리
    part.write(path + fileNameToSave);

    out.print("업로드된 파일명 : " + fileName + "<br>");
    out.print("저장된 파일명 : " + fileNameToSave + "<br>");
    out.print("저장된 경로 : " + path + "<br>");
    out.print("파일 크기 : " + part.getSize() + "<br>");
} else {
    out.print("업로드할 파일이 지정되지 않았습니다.");
}
```

---

## 핵심 요약

1. **폼 작성 시 필수 속성**
   - `method="post"`  
   - `enctype="multipart/form-data"`
2. **업로드 처리**
   - `request.getPart("upload")` → 업로드 파일 객체
   - `part.getSubmittedFileName()` → 원래 파일명
   - `part.write(파일경로)` → 서버에 저장
3. **중복 파일 처리**
   - `<%! %>` 안에서 메소드 정의
   - 기존 파일명 확인 후 `(1), (2)` 등의 이름으로 저장
4. JSP에서 파일 업로드 시 반드시 서버 경로(`application.getRealPath`)와 파일명 관리 필요
