문제 1. 
--
> [마케팅] 자동차 시장 세분화
자동차 회사는 새로운 전략을 수립하기 위해 4개의 시장으로 세분화했습니다.
기존 고객 분류 자료를 바탕으로 신규 고객이 어떤 분류에 속할지 예측해주세요!
예측할 값(y): "Segmentation" (1,2,3,4)
평가: Macro f1-score
data: train.csv, test.csv

### ✏️풀이 
```python

train = pd.read_csv(file1)
test = pd.read_csv(file2)

train.info()
'''
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6665 entries, 0 to 6664
Data columns (total 11 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   ID               6665 non-null   int64  
 1   Gender           6665 non-null   object 
 2   Ever_Married     6665 non-null   object 
 3   Age              6665 non-null   int64  
 4   Graduated        6665 non-null   object 
 5   Profession       6665 non-null   object 
 6   Work_Experience  6665 non-null   float64
 7   Spending_Score   6665 non-null   object 
 8   Family_Size      6665 non-null   float64
 9   Var_1            6665 non-null   object 
 10  Segmentation     6665 non-null   int64  
dtypes: float64(2), int64(3), object(6)
'''
test.info()

from collections import Counter
Counter(train.Segmentation)
# Counter({4: 1757, 3: 1720, 1: 1616, 2: 1572})

X_train = train.drop('Segmentation', axis =1 )
y_train = train.Segmentation 

# 자료형 별 데이터 분리 후 전처리 

# train/test 함께 전처리 
X = pd.concat([X_train, test], axis =0)
X.drop('ID', axis = 1, inplace = True)

X_obj = X.select_dtypes(include = 'O')

from sklearn.preprocessing import LabelEncoder, MinMaxScaler
enc = LabelEncoder()

for col in X_obj.columns: 
    X_obj[col] = enc.fit_transform(X_obj[col])

X_obj = X_obj.reset_index(drop = True)

X_num = X.select_dtypes(exclude = 'O')
cols = X_num.columns
scaler = MinMaxScaler()
X_num = scaler.fit_transform(X_num)

X_num = pd.DataFrame(X_num, columns = cols )

X = pd.concat([X_obj, X_num], axis = 1)

X_train = X.iloc[:train.shape[0],:]

X_test = X.iloc[train.shape[0]:, :].reset_index(drop = True)

X_test # 전처리된 test set  

# validation set 
from sklearn.model_selection import train_test_split

X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, 
                                                  random_state = 42, 
                                                  test_size = 0.3 )

from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier()
rf = model.fit(X_train, y_train)

pred_val = rf.predict(X_val)
pred_test = rf.predict(X_test)
from sklearn.metrics import f1_score 

print('f1 score : ', f1_score(y_val, pred_val, average = 'macro'))
# f1 score :  0.49364208686567357

pred_test

result = pd.DataFrame(pred_test, columns = ['pred'])

result.to_csv('~~.csv', index = False)

```
