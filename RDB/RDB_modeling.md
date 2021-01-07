# Relational DB modeling

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

## Cardinality / Optionality
- 관계를 표현할 땐, **마름모꼴**을 그려 **선을 연결**하여 표현한다.
- 1:1 관계 : ㅁ-ㅁ
- 1:N 관계 : 까마귀발, ㅁ-<ㅁ
