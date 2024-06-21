### 크리스마스 장식 종류와 지역에 따라 판매량에 유의미한 차이가 있는지 이원 분산 분석을 통해 검정하세요  
1. 크리스마스 장식 종류(트리, 조명, 장식품)가 판매량에 미치는 영향을 분석하세요. 이때, 장식 종류의 F-value, p-value를 구하시오  
2. 지역(북부, 남부, 동부, 서부)이 판매량에 미치는 영향을 분석하세요. 이때, 장식 종류의 F-value, p-value를 구하시오  
3. 크리스마스 장식 종류와 지역의 상호작용이 판매량에 미치는 영향을 분석하세요. 이때, 장식 종류의 F-value, p-value를 구하시오

```python
import pandas as pd 
df = pd.read_csv("/kaggle/input/bigdatacertificationkr/christmas_decoration_sales.csv")
df

from statsmodels.formula.api import ols 
from statsmodels.stats.anova import anova_lm 
model = ols('Sales ~ C(Region) + C(Decoration_Type) +C(Region):C(Decoration_Type)', data = df) 
lg = model.fit() 
anova_lm(lg)

```
```
	                             df	  sum_sq	    mean_sq	     F	     PR(>F)
C(Region)	                   3.0	 804.305556	268.101852	0.720381     0.549614
C(Decoration_Type)	           2.0	 1764.500000	882.250000	2.370578     0.114943
C(Region):C(Decoration_Type)	   6.0	 5153.944444	858.990741	2.308081     0.066915
Residual	                   24.0	 8932.000000	372.166667	 NaN	      NaN

# F-value, P-value
# 1. 2.370578, 0.114943
# 2. 0.720381, 0.549614
# 3. 2.308081, 0.066915
```
