선형회귀모델 
==
여러가지 모듈 
--
### 1. statsmodels의 formula 모듈 (R의 formula 문법과 유사)
* ols() 
```python
from statsmodels.formula.api import ols  # 문자열
model = ols('y~ x1 + x2', data = ~)
```
### 2. statsmodels의 api 모듈 
* OLS()
```python
import statsmodels.api as sm 
model = sm.OLS(y, X)
```
### 3. sklearn의 linear_model 모듈 (대표적으로 LinearRegression/ LogisticRegression)
* LinearRegression()
```python
from sklearn.linear_model import LinearRegression 
model = LinearRegression() 
```
