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

# 1. MongoDB Create

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

* MongoDB에서 데이터베이스를 만드는 것은 굉장히 간단합니다. 바로, use 명령어를 사용하는 것입니다.

```
>use  Library
#생성 완료 시
switched to db Library
```

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

1. createCollection()사용하여 생성
collection을 생성할 때는 ```db.createCollection(name, [options])``` 명령어를 사용합니다.

2. insert() 사용하여 생성
MongoDB는 document를 collection에 삽입하는 ```insert()``` 명령을 제공합니다.

## 데이터베이스 생성 실습

```
import pymongo

# 데이터베이스를 연결하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")

# library 데이터베이스를 만드세요.
db = connection['library']

# 컬렉션 생성
col = db["books"]

# 데이터를 삽입하는 코드: 데이터베이스가 잘 생성되었는지 확인하기 위해서는 반드시 아래 주석을 해제하세요.
data = col.insert_one({ "title": "Harry Potter and the Deathly Hallows", "author": "Joanne Kathleen Rowling","publisher": "Bloomsbury Publishing" ,"date_received": "2017-07-21"})


# 데이터베이스 목록을 reuslt 리스트에 저장하세요..
result = connection.list_database_names()
result2 = db.list_collection_names()

# 값을 잘 저장하였는지 확인하기 위한 코드입니다. 수정하지 마세요!
print(result, result2)
```

# 2. MongoDB Find

이번 이론에서는 CRUD 중 R(Read)파트를 알아보겠습니다. 데이터베이스에서 데이터를 가져오거나 가져오는 방법은 쿼리를 사용하여 수행하게 됩니다. 쿼리 작업을 수행하는 동안 데이터베이스에서 특정 데이터를 검색하는데 사용할 수 있는 기준이나 조건을 사용할 수도 있습니다.

MongoDB는 데이터베이스에서 도큐먼트를 검색하는데 사용되는 find() 메소드를 제공합니다. 기본적인 쿼리 문법은 아래와 같습니다. 또한, find() 메소드는 구조화되지 않은 방식으로 모든 도큐먼트를 표현합니다. (조금 더 디테일하게 Find하는 방법은 2장에서 다루게 됩니다.)
```
#문법
db.COLLECTION_NAME.find()
```
저번 MongoDB Create 이론에서 예시를 들었던 컬렉션을 찾는다고 가정할 때, 아래와 같은 쿼리를 작성할 수 있습니다.
```
db.Book(Collection명).find({})
```
> 모든 코드는 MongoDB Javascript 명령 셸에서 실행됩니다.

## Find One
MongoDB의 컬렉션에서 데이터를 선택하려면 find_one()메소드를 사용할 수 있습니다.
```
#문법
컬렉션이름.find_one()
```

## Find All
MongoDB의 테이블에서 데이터를 선택하려면 find()메소드를 사용할 수도 있습니다. find()메소드를 사용하면 **선택 항목의 모든 항목을 출력**합니다.
```
#문법
컬렉션이름.find()
```

💡 Tip!
> 위에서 다루었던 메소드를 사용하여 보고자 하는 데이터를 형식적으로 출력하고 싶을 때 **pprint()**메소드를 사용하여 출력해보세요.

## 콜렉션의 모든 데이터 출력하기

컬렉션 객체에 find() 메소드를 이용해 데이터 검색을 할 수 있습니다. 컬렉션.find()을 통해 반환되는 것은 MongoDB의 Cursor객체 입니다.

이 Cursor객체를 이용해서 데이터, 즉 도큐먼트를 확인할 수 있습니다.

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# books 컬렉션에 들어있는 책들을 출력하세요.
for doc in col.find() :
    print(doc)
```

## 데이터 보기 좋게 출력하기(파이썬 pprint 라이브러리 이용)
```
import pymongo
from pprint import pprint


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# pprint를 이용해 데이터를 보기 좋게 출력하세요.

for doc in col.find() :
    pprint(doc)
```

> 결과
```
{'_id': ObjectId('600a2ce81c88a6828dc0daca'),
 'author': 'William Shakespeare',
 'date_received': '2012-04-01',
 'title': 'Romeo and Juliet'}
{'_id': ObjectId('600a2ce81c88a6828dc0dacb'),
 'author': 'Miguel de Cervantes Saavedra',
 'date_received': '2015-03-31',
 'title': 'Don Quixote'}
{'_id': ObjectId('600a2ce81c88a6828dc0dacc'),
 'author': 'Antoine de Saint-Exupery',
 'date_received': '2018-12-21',
 'title': 'The Little Prince'}
{'_id': ObjectId('600a2ce81c88a6828dc0dacd'),
 'author': 'Joanne Kathleen Rowling',
 'date_received': '2017-06-26',
 'publisher': 'Bloomsbury Publishing',
 'title': "Harry Potter and the Philosopher's Stone"}
{'_id': ObjectId('600a2ce81c88a6828dc0dace'),
 'author': 'John Ronald Reuel Tolkien',
 'date_received': '2014-07-29',
 'publisher': 'Allen & Unwin',
 'title': 'The Lord of the Rings'}
