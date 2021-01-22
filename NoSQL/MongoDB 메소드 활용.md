# 여러가지 메소드 활용하기

## Find 메소드 활용

find() 메소드를 이용하여 데이터를 불러온다면 조건과 일치하는 모든 도큐먼트들을 출력해주기 때문에 내가 원하는 개수를 제한해서 조회하거나, 순서대로 나열할 수는 없습니다.

이번 이론에서는 ```sort(), limit(), skip()``` 메소드에 대해 학습해보겠습니다. 학습하기 위한 샘플 데이터는 아래 코드를 참고해주세요.
```
#예시) 메소드 학습을 위한 샘플 데이터입니다.
[
    { "_id": 1, "item": { "category": "cake", "type": "chiffon" }, "amount": 10 },
    { "_id": 2, "item": { "category": "cookies", "type": "chocolate chip" }, "amount": 50 },
    { "_id": 3, "item": { "category": "cookies", "type": "chocolate chip" }, "amount": 15 },
    { "_id": 4, "item": { "category": "cake", "type": "lemon" }, "amount": 30 },
    { "_id": 5, "item": { "category": "cake", "type": "carrot" }, "amount": 20 },
    { "_id": 6, "item": { "category": "brownies", "type": "blondie" }, "amount": 10 }
]
```
### sort() 메소드
sort()메소드는 데이터를 정렬할 때 사용하는 메소드입니다.
> sort() 메소드는 Cursor객체에서 사용되며, 컬렉션.find()를 통해 반환되는 것이 Cursor객체입니다.
```
#문법
cursor.sort("KEY", value)
```

```
#예시
orders.find().sort("item.category",1)

#결과: category은 알파벳 순서로 정렬되지만 중복 값을 포함하는 도큐먼트의 순서는 동일하지 않습니다.
{ "_id" : 6, "item" : { "category" : "brownies", "type" : "blondie" }, "amount" : 10 }
{ "_id" : 1, "item" : { "category" : "cake", "type" : "chiffon" }, "amount" : 10 }
{ "_id" : 5, "item" : { "category" : "cake", "type" : "carrot" }, "amount" : 20 }
{ "_id" : 4, "item" : { "category" : "cake", "type" : "lemon" }, "amount" : 30 }
{ "_id" : 2, "item" : { "category" : "cookies", "type" : "chocolate chip" }, "amount" : 50 }
{ "_id" : 3, "item" : { "category" : "cookies", "type" : "chocolate chip" }, "amount" : 15 }

#안정적인 정렬 결과를 얻으려면 고유한 값만 포함하는 필드를 추가하여 조회합니다.
orders.find().sort([("item.category",1), ("_id",1)])
```

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# item.category를 기준으로 오름차순 정렬된 데이터를 모두 출력하세요.
for i in orders.find().sort( "item.category", 1 ) :
    print(i)
```

KEY는 필드명이고, value는 1이나 -1입니다.

- 1일 경우: 오름차순
- -1일 경우: 내림차순
또한, **여러 KEY값을 입력할 수 있고 먼저 입력한 순서대로 우선 순위**를 갖게됩니다.

### limit() 메소드
limit()메소드는 출력할 데이터 갯수를 제한하고자 할 때 사용되는 메소드입니다. value값에 출력할 갯수 값을 입력합니다.
```
#문법
cursor.limit(value)
```

```
#예시: 출력 할 갯수 2개로 제한하기(기본적인 우선정렬 순서대로 잘려져서 조회됩니다.)
{ "_id" : 1, "item" : { "category" : "cake", "type" : "chiffon" }, "amount" : 10 }
{ "_id" : 2, "item" : { "category" : "cookies", "type" : "chocolate chip" }, "amount" : 50 }
```

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# 출력 할 갯수를 3개로 제한하여 출력하세요.

for i in orders.find().limit(3) :
    print(i)
```

### skip() 메소드
skip()메소드는 조회할 데이터의 시작부분을 설정하고자 할 때 사용되는 메소드입니다. value 값 갯수의 데이터를 생략하고 그 다음 데이터가 조회됩니다.
```
#문법
cursor.skip(value)
```

skip()메소드를 사용하는 경우 **결과값이 조회되기 전에 기본적인 정렬 후, skip()메소드가 적용되는 점**을 유의해주세요.

