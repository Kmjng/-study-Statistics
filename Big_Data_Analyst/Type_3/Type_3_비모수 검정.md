문제 
--
베스킨라빈스는 쿼트(Quart) 아이스크림의 중앙값이 620g이라고 주장하고 있습니다.   
저는 실제로 이 아이스크림의 중앙값이 620g보다 무겁다고 주장합니다.   
다음은 20개의 쿼트 아이스크림 샘플의 무게 측정 결과입니다.   
이 측정 결과를 바탕으로 나의 주장이 사실인지 비모수 검정(Wilcoxon Signed-Rank Test)을 통해 검정해보십시오.   
p-value값을 반올림하여 소수점 둘째 자리까지 계산  

> 귀무가설: "베스킨라빈스 쿼트 아이스크림의 중앙값은 620g이다."  
> 대립가설: "베스킨라빈스 쿼트 아이스크림의 중앙값은 620g보다 무겁다."

```python
import pandas as pd
data = {
    "weight": [630, 610, 625, 615, 622, 618, 623, 619, 620, 624, 616, 621, 617, 629, 626, 620, 618, 622, 625, 615, 
               628, 617, 624, 619, 621, 623, 620, 622, 618, 625, 616, 629, 620, 624, 617, 621, 623, 619, 625, 618,
               622, 620, 624, 617, 621, 623, 619, 625, 618, 622]
}
df = pd.DataFrame(data)

from scipy import stats 
med = 620  # median값
stats.wilcoxon(df['weight']-med, alternative ='greater')

```
```
WilcoxonResult(statistic=684.0, pvalue=0.029661945180945556)
/opt/conda/lib/python3.10/site-packages/scipy/stats/_morestats.py:4088:
UserWarning: Exact p-value calculation does not work if there are zeros. Switching to normal approximation.
  warnings.warn("Exact p-value calculation does not work if there are "
```
### 경고 메세지 원인 
* 데이터가 정규성을 띄고 있으면 df['weight']-Median 이 0 인 데이터가 다수 존재할 수 있다.

```python
# 정규성 검정 
from scipy import stats 
print(stats.shapiro(df))
# ShapiroResult(statistic=0.9823697209358215, pvalue=0.6552668809890747)
# 정규분포를 따른다고 할 수 있다. 
```
