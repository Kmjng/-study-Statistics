문제 1.
-- 
> 어떤 특정 약물을 복용한 사람들의 평균 체온이 복용하지 않은 사람들의 평균 체온과 유의미하게 다른지 검정해보려고 합니다.
가정:  
* 약물을 복용한 그룹과 복용하지 않은 그룹의 체온 데이터가 각각 주어져 있다고 가정합니다.  
* 각 그룹의 체온은 정규분포를 따른다고 가정합니다.    
검정통계량, p-value, 검정결과를 출력하시오   

### Tip   
* <mark>평균</mark>차이 -> <mark>독립표본 t검정</mark>
* t-test independent    
* stats.ttest_ind(<mark>df['group1'], df['group2']</mark>) # (인수 list, array, series 가능)   
```python
from scipy import stats

# 데이터 수집
group1 = [36.8, 36.7, 37.1, 36.9, 37.2, 36.8, 36.9, 37.1, 36.7, 37.1]
group2 = [36.5, 36.6, 36.3, 36.6, 36.9, 36.7, 36.7, 36.8, 36.5, 36.7]

# 만약 정규성 가정이 없다면?
stat_1, p_val_1 = stats.shapiro(group1)
stat_2, p_val_2 = stats.shapiro(group2) 
print(f'group1의 p_val:{p_val_1}, group2의 p_val:{p_val_2}')
# group1의 p_val:0.20258860290050507, group2의 p_val:0.8497353196144104
# >> 둘 다 유의수준보다 크기 때문에 정규성을 만족한다고 볼 수 있다. 

# ttest independent
statistic, p_val = stats.ttest_ind(group1, group2)
print(p_val)
# 0.001321891476703691
# >> 신뢰수준 0.05 하에, 귀무가설 기각
# >> 두 표본의 평균에는 유의미한 차이가 있다. 
```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

