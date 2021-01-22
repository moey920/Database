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
