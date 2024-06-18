문제 1. 
--
> mart 판매 데이터를 기반으로 판매액을 예측하시오.
> 예측할 칼럼: total
> 사용할 평가지표: RMSE

### ✏️풀이 순서 
1. 데이터 불러오기 
```python
file1 = r"C:\ITWILL\빅분기_실기_기출_7회_자료\p7_type2\mart_train.csv"
file2 = "C:\ITWILL\빅분기_실기_기출_7회_자료\p7_type2\mart_test.csv"
import pandas as pd 
train = pd.read_csv(file1)
test = pd.read_csv(file2)
```

2. 부동소수점으로 표기
```python
pd.set_option('display.float_format','{:.10f}'.format)
train.total.value_counts()
```
```
total
283641.7500000000     2
263875.5000000000     2
415422.0000000000     2
326450.2500000000     2
130851.0000000000     2
                     ..
293391.0000000000     1
137103.7500000000     1
348232.5000000000     1
104107.5000000000     1
1535625.0000000000    1
Name: count, Length: 695, dtype: int64
```

3. EDA 확인 
```python
train.shape # (700,10)
test.shape # (300, 9) 

train.info() # 피쳐별 데이터 타입 확인 
test.info() 

train.isnull().sum() # 결측치 확인 
test.isnull().sum()
```
3-2. *만약 결측치가 있다면?*




4. 수치형 변수 스케일링
* 유의점 : ```pd.concat```또는 데이터프레임 slicing할 때 ```.reset_index(drop=True)```
* * index를 초기화하고 DataFrame을 반환
  * drop= True : index 제거
```python
y_train = train.pop('total') # X, y of train split
y_train.shape

train_num = train.select_dtypes(exclude = 'O')
test_num = test.select_dtypes(exclude = 'O')

train_num.rating.value_counts() # 수치형 맞음 
# (int로 되어있는 명목형일 수 있으므로 확인함)

num = pd.concat([train_num, test_num], axis =0 ).reset_index(drop=True)
num.index # 0~ 1000
num.columns
from sklearn.preprocessing import MinMaxScaler 
scaler = MinMaxScaler() 
num = scaler.fit_transform(num) # 스케일링 fit에는 데이터프레임 들어감 
num = pd.DataFrame(data = num, columns = ['rating'])
num.shape # (1000, 1)
```

* 수치형 변수 전처리 완료
```python
train_num = num.iloc[:train.shape[0],:]
test_num = num.iloc[train.shape[0]:, :].reset_index(drop=True)
```

5. 명목형 변수 인코딩  
```python

```

* 명목형 변수 전처리 완료
```python

```

6. train/validation set split
```python

```

7. 모델링
```python

```

8. 평가모델 
```python

```
