# Relational DB modeling

- 업무파악 - 개념적 데이터 모델링 - 논리적 데이터 모델링 - 물리적 데이터 모델링

# 개념적 데이터 모델링에 대하여

## ERD(Entity Relation Diagram)
- 업무를 파악하는 과정 : table, node, draw 등 DB마다 다른 이름을 포함하기 위해 Entity라고 표현
- 데이터베이스 모델링 과정
- 속성은 네모(Entity-Attribute)에 원(Circle)로 연결되며, 열(column)이 될 것이다.

### drow.io : ERD를 그리게 도와주는 무료 소프트웨어

## Key에 대해

- Primary Key(기본키)
- Candidate Key(후보키, 대체키)
- 중복키(Composite Key)
    - SQL 질의 시 Where(조건)에 키(or 중복키)를 쓰는게 가장 빠르다.
- Foreign Key(외래키)

## Cardinality(기수성) / Optionality
- 관계를 표현할 땐, **마름모꼴**을 그려 **선을 연결**하여 표현한다.

### Cardinality
- 1:1 관계 : ㅁ-ㅁ
- 1:N 관계 : 까마귀발, ㅁ-<ㅁ
- N:M 관계 : ㅁ>-<ㅁ
    - 모델링에선 상관없지만, 실제 DB를 구현할 땐 N:M관계를 구현하기 위해선 각각의 1:N 관계의 Mapping table을 추가해야한다.

### Optionality(옵션) / Mandatory(필수) + Cardinality
- Mandotory : Optional 관계 : ㅁ-|--ㅇ-ㅁ
- M:O, 1:N의 관계 : ㅁ-|---ㅇ<ㅁ
    - 저자와 글의 관계(작성) : Mandatory(M) : Optional(N)
    - 저자와 댓글의 관계(작성) : Mandatory(1) : Optional(N)
    - 글과 댓글의 관계(소속) : Mandatory(1) : Optional(N)

# 논리적 데이터 모델링에 대하여

## MySQL 사용법

1. 터미널에서 사용할 디렉토리 접근 후 
```
mysql.exe -uroot -p
```
입력 후 그냥 엔터를 치면 mysql에 접근된다.
2. MySQL Workbench로 GUI 환경에서 이용할 수도 있다.

















