문제 1. 
-- 
> 고객 정보를 나타낸 데이터이다. 주어진 데이터에서 500개 중 앞에서부터 350개는 train으로, 150개는 test 데이터로 나눈다. 모델을 학습(적합)할 때는 train 데이터를 사용하고, 예측할 때는 test 데이터를 사용한다. 모델은 로지스틱 회귀를 써서 고객이 특정 제품을 구매할지 여부를 예측하되, 페널티는 부과하지 않는다.  

종속변수: purchase (0: 구매 안 함, 1: 구매 함)  

문제 1-1. income 변수를 독립변수로 purchase를 종속변수로 사용하여 로지스틱 회귀 모형을 만들고, income 변수가 한 단위 증가할 때 구매할 오즈비 값을 계산하시오. (반올림하여 소수 넷째자리까지 계산)  

문제 1-2. 독립변수 income만 사용해 학습한 모델에서 test 데이터의 purchase를 예측하고, accuracy (정확도)를 구하시오. (반올림하여 소수 셋째자리까지 계산)  

문제 1-3. 독립변수 income만 사용해 학습한 모델의 로짓 우도를 계산하시오.  

문제 1-4. 독립변수 income만 사용해 학습한 모델의 유의확률(p-value)를 구하시오.  

 ```python
import pandas as pd
df = pd.read_csv("/kaggle/input/bigdatacertificationkr/Customer_Data.csv")
train = df.iloc[:350, :] # 350개 
test = df.iloc[350:, :] # 150 개 

# EDA 생략 ..
X_tr = train.drop('purchase', axis =1)
y_tr = train.purchase 
X_te = test.drop('purchase', axis =1)
y_te = test.purchase 
# 유의: statsmodels 의 formula 모듈 회귀모델에서는 X, y 나누지 않음

y_tr.unique()
# array([0, 1])

# 모델링 
from statsmodels.formula.api import logit 
model = logit('purchase ~ income', data = train)
lg = model.fit()

pred = lg.predict(test) 
pred # 종속변수인 purchase가 1일 확률
'''
pred = lg.get_predict(test)
pred.summary_frame() 하면 자세한 결과 얻을 수 있음. 
'''

lg.summary()
'''
statsmodels의 logit의 pred는 1일 확률을 반환하기 때문에 
1일 확률이 0.5 이상인 것을 1로 하는 코드를 추가해야 한다.'''
from sklearn.metrics import accuracy_score
pred_class = (pred > 0.5).astype(int)
pred_class
accuracy_score(test.purchase, pred_class)
# 0.5066666666666667

# 오즈비 계산 
import numpy as np
odds = np.exp(lg.params['income'])
odds 
# 1.0000019601805765

# 위의 모델에서의 로짓 우도 지표
lg.llf # 선형회귀의 경우 잔차제곱합 (MSE)
# 절대값이 작을수록 설명력이 좋다.
# -242.40981957168498

# 위의 모델에서의 p-value 
lg.pvalues
'''
Intercept    0.712184
income       0.596491
dtype: float64
'''

```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

