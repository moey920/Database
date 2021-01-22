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

# 여러가지 메소드 활용하기

# 넷플릭스 데이터 모델링

# Flask & MongoDB 연동하기
