# SQL과 함수

## SQL의 함수의 종류

- 데이터 값을 계산하거나 조작 : 행 함수
- 행의 그룹을 계산하거나 요악 : 그룹 함수
- 열의 데이터 타입을 변환

## COUNT

> COUNT : 검색한 결과의 데이터의 개수를 가져오는 내장함수, NULL인 데이터는 제외

- COUNT 함수의 기본문법 : 
    ```
    SELECT(명령) COUNT(id)(검색할 컬럼) FROM book;
    ```
    - 검색할 데이터에 *을 입력하면 모든 데이터의 개수 검색
        ```
        SELECT COUNT(*) FROM book;
        ```

## LIMIT

> LIMIT : 테이블에서 출력하고자 하는 데이터의 개수를 제한하는 명령

- LIMIT 함수의 기본문법 : 첫 번째 컬럼의 시작은 0. 즉, ‘LIMIT 1, 5’는 2번째 컬럼부터 5개를 가져오라는 의미
    ```
    -- book 테이블에서 데이터를 5개만 가져오기
    SELECT(명령) * FROM book LIMIT 5;(제한할 숫자)
    ```
    - 시작시점을 지정하여 지정한 데이터 개수만큼 가져오기
        ```
        -- 2번째 데이터부터 5개를 가져오기
        SELECT * FROM book LIMIT 1, 5;
        ```

## SUM&AVG

> SUM(Summation) : 지정한 컬럼들의 값을 모두 더하여 총점을 구해주는 내장함수

- SUM 함수의 기본문법 : SUM을 이용해 원하는 데이터의 합을 구할 수 있다.
    ```
    SELECT(명령) SUM(math)(검색할 컬럼) FROM grade;
    ```

> AVG : 지정한 컬럼들의 값을 모두 더하여 총점을 구해주는 내장함수

- AVG 함수의 기본문법 : 지정한 컬럼들의 평균값을 구해주는 내장함수 (SUM/COUNT를 사용하지 않아도 된다.)
    ```
    SELECT(명령) AVG(korean), AVG(english), AVG(math)(평균을 구할 컬럼) FROM grade;
    ```

## MAX&MIN

> MAX : 테이블에 존재하는 데이터에서 최대값을 가져오는 내장함수(숫자형 뿐만 아니라 문자형도 가능)

- MAX 함수의 기본문법 : 원하는 데이터의 최댓값을 구할 수 있다.
    ```
    SELECT(명령) MAX(korean)(검색할 컬럼) FROM grade;
    ```

> MIN : 테이블에 존재하는 데이터에서 최솟값을 가져오는 함수(숫자형뿐만 아니라 문자형도 가능)

- MIN 함수의 기본문법 : 원하는 데이터의 최솟값을 구할 수 있다
    ```
    SELECT(명령) MIN(english)(검색할 컬럼) FROM grade;
    ```
