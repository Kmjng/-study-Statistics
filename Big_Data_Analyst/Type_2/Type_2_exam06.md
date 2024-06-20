문제 1. 
--
> 다중 분류 문제  
> - 난방 부하 단계를 예측하시오  
> - - 예측할 값 (y) : Heat_Load(Very Low, Low, Medium, High, Very High)  
> - 평가 : f1-macro  
> - - data: train, test.csv  
>   - - 제출 형식 : result.csv 파일을 아래와 같은 형식으로 제출   

### ✏️풀이 순서 
0. 사용할 모델 구상
> 다중분류모델, 여러가지 있지만 RandomForestClassifier 사용   
1. 데이터 불러오기 
```python
file1 = r"C:\ITWILL\빅분기_실기_기출_자료\energy_train.csv"
file2 = r"C:\ITWILL\빅분기_실기_기출_자료\energy_test.csv"
 
import pandas as pd

train = pd.read_csv(file1)
train.info() # 결측치 없음 
test = pd.read_csv(file2)
test.info()
train.Glaze_Distr.value_counts()
```

2. 변수 값 확인
```python
train.Glaze_Distr.value_counts()
'''
3    104
1    103
2    102
5    101
4     96
0     31
''' # int형이지만, 명목형으로 쓰일 것 같음.
train.Glaze_Distr = train.Glaze_Distr.astype('object')
```
3. 데이터 분리
```python
train.Heat_Load.value_counts()
X_train = train.drop('Heat_Load', axis = 1)
y_train = train.Heat_Load

X = pd.concat([X_train, test], axis = 0)
```
4. 데이터 변환
```python
# 명목형 변수 
from sklearn.preprocessing import LabelEncoder, StandardScaler
enc = LabelEncoder()
y_train = enc.fit_transform(y_train) # Series
y_train # 0,1,2,3,4 # 다중 분류 


X_obj = X.select_dtypes(include = 'O')
for col in X_obj.columns:
    X_obj[col] = enc.fit_transform(X_obj[col])

X_obj.info()

# 수치형 변수 
X_num = X.select_dtypes(exclude = 'O')
cols = list(X_num.columns)
scaler = StandardScaler()
X_num = scaler.fit_transform(X_num)
X_num = pd.DataFrame(X_num, columns = cols)

print(X_num.shape)  # (768, 5)
print(X_obj.shape)  # (768, 4)

X = pd.concat([X_obj, X_num], axis = 1)
'''
X = pd.concat([X_obj, X_num], axis = 1)
>> InvalidIndexError: Reindexing only valid with uniquely valued Index objects
'''
X_obj = X_obj.reset_index(drop =True)

X = pd.concat([X_obj, X_num], axis = 1)

X_train = X.iloc[:train.shape[0],:]

X_test = X.iloc[train.shape[0]:,:].reset_index(drop = True)

X_train.shape  # (537, 9)
X_test.shape # (231, 9)
```
5. 모델링
```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, random_state = 0, 
                                                  test_size = 0.3)

from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(random_state = 0 )


model = LogisticRegression()
lr = model.fit(X_train, y_train)
pred_val = lr.predict(X_val) 
pred_test = lr.predict(X_test) # 제출해야할 것 !

```
6. 평가 (f1_score) 및 출력 
```python
from sklearn.metrics import f1_score
f1_score(y_val, pred_val, average = 'macro') 
# 로지스틱회귀 : 0.7769100548850455
# 랜포 : 0.9262295687505773

# 역 라벨인코딩 

pred_test # array 

pred_test = enc.inverse_transform(pred_test)
'''
array(['Low', 'High', 'High', 'Low', 'Low', ...
'''

result = pd.DataFrame(pred_test, columns = ['pred'])
result
```




