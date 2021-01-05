# 데이터를 제어하는 DML(Data Manipulation Language)

## SQL 명령어 종류
- DML(Data Manipulation Language) : 데이터 조작어
- DDL(Data Definition Language) : 데이터 정의어
- DCL(Data Control Language) : 데이터 제어어
- TCL(Tranjection Control Language) : 트랜잭션 제어어

## 데이터에서 유사한 값 찾기

- LIKE : 특정 문자가 포함된 문자열을 찾고 싶을 때 사용하는 명령
    - 기본문법 : 
        ```
        SELECT(명령) *(검색할 컬럼)
        FROM book(테이블명)
        WHERE title LIKE ‘어린왕자';(조건절)
        ```
        - 다양한 형태(1) : 특정 문자열로 **끝나는** 문자열 검색
            ```
            SELECT *
            FROM book
            WHERE title LIKE '%왕자';
            ```
        - 다양한 형태(2) : 특정 문자열로 **시작하는** 문자열 검색
            ```
            SELECT *
            FROM book
            WHERE title LIKE '어린%';
            ```
        - 다양한 형태(3) : 특정 문자열로 **중간에 들어가는** 문자열 검색
            ```
            SELECT *
            FROM book
            WHERE title LIKE '%린왕%';
            ```
    

## 데이터 정렬하기

## 테이블에 데이터 삽입하기

## 테이블의 데이터 수정하기

## 테이블의 데이터 삭제하기
