문제 
--
성별과 시험합격은 독립적인가를 검정하시오  
1 검정 통계량?  
2 p-value?  
3 귀무가설 기준 (기각/채택)?  
4 남자의 합격 기대 빈도?  

```python
from scipy import stats 
st,p, dof,expected = stats.chi2_contingency(df) # 교차분할표가 들어간다.
print(st, p)
# 5.929494712103407 0.01488951060599475
# >> 귀무가설을 기각한다. (독립이 아니다.)
```
