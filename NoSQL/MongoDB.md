# NoSQL이란?

MongoDB를 본격적으로 익히기 전에 NoSQL이라는 개념에 대해 먼저 알아보겠습니다. 기존에 여러분께서 알고 계시고 사용하셨던 대부분의 SQL은 NoSQL이 아닙니다. NoSQL은 **Not Only** SQL 혹은 Not SQL입니다. 하지만, NoSQL 뜻은 딱 한 가지 종류의 DB만을 뜻하지 않습니다. 예를 들어, 초콜릿과 Not 초콜릿을 생각해봅시다. 초콜릿의 종류는 한정적이지만 Not 초콜릿은 다양한 음식과 종류가 있습니다. SQL DB들은 MySQL, SQLite, PostgreSQL 등 DB마다 조금씩 다르겠지만, 결국에는 SQL이라는 카테고리 안에 있습니다. 반대로 NoSQL은 DB마다 서로 각기 다른 종류가 존재합니다.

NoSQL에는 크게 3가지 종류의 DB가 존재합니다.

1. Document DB **(MongoDB)**
Document형태로 데이터가 저장됩니다. 여기에서 나오는 **Document는 JSON, XML등을 칭하며** Column Oriented와 같이 스키마가 유동적입니다.

2. Key-Value DB **(DynamoDB, Redis)**
**Key와 Value**로 이루어진 간단한 데이터 구조입니다. 기본적인 형태로 value에 String, Integer, List, Hash 등의 가본 자료형이 해당됩니다. 이런 데이터 구조 속도는 빠르고, 분산저장 환경에 용이하여 **key에 대한 엑세스 속도는 빠르지만 검색에는 취약**하다는 특징이 있습니다.

3. Graph DB **(Neo4j)**
데이터를 **노드 간의 관계**로 표현한 DB입니다.

* 데이터 간의 관계를 정의하지 않는다.

RDB(관계형 데이터베이스)가 데이터 관계를 Foreign Key 등으로 정의하고 이용해 join 등 연산을 한다고 가정을 한다면, NoSQL은 데이터 간의 관계를 정의하지 않습니다. 데이터 테이블은 그저 하나의 데이블이며 각 테이블 간의 관계를 정의하지 않고 일반적으로 테이블 간의 join 또한 불가능합니다.

## MongoDB의 특징

- 고정된 테이블 스키마를 갖지 않습니다.
RDB와 다르게 테이블의 스키마가 유동적입니다. 데이터를 저장하는 column은 각기 다른 이름과 다른 데이터 타입을 갖는 것이 허용됩니다.

- 문서지향(Document-Oriented) 데이터베이스입니다.
MongoDB는 유연하며 확장성이 높은 문서 지향의 데이터베이스입니다. RDB에서 사용했던 스키마의 제약이 없고 자유로우면 BSON(Binary JSON)형태로 각 문서가 저장됩니다. 또한, 기존 RDB에서 지원하지 않았던 형태로도 저장이 가능합니다. RDB에서 사용했던 JOIN 기능이 필요 없이 이해하기 쉬운 형태로 데이터를 저장할 수 있습니다.

> 💡 결국, NoSQL이 SQL보다 좋고 나쁘고가 아니라 사용하는 용도에 따라 선택하면 됩니다.


## MongoDB란?

> MongoDB는 오픈소스 문서지향(Document Oriented) 크로스 플랫폼 데이터베이스입니다.

MongoDB는 유연하고 JSON과 유사한 문서로 데이터를 저장합니다. 따라서, 필드는 문서마다 다를 수 있으며 시간에 따라 데이터 구조를 변경할 수 있습니다. 또한, 컬렉션을 사용해 데이터를 하나로 묶습니다. (컬렉션(Collection)이란 용도가 같거나 유사한 문서들을 그룹으로 묶는 것 입니다.) 이러한 컬렉션은 기존의 SQL DB의 테이블처럼 동작합니다. 문서(Document)는 MongoDB내에 있는 하나의 실제데이터를 나타내는 표현입니다. 아래 그림을 보며 다시 글을 읽어봅시다.

### MongoDB의 구조

Database - Collection - Document 3단 구조


1. 도큐먼트(Document)
도큐먼트는 RDB에서의 Row(튜플)과 동일한 개념입니다. 예를 들어 아래와 같은 JSON 형태의 Key-Value로 이루어진 데이터 구조를 하나의 도큐먼트로 생각하면 됩니다. 각 도큐먼트는 id를 가지고 있는데, 이 값은 유일합니다. (Primary Key와 동일한 개념) RDB처럼 스키마로 정해진 것이 없어, 아래 코드에서 id, name, age를 적고 phoneNum이나 email을 추가하여 도큐먼트를 생성해도 문제없이 동작합니다.
```
{
    "id": "aslkf87sdljk", # Primary Key와 같다.
    "name": "Elice",
    "age": 12
}
```
2. 컬렉션(Collection)
컬렉션은 도큐먼트의 그룹 개념입니다. RDB로 생각한다면 테이블과 같은 개념입니다. **다만 도큐먼트와 같이 스키마를 가지고 있지 않는다는 특징**이 있습니다.

