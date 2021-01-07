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

- 터미널에서 사용할 디렉토리 접근 후 
    ```
    mysql.exe -uroot -p
    ```
    입력 후 그냥 엔터를 치면 mysql에 접근된다.

- MySQL Workbench로 GUI 환경에서 이용할 수도 있다.


# 물리적 데이터 모델링에 대하여

- MySQL Workbench에서 논리적 모델링 생성 후 Database옵션 클릭 -> Forward Engineer 클릭

    > Reverse Engineering : 제품을 뜯어 설계도를 만듬

    > Forward Engineering : 설계도를 기반으로 제품을 만듬

- MySQL Server 생성 후 연결, workbench에서 다양한 작업 가능


# Python과 MySQL의 연동 (MyPySQL)

- 파이썬에 Python -m pip install MyPySQL
- DB를 읽어와서 출력하는 코드 입력
    ```
    import pymysql

    db = pymysql.connect(
        user='root', 
        passwd='', 
        host='127.0.0.1', 
        db='mydb', 
        charset='utf8'
    )

    cursor = db.cursor(pymysql.cursors.DictCursor)

    sql = "SELECT * FROM topic"
    cursor.execute(sql)
    result = cursor.fetchall()
    print(result)
    ```
## 웹 페이지에 python을 이용하여 DB를 표로 출력 (Flask)

- Python -m pip install flask

- 웹 서버를 만드는 코드
    ```
    from flask import Flask
    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        return "hi"

    if __name__ == '__main__':
        app.run(debug=True)
    ```

    - Static 웹 서버 : 사용자가 요청한 정보만 보여준다.(만들어둔 상품을 파는 것과 같다.)

    - Dynamic 웹 서버 : 사이트가 로드될 때마다 동적으로 동작한다.

        - 요청이 들어올 때마다 제조하여 판매한다. 
        - 커스터마이징에 유리하다.

- 웹 서버에 DB 연동하기
    ```
    import random
    from flask import Flask

    import pymysql
    db = pymysql.connect(
        user='root', 
        passwd='', 
        host='127.0.0.1', 
        db='mydb', 
        charset='utf8'
    )
    cursor = db.cursor(pymysql.cursors.DictCursor)

    app = Flask(__name__)

    @app.route('/')
    def hello_world():
        sql = "SELECT * FROM topic"
        cursor.execute(sql)
        result = cursor.fetchall()
        output = ''
        for row in result:
            print(row['id'], row['title'], row['description'])
            output = output + row['title'] +'<br>'
        return output

    if __name__ == '__main__':
        app.run(debug=True)
    ```











