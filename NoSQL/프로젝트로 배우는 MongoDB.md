# 여러가지 연산자 알아보기

## 비교 쿼리 연산자

앞서 설명한 find()연산자는 아무런 조건을 입력하지 않을 경우 전체 데이터를 조회하는 방법을 학습했습니다. 특정 조건에 해당하는 도큐먼트를 조회하고 싶을 경우에는 조건을 JSON 형태로 입력합니다.

RDB처럼 다양한 비교 연산자를 활용하여 조회할 수도 있습니다. 다만 연산자를 기호 형태로 직접 입력하는 방법이 아닌 대체 문자열을 이용하여 비교문을 작성해야 합니다.

| 기호 | 대체 문자열 | 의미 |
|:---:|:---:|:---:|
| = | $eq | 같은 (equal) |
| != | $ne | 같지 않은 (not equal) |
| < | $lt | 미만 (less than) |
| <= | $lte | 이하 (less than equal) |
| > | $gt | 초과 (greater than) |
| >= | $gte | 이상 (greater than equal) |
|  | $in | 필드의 값이 $in안에 들어있는 값들 중 하나인 필드를 찾는 연산자 |
|  | $nin | 필드의 값이 $nin 안에 값들이 아닌 필드를 찾습니다 |



### $gt연산자
해당 값보다 더 큰 값을 가진 필드를 찾는 연산자입니다. 숫자에 국한되지 않고 날짜와 ObjectId도 비교 가능합니다.
```
#문법
{필드 명: {$gt: value} }
```

```
#예시) 주소가 문자 "S" 이후로 시작하는 도큐먼트 조회
query = { "address": { "$gt": "S" } }

mydoc = mycol.find(query)
```

### $gte연산자
해당 값보다 크거나 같은 값을 가진 필드를 찾는 연산자입니다.
```
#문법
{필드 명: {$gte: value} }
```

```
#예시) 키가 160이상인 사람을 조회
query = { "height": { "$gte": 160 } }

mydoc = mycol.find(query)
```

### $in연산자
필드의 값이 $in안에 들어있는 값들 중 하나인 필드를 찾는 연산자입니다. 예를 들어 value1, value2, value3 등 이 있는데 이 필드의 값이 그 중 하나면 반환하는 것 입니다.
```
#문법
{ 필드 명: { $in: [<value1>, <value2>, <value3>, ... <valueN> ] } }
```

```
#예시) address가 Seoul 342, Incheon 125, Busan 876 중 하나인 값을 조회
query = { "address": { "$in": ["Seoul 342", "Incheon 125", "Busan 876"] } }

mydoc = mycol.find(query)
```

### $lt연산자
해당 값보다 작은 값을 가진 필드를 찾는 연산자입니다.
```
#문법
{필드 명: {$lt: value} }
```

```
#예시) 20살 미만인 사람을 조회
query = { "age": { "$lt": 20 } }

mydoc = mycol.find(query)
```

### $lte연산자
해당 값보다 작거나 같은 값을 가진 필드를 찾는 연산자입니다.
```
#문법
{ 필드 명: { $lte: value} }
```

```
#예시) 19살 이하인 사람을 조회
query = { "age": { "$lte": 19 } }

mydoc = mycol.find(query)
```

### $ne연산자
해당 값과 일치하지 않는 값을 가진 필드를 찾는 연산자입니다.

```
#문법
{필드 명: {$ne: value} }
```

```
#예시) 키가 160이 아닌 사람 조회
query = { "height": { "$ne": 160 } }

mydoc = mycol.find(query)
```

### $nin연산자
필드의 값이 $nin 안에 값들이 아닌 필드를 찾습니다. 예를 들어 value1, value2, value3 등이 있는데 필드의 값이 그 값들이 아니어야 합니다.
```
#문법
{ 필드 명: { $nin: [ <value1>, <value2>, <value3> ... <valueN> ]} }
```

```
#예시) 아래 주소가 아닌 필드 조회
query = { "address": { "$nin": ["Seoul 342", "Incheon 125", "Busan 876"] } }

mydoc = mycol.find(query)
```

## 논리 연산자

