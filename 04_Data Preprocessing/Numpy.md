# Numpy(Numerical Python) 라이브러리
- 다차원 배열(Array)을 다룰 때 주로 사용

- 배열과 리스트의 차이점

    ||배열|리스트|
    |------|---|---|
    |메모리 할당|연속적|비연속적|
    |데이터 타입 (실수,문자, 정수 등)|동일|다양| 
    |사용 용도|수치계산, 이미지 처리 등|다양한 일반적인 데이터 저장|

# Ndarray(N-dimensional Array)
- 벡터(Vector)
    - 1차원 데이터(1차원 배열)
    - 스칼라(scalar)가 연속적으로 여러 개 모여있는 것
        - 스칼라(scalar): 단순하게 측정한 하나의 값
- 행렬(Matrix)
    - 2차원 데이터(2차원 배열)
    - 1차원 배열인 벡터(Vector)가 여러 개 모여있는 것
- 3차원 배열
    - 3차원 데이터
    - 2차원 배열인 행렬(Matrix)가 여러 개 모여있는 것

## 주요 속성
- ndarray.ndim : 배열의 차원 수
- ndarray.shape : 배열의 각 차원의 크기(튜플로 반환 - 샘플의 개수, 행, 열)
- ndarray.size : 배열 내 요소(스칼라 값)의 총 개수
- ndarray.dtype : 배열내 요소의 데이터 타입

## 데이터 타입
- int32 : 4바이트 크기의 정수
- int64 : 8바이트 크기의 정수
- unit8 : 이미지 데이터 / 색을 표현하는 256개(0~255)만 표현 가능
- float32 : 4바이트 크기의 실수 
- float64 : 8바이트 크기의 실수
- bool : true(1) / false(0)

# 연산

## 인덱싱 / 슬라이싱
```python
arr = [ [1 2 3]
        [4 5 6],
        [7 8 9],
        [10 11 12] ]

# 다차원 슬라이싱
arr[1:4, :2] -> [  [ 4  5],
                   [ 7  8],
                   [10 11]    ]

# 다차원 인덱싱
arr[0,2] -> arr[3]

# 비교
arr[:,:0] -> 슬라이싱 [] (4,0) = 행렬
arr[:,0] -> 인덱싱 [1 4 7 10] (4, ) = 벡터

# 범위 안에 리스트 넣기
arr[ [0,1],[1] ] -> [2 5] (2, )
arr[ [ [1,2,3] ] ] -> [ [ [4 5 6]
                          [7 8 9]
                          [10 11 12] ] ] (1,3,3)                    
```

## 마스킹(masking)
- bool 배열을 마스크로 사용하여 데이터의 특정 부분들을 선택
```python
arr = [ [1 2 3]
        [4 5 6],
        [7 8 9],
        [10 11 12] ]

mask = [True, False, True, False]
arr[mask] -> [ [1 2 3]
               [7 8 9] ]

mask = arr > 8
arr[mask] -> [ [False False False]
               [False False False]
               [False False True]
               [True True True] ]
```
## 배열 연산
### Element-wise
- 배열 간의 동일한 인덱스의 요소끼리 연산
```python
np.add(arr1, arr2)         # arr1 + arr2
np.subtract(arr1, arr2)    # arr1 - arr2 - arr2
np.multiply(arr1, arr2)    # arr1 * arr2
np.divide(arr1, arr2)      # arr1 / arr2
arr1 % arr2                # 나머지
arr // arr2                # 몫
```
### Broadcasting
- 배열의 모양이 다르더라도 어떠한 조건이 만족했을 때 연산이 가능해지도록 작은 배열을 큰 배열의 크기로 맞추는 기능
```python
# 정수를 곱하기
arr * 2 -> 해당 숫자로 동일한 크기의 행렬을 생성해서 계산

# 열 개수를 맞춰줄 경우
arr * np.array([2,2,2]) -> 행 개수를 동일하게 생성해서 계산

# 행 개수(첫 번째 차원)를 맞춰줄 경우
arr * np.array([[2],    -> 열 개수를 동일하게 생성해서 계산
                [2],
                [2],
                [2]])
```

### Norm
- 벡터의 크기를 측정하는 함수
$$
L_1 = {\sum_{i=1}^{n}|x_i|}
$$

$$
L_2 = \sqrt{{\sum_{i=1}^{n}x_i^2}}
$$
```python
# L1 Norm
np.linalg.norm([-1,2,3], 1) 
# L2 Norm
np.linalg.norm([-1,2,3], 2)
```

### 내적(dot product)
- 두 벡터의 각 요소끼리의 곱의 합
- 결과 값이 스칼라 값으로 나옴
```python
a = [1 2 3]
b = [4 5 6]

np.dot(a,b) = a @ b -> 32 = (1*4) + (2*5) + (3*6)
```

