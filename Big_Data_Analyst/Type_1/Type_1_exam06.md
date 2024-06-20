### 문제 1. 
> 주어진 데이터는 각 소방서의 출동/도착 시간 데이터이다. 
출동시간과 도착시간 차이가 <mark>평균적으로 가장 오래 걸린 소방서의 시간</mark>을
분으로 변환해 출력하시오 
(반올림 후 정수)
```python
df1 = pd.read_csv(file1)
df1.info()
```
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 100 entries, 0 to 99
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   소방서     100 non-null    object
 1   출동시간    100 non-null    object
 2   도착시간    100 non-null    object
dtypes: object(3)
memory usage: 2.5+ KB
```
시계열데이터 형 변환 
* ```pd.to_datetime```   
* ```pd.to_timedelta```   
```python
df1.출동시간 = pd.to_datetime(df1['출동시간']) # datetime 형으로 변환 
df1.도착시간 = pd.to_datetime(df1['도착시간'])
df1['소요시간'] = df1['도착시간'] - df1['출동시간']
# dt.total_seconds()
dir(df1['소요시간'].dt)
'''
AttributeError: Can only use .dt accessor with datetimelike 
values
'''

print(df1['소요시간'].dtype) # numpy.float64
df1['소요시간'] = pd.to_timedelta(df1['소요시간']) # timedelta 형으로 변환

df1.groupby('소방서')['소요시간'].mean().sort_values(ascending =False)
# 소방서9     80.658889
# 80분 0.66*60 초 ★★★★
0.66*60 # 39.6초
# 정답 : 81 분

```
주의 : ```dt.seconds```는 일 단위는 무시되고 나머지 시간, 분, 초 부분만 초 단위로 계산됨   

### 문제 2. 
> 학교에서 교사 한명당 맡은 학생 수가 가장 많은 학교를 찾고, 
그 학교의 전체 교사 수를 구하시오 (정수 출력)

```python
df2= pd.read_csv(file2)
df2.info() # 결측치 없음 
df2.교사수.value_counts() # 교사 0명 없음 
df2['교사당 학생수'] = df2.iloc[:,2:-1].sum(axis = 1)/df2.교사수
df2.sort_values('교사당 학생수', ascending = False)
# 학교 8, 교사 19명 
```
### 문제 3. 
> <mark>연도별로</mark> 총 범죄 건수(범죄 유형의 총합)의 월평균 값을 구한후 
그 값이 가장 큰 연도를 찾아, 해당 연도의 총 범죄 건수의 월평균 값을 
출력하시오 (정수 출력)
```python
df3 = pd.read_csv(file3)
df3.info() # 결측치 없음 
df3

df3.columns

df3['총 범죄 건수'] = df3.iloc[:, 1:-1].sum(axis =1)
# 주의 ★★★★ -2 까지 
df3.head(1)
# 연도 추출 
df3['연도']= df3['날짜'].str[:4]
df3.groupby('연도')['날짜'].count()
'''
연도
2020    11
2021     9
2022    12
2023     9
2024     9
Name: 날짜, dtype: int64 

월평균이기 때문에 12로 나눠야 한다. ★★★★
'''

df3_1 = df3.groupby('연도')['총 범죄 건수'].sum()/12
df3_1.sort_values(ascending =False)
# 2022    514.916667

# 515 건
```