3. 데이터베이스(Database)
데이터베이스는 컬렉션들의 물리적인 컨테이너이자 가장 상위 개념입니다. **RDB로 생각한다면 Database와 동일**합니다.

위 구조에서 도큐먼트는 JSON과 비슷한 BSON 구조로 되어있습니다.

> **BSON은 Binary JSON을 의미**합니다. JSON의 일부로 **MongoDB에서 데이터를 저장하기 위한 형식**입니다. BSON은 **Field와 Value**를 가지고 있으며 Key값과 Value값을 가진 **Java의 HashMap과 유사**합니다.
```
{
    name: "Elice",
    age: 12,
    status: "InProgress"
    groups: ["Python", "Javascript"]
}
```

| RDB | MongoDB |
|:---:|:---:|
| 데이터베이스(Database) | 데이터베이스(Database) | 
| 테이블(Table) | 컬렉션(Collection) | 
| 튜플(Row) | 도큐먼트(Document) | 

▶ MongoDB 공식 홈페이지 소개글 <https://www.mongodb.com/what-is-mongodb>

----
# MongoDB의 CRUD

## MongoDB Create

대부분의 데이터베이스에는 CRUD라는 개념이 있습니다. Create, Read, Update, Delete의 약자로 데이터 베이스의 자료를 생성, 조회, 수정, 삭제하는 기능을 뜻합니다.

MongoDB에는 ```create database```라는 명령어가 존재하지 않습니다. MongoDB는 데이터베이스 생성 명령을 제공하고 있지 않습니다. MongoDB에서는 **처음에 정의된 collection에 값을 저장할 때 MongoDB가 자동으로 데이터베이스를 생성**하므로 MongoDB에서는 우리가 **직접 수동으로 생성할 필요가 없습니다**. 기존의 SQL을 사용하셨던 분들이라면 이 부분에서 굉장히 어색해하시고, 이상해하실 것 같습니다. 그렇다면 어떻게 생성을 해야 할까요?
```
{
    "id" : 10,
    "name"(Field name) : "Elice"(Field value)
}
```


위 도큐먼트의 **Field name은 id와 name**이고, 각각의 **value는 10과 Elice**입니다. 이러한 **도큐먼트 묶음이 MongoDB에서 collection**을 구성하게 됩니다.

## Database 생성

MongoDB에서 첫번째 기본적인 단계는 데이터베이스와 컬렉션을 배치하는 것입니다. 데이터베이스는 모든 컬렉션을 저장하는데 사용되면 컬렉션은 모든 도큐먼트를 저장하는데 사용됩니다.

1. 데이터베이스를 생성할 포트 연결하기

MongoDB에서 데이터베이스를 만드는 것은 굉장히 간단합니다. 파이썬에서 MongoDB와 상호작용 하기 위해서는 ```pymongo``` 라이브러리를 사용해야 합니다. pymongo를 import한 뒤 아래의 코드를 이용해 MongoDB를 연결할 수 있습니다. **27017**은 MongoDB를 연결할 때 디폴트로 사용하는 포트 번호입니다.

```
import pymongo

pymongo.MongoClient("mongodb://localhost:27017/")
```

2. 데이터베이스 생성하기

MongoDB가 연결된 객체를 이용해 데이터베이스를 생성할 수 있습니다. 만약 **연결된 객체의 변수명이 connection**이라고 할 때, 데이터베이스를 생성하기 위해서는 아래의 코드처럼 작성되어야 합니다.

```
db = connection["생성할 데이터베이스"]
```

마찬가지로 연결된 객체 connection에 ```list_database_names()``` 메소드를 이용하면 **생성되어 있는 데이터베이스 목록**을 확인할 수 있습니다. 다만 직접 확인해 보면 출력된 데이터베이스 목록에 library 데이터베이스가 없어 아래처럼 기본 데이터베이스만 출력이 됩니다. 데이터베이스에 내용이 없으면 실제로 생성되지 않기 때문입니다.

```
['admin', 'config', 'local']
```

## Collection 생성

MongoDB에서 컬렉션을 만들려면 데이터베이스를 사용하고 만들려는 컬렉션의 이름을 지정합니다. 위에서 생성한 db 변수로 컬렉션을 생성하기 위해서는 아래의 코드처럼 작성되어야 합니다.

```
col = db["생성할 컬렉션"]
```

그리고 데이터베이스 객체 db에 list_collection_names() 함수를 이용하면 컬렉션 목록을 확인할 수 있습니다. 다만 직접 확인해 보면 아무것도 출력이 되지 않습니다.
컬렉션 역시 데이터가 없으면 실제로 생성되지 않기 때문입니다.


