---
title: "[web] python flask-02:data 전송"
last_modified_at: 2023-01-06T11:50:00
categories:
  - python web 
tags:
  - python
  - web
  - flask
---
Pytorch model을 웹에 서빙하기 위한 web 공부

server와 client 간의 데이터 이동

### Server to client
python 파일에서 html을 띄우는 renter_template에 argument를 추가하여 보내면 됩니다.

```python
# app.py
from flask import Flask, render_template, request

#Flask 객체 인스턴스 생성
app = Flask(__name__)

@app.route('/',methods=('GET', 'POST')) # 접속하는 url
def index():
    if request.method == "POST":
        # user=request.form['user'] # 전달받은 name이 user인 데이터
        print(request.form.get('user')) # 안전하게 가져오려면 get
        user = request.form.get('user')
        data = {'level': 60, 'point': 360, 'exp': 45000}
        return render_template('index.html', user=user, data=data)
    elif request.method == "GET":
        user = "지용"
        data = {'level': 60, 'point': 360, 'exp': 45000}
        return render_template('index.html', user=user, data=data)

if __name__=="__main__":
  app.run(debug=True)
  # host 등을 직접 지정하고 싶다면
  # app.run(host="127.0.0.1", port="5000", debug=True)
```
- client가 server에 data를 보내는 방식에 따라 get방식과 post방식으로 나뉠 수 있습니다. 
- client의 요청 데이터는 request를 통해 전송되기 때문에 request를 참조하여야 합니다.

### client data처리

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}">
</head>
<body>
    <h1>대문 페이지</h1>
    이름 : {{user}}
    레벨 : {{data.level}}
    포인트 : {{data.point}}
    경험치 : {{data.exp}}

   <form action="/" method="post">
        다음 이름 : <input type="text" name="user">
        <input type="submit" value="전송하기">
   </form>
</body>
</html>
```
- flask는 jinja2 템플릿 엔진을 사용합니다.
- 변수를 가져오는 템플릿은 {{변수명}} 입니다.
- key로 value를 가져올 땐 {{변수명.key}}입니다.
client에서 server로 데이터 전송 시 `name`은 server에서 사용할 변수명 또는 키 값이 됩니다.


#### get, post 차이
`get` 방식은 캐시가 가능하고, 브라우저 히스토리에 남습니다다. get 요청에는 길이 제한이 있기에, 보통 데이터를 요청할때만 사용된다.
`post` 방식은 캐시되지 않고, 브라우저 히스토리에 남지 않습니다. post 요청에는 길이 제한도 없기에 보안이 필요한 전송에서는 post 방식을 사용해야 합니다.

### Server 구동

File directory는 다음과 같습니다.

<img src="https://velog.velcdn.com/images/jyong0719/post/17fe9359-aaba-4544-a683-8cb6fa9757d5/image.png" width="20%">

#### style.css
<img src="https://velog.velcdn.com/images/jyong0719/post/6ea1f520-0971-4729-8f66-29da67ec0134/image.png" width="35%">

#### index.html
<img src="https://velog.velcdn.com/images/jyong0719/post/91ca0cad-55e0-48a0-8520-77331cf41f8e/image.png" width="70%">

#### app.py
<img src="https://velog.velcdn.com/images/jyong0719/post/be7b8747-ba87-4b1a-b0e9-882d0fac9def/image.png" width="70%">

### python 실행
<img src="https://velog.velcdn.com/images/jyong0719/post/75511245-dd2a-4936-b1de-92b4a9484808/image.png" width="70%">

#### 실행 화면
<img src="https://velog.velcdn.com/images/jyong0719/post/ca6b8409-cefd-4553-bc3b-8f2f4414eed0/image.png" width="70%">

- 새로운 이름 전송하기

<img src="https://velog.velcdn.com/images/jyong0719/post/0bd2a8c1-7f94-4419-93dd-db74b8865f21/image.png" width="70%">

- 결과

<img src="https://velog.velcdn.com/images/jyong0719/post/f3c21687-0cf8-4d89-bb96-9aa6e9b41f5d/image.png" width="70%">
