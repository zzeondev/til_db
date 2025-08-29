# Maria DB

- 관계형 DB 관리 : `SQL 문법` 사용
- MySql(99.99%), Oracle, PostgreSQL(supabase) 등
- 참고로 비관계형 DB 관리 : `NoSql`
  - Firebase, MongoDB, Redis 등

## 1. 설치

- https://tlseoqja.tistory.com/85
- https://mariadb.org/download/?t=mariadb&p=mariadb&r=11.6.2&os=windows&cpu=x86_64&pkg=msi&mirror=blendbyte

## 2. 설치 이후 확인

- `Power Shell` 에서 진행

```bash
mysql --version
```

## 3. Maria DB 서버 접속하기

- `Power Shell` 에서 진행

```bash
  mariadb -u root -p 엔터
```

## 4. 다양한 명령어

- `Power Shell` 에서 진행

### 4.1. 접속 종료하기

```bash
 exit 또는 Ctrl + C 입력
```

### 4.2. 시스템 안에 어떤 데이터베이스가 있는지 파악하기

```bash
 SHOW DATABASES;
 반드시 ; 을 입력해서 실행한다.
```

### 4.3. 나만의 데이터베이스(Scheme) 만들기

```bash
 CREATE DATABASE board;
 반드시 ; 을 입력해서 실행한다.
```

### 4.4. 나만의 데이터베이스(Scheme) 삭제하기

```bash
 DROP DATABASE board;
 반드시 ; 을 입력해서 실행한다.
```

### 4.5. 나만의 데이터베이스(Scheme) 만들고, 사용하기

```bash
 CREATE DATABASE board;
 SHOW DATABASES;
 USE board;
 반드시 ; 을 입력해서 실행한다.
```

### 4.5. 나만의 데이터베이스(Scheme) 이미 존재하는 TABLE 목록 보기

```bash
 SHOW TABLES;
 반드시 ; 을 입력해서 실행한다.
```

## 5. 편리한 도구 workbench 설치하기