```
#예시: 3개의 데이터를 생략하고 조회
{ "_id" : 4, "item" : { "category" : "cake", "type" : "lemon" }, "amount" : 30 }
{ "_id" : 5, "item" : { "category" : "cake", "type" : "carrot" }, "amount" : 20 }
{ "_id" : 6, "item" : { "category" : "brownies", "type" : "blondie" }, "amount" : 10 }
```

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# 3개의 데이터를 생략하고 출력하세요.
for i in orders.find().skip(3) :
    print(i)
```

> 💡Tip! 우리가 학습할 메소드는 cursor 객체이기 때문에 중첩하여 사용할 수 있습니다.


## Update 메소드 활용
앞서 update()메소드를 사용히여 데이터를 수정하는 방법을 학습했습니다. 이번 이론에서는 update를 다양하게 하는 방법에 대해서 학습해보도록 하겠습니다.
```
#예시) 메소드 학습을 위한 샘플 데이터입니다.
[
    { "_id": 1, "item": { "category": "cake", "type": "chiffon" }, "amount": 10 },
    { "_id": 2, "item": { "category": "cookies", "type": "chocolate chip" }, "amount": 50 },
    { "_id": 3, "item": { "category": "cookies", "type": "chocolate chip" }, "amount": 15 },
    { "_id": 4, "item": { "category": "cake", "type": "lemon" }, "amount": 30 },
    { "_id": 5, "item": { "category": "cake", "type": "carrot" }, "amount": 20 },
    { "_id": 6, "item": { "category": "brownies", "type": "blondie" }, "amount": 10, "nickName": "brown", "taste" : ["sweet", "creamy" ] }
]
```

### $set 연산자
$set을 이용하면 기존에 있는 필드 값을 수정하는 것 뿐 아니라, 도큐먼트에 새로운 필드를 추가할 수도 있습니다.
```
#문법
{  $ set :  {  < field1 > :  < value1 > ,  ...  }  }
```

```
#예시: item.category 필드 값이 brownies인 도큐먼트의 amount값을 20으로 수정
orders.update_one( { "item.category":"brownies"}, { "$set" : { "amount" : 20 } } )
```

### $unset 연산자
$unset연산자를 사용하면 특정 필드를 제거할 수 있습니다. 이 연산자는 **객체 타입**으로, **삭제할 필드와 value에 1이나 true을 삽입**합니다. (1의 의미: true)
```
#문법
{  $ unset :  {  < field1 > :  "" ,  ...  }  }

```
```
#예시: item.category 필드 값이 brownies인 도큐먼트의 nickName 필드를 제거
orders.update_one( { "item.category" : "brownies" }, { "$unset" : { "nickName": 1 } } )
```
* 도큐먼트 자체가 삭제되는 것이 아님에 유의하자

```
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# brownies의 nickName 필드를 제거하세요.
orders.update_one( { "item.category" : "brownies" }, { "$unset" : { "nickName": 1 } } )

for x in orders.find():
    print(x)
```

### $push 연산자
이 연산자는 단어 그대로 push! 무언가를 밀어 넣습니다. 이 연산자는 **새로운 데이터를 기존 데이터에 추가**할 수 있는 연산자입니다.

하지만, 이 연산자에서 조심해야 할 점이 있습니다. **추가하는 값이 배열일 경우 한 번에 push**를 합니다. 예를 들어 [0,1] 이라는 배열에 [2,3,4]를 push할 경우 [0,1[2,3,4]]가 됩니다. **따로 push를 하고 싶다면 $each 연산자**를 이용해야 합니다.
```
#문법
{  $ push :  {  < field1 > :  < value1 > ,  ...  }  }
```

```
#each연산 사용 시
{  $push : { <field1> : { $each : 배열 } } }
```

```
#예시: item.category의 필드 값이 brownies인 도큐먼트의 taste 필드의 salty를 추가
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# brownies의 taste 필드를 추가하세요.
orders.update_one(
    { "item.category" : "brownies" },
    { "$push" : { "taste" : "salty" } }
)

for x in orders.find():
    print(x)
```

*  마치 python의 list.append()와 같이 새로운 데이터를 기존 데이터에 추가할 수 있다.

### $pull 연산자(push와 반대)
$pull은 기존의 **필드 배열로부터 제거하는 연산자**입니다.
```
#문법
{ $pull: { <field1>: <value|condition>, <field2>: <value|condition>, ... } }
```

```
#예시: item.category의 필드 값이 brownies인 도큐먼트의 taste 필드의 creamy를 제거
import pymongo


