# 넷플릭스 데이터로 MongoDB 만들기

## 1번 실습

MongoDB는 앞선 수업에서 배운것처럼 NoSQL중 하나입니다. 이러한 MongoDB는 **문서지향(Document-Oriented) 데이터베이스**로 **고정된 테이블 스키마를 갖지 않는다는 특징**을 기억하시나요? 이러한 특징 때문에 MongoDB는 **확장성이 크고 관계를 정의하기 까다로운 빅데이터를 다룰 때 주로 사용**됩니다.

아래 주소에 캐글에서 제공하는 넷플릭스 데이터가 있습니다.
Netflix Movies and TV Shows <https://www.kaggle.com/shivamb/netflix-shows>

매번 새로운 데이터가 생겨나는 데이터베이스에서 생겨난 모든 데이터 별로 매번 관계를 연결해줘야 하는 RDB를 이용하면 처리 비용이 매우 커집니다. 따라서 MongoDB를 이용해 넷플릭스 데이터를 처리해보겠습니다.

본격적으로 데이터를 처리하기 전 먼저 데이터베이스와 컬렉션을 생성해봅시다.

### 1번 지시사항
1. pymongo 라이브러리를 이용해 파이썬과 MongoDB를 연결하세요.
2. netflix 데이터베이스를 생성하세요.
3. titles 컬렉션을 생성하세요.
4. 생성한 컬렉션에 아무런 데이터를 넣은 후, 생성이 잘 되었는지 확인하기 위해 데이터베이스 목록과 컬렉션 목록을 result1, result2 변수에 저장하세요.

### 1번 소스 코드

```
# pymongo 라이브러리를 이용해 파이썬과 MongoDB를 연결하세요.
import pymongo

connection = pymongo.MongoClient('localhost', 27017)


# netflix 데이터베이스를 생성하세요.
db = connection["netflix"]

# titles 컬렉션을 생성하세요.
col = db["titles"]

# 데이터를 삽입하는 코드
data = col.insert_one({ "title": "Harry Potter and the Deathly Hallows", "author": "Joanne Kathleen Rowling","publisher": "Bloomsbury Publishing" ,"date_received": "2017-07-21"})

# 데이터베이스와 컬렉션 목록을 reuslt 변수에 저장하세요.
result1 = connection.list_database_names()
result2 = db.list_collection_names()

# 목록이 잘 들어갔는지 확인하기 위한 코드입니다. 수정하지 마세요!
print(result1)
print(result2)
```

## 2번 실습

넷플릭스 데이터 삽입하기
앞으로 처리해야 할 넷플릭스 데이터가 netflix_titles.csv에 들어있습니다.

csv파일에 있는 데이터를 처리하기 위해 csv 라이브러리를 import해야 합니다. 그리고 open() 메소드를 이용해 파일을 엽니다.
```
reader = open('읽을 파일', 'r')
```
여기서 r은 읽기 모드로 파일을 연다는 것을 의미합니다.

그리고 csv.DictReader를 이용해 MongoDB에 넣을 딕셔너리 형태로 데이터를 저장할 수 있습니다.
```
data = csv.DictReader(reader, ('필드1', '필드2', ...))
```
netflix_titles.csv 데이터는 아래와 같은 필드로 이루어져 있습니다.

| 필드명 | 설명 |
|:---:|:---:|
| show_id | id |
| type | 유형 |
| title | 제목 |
| director | 감독 |
| cast | 배우 |
| country | 국가 |
| date_added | 넷플릭스에 추가된 일자 |
| release_year | 실제 개봉 일자 |
| rating | 상영 등급 |
| duration | 상영 시간 |
| listed_in | 장르 분류 |
| description | 설명 |

csv파일을 읽고 컬렉션에 데이터를 추가해봅시다.

### 2번 지시사항

1. csv 라이브러리를 import하세요.
2. open() 메소드를 이용해 netflix_titles.csv을 읽기 모드로 여세요.
3. csv.DictReader() 메소드를 이용해 csv 파일의 데이터를 딕셔너리 형태로 읽으세요.
4. 읽은 데이터를 titles 컬렉션에 삽입하세요.

> Tips :마지막 코드 컬렉션.find_one()을 통해 출력되는 것은 첫 번째 데이터인 필드명입니다.

### 2번 소스코드

```
# csv 라이브러리를 import하세요.
import pymongo
import csv


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

# netflix_titles.csv을 읽기 모드로 여세요. 파일을 불러오는 open() 메소드를 이용해 파일을 엽니다
reader = open('netflix_titles.csv', 'r')

# csv 파일의 데이터를 딕셔너리 형태로 읽으세요. csv.DictReader 이용
# MongoDB에 넣을 딕셔너리 형태로 데이터를 저장할 수 있습니다.
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year',
 'rating', 'duration', 'listed_in', 'description'))

# 읽은 데이터를 titles 컬렉션에 삽입하세요.
result = col.insert_many(data)

# 목록이 잘 들어갔는지 확인하기 위한 코드입니다. 수정하지 마세요!
print(col.find_one())
```

## 3번 실습

넷플릭스에 새로운 작품이 추가되었습니다. 따라서 csv 파일에 없는 새로운 작품 정보를 데이터베이스에 삽입해봅시다.

기존의 작품 정보는 유지한 채 아래 데이터만 추가하면 됩니다.

| 필드명 | 데이터 |
|:---:|:---:|
| show_id | 95889578 |
| type | 'Movie' |
| title | 'Sweet Home' |
| director | 'Eungbok Lee' |
| cast | 'Gang Song' |
| country | 'Korea' |
| date_added | '18-Dec-20' |
| release_year | '2020' |
| rating | 'TV-MA' |
| duration | '497min' |
| listed_in | 'Thrillers' |
| description | 'A teenage boy shutting off the world and stuck in a room. Hyunsoo comes out of the world. Humans turned into monsters. You still have to live. I'm still a person. You have to fight with your neighbors.' |

### 3번 지시사항

1. 새로운 데이터 딕셔너리를 만들고 컬렉션에 삽입하세요. 삽입한 결과를 new_add_data 변수에 저장하세요.

> Tips
- 딕셔너리에 데이터 저장시 작은 따옴표를 주의하세요. show_id를 제외한 필드는 모두 문자열입니다.
- ObjectId는 다를 수 있으며 채점 기준에서는 제외됩니다.

### 3번 소스코드

```
import pymongo
import csv


# 데이터를 삽입하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

reader = open('netflix_titles.csv', 'r')
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')) 

result = col.insert_many(data)

# 새로운 데이터 딕셔너리를 만들고 컬렉션에 삽입하세요.
sweet = {
'show_id' : 95889578, 'type' : 'Movie', 'title' : 'Sweet Home', 'director' : 'Eungbok Lee', 
'cast' : 'Gang Song', 'country' : 'Korea', 'date_added' : '18-Dec-20', 'release_year' : '2020', 
'rating' : 'TV-MA', 'duration' : '497min', 'listed_in' : 'Thrillers', 
'description' : "A teenage boy shutting off the world and stuck in a room. Hyunsoo comes out of the world. Humans turned into monsters. You still have to live. I'm still a person. You have to fight with your neighbors."
}


# 데이터를 삽입한 결과를 저장하세요.
new_add_data = col.insert_one(sweet)

# 삽입된 데이터를 확인하기 위한 코드입니다. 수정하지 마세요!
print(col.find_one(new_add_data.inserted_id))
```



