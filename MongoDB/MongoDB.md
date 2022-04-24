# mongodb

수강 날짜: 2022/04/24

### 리눅스 초기 설정

dir -a : 숨긴 파일까지 조회

.ssh 들어가서 .pem 파일 만들기

sudo apt update -y : 최신버전 업뎃

sudo apt upgrade -y

## aws cloudshell에서 MongoDB 실행하기

1. 권한 주기

```python
chmod 400 [펨 파일 이름] #은아는 이거 mc_mysql.pem
```

1. 인스턴스에 연결하기 (SSH 클라이언트 > 참고 바로 위의 주소)

```python
# ssh 주소 붙여넣기
ssh -i "mc_mysql.pem" ubuntu@ec2-54-180-90-21.ap-northeast-2.compute.amazonaws.com
```

1. 저장소 업데이트

```python
sudo apt update -y 
sudo apt upgrade -y
```

1. apt를 이용해 MongoDB 설치

```python
sudo apt install -y mongodb 
```

1. 부트시 실행되도록 서비스에 추가

```python
sudo systemctl start mongod

sudo systemctl enable mongod
```

1. 서비스모드로 실행하기

```python
sudo systemctl status mongodb 
```

1. mongodb 외부 접근이 가능하도록 잠금 풀기 (인증 활성화)

```python
. sudo vi /etc/mongodb.conf
```

6-1. 내부로 접근해 bind_ip 변경하기

- bind_ip = `0.0.0.0`으로 변경
- 인서트모드는 `i` 로 들어갈 수 있다.
- port 는 27017(몽고디비가 사용하는 포트)로 변경

6-2. 인서트모드 나오기

- esc 누른 후  `:wq` 입력

![스크린샷 2022-04-24 오후 8.04.16.png](mongodb%205928f7fa0ed94b7e81f818df1cad0648/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.04.16.png)

1. mongodb 재시작

```python
sudo systemctl restart mongodb
```

1. aws에서 인바운드 규칙 편집하기
    - 인스턴스 > 해당 키 > 보안 > 인바운드 규칙 편집 > 규칙추가

![스크린샷 2022-04-24 오후 8.11.29.png](mongodb%205928f7fa0ed94b7e81f818df1cad0648/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.11.29.png)

7-1. MongoDB셋팅 (27017 포트 셋팅)

![스크린샷 2022-04-24 오후 8.14.39.png](mongodb%205928f7fa0ed94b7e81f818df1cad0648/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.14.39.png)

1. mongoDB 셀 실행

```python
# 클라우드셸 들어가 확인 후 나오기
mongodb

quit()
```

1. 데이터베이스 만들기

```python
use [인스턴스 이름] #은아는 이거 mc_mysql
```

1. 데이터베이스 확인하기

```python
db
```

1. 도큐먼트 입력

```python
db.user.insert({'name':'euna', 'age':28})
```

1. 컬렉션 확인

```python
how collections (컬렉션 확인)
```

![스크린샷 2022-04-24 오후 9.08.42.png](mongodb%205928f7fa0ed94b7e81f818df1cad0648/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-04-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_9.08.42.png)

1. 데이터 찾기
- 출력된 id는 프라이머리키의 역할을 해준디
- 입력시간, 메모리 위치, 고유 아이디 등이 추가된 핵심 문자열)

```python
# 유저찾기
db.user.find()
```

1. db 입력

```python
db.info1.insert({ "subject":"python", "level":3 })
db.info1.insert({ "subject":"web", "level":1 })
db.info1.insert({ "subject":"sql", "level":2 })
```

```python
# 배열 형식

db.info1.insert( [
{ "subject":"python", "level":3 },
{ "subject":"web", "level":1 },
{ "subject":"sql", "level":2 },
{ "subject":"python", "level":3 },
{ "subject":"web", "level":1 },
{ "subject":"sql", "level":2 },
]) 
```

1. collection 의 데이터 SELECT
- 앞쪽은 조건문서 서브젝트가 파이썬인 조건들의 데이터만 찾음
- 인자가 없으면 hems documents 조회한다.