```


# MongoDB Insert

생성된 컬렉션에 도큐먼트를 생성하는 방법은 기본적으로 가장 크게 3가지 입니다. 
insert(), insertOne(), insertMany() 로 3가지입니다.

## 1. ```insert_one()``` 메소드 사용하여 생성하기

```
#문법
collection.insert_one( 컬렉션에 삽입할 도큐먼트 혹은 변수 ) 
```

```
#예시
mycol = mydb["customers"]

mydict = { "name": "John", "address": "Highway 37" }

x = mycol.insert_one(mydict)
```

## 2. ```insert_many()```메소드 사용하여 생성하기

```
#문법
collection.insert_many( 컬렉션에 삽입한 도큐먼트 혹은 변수)
```

```
#예시
mycol = mydb["customers"]

mylist = [
  { "name": "Amy", "address": "Apple st 652"},
  { "name": "Hannah", "address": "Mountain 21"},
  { "name": "Michael", "address": "Valley 345"},
  { "name": "Sandy", "address": "Ocean blvd 2"},
  { "name": "Betty", "address": "Green Grass 1"},
  { "name": "Richard", "address": "Sky st 331"},
  { "name": "Susan", "address": "One way 98"},
  { "name": "Vicky", "address": "Yellow Garden 2"},
  { "name": "Ben", "address": "Park Lane 38"},
  { "name": "William", "address": "Central st 954"},
  { "name": "Chuck", "address": "Main Road 989"},
  { "name": "Viola", "address": "Sideway 1633"}
]

x = mycol.insert_many(mylist)
```

## 데이터 삽입하기

책 데이터를 삽입하기 위해서는 아래처럼insert_one() 함수를 사용하며 매개변수로 딕셔너리를 넘겨주면 됩니다.
```
data = 컬렉션.insert_one(딕셔너리)
```

또한 inserted_id를 이용해 삽입된 데이터의 id를 확인할 수 있습니다.
```
data.inserted_id
```

```
import pymongo
from pprint import pprint


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 딕셔너리 정의
dic = { 
    "title" : "Romeo and Juliet",
    "author" : "William Shakespeare",
    "data_received" : "2012-04-01"
}

# 데이터를 만들고 삽입하세요.
data = col.insert_one(dic)

# 삽입된 데이터 id를 이용해 데이터를 보기 좋게 출력하세요.
pprint(data.inserted_id)
```

## 여러 데이터 삽입하기

여러 데이터 삽입하기
도서관에 새로운 책이 들어왔는데 이번에는 권수가 좀 많습니다. 따라서 이번에는 여러 책을 한 번에 데이터베이스에 삽입하려고 합니다.

한 번에 여러 데이터를 삽입하기 위해서는 insert_many() 함수를 이용합니다. 또한 삽입할 딕셔너리 데이터를 리스트로 묶어 매개변수에 넘겨주어야 합니다.
```
data = 컬렉션.insert_many(리스트)
```

삽입된 데이터들의 id를 확인하려면 **컬렉션 객체에 inserted_ids**를 이용하면 됩니다. 그리고 아래처럼 반복문을 이용하면 각 id를 출력할 수 있습니다.
```
for book_id in data.inserted_ids:
    print(book_id)
```

```
import pymongo
from pprint import pprint


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 새로 들어온 책들입니다. 리스트로 묶어 선언하세요.
new_book1 = {"title": "Alice\'s Adventures in Wonderland", "author": "Lewis Carroll", "publisher": "Macmillan", "date_received": "2015-11-26"}
new_book2 = {"title": "The Old Man and the Sea", "author": "Ernest Miller Hemingway","publisher": "Charles Scribner\'s Sons" ,"date_received": "2014-11-02"}
new_book3 = {"title": "The Great Gatsby", "author": "Francis Scott Key Fitzgerald", "date_received": "2019-01-12"}

new_book = [new_book1, new_book2, new_book3]

# 데이터를 만들고 삽입하세요.
data = col.insert_many(new_book)

# 삽입된 데이터 id를 이용해 데이터를 보기 좋게 출력하세요.
for book_id in data.inserted_ids:
    pprint(book_id)
```


# MongoDB Update

CRUD 중 U(Update)파트를 알아보겠습니다. 우리가 생성한 도큐먼트에 대해 수정을 하는 것입니다. MongoDB에서는 ```update_one()```과 ```update_many()```라는 메소드를 사용하여 수정 작업을 진행합니다.

## 1. update_one()를 사용하여 수정하기

update_one()는 기본적으로 단일 도큐먼트를 수정할 수 있습니다. 다만 유의해야 할 점이있습니다. 수정할 데이터는 데이터를 삽입할 때처럼 딕셔너리를 넣으면 되지만, 수정 내용은 ```$set``` 연산자를 명시해주어야 합니다.
```
#문법
collection.update_one(수정할 데이터, 수정 내용)
```

```
#예시) 주소 "Vally 345"에서 "Canyon 123"으로 수정

