# Flask

수강 날짜: 2022/04/25

### Django vs Flask

구조화 되고, 여러 명의 팀원들과 개발해야 하는 상황 → Django

간단하고, 가볍게, 빨리 웹 서버를 구성 해야 하는 경우 → Flask

1. select inter ... > conda base 선택

![스크린샷 2022-04-25 오전 9.19.24.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.19.24.png)

1. flask 웹서버 생성 

```python
from flask import *

app = Flask(__name__) # __name__: 실행될 함수, 모듈 ... (실행환경) __main__ 이 들어가 있다.

@app.route("/") #데코레이터 
def home():
    return "Hello Flask."

# "__main__" : 최초 실행된 위치(프로그램 시작 위치)
if __name__ == "__main__":
    app.run(debug=True)
```

1. 터미널에서 플라스크 실행 후 웹서버 확인

```python
flask run
```

![스크린샷 2022-04-25 오전 9.52.30.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.52.30.png)

4. 필요한 파일 만들기

![스크린샷 2022-04-25 오전 9.37.55.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.37.55.png)

- FLASK > templates 폴더 만들기
- templates > index.html 파일 만들기

1. 홈 화면 설정하기

```python
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello Flask.</title>
</head>
<body>
   <h1>{{name}}님 반갑습니다</h1> #머스트식 표현법
</body>
</html>
```

1. user페이지 생성 후 확인 (/path parameter/ 생성)

```python

@app.route("/user/<name>")
def user(name): #name을 path parameter로 (위의 매개변수와 name과 일치해야 한다.)
    
    first_char = name[0]
    last_char = name[-1]
    stars = "*" * (len(name)-2)
    name_blind =  f"{first_char}{stars}{last_char}"
    
    # attribs = {'name' : name_blind}
    
    # rander_template : template 폴더를 탐색해서 index.html을 찾아낸다.
    return render_template("index.html", name=name_blind)

# "__main__" : 최초 실행된 위치(프로그램 시작 위치)
if __name__ == "__main__":
    app.run(debug=True)
```

![스크린샷 2022-04-25 오전 9.52.27.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_9.52.27.png)

1. 데이터 받아 화면에 뿌려주기(동기통신)

![스크린샷 2022-04-25 오전 10.07.30.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.07.30.png)

1. 동기통신 가능한 버튼 만들기 (동기통신 : 처리가 다 될때까지 대기하고 있어야 한다)
    - 빨간색 : 동기통신 (빨간색 루틴의 페이지가 새로고침됨)
    - 파란색 : 비동기통신 (페이지의 변화 없이 받아온 내용을 안쪽에 뿌려줌) (데이터만 받아오고 페이지는 변하지 않음)
    
    ![스크린샷 2022-04-25 오전 10.18.13.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.18.13.png)
    
    - 기본적인 웹 통신 방식
    - 빨간 화살표 통신을 한번 돌면 연결이 끊어진다
    
    1. 이벤트 생성 (이벤트(~했을때) : 이벤트가 데이터가 되기도 한다.)
    
    ```python
    # jquery (자바스크립트 코드)
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js"></script>
    ```
    
    - 스크립트의 위치
        - 스크립트를 먼저 실행시키는 순서대로 위→아래
    
    ```python
    # $ 셀렉터
    # (파라미터){코드컨텍스트}
    
    #구버전
    <script>
           $("#load_data").on("click", funcion(){})
    
       </script>
    
    #신버젼
    <script>
           $("#load_data").on("click", ()=>{
               alert('hello');
           });
    
       </script>
    ```
    

![스크린샷 2022-04-25 오전 10.30.34.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.30.34.png)

1. 서버의 응답 받기

```python
1.
$.getJSON("/api/data", (response)=>{}) 
//api 에 대한요청에 대한 결과를 함수로 받아온다 //response로 받는다

2.
$.getJSON("/api/data", (response)=>{
               console.log(response)
           })
//print와 같은 기능 consloe.log(response)
```

![스크린샷 2022-04-25 오전 10.42.56.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.56.png)

- 클릭 할 때마다 콘솔에 데이터가 찍힌다.

```python
# const : 상수를 만들때 사용
console.log(response);
               const tag = "<li>"+response.name + " / " + response.age + "</li>"//li 태그 문자열로 만들기 
               $("#data").append(tag);
```

![스크린샷 2022-04-25 오전 10.48.59.png](Flask%2041f05bca818e4a77ab811b295d06dba0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-25_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.48.59.png)

**[ app 전체 코드 ]**

```python
from flask import *

app = Flask(__name__) # __name__: 실행될 함수, 모듈 ... (실행환경) __main__ 이 들어가 있다.

@app.route("/") #데코레이터 
def home():
    return "Hello Flask." #Plain Text (평문)

@app.route("/user/<name>")
def user(name): #name을 path parameter로 (위의 매개변수와 name과 일치해야 한다.)
    
    first_char = name[0]
    last_char = name[-1]
    stars = "*" * (len(name)-2)
    name_blind =  f"{first_char}{stars}{last_char}"
    
    # attribs = {'name' : name_blind}
    
    # rander_template : template 폴더를 탐색해서 index.html을 찾아낸다.
    return render_template("index.html", name=name_blind)

@app.route("/api/data")
def api_data():
    
    ############
    # mysql에서 조회를 하든, mongodb를 활용해서 조회를 하던
    # 크롤링을 수행해서 데이터를 스크래이핑 하던...
    ############
    
    
    # json으로 응답하기 위해서는 dict 형태의 데이터가 필요하다.
    data = {
        "name" : "euna",
        "age" : 28
    }
    
    # flask에 dict형태의 데이터를 json으로 만들어 줄 수 있는 함수 - jsonify
    return jsonify(data)

# "__main__" : 최초 실행된 위치(프로그램 시작 위치)
if __name__ == "__main__":
    app.run(debug=True)
```

**[ index.html ] 전체 코드**

```python
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello Flask.</title>
</head>
<body>
   <h1>{{name}}님 반갑습니다</h1> 
   <button id="load_data">
       click.
   </button>
   <ul id = "data"></ul>

   <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js"></script>
   <script>
       $("#load_data").on("click", ()=>{
           alert('hello');

           // load data 버튼을 클릭 했을 때 /api/data에 요청 -(비동기 요청)
           // 비동기 요청 - $.ajax, $.getJSON, ...
           $.getJSON("/api/data", (response)=>{
               console.log(response);
               const tag = "<li>"+response.name + " / " + response.age + "</li>"//li 태그 문자열로 만들기 
               $("#data").append(tag);
           });
       });

   </script>
</body>
</html>
```