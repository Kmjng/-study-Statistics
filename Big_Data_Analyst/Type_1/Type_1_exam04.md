## 문제 1
> age 컬럼의 3사분위수와 1사분위수의 차를 절대값으로 구하고, 소수점 버려서, 정수로 출력
```python
import pandas as pd
file1 = r"C:\ITWILL\빅분기_실기_기출_자료\4회\basic1.csv"
file2 = r"C:\ITWILL\빅분기_실기_기출_자료\4회\fb.csv"
file3 = r"C:\ITWILL\빅분기_실기_기출_자료\4회\nf.csv"


df1 = pd.read_csv(file1)
df1.info()

help(df1.age.quantile)
Q1 = df1.age.quantile(0.25) # 26.875
Q3 = df1.age.quantile(0.75) # 77.0

print("답:",int(abs(Q3-Q1))) # int로 묶으면 소수점 버림
# 답: 50
```

## 문제 2
> (loves반응+wows반응)/(reactions반응) 비율이 0.4보다 크고 0.5보다 작으면서, type 컬럼이 'video'인 데이터의 갯수
```python
df2 = pd.read_csv(file2)
df2.info()

df2[df2['reactions'] ==0 ].shape # 121개 
# 0으로 나누면 inf이지만 일단 진행 

df2['ratio'] = (df2['loves']+df2['wows'])/df2['reactions']

con1 = df2['ratio'] > 0.4 
con2 = df2['ratio'] < 0.5 
con3 = df2['type'] == 'video'

df2[con1 & con2 & con3].shape 
 #90 개 
```

## 문제 3
> date_added가 2018년 1월 이면서 country가 United Kingdom 단독 제작인 데이터의 갯수
```python
df3 = pd.read_csv(file3)

df3.info()



df3[['show_id','title','date_added','country']].head(3)

'''
  show_id                 title          date_added        country
0      s1  Dick Johnson Is Dead  September 25, 2021  United States
1      s2         Blood & Water  September 24, 2021   South Africa
2      s3             Ganglands  September 24, 2021            NaN
'''


df3.date_added = pd.to_datetime(df3['date_added'])
df3.date_added = df3['date_added'].astype('str')

con1 = df3['date_added'].str.contains('2018-01')
con2 = df3['country']=='United Kingdom'

df3[con1 & con2]
 
# 6개 
```
