# 문제 11
## x_train 데이터셋에서 데이터의 20%에 대한 인덱스들을 리스트에 담아 5번 랜덤하게 반환하는 제너레이터 함수

- 5번 랜덤하게 반환하는 과정에서 동일한 인덱스가 나오면 안된다.
- 모든 인덱스가 한번씩은 무조건 나와야한다.
- 마지막 리스트는 앞서 나온 크기와 차이가 나도 된다.  

### 빈 리스트에 append
```python
def split_dataset(dataset):

    length = len(dataset)       # 총 개수
    index = np.arange(length)   # 인덱스 번호 리스트
    num = length//5             # 20% 개수

    np.random.seed(42)
    np.random.shuffle(index)    # 인덱스 번호 섞기

    result = []
    for i in range(5):          # 5번 뽑기
        a = i * num
        b = a + num
        if i == 4:              # 마지막은 끝까지
            b = length
        result.append(index[a:b])

    return result
```

### 제너레이터 - yield
```python
def split_dataset(dataset, n_split = 5, seed = None):

    n_row = len(dataset)
    index_arr = np.arange(n_row)
    n_samples = n_row // n_split

    if seed is not None:
        np.random.seed(seed)
    np.random.shuffle(index_arr) 

    for i in range(n_split):
        start = i * n_samples
        end = start + n_samples
        if i + 1 == 5:
            end = n_row
        yield index_arr[start:end]
```

### 제너레이터 - yield from
```python
def split_dataset(dataset, n_split = 5, seed = None):

    n_row = len(dataset)
    index_arr = np.arange(n_row)
    n_samples = n_row // n_split

    if seed is not None:
        np.random.seed(seed)
    np.random.shuffle(index_arr) 

    n_sub = n_row - (n_samples * n_split)       # 나머지의 개수

    index_list = index_arr[:n_row-n_sub].reshape(n_split, n_samples).tolist()
    if n_sub > 0:
        index_list[-1].extend(index_arr[n_row-n_sub:].tolist())

    yield from index_list
```

# 문제 12

### 딥러닝 모델에서 시계열 학습데이터 형태로 반환하는 함수
- samsung_stock 변수의 데이터는 2차원 데이터이다.
- 입력데이터는 샘플별로 10일치의 행과 4열(시작가 ,상한가 , 하한가, 종가) 형태의 행렬을 담고 있는 3차원 형태의 데이터셋으로 변경하시오.
- 정답데이터는 미래의 5일치의 종가를 예측해야 한다는 가정하에 샘플별로 미래의 5일치의 종가가 담긴 2차원 형태의 데이터셋으로 변경하시오.
- 1일씩 이동하면서 데이터셋을 구성하시오.

```python
# i = start값 = 0 -> end 값을 설정

def transform_data(samsung_stock,seq_len=10,pred_len=5):
    x_train = []
    y_train = []

    for i in range(0, len(samsung_stock)-pred_len-seq_len +1):
        # for문이 0 부터 시작하기 때문에 +1 해야함

        x = samsung_stock[i:i+seq_len]
        x_train.append(x)

        y = samsung_stock[i+seq_len:i+seq_len+pred_len][:,3]
        y_train.append(y)

    x_train = np.array(x_train)
    y_train = np.array(y_train) 
    return x_train, y_train
```

```python
# 강사님 스타일 : for문의 범위 지정이 다름 
# i = end값 = 10 -> start 값을 설정

def transform_data(samsung_stock,seq_len=10,pred_len=5):
    x_train = []
    y_train = []

    for i in range(seq_len,samsung_stock.shape[0]-pred_len + 1):
        # for문이 0 부터 시작하기 때문에 +1 해야함

        x = samsung_stock[i-seq_len:i]
        y = samsung_stock[i:i+pred_len, 3]  # [i:i+pred_len, -1]
        x_train.append(x)
        y_train.append(y)

    return np.array(x_train), np.array(y_train)
```