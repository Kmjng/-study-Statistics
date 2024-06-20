문제 1. 
--
> [가격 예측] 중고 자동차
자동차 가격을 예측하시오
예측할 값(y): price
평가: RMSE (Root Mean Squared Error)
data: train.csv, test.csv
제출 형식: result.csv파일을 아래와 같은 형식(수치형)으로 제출

### ✏️풀이 
```python

import pandas as pd 

train = pd.read_csv(file1)
test = pd.read_csv(file2)
y_test = pd.read_csv(file3) # test y 

train.columns
X_train = train.drop('price', axis =1)
y_train = train.price

train.info()
'''
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3759 entries, 0 to 3758
Data columns (total 9 columns):
 #   Column        Non-Null Count  Dtype  
---  ------        --------------  -----  
 0   model         3759 non-null   object 
 1   year          3759 non-null   int64  
 2   price         3759 non-null   int64  
 3   transmission  3759 non-null   object 
 4   mileage       3759 non-null   int64  
 5   fuelType      3759 non-null   object 
 6   tax           3759 non-null   int64  
 7   mpg           3759 non-null   float64
 8   engineSize    3759 non-null   float64
dtypes: float64(2), int64(4), object(3)
memory usage: 264.4+ KB
'''
train.shape # (3759, 9)
test.shape #  (1617, 8)

# 선형회귀!! price 예측 

# 결측치 없음 

# 전처리 

# 명목형 

X = pd.concat([X_train, test], axis =0 )

X_obj = X.select_dtypes(include = 'O') 

from sklearn.preprocessing import LabelEncoder, MinMaxScaler
enc = LabelEncoder() 

for col in X_obj.columns:
    X_obj[col] = enc.fit_transform(X_obj[col])

X_obj = X_obj.reset_index(drop = True)

# 수치형 

X_num = X.select_dtypes(exclude = 'O')
cols = X_num.columns
scaler = MinMaxScaler()

X_num = scaler.fit_transform(X_num)
X_num = pd.DataFrame(X_num , columns = cols)

# concat 

X = pd.concat([X_obj, X_num], axis = 1)

X_train = X.iloc[:train.shape[0],: ]
X_test = X.iloc[train.shape[0]:, :].reset_index(drop = True)
X_test.columns
# 전처리 완료 

# 모델링 

from statsmodels.formula.api import ols 
X_train.info()

train_data = pd.concat([X_train, y_train], axis = 1)
model = ols('price ~ model + transmission +fuelType + year + tax + mpg + engineSize', data = train_data)
# 명목형 변수 전처리했으므로 C()로 안묶어줘도 된다. 
lr = model.fit()
lr.summary()
pred = lr.get_prediction()
pred.summary_frame()

X_test # 전처리 된 

pred = lr.predict(X_test)
pred

from sklearn.metrics import mean_squared_error
print("rmse:", mean_squared_error(y_test, pred)**0.5 )
# rmse : 2642.517334914303

# 모델링 2 

from sklearn.linear_model import LinearRegression 
model = LinearRegression() 
lr = model.fit(X_train, y_train) 

pred = lr.predict(X_test)

print("rmse:", mean_squared_error(y_test, pred)**0.5 )
# 2467.351522566251

# 모델링 3 
from sklearn.ensemble import RandomForestRegressor 
model = RandomForestRegressor() 
rf = model.fit(X_train, y_train)
pred= rf.predict(X_test)

print("rmse:", mean_squared_error(y_test, pred)**0.5 )
# rmse: 1422.9578016160228

```
결론 : 선형회귀 모델에서 RandomForestRegressor()를 사용하자.
