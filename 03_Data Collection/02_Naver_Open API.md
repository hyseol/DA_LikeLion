# 네이버 Open API
- https://developers.naver.com/

## API Key 설정
```python
import yaml
DATA_PATH = "data/"

with open(f"{DATA_PATH}0925_config.yml", "r") as f:
# config 파일을 읽기 형식으로 열어서 f 변수에 반환
    CFG = yaml.full_load(f)
    # config 파일의 전체 내용 불러오기 -> 딕셔너리 형태로 반환

client_id = CFG["NAVER"]["CLIENT_ID"]
client_secret = CFG["NAVER"]["CLIENT_SECRET"]
# CFG 변수에서 해당 key 값에 해당하는 value 값 반환
```

## requests 라이브러리 - get 함수
- requests.get(요청 url, params(딕셔너리), headers(딕셔너리))

### Naver 검색 - 블로그
- 매서드 : get
- 요청 URL : https://openapi.naver.com/v1/search/blog.xml / https://openapi.naver.com/v1/search/blog.json
- 헤더 :  X-Naver-Client-Id: {클라이언트 아이디 값} / X-Naver-Client-Secret: {클라이언트 시크릿 값}
- 파라미터 : query(검색어/str)

```python
# json 형식
url = "https://openapi.naver.com/v1/search/blog.json"
params = {
    "query" : "딥러닝",
    "sort" : "date"
}
headers = {
    "X-Naver-Client-Id" : client_id,
    "X-Naver-Client-Secret" : client_secret
}

# get 함수 실행
res = requests.get(url, params=params, headers=headers)

if res.status_code == 200:
    data = res.json()     
    # json 형태로 받아온 데이터를 딕셔너리 형태로 반환
    pprint(data)
else:
    print(f"Error code : {res.status_code}")
```

```python
# xml 형식
url = "https://openapi.naver.com/v1/search/blog.xml"
params = {
    "query" : "머신러닝"
}
headers = {
    "X-Naver-Client-Id" : client_id,
    "X-Naver-Client-Secret" : client_secret
}

res = requests.get(url, params=params, headers=headers)

# xml 형식을 딕셔너리 형태로 변환
import xmltodict    # xmltodict 라이브러리 활용
data = xmltodict.parse(res.text)
```

### Naver 검색 API 함수 만들기
```python
# search_naver 함수 생성
def search_naver(url, client_id, client_secret, params):
    headers ={
        "X-Naver-Client-Id" : client_id,
        "X-Naver-Client-Secret" : client_secret
    }
    res = requests.get(url, params=params, headers=headers)
    data = res.json() 

    if res.status_code == 200:
        return data
    else:
        return (f"Error code : {res.status_code}")

# search_naver 함수 실행
url = "https://openapi.naver.com/v1/search/book.json" # 요청 url
client_id = CFG["NAVER"]["CLIENT_ID"]
client_secret = CFG["NAVER"]["CLIENT_SECRET"]
params = {                  # 요청 파라미터 
    "query" : "파이토치",
    "display" : 1
}

data = search_api(url,client_id,client_secret,params)

# 검색 결과 활용하기
data_list = []
for i in range(1,4):
    params = {                  
        "query" : "파이토치",
        "start" : i
    }
    data = search_api(url,client_id,client_secret,params)
    data_list.extend(data["items"])

data_list

# 표 형태로 결과 조회
import pandas as pd     # pandas 라이브러리 활용
df = pd.DataFrame(data_list)
# df.head(1) : 한 줄만 반환
```
