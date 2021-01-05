# DATABASE

## 데이터베이스(DB)란?

## DB를 사용하는 이유
- 데이터 타입을 지정해서 선언, 삽입한다.(Java에서 변수 선언 시 데이터 타입을 지정하는 것과 원리가 동일하다)
- 식별자(Key), 인덱스(Index)를 이용한다.
- 대부분의 DB System은 Server-Client 구조를 이용한다.
- DB를 사용하려면 반드시 인증 시스템을 거쳐야 한다.(보안)

## DB의 종류

### 관계형 DB (Relational DB)
#### 종류
- Oracle
- MySQL
- MariaDB(MySQL과 쌍둥이)
- Microsoft SQL Server
- PostareSQL
- IBM DB2
- SQLite 등

#### 엑셀과 관계형 DB의 차이점
Excle은 자료형에 관계 없이 삽입 가능하다.(추출할 때 데이터 타입을 일일이 체크해야해서 까다롭다.)
관계형 DB는 지정한 자료형만 삽입할 수 있다.
관계형 DB는 SQL이라는 언어를 활용해서 다룬다.
공통점 : 표를 기반으로 한다.

#### 개념
Structured : 구조화 되어 있다. (tuple과 attribute이 있는 표)
표를 만드려면 Base가 필요하다.

#### 로컬 환경에서 MySQL 사용하기

##### MySQL Server
- Clinet를 이용해서 Server에 접속하려면 Hostname, ID와 Password가 필요하다.

##### MySQL Client
- MySQL Monitor를 이용하여 Server에 connect하기(bitnami의 WAMP를 설치하여 이용)
- MySQL workbench : GUI 방식으로 MySQL을 이용할 수 있음.

--------------------------------------------------------------------------------------------------------------------------------------

### 그래프형 DB (Graph DB)

### 계층형 DB

### 문서형 DB (Document DB)




## SQL(구조화된 질의하는 언어)

##### Structured
##### Query
##### Language
- 프로그래밍 언어에 SQL을 활용하여 자동화할 수 있다.

### CREATE
- CREATE TABLE 테이블명(attribute이름 자료형 null여부 추가조건 등);

### READ
- SELECT 어트리뷰트명 FROM 테이블명 WHERE 상세조건;

### UPDATE
#### INSERT
- INSERT INTO 테이블명 (어트리뷰트 이름) VALUES(값);
#### UPDATE
- UPDATE 테이블명 SET 바꾸려는 칼럼이름='' WHEHE 애트리뷰트이름='' 

### DELETE
- DELETE 테이블명 WHERE 필드이름=데이터값
- Truncate 테이블명 : 테이블 전체 삭제

## DB 공부방법

### CRUD 사용방법을 체크하기
- Create
- Read
- Update
- Delete

## 추가정보
- DB 랭킹 사이트 : https://db-engines.com/en/ranking
- 네트워크에 접속된 각각의 장치 : host
- port : 약속된 서버마다의 포트 번호가 존재한다.(웹:80, MySQL:3306)
- 스키마(scheme) or database : 테이블의 집합체(바탕)
