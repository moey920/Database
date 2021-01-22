# Flask 연동하기

## 1번 실습

앞서 모델링한 넷플릭스 데이터베이스를 Flask와 연동하는 방법에 대해 알아봅시다.

먼저 flask 라이브러리를 이용하기 위해 다음을 import 해줍니다.
```
from flask import Flask, render_template
```

그리고 Flask 모듈이 실행될 수 있도록 아래 코드를 추가해줍니다.
```
app = Flask(__name__)
```

여기서 선언된 app 변수에 route()를 이용한 장식자(데코레이터)로 라우팅을 하면됩니다. 
> 장식자란 기존 메소드를 변경하지 않고 추가 기능을 덧붙일 수 있도록 해주는 메소드를 의미합니다.

Flask에서 html을 띄우기 위해서 아래와 같은 형태로 자주 사용됩니다.
```
@app.route("/")
def 메소드명():
    return render_template('출력할 html')
```

route()의 매개변수에는 메소드가 출력할 URL을 넘겨줍니다. 그리고 미리 만들어 놓은 템플릿을 render_template() 메소드를 이용해 렌더링합니다. 쉽게 말해 해당 메소드로 html을 반환하여 지정한 URL에서 화면을 출력하는 것입니다.

render_template()에서 사용할 템플릿은 templates 폴더에 미리 만들어 두었으니, 그대로 사용하면 됩니다.

먼저 메인이 되는 페이지를 반환하는 메소드를 작성해봅시다.

### 1번 지시사항

1. 장식자를 이용해 / URL에서 main.html을 출력하는 main()메소드를 만드세요.

### 1번 main.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Netflix Database</h1>
<a href="/">home</a> <a href="/list">list</a>

<h2>Add a netflix</h2>
<form action="/save" method="POST">
  <table>
    <tr><td>Show_id: </td><td><input name="show_id"></td></tr>
    <tr><td>Type: </td><td><input name="type"></td></tr>
    <tr><td>Title: </td><td><input name="title"></td></tr>
    <tr><td>Director: </td><td><input name="director"></td></tr>
    <tr><td>Cast: </td><td><input name="cast"></td></tr>
    <tr><td>Country: </td><td><input name="country"></td></tr>
    <tr><td>Date_added: </td><td><input name="date_added"></td></tr>
    <tr><td>Release_year: </td><td><input name="release_year"></td></tr>
    <tr><td>Rating: </td><td><input name="rating"></td></tr>
    <tr><td>Duration: </td><td><input name="duration"></td></tr>
    <tr><td>Listed_in: </td><td><input name="listed_in"></td></tr>
    <tr><td>Description: </td><td><input name="description"></td></tr>
  </table>
<input type="submit" value="Save">
</form>

<h2>List netflix by title</h2>
<form action="/get" method="POST">
Title: <input name="title">
<input type="submit" value="Get">
</form>

{% if error %}
<div style="color:red;">
  {{ error }}
</div>
{% endif %}

</body>
</html>
```

### 1번 main.py

```
import pymongo
import csv

# Flask를 연동합니다.
from flask import Flask, render_template

app = Flask(__name__)

# 데이터를 삽입하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

reader = open('netflix_titles.csv', 'r')
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')) 

result = col.insert_many(data)

# main.html을 반환하는 메소드를 작성하세요.
@app.route("/")
def main(): # 해당 메소드로 html을 반환하여 지정한 URL에서 화면을 출력하는 것입니다.
    return render_template('main.html') # 미리 만들어놓은 템플릿 호출하기
```

## 2번 실습

데이터 저장하기

이번에는 Flask를 이용해 구동한 화면에서 직접 데이터를 입력받고 저장하는 방법에 대해 알아보겠습니다.

앞의 실습과 마찬가지로 route()를 이용한 장식자 코드를 작성하되, 매개변수에 아래처럼 추가해주어야 합니다.
```
@app.route("경로", methods=['POST'])
```
해당 코드는 URL 라우팅을 POST 방식으로 한다는 의미입니다.

그리고 request 라이브러리의 form을 이용해 POST를 통해 전송된 데이터를 저장 확인할 수 있습니다.
```
data = {
    "show_id": request.form['show_id']
}
```
예를 들어, 위의 코드는 show_id에 입력된 데이터를 딕셔너리 형태로 저장하는 코드입니다. 전송되는 데이터의 이름은 main.html의 table에서 확인할 수 있으며, 필드명과 동일하게 설정되어 있습니다.

## 2번 지시사항

1. 장식자를 이용해 /save URL에서 POST 방식으로 main.html을 출력하는 save()메소드를 만드세요.
2. 전송된 데이터 12개('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')를 딕셔너리 형태로 저장하세요.
3. 저장한 딕셔너리를 titles 컬렉션에 삽입하세요.

## 2번 소스코드

```
import pymongo
import csv

