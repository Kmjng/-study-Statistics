문제 1. 
-- 
> 세 가지 다른 교육 방법(A, B, C)을 사용하여 수험생들의 시험 성적을 개선시키는 효과를 평가하고자 한다. 30명의 학생들을 무작위로 세 그룹으로 배정하여 교육을 실시하였고, 시험을 보고 성적을 측정하였습니다. 다음은 각 그룹의 학생들의 성적 데이터입니다.
* 귀무가설(H0): 세 그룹(A, B, C) 간의 평균 성적 차이가 없다.    
* 대립가설(H1 또는 Ha): 세 그룹(A, B, C) 간의 평균 성적 차이가 있다.      
다음 주어진 데이터로 일원배치법을 수행하여 그룹 간의 평균 성적 차이가 있는지 검정하세요  
1-1. f값 (소수 둘째자리)  
1-2. p값 (소수 여섯째자리)  
1-3. 검정결과 출력  

### Tip
* 두 개 이상 그룹의 평균차이 검정 -> 분산분석
* 요인(독립변수)이 하나 -> 일원분산분석 (f_oneway, anova_lm)  
* 방법 1. stats.f_oneway(groupA, groupB, groupC)
* 방법 2. 코드 참고 (statsmodels 모듈 사용)   

 ```python
# 각 그룹의 데이터
groupA = [85, 92, 78, 88, 83, 90, 76, 84, 92, 87]
groupB = [79, 69, 84, 78, 79, 83, 79, 81, 86, 88]
groupC = [75, 68, 74, 65, 77, 72, 70, 73, 78, 75]

# 정규성 검정
from scipy import stats
s1, p1 = stats.shapiro(groupA)
s2, p2 = stats.shapiro(groupB)
s3, p3 = stats.shapiro(groupC)
print(p1, p2, p3) # 셋 다 정규성을 만족한다. 

# 등분산성 검정 
s, p = stats.levene(groupA,groupB, groupC)
print(p) # 모든 분산이 등분산이다.
# 0.5142781734466553 0.3544830083847046 0.7401155233383179


# One-way ANOVA 

# 방법 1
from scipy import stats 
s_a, p_a = stats.f_oneway(groupA, groupB, groupC)
print(p_a) # 유의하다. 세 그룹간 평균 차이가 있다. 
# 0.6834162323267926
# 1.752985223798021e-05

# 방법 2
'''
from statsmodels.stats.anova import anova_lm
from statsmodels.formula.api import ols 
# 종속변수가 문제에서 안주어졌으므로 방법 2로 하기에 어려움이 있다.
s_a, p_a = stats.anova_lm(model)'''
```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