# 데이터베이스와 컬렉션을 생성하는 코드입니다. 수정하지 마세요!
connection = pymongo.MongoClient("mongodb://localhost:27017/")
db = connection["cafe"]
orders = db["menu"]

# brownies의 taste 필드의 값을 제거하세요.
orders.update_one(
    { "item.category" : "brownies" },
    { "$pull" : { "taste" : "sweet" } }
)

for x in orders.find():
    print(x)
```

* $pull 연산자는 기존의 데이터를 제거하는 연산자이다.

지금까지 배운 메소드들을 이용한다면 MongoDB의 데이터를 update할 때 부족함은 없을 것입니다. 만약 여기에 해당되지 않는 핸들링이 필요하시다면 아래 메뉴얼을 참고하시길 바랍니다.
<https://docs.mongodb.com/v3.2/reference/operator/update/>


## $lookup 연산자
MongoDB는 SQL이 아닌 NoSQL이기 때문에 **조인이라는 기능이 없습니다.** 하지만, 쿼리를 작성하다보면 조인이 필요한 경우가 있습니다. 이럴 때 우리는 ```$lookup``` 이라는 연산자를 활용하여 조인과 동일하게 컬렉션을 합칠 수 있습니다.
```
#문법
{
   $lookup:
     {
       from: <조인 할 컬렉션>,
       localField: <입력할 도큐먼트의 필드>,
       foreignField: <'from' 컬렉션의 도큐먼트에 있는 필드>,
       as: <출력할 배열 필드>
     }
}
```

| 필드 | 의미 |
|:---:|:---:|
| from | 동일한 데이터베이스 내 수행할 컬렉션을 지정합니다. |
| localField | 도큐먼트로부터 $lookup에 입력할 필드를 지정합니다. |
| foreignField | **from 컬렉션에 있는 도큐먼트에서 필드를 지정**합니다. |
| as | 입력 도큐먼트에 **추가될 새 배열 필드**를 지정합니다. |
	
### SQL과 비교하기
```
#MongoDB
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

```
#SQL
SELECT *, <output array field>
FROM collection
WHERE <output array field> IN (SELECT *
                               FROM <collection to join>
                               WHERE <foreignField>= <collection.localField>);
```

### 예시
아래와 같이 orders 컬렉션과 inventory 컬렉션이 있다고 가정합니다. orders 컬렉션 내에 inventory 컬렉션을 넣어 Embedded Document 구조로 컬렉션을 만들어 보겠습니다.
```
db.orders.insert( [
    { "_id": 1, "item": "cake", "price": 10, "quality": 3 },
    { "_id": 2, "item": "cookies", "price": 5, "quality" : 2 },
    { "_id": 3  }
]);

db.inventory.insert([
    {"_id": 1, "store": "cake", "description": 1, "sweet": 10},
    {"_id": 2, "store": "chocolate", "description": 2, "sweet": 15},
    {"_id": 3, "store": "candy", "description": 3, "sweet": 13},
    {"_id": 4, "store": "cookies", "description": 4, "sweet": 7},
    {"_id": 5, "store": "sandwitch", "description": null},
    {"_id": 6 }
])
```
먼저, 두 컬렉션을 합쳐 새로운 컬렉션 하나를 생성합니다.
```
db.orders.aggregate([
 {
$lookup:
        {
          from: "inventory",
          localField: "item",
          foreignField: "store",
          as: "inventory_docs"
        }
   },
   { $out : "newcol1" }
 ])
```
localField와 foreignField는 조인할 키 값이라고 생각하고, as는 새로 생성되는 필드명입니다. 이렇게 쿼리를 작성하게 되면 $out 메서드에 영향을 받아 newcol1 이라는 컬렉션이 생성됩니다. 그리고, 출력된 결과가 아래와 같이 입력 됩니다.
```
#결과값
{ "_id" : 1, "item" : "cake", "price" : 10, "quantity" : 3, "inventory_docs" : [ { "_id" : 1, "store" : "cake", "description" : 1, "sweet" : 10 } ] }

{ "_id" : 2, "item": "cookies", "price": 5, "quality" : 2, "inventory_docs" : [ {"_id": 4, "store": "cookies", "description": 4, "sweet": 7} ] }

{ "_id" : 3, "inventory_docs" : [ {"_id": 5, "store": "sandwitch", "description": null},
    {"_id": 6 } ] }
```