# Flask를 연동합니다.
from flask import Flask, render_template, request

app = Flask(__name__)

# 데이터를 삽입하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

reader = open('netflix_titles.csv', 'r')
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')) 

result = col.insert_many(data)

# html 화면을 출력합니다.
@app.route("/") # 해당 코드는 URL 라우팅을 POST 방식으로 한다는 의미입니다.
def main():
    return render_template('main.html')

# 전송된 데이터를 저장하는 save()메소드를 만드세요.
@app.route("/save", methods=['POST']) # 해당 코드는 URL 라우팅을 POST 방식으로 한다는 의미입니다.
def save():
    # request 라이브러리의 form을 이용해 POST를 통해 전송된 데이터를 저장 확인할 수 있습니다.
    data = { # 전송된 데이터를 딕셔너리 형태로 저장하세요.
        "show_id": request.form['show_id'],
        "type": request.form['type'],
        "title": request.form['title'],
        "director": request.form['director'],
        "cast": request.form['cast'],
        "country": request.form['country'],
        "date_added": request.form['date_added'],
        "release_year": request.form['release_year'],
        "rating": request.form['rating'],
        "duration": request.form['duration'],
        "listed_in": request.form['listed_in'],
        "description": request.form['description'],
    }
    res = col.insert(data) # 저장한 딕셔너리를 titles 컬렉션에 삽입하세요.
    return render_template('main.html')
```

## 3번 실습

데이터 리스트 출력하기

titles 컬렉션에 저장된 데이터의 수가 몇 개이고 어떠한 데이터가 있는지 리스트를 확인해보겠습니다.

그러기 위해서 이번에는 장식자에서 GET 방식으로 URL 라우팅을 해야합니다.
```
@app.route("경로", methods=['GET'])
```
list.html에는 작품의 수와 정보를 출력하는 코드가 작성되어 있습니다. 따라서 render_template() 메소드로 list.html을 반환할 때 매개변수로 해당 정보를 같이 넘겨주면 됩니다.

```
def 메소드명():
    count = 데이터의 수
    title = 전체 데이터
    return render_template('list.html', count=count, title=title)
```

list.html에서 사용되는 count와 title이 어떻게 매개변수로 전달되어야 하는지 생각하며 코드를 작성해봅시다.

### 3번 지시사항

1. 장식자를 이용해 /list URL에서 GET 방식으로 list.html을 출력하는 list_title()메소드를 만드세요.
2. 전체 작품의 수와 전체 작품 정보를 render_template()의 매개변수에 전달하세요. 매개변수는 각각 count와 title입니다.

> Tips : render_template()의 매개변수로 전달되는 데이터 중 작품 정보는 컬렉션.find()을 전달해주면 됩니다. list.html에서는 모든 정보 중 title에 대한 데이터만 출력합니다.

### 3번 list.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Netflix Database</h1>
<a href="/">home</a> <a href="/list">list</a>

<h2>List Title</h2>
Total: {{ count }}

<ul>
{% for t in title %}
  <li><a href="/netflix/{{ t.show_id }}">{{ t.title }}</a></li>
{% endfor %}
</ul>

</body>
</html>
```

### 3번 main.py

