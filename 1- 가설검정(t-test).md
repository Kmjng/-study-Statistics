> d  
단일표본검정 
===
* 한집단의 평균이 모집단의 평균(기준값)과 같은지 검정한다. 
> * 귀무가설 : 표본의 평균이 모집단의 평균과 같다. (차이가 없다)
> * 대립가설 : 표본의 평균이 모집단의 평균과 다르다.
> > 기본 가정 ★  
> > 해당 표본이 <mark>**정규성**</mark>을 만족한다.  
> > *만족한다는 가정이 없다면?*
> > <span style="color:green"> Shaprio-Wilks test 또는 Kolmogorove-Smirnov test 시행해서 확인할 것 </span>
> > > Shapiro-Wilks test
> > > * 귀무가설 : 해당 표본은 정규분포를 따른다.
> > > * 대립가설 : 해당 표본은 정규분포를 따르지 않는다. 

```python
from scipy import stats
  statistic, p_val = stats.shapiro(data) # data: 리스트, 어레이, 시리즈 
  # 검정통계량, p-value 
```
**유의수준 5% 기준에서 p-value > 0.05 이어야 정규성 확보 ★**

☑️정규성을 만족하지 않으면 ?  
---
* 단일표본 t-test 대신 <U>비모수 검정</U>을 실시한다.    
📈<span style="color:green">**Wilcoxon 부호순위 검정** </span>
```python from scipy import stasts 
  statistic, p_val = stasts.wilcoxon(df[col], alternative = 'less') 
  # alternative : 대립가설 입장으로 작성
  # => less, greater, two-sided (단측/양측) ; default : two-sided
```
☑️정규성을 만족하면 단일표본t-test를 진행한다. (<mark>ttest_1samp</mark>)
---
``` python
from scipy import stats 
statistic, p_val, _ = stats.ttest_1samp(df[col], 120, alternative = 'two-sided') # 120은 μ₀ 
```
**유의수준 5% 기준에서 p-value < 0.05 이면 귀무가설을 기각한다. ★**

대응표본검정 
===
* 사전(Pre)과 사후(Post)를 비교한다.
* * 유의점 : Data 길이가 같아야 한다. 
> * 귀무가설 : 사전점수와 사후점수가 같다. (효과가 없다)  
> * 대립가설 : 사후점수가 사전점수보다 월등하다. (효과가 있다)   
> > 기본 가정 ★  
> > **위의 단일표본검정과 동일(참고)**  
> > * 정규성검정 : <mark>단, data 인자에 df['Diff'] = df['After']-df['Before'] 가 온다.</mark>  
> > * Shapiro test p-val < 0.05 이어서 비모수 검정을 시행한다면  
> > ``` python
> > stats.wilcoxon(df['After'],df['Before'], alternative= 'less')
> > ```

☑️정규성을 만족하면 대응표본t-test를 진행한다. (<mark>ttest_rel</mark>)
---
```python
from scipy import stats 
statistic, p_val, _ = stats.ttest_rel(df['After'], df['Before'], alternative = 'less') 
  # After가 먼저 오는게 좋다. 
  # alternative = 'less' => 다이어트 약 복용 전/후 로 생각하면 쉬움
```
**유의수준 5% 기준에서 p-value < 0.05 이면 귀무가설을 기각한다. ★**


독립표본검정
===
* 두 집단의 평균이 차이가 있는지를 검정
* 독립적인 두 표본에 대해 검정
> * 귀무가설 : 두 표본의 평균은 차이가 없다.   
> * 대립가설 : 두 표본의 평균은 차이가 있다.  
> > 기본 가정 ★    
> > 두 표본이 <mark>**정규성**</mark>을 만족한다. (하나라도 만족하지 않으면 비모수 검정(Mann-Whitney U 검정) 실시  
> > 두 표본이 <mark>**등분산**</mark>을 만족한다. (하나라도 만족하지 않으면 t-test의 <mark>equal_var</mark>매개변수에 <mark>False</mark>  
> > *만족한다는 가정이 없다면?*    
<mark>☑️1 - 정규성 검정 (Shapiro-Wilks test)</mark>
```python
from scipy import stats
print(stats.shapiro(A)) # 집단 A
print(stats.shapiro(B)) # 집단 B 
```
**유의수준 5% 기준에서 p-value > 0.05 이어야 정규성 확보 ★**

☑️정규성을 만족하지 않으면 ?    
---
* 독립표본 t-test 대신 <U>비모수 검정(Mann-Whitney U test)</U>을 실시한다.
* 📈 **Mann-Whitney U 검정**
```python
from scipy import stats
statistic, p_val = stats.mannwhitneyu(A, B, alternative='less') # 대립이 A < B 일 때
'''
예) 귀무가설(H0) : A그룹과 B그룹의 평균 점수 차이가 없다. (μ1 = μ2)
대립가설 (H1) : B그룹의 평균 점수가 더 높다. (μ1< μ2) 
'''
```
☑️정규성을 만족하면 독립표본t-test를 진행한다. (<mark>ttest_ind</mark>) 
---
``` python
from scipy import stats 
statistic, p_val = stats.ttest_ind(A, B, alternative = 'less', equal_var = {True | False} )
# 문제에서 등분산을 만족한다는 가정이 았다면 equal_var = True  
```
<mark>☑️2 - 등분산 검정 (Levene test)</mark>
```python
from scipy import stats
statistic, p_val = stats.levene(A, B)
```
>  Levene test (래빈검정)
> > * 귀무가설 : 두 표본의 분산이 같다. (등분산이다)
> > * 대립가설 : 두 표본의 분산이 다르다. (등분산이 아니다)
Levene test의 p_val < 0.05 이면 독립표본검정 equal_val 인수에 False가 온다.






