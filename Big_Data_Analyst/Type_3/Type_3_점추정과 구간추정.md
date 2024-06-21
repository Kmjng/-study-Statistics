점 추정과 구간 추정  
--
데이터셋은 어떤 도시의 일일 평균 온도 입니다.  
점추정: 데이터셋을 기반으로 이 도시의 평균 연간 온도를 점추정하세요. (반올림하여 소수 둘째자리까지)  
구간추정: 95% 신뢰수준에서 이 도시의 평균 연간 온도에 대한 신뢰구간을 구하세요. (반올림하여 소수 둘째자리까지)  

```python
import pandas as pd
temp = pd.read_csv("/kaggle/input/bigdatacertificationkr/daily_temperatures.csv")

# 평균 점추정 
point_estimate = temp['Daily Average Temperature'].mean().round(2)
point_estimate

# 신뢰구간을 위한 
n = temp.shape[0]
std = temp['Daily Average Temperature'].std(ddof=1)
# ddof ; delta DoF 1 (default는 0)

from scipy import stats

interval = stats.t.interval(0.95, df= n-1, loc = point_estimate, scale = std/(n**0.5) )
# loc ; 점추정 평균값
# scale ; 표준 오차 
interval
```
