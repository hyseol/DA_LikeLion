# HTTP
- Hyper Text Transfer Protocol
- HTML, Plain text, JSON, XML, 이미지, 동영상 등 다양한 형태의 정보를 전송하는 프로토콜
- HTTP는 서버/클라이언트 모델 -> 클라이언트에서 요청(request)을 보내면 서버는 요청을 처리해서 응답(response)

![화면 캡처 2024-09-25 232901](https://github.com/user-attachments/assets/9a0279fe-8441-47a3-bdf6-dd870fc6b70c)

## HTTP Packet
- HTTP 통신으로 서버와 클라이언트 사이에 주고 받는 데이터
- HPPT Header
    - Request Header : 클라이언트가 서버에 요청할 때 존재하는 헤더 / 요청 URL, Method, 브라우저 정보
    - Response Header : 서버가 클라이언트의 요청에 대한 응답을 할 때 존재하는 헤더 / 인코딩, 서버 소프트웨어, 기타 정보
- HPPT Body : 서버에서 응답시 클라이언트가 요청한 실제 데이터 / HTML, 이미지, css, javascript, text 등

![화면 캡처 2024-09-25 232932](https://github.com/user-attachments/assets/f1b3dee2-1188-4e0c-9ead-89e608b9b93f)

## HTTP 매서드(Method)
- 서버에 요청을 보내는 방식
- get : URL에 데이터를 담아 요청
- post : body 영역에 데이터를 담아 요청

# Open API 
- API(Application Programming Interface)
    - 프로그램의 특정한 부분에 요청해서 그 안에 있는 데이터와 서비스(기능)를 이용할 수 있게 해주는 소프트웨어 인터페이스
    - 소프트웨어 간의 상호작용을 가능하게 하는 매개체
- Open API
    - 클라이언트가 특정 소프트웨어나 애플리케이션의 기능에 접근할 수 있도록 공개된 인터페이스
    - 다양한 데이터를 어디서나 쉽게 이용할 수 있도록 개방된 API

![화면 캡처 2024-09-25 233157](https://github.com/user-attachments/assets/106b7453-9491-4661-bfd5-bd42df184d2e)

# requests 라이브러리
- http 요청을 보내 응답을 받을 수 있는 라이브러리
```python
# 서버에 데이터 요청
import requests
res = requests.get("http://www.naver.com/")

# 상태 코드 확인
res.status_code

# html 문서 확인
res.text

# requests 요청 정보 확인
res.request.method  # 요청 매서드
res.request.headers # 요청 헤더
res.request.body    # 요청 바디
# get 함수로 데이터를 요청 -> body 영역에 데이터 없음

# 응답 정보 확인
res.headers     # 응답 헤더
res.encoding    # 인코딩 정보
```
