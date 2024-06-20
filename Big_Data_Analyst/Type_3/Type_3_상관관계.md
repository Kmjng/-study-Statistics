문제 1. 
-- 
> iris에서 Sepal Length와 Sepal Width의 상관계수 계산하고 소수 둘째자리까지 출력하시오


 ```python
import pandas as pd
from sklearn.datasets import load_iris

# iris 데이터셋 로드
iris = load_iris()
df = pd.DataFrame(iris.data, columns=iris.feature_names)
df.columns

# 방법 1. 간단한 방법
correlation = df.corr()
result = correlation.loc['sepal length (cm)', 'sepal width (cm)']
print(round(result,2))
# -0.12

# 방법 2. scipy 의 stats 활용  
from scipy import stats 
statistic, p_val = stats.pearsonr(df['sepal length (cm)'], df['sepal width (cm)'])
print(round(statistic,2))
# -0.12

```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