query = { "address": "Valley 345" }
newvalues = { "$set": { "address": "Canyon 123" } }

mycol.update_one(query, newvalues)
```

## 2. update_many()를 사용하여 수정하기

위 update_one()메소드는 하나의 데이터를 수정하는 방법입니다. 이번에는 여러 데이터를 한 번에 수정하는 update_many()메소드에 대해 학습하겠습니다.

매개변수로 수정할 데이터와 수정 내용을 넘겨주어야 하는데, 수정 내용은 하나의 데이터를 수정할 때와 마찬가지로 ＄set연산자를 이용하여 넘겨주면 됩니다. 반대로 update_many()는 수정할 데이터가 여러 개이므로 정규 표현식을 이용해야 합니다. 정규 표현식을 이용하기 위해서 ＄regex연산자를 사용합니다.
```
#문법
collection.update_many(수정할 데이터, 수정 내용)
```

```
#예시) 주소가 문자"S"로 시작하는 모든 도큐먼트 수정
query = { "address": { "$regex": "^S" } }
newvalues = { "$set": { "name": "Minnie" } }

mycol.update_many(query, newvalues)
```


# MongoDB Delete

CRUD 중 D(Delete)파트를 알아보겠습니다. 도큐먼트를 지우는 작업은 매우 신중해야 합니다. MongoDB에서는 이전으로 되돌리는 **롤백 기능이 없기 때문**입니다.(롤백과 유사한 효과를 볼 수 있는 기능이 하나 있긴 합니다. 하지만, 저희는 아직 사용하지 않으니 공식 문서를 참고해주세요.)(oplog)
deleteOne()은 매칭되는 첫번째 도큐먼트만 삭제하고, deleteMany()는 매치되는 모든 도큐먼트를 삭제한다는 차이가 있습니다.

## 1. delete_one()를 사용하여 삭제하기

하나의 데이터를 삭제하려면 delete_one()메소드를 사용해야 합니다. 만약, 쿼리에서 두 개 이상의 도큐먼트를 작성한다면 첫 번째 항목만 삭제됩니다.
```
#문법
컬렉션.delete_one(삭제할 데이터)
```

```
#예시) 주소가 "Mountain 21"인 도큐먼트 삭제
query = { "address": "Mountain 21" }

mycol.delete_one(query)
```

## 2. delete_many()를 사용하여 삭제하기

delete_one()과 다르게 두 개 이상의 도큐먼트를 삭제하고 싶을 때 delete_many()메소드를 사용합니다.
```
#문법
컬렉션.delete_many(삭제할 데이터)
```

```
#예시) 주소가 문자 "S"로 시작하는 모든 도큐먼트 삭제
query = { "address": {"$regex": "^S"} }

mycol.delete_many(query)
```

## 데이터 수정하기

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 잘못 입력된 책 제목을 수정하세요.
# 두 변수에 바꾸고자하는 내용과, 바꿀 내용을 입력하여 update_one() 메서드의 매개변수로 줍니다.
query = { "title": "The Rings of Lord" }
newvalues = { "$set": { "title": "The Lord of the Rings" } }
col.update_one(query, newvalues)

# 책이 잘 수정되었는지 확인하는 코드입니다. 수정하지 마세요!
for x in col.find():
    print(x)
```

## 여러 데이터 수정하기

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 잘못 입력된 책 작가를 수정하기 위한 딕셔너리를 만드세요.
before = { "title": { "$regex": "^Harry Potter" } }

# 잘못 입력된 책 작가를 수정하세요.
update_book = { "$set": { "author": "Joanne Kathleen Rowling" } }
a = col.update_many(before, update_book)

# 몇 권의 책이 수정되었는지 확인하세요. 몇 개의 데이터가 바뀌었는지 확인하기 위해서는 수정 된 결과를 저장한 변수에 modified_count를 이용하면 됩니다.
a.modified_count

# 책이 잘 수정되었는지 확인하는 코드입니다. 수정하지 마세요!
for x in col.find():
    print(x)
```

## 하나의 데이터 삭제하기

```
import pymongo

# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 사라진 책을 데이터베이스에서 삭제하세요.
dic = { "title": "Alice's Adventures in Wonderland" }
col.delete_one(dic)


# 책이 잘 삭제되었는지 확인하는 코드입니다. 수정하지 마세요!
for x in col.find():
    print(x)
```

## 여러 데이터 삭제하기

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# 2015년에 받은 책을 삭제하기 위한 딕셔너리를 만드세요.
dic = { "date_received": {"$regex": "^2015"} }

# 데이터베이스에서 2015년도에 받았던 책들을 삭제하세요.
delete_book = col.delete_many(dic)

# 몇 권의 책이 삭제되었는지 확인하세요.
delete_book.deleted_count

# 책이 잘 삭제되었는지 확인하는 코드입니다. 수정하지 마세요!
for x in col.find():
    print(x)
```
