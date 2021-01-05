# 데이터를 제어하는 DML(Data Manipulation Language)

## SQL 명령어 종류
- DML(Data Manipulation Language) : 데이터 조작어
- DDL(Data Definition Language) : 데이터 정의어
- DCL(Data Control Language) : 데이터 제어어
- TCL(Tranjection Control Language) : 트랜잭션 제어어

## 중요하지만 헷갈리는 SQL 문법
- BETWEEN A AND B : A와 B 사이에 값이 있다. 나이대, 날짜에 주로 사용
- IN **(list)** : list에 오는 어느 값이라도 일치하면 된다. **괄호**가 반드시 필요
    - IN (list A, list B, list C) : list가 여러개일 때도 사용, 다수의 리스트를 검색할 때 용이
- LIKE '비교문자' : 일치 여부, **% 활용**
- 다양한 비교연산자 : 숫자, 문자 비교. 문자는 ""를 사용함에 유의

## 데이터에서 유사한 값 찾기

> LIKE : 특정 문자가 포함된 문자열을 찾고 싶을 때 사용하는 명령

- 기본문법 : 
    ```
    SELECT(명령) *(검색할 컬럼)
    FROM book(테이블명)
    WHERE title LIKE ‘어린왕자';(조건절)
    ```
    - % : 와일드카드(어떠한 문자열이라도 Ture이다.)

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

> ORDER BY : 데이터를 검색할 때 정렬하여 결과를 출력하는 명령어

- 기본 문법 : score 테이블에서 수학(math) 값이 **높은 데이터(오름차순, DESC)**부터 정렬하여 검색
    ```
    SELECT(명령) *(검색할 컬럼)
    FROM score(테이블)
    ORDER BY math DESC;(정렬 조건)
    ```

    - 다양한 형태 : score 테이블에서 수학(math) 값이 **낮은 데이터(내림차순, ASC)**부터 정렬하여 검색
        ```
        SELECT *
        FROM score
        ORDER BY math ASC;
        ```


## 테이블에 데이터 삽입하기

## 테이블의 데이터 수정하기

## 테이블의 데이터 삭제하기
