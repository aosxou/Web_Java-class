# 웹 자바 프로그래밍 5장 정리 – MariaDB 개요

---

## 1. 관계형 데이터베이스(RDB)

- 데이터를 **테이블** 형태로 저장하는 데이터베이스  
- **테이블 구조**  
  - **행(Row, 레코드)**: 테이블의 한 줄  
  - **열(Column, 필드)**: 테이블의 세로 열  

| 개념 | 설명 |
| --- | --- |
| 레코드 | 테이블의 한 행(row) |
| 필드 | 테이블의 열(column) |

---

## 2. 데이터베이스(DB)

- 관련 있는 **테이블들의 집합**  
- 하나의 컴퓨터에 여러 개의 DB 존재 가능

---

## 3. DBMS (DataBase Management System)

- DB를 관리해주는 소프트웨어  
- 사용자는 DBMS에게 요청 → DBMS가 실제 동작 수행  
- 예시: MariaDB(MySQL 호환, 무료), MS-Access, Oracle, MS-SQL Server  

---

## 4. MariaDB 구조 및 동작

- **서버**: DBMS (데이터 저장, 쿼리 처리)  
- **클라이언트**: 사용자 명령 전달, 결과 확인  

### MySQL 클라이언트
- 명령행(CLI) 방식: 쿼리 입력/수정 불편  
- GUI 방식: HeidiSQL, 대부분 메뉴 선택 가능  
- **웹 프로그램**에서는 항상 쿼리로 데이터 조작 필요  

---

## 5. 계정 관리

- **root 계정**: 관리자 계정, 권한 막강 → 평상시에는 일반 계정 사용 권장  
- **사용자 계정 생성 예시**:

```sql
create database DB명;

create user 'ID'@'localhost' identified by '비밀번호';

grant all privileges on DB명.* to 'ID'@'localhost' with grant option;

flush privileges;
```

---

## 6. 주요 데이터베이스 명령어

| 명령어 | 설명 |
| --- | --- |
| `create database DB명;` | 데이터베이스 생성 |
| `show databases;` | 데이터베이스 목록 보기 |
| `drop database DB명;` | 데이터베이스 삭제 |

---

## 7. 테이블 관련 명령어

| 명령어 | 설명 |
| --- | --- |
| `create table 테이블명 (필드명 자료형 [옵션], …);` | 테이블 생성 |
| `show tables;` | 현재 DB의 테이블 목록 |
| `desc 테이블명;` | 테이블 구조 확인 |
| `drop table 테이블명;` | 테이블 삭제 |
| `alter table …` | 필드 삭제/수정, 테이블명 수정 |

---

## 8. 데이터 조작 (CRUD)

### 1) 삽입 (Insert)

```sql
insert into 테이블명 (필드1, 필드2, …) values (값1, 값2, …);
-- 모든 필드에 값을 넣는 경우
insert into score values (2, '이순신', 65, 75, 85);
```

### 2) 조회 (Select)

```sql
select 필드1, 필드2 from 테이블명 [where 조건];

-- 모든 필드 조회
select * from score where num = 1;

-- 테이블 전체 조회
select * from score;
```

### 3) 수정 (Update)

```sql
update 테이블명 set 필드1 = 값1, 필드2 = 값2 [where 조건];
-- where 생략하면 모든 레코드 수정
```

### 4) 삭제 (Delete)

```sql
delete from 테이블명 [where 조건];
-- where 생략하면 모든 레코드 삭제
```

---

## 9. 배치(Batch) 파일 활용

- 여러 명령어를 반복 실행해야 할 때, 미리 **배치 파일**에 작성  
- HeidiSQL에서 배치 파일 실행 → 쿼리 창에 입력한 것처럼 처리

---

## 핵심 요약

1. **RDB**: 데이터는 테이블 형태, 행=row, 열=column  
2. **DBMS**: 데이터베이스 관리 소프트웨어  
3. **MariaDB**: MySQL 호환 무료 DB, 서버+클라이언트 구조  
4. **사용자 계정 생성**: root 외 일반 계정으로 안전하게 사용  
5. **CRUD 명령어 숙지**: insert, select, update, delete  
6. **배치 파일**: 반복 명령어 실행 시 활용