- [사전설치필요](https://12716.tistory.com/entry/MySQL-MySQL-Workbench-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%98%A4%EB%A5%98-%EB%B0%9C%EC%83%9D%EC%8B%9C-%ED%95%B4%EA%B2%B0)
- https://dev.mysql.com/downloads/workbench/

### 5.1. 커넥션을 생성해 준다.

### 5.2. Query Editor 가 PowerShell 이라고 생각하자

- 기본은 `SQL 명령어 입력`하고 `;` 을 입력하고 `Ctrl +Enter` 로 실행

## 6. 실습

```bash
USE board;
```

### 6.1. author 테이블 만들기

- `작가(authors)` 라는 테이블 만들기
  - id : 자동 증가 및 PK 역할을 한다.
  - name : 255 자
  - email : 255 자
  - password : 255 자

```sql
use board;
-- 작가 테이블 생성
create table authors(
	id int Primary key,
    name varchar(255),
    email varchar(255),
    password varchar(255)
);
-- 생성된 테이블의 정보 보기
describe authors;
-- 실행되어진 테이블 생성 코드 보기
show create table author;
```

### 6.2. posts 테이블 만들기

- `게시글(posts)` 이라는 테이블 만들기
  - id : 자동증가(AI), PK
  - title : 255자
  - content : 3000자
  - `authors_id : FK`

```sql
-- 게시글(posts) 테이블 만들기
create table posts(
   id int,
    title varchar(255),
    content varchar(3000),
    authors_id int not null,
    primary key (id),
    foreign key (authors_id) references authors(id)
);

describe posts;
```

### 6.3. 테이블 명 변경하기

```sql
USE 데이터베이스명;
ALTER TABLE 테이블명 RENAME 테이블명;

ALTER TABLE posts RENAME post;
SHOW TABLES;
```

### 6.4. 테이블에 컬럼 추가/삭제하기

- 추가하기

```sql
USE 데이터베이스명;

-- 반드시 테이블 구조 확인 후 실행
DESCRIBE 테이블명;

ALTER TABLE 테이블명 ADD 컬럼명 데이터타입;

alter table authors add age int;
describe authors;
```

- 삭제하기

```sql
USE 데이터베이스명;

-- 반드시 테이블 구조 확인 후 실행


ALTER TABLE 테이블명 DROP 컬럼명;

alter table authors drop age;
describe authors;
```

### 6.5. 테이블에 컬럼 변경하기

```sql
USE 데이터베이스명;

-- 컬럼 변경하기
DESCRIBE 테이블명;
ALTER TABLE 테이블명 CHANGE COLUMN 컬럼명 새컬럼명 데이터타입
DESCRIBE 테이블명;

describe posts;
alter table posts change column content contents varchar(3000);
describe posts;
```

### 6.6. 특정 컬럼의 속성 변경하기

```sql
USE 데이터베이스;
DESCRIBE 테이블명;
ALTER TABLE 테이블명 MODIFY COLUMN 컬럼명 데이터타입;
DESCRIBE 테이블명;

describe authors;
-- 컬럼 변경 시 데이터 타입이 있어야 합니다.
alter table authors modify column email varchar(30) not null;
describe authors;
```

### 6.7. 데이터 추가하기(insert)

- DML : 추가(insert), 수정(update), 삭제(delete)

```sql
USE 데이터베이스명;
DESCRIBE 테이블명;
-- 데이터 추가
INSERT INTO 테이블명 (칼럼1, 칼럼2, 칼럼3) VALUES (값1, 값2, 값3)

insert into authors (id, name, email) values (1, "홍길동", "hong@a.com");
-- 데이터 입력 결과 확인하기 (전체확인)
select * from authors;
```

### 6.8. `authors 테이블의 id 가 1 인 데이터`를 posts 테이블에 추가

- FK 가 있는 경우

```sql
-- FK 가 있는 경우
describe posts;
insert into posts (id, title, contents, authors_id) value (1, "제목1", "내용1", 1);
select * from posts;
```

- FK 가 없는 경우에는 데이터오류가 발생하며 데이터 추가 안됨

```sql
IMSERT INTO 테이블명 (칼럼명, 칼럼명, ...) VALUES (값, 값, ...);
-- FK 에 없는 사용자 id 를 넣는다면 ?
insert into posts (id, title, contents, authors_id) values (3, "제목3", "내용3", 3);
select * from posts;
```

### 6.9. 데이터 수정 (Update)

- `반드시 조건절이 있어야 한다.`
- 만약, 조건절이 없으면 전체 데이터가 수정 된다.
- `WHERE 조건` 을 누락하면 모든 데이터가 수정된다. 주의사항
- 주의사항 : 기존 프로그래머들
  - `==` 을 사용하지 않습니다. 즉, 같다는 표현이 아닙니다.
  - `=` 을 사용하여 같다. 즉, 비교 한다.

```sql
UPDATE 테이블명 SET 칼럼명 = 값, 칼럼명 = 값 WHERE 조건;
UPDATE 테이블명 SET 칼럼명 = 값, 칼럼명 = 값 WHERE 필드이름=값;
-- WHERE 필드이름=값 은 비교값 필드이름과 값이 같은 거 찾음

-- 사용자 id 1 의 email 을 변경하기
update authors set email = "hong@gmail.com" where id = 1;
select * from authors;
```

### 6.10. 데이터 삭제(delete)

- 퇴사..당한다.
- `WHERE 를 반드시 사용한다.`

```sql
DELETE FROM 테이블명 WHERE 조건;
DELETE FROM 테이블명 WHERE 필드이름 = 값;

-- id 2 번 유저 삭제하기
select * from authors;
delete from authors where id = 2;
```

### 6.11. 테이블의 모든 데이터 삭제하기

- `DELETE FROM 테이블명`

  - 데이터를 지우고 테이블은 남겨둔다.
  - `복구가 가능하다.`
  - storage 에 파일형태로 자료는 남아있음

- `TRUNCATE TABLE 테이블명`

  - 데이터도 지우고 테이블도 지운다.
  - `복구가 불가능하다.`

- `DROP TABLE 테이블명`
  - 데이터 삭제, 테이블 구조 삭제, 복구 불가

### 6.12. 데이터 조회 (Select)

- 개발자가 가장 많이 사용함
- 특정한 데이터에서 원하는 결과를 도출해 내는 작업
- 알고리즘 시험에 늘 나옴

- 모든 데이터 조회하기

```sql
SELECT * FROM 테이블명;
select * from authors;
```

### 6.13. WHERE 절을 이용한 원하는 데이터 출력하기

- PK 를 이용해서 데이터 추출하기 (유일한 값)

```sql
select *
from authors
where id = 1;
```

- 문자열 비교는 반드시 `작은 따옴표`를 사용한다.

```sql
select *
from authors
where name = '또치';
```

- 숫자는 비교시 =, >, <, >=, <= 가능하다.

```sql
select *
from authors
where id >= 1;
```

- where 조건이 2개 이상이면 AND 를 사용하자

```sql
select *
from authors
where id > 1 and email = 'd@d.com';

select *
from authors
where id > 1 and password = '1234';
```

- 특정 항목만 조회하기

```sql
select name
from authors;

select name
from authors
where name = '아이유';
```

- 정렬로 조회하기 (pk를 기준으로)

```sql
-- 오름차순 정렬
select *
from authors
order by name;

-- 오름차순 정렬
select *
from authors
order by id asc;

-- 내림차순 정렬
select *
from authors
order by id desc;

-- 오름차순 정렬
select *
from authors
order by name;

-- 오름차순 정렬
select *
from authors
order by name asc;

-- 내림차순 정렬
select *
from authors
order by name desc;
```

- 여러 칼럼을 정렬함 (앞쪽 처리후, 나머지 순으로 처리 됨)

```sql
select *
from authors
order by name desc, email desc;
```

- 조회수 개수 제한하기

```sql
select *
from authors
limit 2;

-- 조합도 가능
select *
from authors
where password = '1234'
order by id desc
limit 2;
```

- null 을 조회 조건으로 활용

```sql
select *
from authors
where password is null;

select *
from authors
where password is not null;
```

- https://school.programmers.co.kr/learn/challenges?order=recent&page=1&languages=mysql

## 7. 데이터 종류

### 7.1. tinyint

- `-128 ~ 127` 숫자

### 7.2. int

- `-20억 ~ 20억`

### 7.3. bigint

- `글로벌 서비스`

### 7.4. unsigned

- 양수, 음수를 제외하고 2배로
- `int unsigned` : 40억
- `tinyint unsigned` : 0 ~ 255

### 7.5. 실수

- `decimal(m, d)`
- `float`
- `double`

### 7.6. 문자 char(길이)

- 길이에는 최대 길이를 작성함
- 길이 만큼 공간차지
- 성별, 주민번호, 전화번호
- 길이가 정해진 데이터를 정의할 때

### 7.7. 문자 varchar(길이)

- 웬만하면 이거 사용하자
- 길이는 문자열의 최대 길이를 말함
- 가변 길이로서 딱 글자만큼만 저장함
- 이름, 상품명, 주소 등
- 빠름

### 7.8. 문자 text

- 길이 지정 못함
- 가변적으로 길이 정해짐
- 최대 65536 글자 가능
- 느림

### 7.9. blob

- 바이너리 데이터
- png 등의 이미지를 굳이 보관할 때
- 안쓴다.

### 7.10. enum

- 미리 들어갈 수 있는 특정 데이터의 목록값을 지정

```sql
ALTER TABLE author ADD COLUMN role ENUM('admin', 'user') not null default 'user';
```

### 7.11. 날짜 date

- YYYY-MM-DD
- 날짜만 저장
- 생년월일

### 7.12. 날짜 datetime

- 날짜와 시간을 저장
- 옵션을 주면 m 지정시 소수점 microseconds 단위까지 저장
- YYYY-MM-DD HH:MM:SS
- 가장 많이 사용
- 주문 시간, 글 작성 시간 등
- `칼럼명 datetime default current_timestamp()` : 현재 시간을 기본으로

### 7.13. 날짜 now() 함수

- 현재 날짜와 시간을 반환