| 문자 | 의미 |
|:---:|:---:|
| $and | 2개 이상 쿼리의 조건이 일치하는 모든 도큐먼트를 반환합니다. |
| $not | 쿼리 조건과 일치하지 않는 도큐먼트를 반환합니다. |
| $nor | 2개 이상 쿼리의 조건 중 하나라도 일치하지 않는 모든 도큐먼트를 반환합니다. |
| $or | 2개 이상 쿼리의 조건 중 하나라도 일치하는 모든 도큐먼트를 반환합니다. |
	
	
## $and
한 개 이상의 조건에 모두 만족할 때 조회됩니다. 예를 들어 조건1은 만족하지만, 조건2는 만족하지 않는다면 조회되지 않습니다.
```
#문법
{ $and: [ { 조건1 }, { 조건2 } , ... , { 조건N } ] }
```

```
#예시) 키가 160 이상이고, 주소가 "S"로 이후로 시작하는 값 조회
query = { "$and": [ { "height": { "$gte": 160 } }, { "address": { "$gt": "S" } } ] }

mydoc = mycol.find(query)
```

## $not
다른 연산자나 정규 표현식으로부터 얻은 결과의 여집합을 조회합니다.
```
#문법
{ 필드 명: { $not: { 조건 } } }
```

```
#예시) 이름이 A로 시작하지 않는 모든 user의 도큐먼트를 찾는다.
query = { "first_name": { "$not": /^A/ } }

mydoc = mycol.find(query)
```

## $nor
여러 개의 조건을 모두 만족하지 않는 도큐먼트를 조회합니다. 조건1, 조건2 등 모두 만족하지 않아야 합니다.
```
#문법
{ $nor: [{ 조건1 }, { 조건2 }, ...] }
```

```
#예시) 나이가 19살 이하고, 키가 155 이상이 아닌 값 조회
query = { $nor: [ {"age": { "$lte": 19 } }, { "height": { "$gte": 155 } } ] }

mydoc = mycol.find(query)
```

## $or
여러 개의 조건 중에서 적어도 하나의 조건을 만족하는 도큐먼트를 조회합니다. 예를 들어 조건1만 만족해도 조회가 됩니다.
```
#문법
{ $or: [{ 조건1 }, { 조건2 }, ...] }
```

```
#예시) 주소가 Seoul이고, 나이가 20이 넘는 값을 조회
query = { "$or": [ { "address": "Seoul" }, { "age": { "$gt": 20 } } ] }

mydoc = mycol.find(query)
```

> Tip! : 💡 ＄or vs ＄in : 같은 필드의 값에 대해 조건을 주어 조회를 한다면 ＄or < ＄in
```
#예시
account.find( { "qty": { "$in": [ 5, 15 ] } } )
```

> 다른 필드의 값에 대해 조건을 주어 조회를 한다면 ＄or > ＄in
```
#예시
account.find( { "$or": [ { "quantity": { "$lt": 20 } }, { "price": 10 } ] } )
```

### 논리 쿼리 연산자 예제
```
import pymongo

# 데이터베이스와 Collection을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["library"]
col = db["books"]

# books Collection에 들어있는 책들을 출력하세요.
for find_book in col.find({"$or":[ {"author":"Antoine de Saint-Exupery"}, {"author":"Ernest Miller Hemingway"}]}):
    print(find_book["title"])

for find_book in col.find({"$and":[ {"author":"Joanne Kathleen Rowling"}, {"date_received":"2017-07-21"}]}):
    print(find_book["title"])
```

## 요소 연산자

### $exists 연산자
해당 필드가 존재해야 하는지 존재하지 않아야 하는지 여부를 결정합니다. 이 연산자는 **필드의 값이 없는 경우 값을 추가하지 위해서 주로 사용**이 됩니다.

#문법
{ 필드 명: { $exists: <boolean> } }
#예시) age 필드의 값이 없는 경우를 조회
query = { "age": { "$exists": false } }
$type 연산자
해당 필드의 자료형이 일치하는 도큐먼트를 선택합니다.

선택 가능한 자료형
double, string, object, array, binData, objectId, bool, date, null, regex, dbPointer, javascript, symbol, javascriptWithScope, int, timestamp, long, minKey, maxKey

#문법
{ 필드 명: { $type: <BSON type> } }
#예시) zipcode 필드의 타입이 string인 모든 도큐먼트를 조회

