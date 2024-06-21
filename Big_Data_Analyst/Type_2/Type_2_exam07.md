문제 1. 
--
> mart 판매 데이터를 기반으로 판매액을 예측하시오.  
> 예측할 칼럼: total  
> 사용할 평가지표: RMSE

### ✏️풀이 순서 
0. 사용할 모델 구상
> 판매액이 수치형이므로, 선형회귀모델링을 한다.   
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
* ```from sklearn.impute import SimpleImputer``` 
```python
# Imputer 활용한 결측치 채우기
from sklearn.impute import SimpleImputer

imp = SimpleImputer()
train = imp.fit_transform(train)
test = imp.transform(test)
# imputer는 numpy 형태로 반환함
```



4. 수치형 변수 스케일링
* 유의 1. 스케일링 fit에는 <mark>DataFrame type</mark>이 들어감 
* 유의 2 : ```pd.concat```또는 데이터프레임 slicing할 때 ```.reset_index(drop=True)```
* * index를 초기화하고 DataFrame을 반환
  * drop= True : index 제거
* ```sklenarn.preprocessing```의 ```StandardScaler```로 **표준화** 
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
* 유의 1. 인코딩 fit에는 <mark>Series type</mark>이 들어감
* ```sklenarn.preprocessing```의 ```LabelEncoder```로 **라벨인코딩** 사용
* * 원-핫 인코더는 명목형 피쳐가 많을 경우, 다중공선성 우려가 있음
```python
train_ob = train.select_dtypes(include ='O')
test_ob = test.select_dtypes(include = 'O')

ob = pd.concat([train_ob, test_ob], axis =0 ).reset_index(drop=True)
ob.index # 0 ~1000 

from sklearn.preprocessing import LabelEncoder 

enc = LabelEncoder() 
for col in ob.columns : 
    ob[col] = enc.fit_transform(ob[col]) 
```

* 명목형 변수 전처리 완료
```python
train_ob = ob.iloc[:train.shape[0], :]
test_ob = ob.iloc[train.shape[0]:,:].reset_index(drop= True)
```

* 전처리 마무리
```python
train_scaled = pd.concat([train_ob, train_num], axis =1).reset_index(drop=True)
test_scaled = pd.concat([test_ob, test_num], axis =1).reset_index(drop=True)
```
6. train/validation set split
```python
from sklearn.model_selection import train_test_split 
X_train, X_val, y_train, y_val = train_test_split(train_scaled, y_train, 
                                                  random_state =123, 
                                                  test_size = 0.3)

test_scaled.shape #(300, 9) 확인 
```

7. 모델링
```python
from sklearn.linear_model import LinearRegression 
model = LinearRegression() 
lr = model.fit(X_train,y_train)
pred_val = lr.predict(X_val)
pred = lr.predict(test_scaled) # test set에 대한 예측 
```

8. 평가 및 내보내기
```python
from sklearn.metrics import mean_squared_error 
print('RMSE:',mean_squared_error(y_val, pred_val)**0.5) # RMSE 적용할 것
# RMSE: 356806.1620006393
type(pred)

pred = pd.DataFrame(pred, columns =['pred'])

pred.shape # (300,1)
pred.to_csv('./result.csv',index = False)

pd.read_csv('./result.csv')
```