```python
db.info1.find()
{ "_id" : ObjectId("626539add9b91a2f20f38fa9"), "subject" : "python", "level" : 3 }
{ "_id" : ObjectId("626539add9b91a2f20f38faa"), "subject" : "web", "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fab"), "subject" : "python", "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fac"), "subject" : "web", "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fad"), "subject" : "sql", "level" : 2 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fae"), "subject" : "python", "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38faf"), "subject" : "web", "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fb0"), "subject" : "sql", "level" : 2 }

db.info1.find({"subject":"python"})
{ "_id" : ObjectId("626539add9b91a2f20f38fa9"), "subject" : "python", "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fab"), "subject" : "python", "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fae"), "subject" : "python", "level" : 3 }
```

1. 조건 없이 레벨만 찾기

```python
db.info1.find({"_id":0}, {"level":1}) #1은 보여주겠다는 의미
# db.info1.find({}, {"level":1})와 동일합니다.

{ "_id" : ObjectId("626539add9b91a2f20f38fa9"), "level" : 3 }
{ "_id" : ObjectId("626539add9b91a2f20f38faa"), "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fab"), "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fac"), "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fad"), "level" : 2 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fae"), "level" : 3 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38faf"), "level" : 1 }
{ "_id" : ObjectId("62653aabd9b91a2f20f38fb0"), "level" : 2 }
```

## colab에서 pymongo 실행하기 (파이썬으로 연동)

1. pymongo 설치
    - python에서 mongodb 작업을 위한 드라이버

```python
!pip install pymongo

!pip install geohash2
```

1. 데이터 함수 가져오기 (예제로 직방 데이터 가져옴)

```python
import geohash2
import requests
import pandas as pd

# 함수로 만들기    
def oneroom(addr):
    
    # 1. 동이름으로 위도 경도 구하기
    url = "https://apis.zigbang.com/search?q={}".format(addr)
    response = requests.get(url)
    lat, lng = response.json()["items"][0]["lat"], response.json()["items"][0]["lng"]
    
    # 2. 위도 경도로 geohash 알아내기
    geohash = geohash2.encode(lat, lng, precision=5) 
    
    # 3. geohash로 매물 리스트 가져오기
    url = "https://apis.zigbang.com/v2/items?\
deposit_gteq=0&domain=zigbang&geohash={}&rent_gteq=0&sales_type_in=전세|월세&service_type_eq=원룸".format(geohash)
    response = requests.get(url)
    items = response.json()["items"]
    ids = [item["item_id"] for item in items]
    
    # 4. id로 매물 정보 가져오기
    url = "https://apis.zigbang.com/v2/items/list"
    params = {
        "domain": "zigbang",
        "withCoalition": "false",
        "item_ids": ids[:900],
    }

    response = requests.post(url, params)
    items = response.json()["items"]
    return [item for item in items if addr in item["address"]]
```

1. pymongo를 이용하여 파이썬에서 mongodb 연결하기
    - aws인스턴스와 연동한 SSH클라이언트 > 퍼블릭 DNS 주소 연결하기 > 뒤에 mongodb port 입력 `:27017`

```python
import pymongo

client = pymongo.MongoClient("mongodb://ec2-54-180-90-21.ap-northeast-2.compute.amazonaws.com:27017")
client
```

1. zigbang 변수에 크롤링 데이터 넣어주기
    - 몽고디비에 크롤링된 아이템들 담은 것을 확인

```python
# client.<db이름>.<컬렉션 이름>
zigbang = client.crawling.zigbang
zigbang
```

1. JSON 형식의 데이터를 dict으로 얻어오기

```python
items = oneroom("성수동")
items # JSON 형식으로 데이터를 얻어옴! (dict)
```

1. 바로 인서트로 가져와보기

```python
zigbang.insert_many(items)
```

1. 데이터프레임으로 만들기

```python
oneroom_df = pd.DataFrame(items)
oneroom_df.head()
```

1. 컬렉션에서 쿼리 작성하기

```python
# 월세가 30만원 이상인 쿼리
QUERY = {
    "rent":{"$gte":30} #grater than equr = 이상 (이하 : $lte)
}

result = zigbang.find(QUERY)
```

1. aws cloudshell에서 mongodb활용하여 데이터 확인하기

```python
# db 데이터들을 확인
> show dbs
admin     0.000GB
config    0.000GB
crawling  0.000GB
local     0.000GB
mc_mysql  0.000GB

# 크롤링 데이터 사용
> user crawling
2022-04-24T12:30:12.603+0000 E QUERY    [thread1] SyntaxError: missing ; before statement @(shell):1:5
> use crawling
switched to db crawling

# 콜렉션 확인하기
> show collections
zigbang
```