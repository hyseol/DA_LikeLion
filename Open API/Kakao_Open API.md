# 카카오 Open API
- https://developers.kakao.com/

## API Key 설정
```python
import yaml
DATA_PATH = "data/"

with open(f"{DATA_PATH}0925_config.yml", "r") as f:
# config 파일을 열어서 f 변수에 반환
    CFG = yaml.full_load(f) 
    # config 파일의 전체 내용을 CFG 변수에 딕셔너리 형태로 반환

api_key = CFG["KAKAO"]["API_KEY"]
# CFG 딕셔너리에서 불러올 내용의 키값 지정
```

## requests 라이브러리 - get 함수
- request.get(요청url, params(딕셔너리), headers(딕셔너리))

### Daum 검색 - 웹문서 검색
- 매서드 : get
- 요청 URL : https://dapi.kakao.com/v2/search/web
- 헤더 : Authorization: KakaoAK ${REST_API_KEY}
- 쿼리 파라미터 : query(검색어/str)
```python
import requests
url = "https://dapi.kakao.com/v2/search/web"    # 요청 URL
params = {"query" : "머신러닝"}                 # 요청 파라미터
headers = {"Authorization": "KakaoAK " + api_key}  
# headers = {"Authorization" : f"KakaoAK {api_key}"} -> f-string 활용 

res = requests.get(url, params= params, headers=headers)
# get 함수 실행 후 결과값을 json 형태로 res 변수에 반환 = 응답 객체(요청 헤더 + 응답 헤더 + 데이터)
res.json()
# json 형태의 데이터를 딕셔너리 형태로 반환
```
### Daum 검색 API 함수 만들기
```python
# search_daum 함수 생성
def search_daum(url, api_key, **params):    
    # 인수로 url, api_key, params(딕셔너리) 받음
    # **kwargs(키워드 가변 파라미터) : 변수명 = key, 값 = value가 되어 딕셔너리 형태로 반환
    headers = {"Authorization": "KakaoAK " + api_key}           # 헤더 값 지정
    res = requests.get(url, params= params, headers=headers)    # get 함수 실행
    data = res.json()               # 데이터를 딕셔너리 형태로 반환
    if res.status_code != 200:      # 응답이 실패한 경우
        print(res.status_code)      # 상태코드 반환
        data = None                 
    return data                     # 최종 데이터 반환


# search_daum 함수 실행
url = "https://dapi.kakao.com/v2/search/web"
api_key = CFG["KAKAO"]["API_KEY"]

data = search_kakao_api(url, api_key, query = "머신러닝" , size = 2)
# params = {"query" : "머신러닝", "size" : 2} 로 반환

from pprint import pprint 
# print 함수는 한 줄로 출력되어 보기 어려움
## pprint 함수는 데이터 구조를 보기 편하게 출력해줌
pprint(data["documents"])
# data에서 키 값이 documents인 value 값 출력 -> data는 딕셔너리 형태
```

## requests 라이브러리 - post 함수
- request.post(요청url, data(딕셔너리), headers(딕셔너리))

### KoGPT - 요청하기
- 매서드 : post
- 요청 URL : https://api.kakaobrain.com/v1/inference/kogpt/generation
- 헤더 : Authorization: KakaoAK ${REST_API_KEY} / Content-Type: application/json(요청 데이터 타입)
- 본문 : prompt(KoGPT에게 전달할 제시어(한국어)/str) / max_tokens(생성 결과의 최대 토큰 수/int)

```python
url = "https://api.kakaobrain.com/v1/inference/kogpt/generation"    # 요청 URL
headers = {"Authorization": "KakaoAK " + api_key}                   # 헤더 값 지정
data = {                                                            # 본문
    "prompt" : "데이터 분석이란 ",
    "max_tokens" : 200
    }

res = requests.post(url, json=data, headers=headers)    
# post 함수 실행 -> 요청 데이터 타입에 본문 전달 = json 형식으로 반환
res.json()  # json 형식의 데이터를 딕셔너리 형태로 반환
```
- 카카오에서 제공한 활용 예시
```python
# coding=utf8
# REST API 호출에 필요한 라이브러리
import requests
import json

# [내 애플리케이션] > [앱 키] 에서 확인한 REST API 키 값 입력
REST_API_KEY = '${REST_API_KEY}'

# KoGPT API 호출을 위한 메서드 선언
# 각 파라미터 기본값으로 설정
def kogpt_api(prompt, max_tokens = 1, temperature = 1.0, top_p = 1.0, n = 1):
    r = requests.post(
        'https://api.kakaobrain.com/v1/inference/kogpt/generation',
        json = {
            'prompt': prompt,
            'max_tokens': max_tokens,
            'temperature': temperature,
            'top_p': top_p,
            'n': n
        },
        headers = {
            'Authorization': 'KakaoAK ' + REST_API_KEY,
            'Content-Type': 'application/json'
        }
    )
    # 응답 JSON 형식으로 변환
    response = json.loads(r.content)
    return response

# KoGPT에게 전달할 명령어 구성
prompt = '''인간처럼 생각하고, 행동하는 '지능'을 통해 인류가 이제까지 풀지 못했던'''

# 파라미터를 전달해 kogpt_api()메서드 호출
response = kogpt_api(
    prompt = prompt,
    max_tokens = 32,
    temperature = 1.0,
    top_p = 1.0,
    n = 3
)

print(response)
```

### 카카오 AI API 활용하는 클래스 만들기
```python
# KakaoAI 클래스 생성
class KakaoAI:
    def __init__(self, api_key, url):   # 초기값 설정 -> 파라미터 지정
        self.url = url                  # 인스턴스 변수 지정
        self.headers = {"Authorization": "KakaoAK " + api_key}

    def generate(self, data):   # generate 함수 생성
        res = requests.post(self.url, json= data, headers= self.headers)
        # 지역변수 지정 / post 함수 실행하여 json 형태로 응답 객체 생성
        return res.json()   # json 형태의 데이터를 딕셔너리 형태로 반환


# KakaoAI 클래스 실행 - KoGPT
url = "https://api.kakaobrain.com/v1/inference/kogpt/generation"  # 인수 설정
api_key = CFG["KAKAO"]["API_KEY"]
data = {
    "prompt" : "데이터 분석이란 ",
    "max_tokens" : 200
    }

ko_gpt = KakaoAI(api_key, url)                  # 클래스 객체 생성
ko_gpt.generate(data)["generations"][0]["text"] # generate 함수 실행
                                                # 지정 key 값에 해당하는 value 값 출력

# KakaoAI 클래스 실행 - Karlo - 이미지 생성
url = "https://api.kakaobrain.com/v2/inference/karlo/t2i"   # 인수 설정
api_key = CFG["KAKAO"]["API_KEY"]
data = {
    "version" : "v2.1",
    "prompt" : "like lion",
    "width" : 768,
    "height" : 768
    }

karlo = KakaoAI(api_key, url)   # 클래스 객체 생성
karlo.generate(data)            # generate 함수 실행

```