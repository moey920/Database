# NoSQL이란?

MongoDB를 본격적으로 익히기 전에 NoSQL이라는 개념에 대해 먼저 알아보겠습니다. 기존에 여러분께서 알고 계시고 사용하셨던 대부분의 SQL은 NoSQL이 아닙니다. NoSQL은 **Not Only** SQL 혹은 Not SQL입니다. 하지만, NoSQL 뜻은 딱 한 가지 종류의 DB만을 뜻하지 않습니다. 예를 들어, 초콜릿과 Not 초콜릿을 생각해봅시다. 초콜릿의 종류는 한정적이지만 Not 초콜릿은 다양한 음식과 종류가 있습니다. SQL DB들은 MySQL, SQLite, PostgreSQL 등 DB마다 조금씩 다르겠지만, 결국에는 SQL이라는 카테고리 안에 있습니다. 반대로 NoSQL은 DB마다 서로 각기 다른 종류가 존재합니다.

NoSQL에는 크게 3가지 종류의 DB가 존재합니다.

1. Document DB **(MongoDB)**
Document형태로 데이터가 저장됩니다. 여기에서 나오는 **Document는 JSON, XML등을 칭하며** Column Oriented와 같이 스키마가 유동적입니다.

2. Key-Value DB **(DynamoDB, Redis)**
**Key와 Value**로 이루어진 간단한 데이터 구조입니다. 기본적인 형태로 value에 String, Integer, List, Hash 등의 가본 자료형이 해당됩니다. 이런 데이터 구조 속도는 빠르고, 분산저장 환경에 용이하여 **key에 대한 엑세스 속도는 빠르지만 검색에는 취약**하다는 특징이 있습니다.

3. Graph DB **(Neo4j)**
데이터를 **노드 간의 관계**로 표현한 DB입니다.

## MongoDB의 특징

- 고정된 테이블 스키마를 갖지 않습니다.
RDB와 다르게 테이블의 스키마가 유동적입니다. 데이터를 저장하는 column은 각기 다른 이름과 다른 데이터 타입을 갖는 것이 허용됩니다.

- 문서지향(Document-Oriented) 데이터베이스입니다.
MongoDB는 유연하며 확장성이 높은 문서 지향의 데이터베이스입니다. RDB에서 사용했던 스키마의 제약이 없고 자유로우면 BSON(Binary JSON)형태로 각 문서가 저장됩니다. 또한, 기존 RDB에서 지원하지 않았던 형태로도 저장이 가능합니다. RDB에서 사용했던 JOIN 기능이 필요 없이 이해하기 쉬운 형태로 데이터를 저장할 수 있습니다.
💡 결국, NoSQL이 SQL보다 좋고 나쁘고가 아니라 사용하는 용도에 따라 선택하면 됩니다.

MongoDB란?
MongoDB는 오픈소스 문서지향(Document Oriented) 크로스 플랫폼 데이터베이스입니다.

MongoDB는 유연하고 JSON과 유사한 문서로 데이터를 저장합니다. 따라서, 필드는 문서마다 다를 수 있으며 시간에 따라 데이터 구조를 변경할 수 있습니다. 또한, 컬렉션을 사용해 데이터를 하나로 묶습니다. (컬렉션(Collection)이란 용도가 같거나 유사한 문서들을 그룹으로 묶는 것 입니다.) 이러한 컬렉션은 기존의 SQL DB의 테이블처럼 동작합니다. 문서(Document)는 MongoDB내에 있는 하나의 실제데이터를 나타내는 표현입니다. 아래 그림을 보며 다시 글을 읽어봅시다.

MongoDB의 구조
Database, Collection, Document 3단 구조


1. 도큐먼트(Document)
도큐먼트는 RDB에서의 Row(튜플)과 동일한 개념입니다. 예를 들어 아래와 같은 JSON 형태의 Key-Value로 이루어진 데이터 구조를 하나의 도큐먼트로 생각하면 됩니다. 각 도큐먼트는 id를 가지고 있는데, 이 값은 유일합니다. (Primary Key와 동일한 개념) RDB처럼 스키마로 정해진 것이 없어, 아래 코드에서 id, name, age를 적고 phoneNum이나 email을 추가하여 도큐먼트를 생성해도 문제없이 동작합니다.

{
    "id": "aslkf87sdljk",
    "name": "Elice",
    "age": 12
}
2. 컬렉션(Collection)
컬렉션은 도큐먼트의 그룹 개념입니다. RDB로 생각한다면 테이블과 같은 개념입니다. 다만 도큐먼트와 같이 스키마를 가지고 있지 않는다는 특징이 있습니다.

3. 데이터베이스(Database)
데이터베이스는 컬렉션들의 물리적인 컨테이너이자 가장 상위 개념입니다. RDB로 생각한다면 Database와 동일합니다.

위 구조에서 도큐먼트는 JSON과 비슷한 BSON 구조로 되어있습니다.

BSON은 Binary JSON을 의미합니다. JSON의 일부로 MongoDB에서 데이터를 저장하기 위한 형식입니다. BSON은 Field와 Value를 가지고 있으며 Key값과 Value값을 가진 Java의 HashMap과 유사합니다.

{
    name: "Elice",
    age: 12,
    status: "InProgress"
    groups: ["Python", "Javascript"]
}
RDB	MongoDB
데이터베이스(Database)	데이터베이스(Database)
테이블(Table)	컬렉션(Collection)
튜플(Row)	도큐먼트(Document)
▶ MongoDB 공식 홈페이지 소개글
