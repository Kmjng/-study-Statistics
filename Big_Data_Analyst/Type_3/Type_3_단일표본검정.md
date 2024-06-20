문제 1. 
-- 
> 다음은 22명의 학생들이 국어시험에서 받은 점수이다. 학생들의 평균이 75보다 크다고 할 수 있는가?  
* 귀무가설(H0): 모평균은 mu와 같다. (μ = mu), 학생들의 평균은 75이다  
* 대립가설(H1): 모평균은 mu보다 크다. (μ > mu), 학생들의 평균은 75보다 크다  
가정:    
* 모집단은 정규분포를 따른다.
* 표본의 크기가 충분히 크다.
검정통계량, p-value, 검정결과를 출력하시오

### Tip
* 하나의 집단에 대해 검정 -> 단일표본 t검정
* t-test 1 sample
* stats.ttest_1samp(<mark>표본</mark>, (<mark>검정할 모평균</mark>, alternative = {'greater'|'less'|'two-sided'})

 ```python
# 데이터
scores = [75, 80, 68, 72, 77, 82, 81, 79, 70, 74, 76, 78, 81, 73, 81, 78, 75, 72, 74, 79, 78, 79]

from scipy import stats 
statistic, p_val = stats.ttest_1samp(scores, 75, alternative ='greater')
print(p_val)
# 0.04597614747709146
# >> 유의수준 0.05 하에, 귀무가설 기각
# >> 표본의 평균이 75보다 크다.

```



> 출처 : https://www.kaggle.com/datasets/agileteam/bigdatacertificationkr (빅데이터 분석기사 실기 준비를 위한 캐글 놀이터)