### 행렬곱
- 앞의 행렬의 열 개수 = 뒤의 행렬의 행 개수
- 결과는 행렬 (앞의 행렬의 행 개수 = 뒤의 행렬의 열 개수)
```python
arr1 = [ [1,2,3],
         [4,5,6] ] (2,3)
arr2 = [ [2],
         [1],
         [1] ]  (3,1)

np.dot(arr1, arr2) = arr1 @ arr2
-> [ [7],               -> (1*2) + (2*1) + (3*1)
     [19] ]  (2,1)      -> (4*2) + (5*1) + (6*1)
```
## 집계 함수
```python
np.sum(arr)         arr.sum()       # 총합  
np.mean(arr)        arr.mean()      # 평균  
np.median(arr)                      # 중앙값
np.std(arr)         arr.std()       # 표준편차
np.var(arr)         arr.var()       # 분산
np.min(arr)         arr.min()       # 최솟값  
np.max(arr)         arr.max()       # 최댓값
np.argmax(arr)      arr.argmax()    # 최댓값이 위치한 인덱스 반환  
np.argmin(arr)      arr.argmin()    # 최솟값이 위치한 인덱스 반환  
np.unique(arr)                      # 중복값 제거
np.square(arr)                      # 제곱 (arr ** 2)
np.sqrt(arr)                        # 루트 (arr ** 0.5)

np.round(arr)                       # 반올림
np.ceil(arr)                        # 올림
np.floor(arr)                       # 버림
```
### 함수
```python
np.exp(arr)             # exp 함수
np.log(arr)             # 자연 로그 함수
np.quantile(arr, x)     # quantile 함수
                        # 분위수 : 데이터의 크기 순서에 따른 위치값
                        # 0.5 = 중앙값
```
## 정렬
```python
np.sort(arr)            # 오름차순 = 기본값
np.sort(arr)[::-1]      # 내림차순
```
## 배열 조건 연산
```python
np.any(arr > 1)  # 요소 중 하나라도 참이면 True / 모두 거짓이면 False
np.all(arr > 1)  # 모든 요소가 참이면 True / 하나라도 거짓이면 False
np.where(arr > 10, a, b)  # 조건에 참이면 a / 거짓이면 b
```
## 이상치 처리
```python
np.clip(arr, min, max)

# 배열 요소에서 min보다 작은 값을 min 값으로, max보다 큰 값을 max 값으로 변경
```
## inf(무한대값) / nan(결측치) 처리
```python
np.isinf(arr)               # 무한대값 찾기
np.isnan(arr)               # 결측치 찾기
np.isfinite(arr)            # 연산 가능한 데이터 찾기
np.array_equal(arr1,arr2)   # 두 배열의 요소가 모두 같은지 비교

# 결과가 bool 값(true/false)으로 나옴
```
## numpy random
```python
# seed : 같은 seed값에서 같은 랜덤 결과가 나옴
np.random.seed(seed값)

# 0 ~ 1 사이의 랜덤값
np.random.rand(개수)

# start ~ end-1 사이의 랜덤한 정수
np.random.randint(start, end, 개수)

# 전달 받은 배열의 원본을 섞음(반환 X)
np.random.shuffle(arr)

# 지정한 개수만큼 랜덤하게 선택해서 추출(복원추출 / 비복원추출)
np.random.choice(범위, 개수, replace = true/false)
```
## axis
- 행렬 (행,열) : axis = 0 행 방향(가로) / axis = 1 열 방향(세로)
- 3차원 (샘플,행,열) : axis = 0 샘플 개수(뒤) / axis = 1 행 방향(가로) / axis = 2 열 방향(세로)

### 배열 차원 변경하기
```python
arr[:, np.newaxis] # 첫번째 차원 전부 사용 / 두번째 차원에 축 추가
                   # (x, ) -> (x, y)
                   # arr.reshape(-1,1)

np.reshape(arr, [3,2])  # arr을 (3,2)로 만들기

arr.transpose() = arr.T  # (x,y) -> (y,x)
arr.transpose(0,2,1)     # 첫번째 차원을 0번째, 두번째 차원을 2번째, 세번째 차원을 1번째로 바꾸자

arr.flatten() = arr.reshape(-1) # 벡터로 만들기
```
### 배열 합치기
```python
np.concatenate([arr1, arr2])    # axis = 0(기본값) / 첫번째 차원 기준으로 합치기
np.concatenate([arr1, arr2], axis = 1)  # 두번째 차원 기준으로 합치기

np.stack([arr1, arr2], axis = 0)    # (3,2) -> (2,3,2)

arr.tolist()    # 리스트로 변경하기
```



                