```
import pymongo
import csv

# Flask를 연동합니다.
from flask import Flask, render_template, request

app = Flask(__name__)

# 데이터를 삽입하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

reader = open('netflix_titles.csv', 'r')
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')) 

result = col.insert_many(data)

# html 화면을 출력합니다.
@app.route("/")
def main():
    return render_template('main.html')

@app.route("/save", methods=['POST'])
def save():
    data = {
        "show_id": request.form['show_id'],
        "type": request.form['type'],
        "title": request.form['title'],
        "director": request.form['director'],
        "cast": request.form['cast'],
        "country": request.form['country'],
        "date_added": request.form['date_added'],
        "release_year": request.form['release_year'],
        "rating": request.form['rating'],
        "duration": request.form['duration'],
        "listed_in": request.form['listed_in'],
        "description": request.form['description'],
    }
    res = col.insert(data)
    return render_template('main.html')


# 데이터의 수와 목록을 확인하는 메소드를 작성하세요.
@app.route("/list", methods=['GET'])
def list_title():
    count = col.count_documents({})
    title = col.find({})
    return render_template('list.html', count=count, title=title)
```

## 4번 실습

넷플릭스 작품 정보 출력하기

전체 작품 리스트에서 하나의 작품을 클릭하면 해당 작품의 상세 정보를 출력하는 코드를 작성해봅시다.

**매 작품마다 새로운 URL 라우팅**이 되어야 하므로, 작품별 id인 show_id를 이용해봅시다. 이러한 동적 URL을 이용하려면 꺽쇠('<', '>,)를 이용해야 합니다.
```
@app.route("/url/<show_id>")
```

그리고 메소드의 매개변수로 show_id를 받아, 컬렉션에서 해당 데이터의 정보를 찾아 반환해봅시다. **데이터를 출력하는 화면은 netflix.html**에 구현이되어 있으므로 해당 템플릿을 반환하여 렌더링해주면 됩니다.

### 4번 지시사항
1. 장식자를 이용해 /netflix/<show_id> URL에서 GET 방식으로 netflix.html을 출력하는 netflix(show_id)메소드를 만드세요.
2. 메소드의 매개변수인 show_id를 이용해 해당 작품의 데이터를 검색하세요.
3. 검색한 데이터를 render_template()의 매개변수에 전달하세요.

### 4번 netflix.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>Netflix Database</h1>
<a href="/">home</a> <a href="/list">list</a>

<h2>{{ netflix.title }}</h2>

Show_id: {{ netflix.show_id }}<br>
Type: {{ netflix.type }}<br>
Title: {{ netflix.title }}<br>
Director: {{ netflix.director }}<br>
Cast: {{ netflix.cast }}<br>
Country: {{ netflix.country }}<br>
Date_added: {{ netflix.date_added }}<br>
Release_year: {{ netflix.release_year }}<br>
Rating: {{ netflix.rating }}<br>
Duration: {{ netflix.descridurationption }}<br>
Listed_in: {{ netflix.listed_in }}<br>
Description: {{ netflix.description }}<br>

</body>
</html>
```

### 4번 main.py

```
import pymongo
import csv

# Flask를 연동합니다.
from flask import Flask, render_template, request

app = Flask(__name__)

# 데이터를 삽입하는 코드입니다. 수정하지 마세요!
client = pymongo.MongoClient('localhost', 27017)

db = client["netflix"]
col = db["titles"]

reader = open('netflix_titles.csv', 'r')
data = csv.DictReader(reader, ('show_id', 'type', 'title', 'director', 'cast', 'country', 'date_added', 'release_year', 'rating', 'duration', 'listed_in', 'description')) 

result = col.insert_many(data)

# html 화면을 출력합니다.
@app.route("/")
def main():
    return render_template('main.html')

@app.route("/save", methods=['POST'])
def save():
    data = {
        "show_id": request.form['show_id'],
        "type": request.form['type'],
        "title": request.form['title'],
        "director": request.form['director'],
        "cast": request.form['cast'],
        "country": request.form['country'],
        "date_added": request.form['date_added'],
        "release_year": request.form['release_year'],
        "rating": request.form['rating'],
        "duration": request.form['duration'],
        "listed_in": request.form['listed_in'],
        "description": request.form['description'],
    }
    res = col.insert(data)
    return render_template('main.html')

@app.route("/list", methods=['GET'])
def list_title():
    count = col.count_documents({})
    title = col.find({})
    return render_template('list.html', count=count, title=title)


# 특정 작품의 상세 정보를 출력하는 메소드를 만드세요.
@app.route("/netflix/<show_id>", methods=['GET'])
def netflix(show_id):
    netflix = col.find_one({ 'show_id': show_id })
    return render_template('netflix.html', netflix=netflix)
```

## 

