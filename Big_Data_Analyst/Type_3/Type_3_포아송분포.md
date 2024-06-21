문제
--
> 한 서점에서는 평균적으로 하루에 3명의 고객이 특정 잡지를 구매합니다.   
> 이 데이터는 포아송 분포를 따른다고 가정할 때, 다음 질문에 대한 답을 구하세요.  

1-1. 하루에 정확히 5명의 고객이 잡지를 구매할 확률은 얼마입니까? (%로 값을 정수로 입력하시오)  

* 포아송 분포의 확률질량함수 : Probability Mass Function  
* ```stats.poisson.pmf(k, lamb)``` # k는 기대횟수, lambda는 알려진 횟수  
* λ는 단위 시간(또는 단위 공간)당 평균 발생 횟수이고, k는 특정 시간(또는 공간) 동안의 이벤트 발생 횟수

```python
# 모듈 사용 
from scipy import stats 
p = stats.poisson.pmf(5, lam)
p # probability mass function # 기대 확률 
# 0.10081881344492458

# 방법 2.
# 하루 당 평균 구매 횟수 Lambda = 3
# 하루 당 구매 발생 횟수 k = 5
import numpy as np 
import math 
lam = 3
k = 5
p = (np.exp(-lam)*(lam**k))/math.factorial(k)
p
# 0.10081881344492448
```

1-2. 하루에 적어도 2명의 고객이 잡지를 구매할 확률은 얼마입니까? (%로 값을 정수로 입력하시오)   
* 포아송 분포의 누적확률분포 : Cumulative Distribution Function
* ```stats.poisson.cdf(k, lamb)```
```python
c = stats.poisson.cdf(2, lam)
c # cumulative distribution function # 누적 확률 분포 
# 0.42319008112684364
```
