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

- LIKE문의 기본문법 : 
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

- ORDER BY문의 기본 문법 : score 테이블에서 수학(math) 값이 **높은 데이터(내림차순, DESC)**부터 정렬하여 검색
    ```
    SELECT(명령) *(검색할 컬럼)
    FROM score(테이블)
    ORDER BY math DESC;(정렬 조건)
    ```

    - 다양한 형태 : score 테이블에서 수학(math) 값이 **낮은 데이터(오름차순, ASC)**부터 정렬하여 검색
        ```
        SELECT *
        FROM score
        ORDER BY math ASC;
        ```


## 테이블에 데이터 삽입하기

> INSERT : 관계형 데이터베이스의 테이블에 값을 저장하는 명령

- INSERT문의 기본 문법 : ‘햄릿’ 책 데이터를 book 테이블에 추가
    ```
    INSERT(명령) INTO book(id, title, author, publisher)(테이블 컬럼)
    VALUES('3', '햄릿', ＇윌리엄 셰익스피어', '엘리스 출판')(추가할 데이터);
    ```
    - 팁 : 컬럼을 명시하지 않으면 순서대로 값을 삽입
        ```
        INSERT INTO book(id, title, author, publisher)
        VALUES('3', '햄릿', ＇윌리엄 셰익스피어', '엘리스 출판');
        ```

## 테이블의 데이터 수정하기

> UPDATE :  관계형 데이터베이스의 테이블에서 이미 저장된 값을 수정하는 명령

- UPDATE 문의 기본 문법 : 책 제목(title)이 ‘돈키호테’인 데이터의 제목(title)을 ‘돈키호테 1’로 변경
    ```
    UPDATE(명령) book(테이블)
    SET title = '돈키호테 1'(변경할 값)
    WHERE title = '돈키호테';(조건)
    ```

## 테이블의 데이터 삭제하기

> DELETE : 관계형 데이터베이스의 테이블에서 이미 저장된 값을 삭제하는 명령

- DELETE 문의 기본 문법 : 제목이 ‘돈키호테 1’인 책 데이터를 book 테이블에서 삭제
    ```
    DELETE(명령)
    FROM book(테이블)
    WHERE title = '돈키호테 1';(조건)
    ```
    - 팁 : WHERE 조건이 없을 시 모든 데이터 삭제**(조삼!)**
        ```
        DELETE
        FROM book
        WHERE title = '돈키호테 1';
        ```
