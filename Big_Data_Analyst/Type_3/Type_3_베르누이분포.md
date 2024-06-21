베르누이 분포와 이항분포
--
문제 1. 
-- 
[베르누이 분포] 다음 데이터는 100번의 시도에서 각각 성공(1) 또는 실패(0)를 나타냅니다.   
이 데이터를 바탕으로 각 시도의 <mark>성공 확률</mark>을 계산하시오.
```python
import pandas as pd 
df = pd.read_csv("/kaggle/input/bigdatacertificationkr/t3_success.csv")
df

# 성공확률 ★★ 구하기
num = len(df)
suc = df.Success.sum()
p = suc/num
print(num, suc, p)
# 100 62 0.62
```

문제 2. 
-- 
[이항분포] 1번 문제에서 계산한 성공 확률을 사용하여, 100번의 시도 중 정확히 60번 성공할 확률을 계산하시오.
```python
from scipy import stats 
pmf = stats.binom.pmf(60, 100, p) # 성공해야할 횟수, 전체 횟수, 성공확률 ★★
pmf # 0.07464985555860272
```