#예시를 위한 가상의 데이터
[
      { "_id" : 1, address : "2030 Martian Way", zipCode : "90698345" },
      { "_id" : 2, address: "156 Lunar Place", zipCode : 43339374 },
      { "_id" : 3, address : "2324 Pluto Place", zipCode: NumberLong(3921412) },
      { "_id" : 4, address : "55 Saturn Ring" , zipCode : NumberInt(88602117) },
      { "_id" : 5, address : "104 Venus Drive", zipCode : ["834847278", "1893289032"]}
   ]

# 쿼리문
query = { "zipCode" : { "$type" : "string" } }

mydoc = mycol.find(query)

#결과
{ "_id" : 1, address : "2030 Martian Way", zipCode : "90698345" },
{ "_id" : 5, address : "104 Venus Drive", zipCode : ["834847278", "1893289032"]}
평가 연산자
$mod연산자
나머지를 구하는 연산자입니다. 예를 들어, 번호가 짝수인 사람을 모두 찾는 경우에 사용이 됩니다.

#문법
{ 필드 명: { $mod: [ divisor(나눌값), remainder(나머지) ] } }
#예시) _id가 짝수인 값을 모두 조회
query = { "_id": { "$mod": [2, 0] } }

mydoc = mycol.find(query)
$regex연산자
정규표현식 조회를 가능하게 합니다. 아래 3가지 유형 중 하나로 사용하실 수 있습니다. ＄regex만 사용하셔도 되고, $options를 함께 사용하셔도 되고, 정규표현식(MongoDB는 UTF-8을 지원하는 Perl 호환 정규식 버전 8.42 를 사용합니다.)처럼 사용하셔도 됩니다.

#문법
1번째 유형: { 필드 명: { $regex: /pattern/, $options: '<options>' } }
2번째 유형: { 필드 명: { $regex: 'pattern', $options: '<options>' } }
3번째 유형: { 필드 명: { $regex: /pattern/<options> } }
#예시) sku 필드의 789가 있는 데이터를 조회

#예시를 위한 가상의 데이터
[
    { "_id" : 100, "sku" : "abc123", "description" : "Single line description." }
    { "_id" : 101, "sku" : "abc789", "description" : "First line\nSecond line" }
    { "_id" : 102, "sku" : "xyz456", "description" : "Many spaces before     line" }
    { "_id" : 103, "sku" : "xyz789", "description" : "Multiple\nline description" }
]

#쿼리문
query = { sku: { $regex: /789$/ } }

mydoc = mycol.find(query)

#결과
{ "_id" : 101, "sku" : "abc789", "description" : "First line\nSecond line" }
{ "_id" : 103, "sku" : "xyz789", "description" : "Multiple\nline description" }
$text연산자
텍스트 조회를 하는 연산자입니다. 이 연산자를 사용할 때는 제약사항이 많다는 점을 유의해야 합니다. 대표적으로 필드에 text index가 설정 되어 있어야 합니다. 이 연산자의 유용한 점은 꼭 정확한 텍스트가 아니더라도 유사한 텍스트를 찾아준다는 장점이 있습니다.

#문법
{
  $text:
    {
      $search(문자): <string>,
      $language(언어): <string>,
      $caseSensitive(대소문자): <boolean>
    }
}
#예시) subject 필드의 값에 "coffee"라는 단어가 포함되어있는 데이터를 조회

#예시를 위한 가상의 데이터
[
 { _id: 1, subject: "coffee", author: "xyz", views: 50 },
 { _id: 2, subject: "Coffee Shopping", author: "efg", views: 5 },
 { _id: 3, subject: "Baking a cake", author: "abc", views: 90  },
 { _id: 4, subject: "baking", author: "xyz", views: 100 },
 { _id: 5, subject: "Café Con Leche", author: "abc", views: 200 },
 { _id: 6, subject: "Сырники", author: "jkl", views: 80 },
 { _id: 7, subject: "coffee and cream", author: "efg", views: 10 },
 { _id: 8, subject: "Cafe con Leche", author: "xyz", views: 10 }
]

#쿼리문
query = { $text: { $search: "coffee" } }

mydoc = mycol.find(query)

#결과
{ "_id" : 2, "subject" : "Coffee Shopping", "author" : "efg", "views" : 5 }
{ "_id" : 7, "subject" : "coffee and cream", "author" : "efg", "views" : 10 }
{ "_id" : 1, "subject" : "coffee", "author" : "xyz", "views" : 50 }

# 여러가지 메소드 활용하기

# 넷플릭스 데이터 모델링

# Flask & MongoDB 연동하기
