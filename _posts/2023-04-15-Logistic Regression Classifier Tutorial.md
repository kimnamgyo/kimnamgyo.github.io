---
layout: single
title:  "로지스틱 회귀"
categories: coding
tag: [python, blog, jekyll]
toc: true
author_profile: false
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>


<a class="anchor" id="0"></a>

# **로지스터 회귀**





로지스틱 회귀 분석을 사용하여 이항 분류 모델을 훈련해 내일 호주에 비가 올지 여부를 예측하는 로지스틱 회귀 분류기를 만들겠습니다.


<a class="anchor" id="0.1"></a>

# **목차**





1.	[Introduction to Logistic Regression](#1)

2.	[Logistic Regression intuition](#2)

3.	[Assumptions of Logistic Regression](#3)

4.	[Types of Logistic Regression](#4)

5.	[Import libraries](#5)

6.	[Import dataset](#6)

7.	[Exploratory data analysis](#7)

8.	[Declare feature vector and target variable](#8)

9.	[Split data into separate training and test set](#9)

10.	[Feature engineering](#10)

11.	[Feature scaling](#11)

12.	[Model training](#12)

13.	[Predict results](#13)

14.	[Check accuracy score](#14)

15.	[Confusion matrix](#15)

16.	[Classification metrices](#16)

17.	[Adjusting the threshold level](#17)

18.	[ROC - AUC](#18)

19.	[k-Fold Cross Validation](#19)

20.	[Hyperparameter optimization using GridSearch CV](#20)

21.	[Results and conclusion](#21)

22. [References](#22)



# **1. Introduction to Logistic Regression** <a class="discovery" id="1"></a>



데이터 과학자들이 새로운 분류 문제를 발견할 수 있는 경우, 가장 먼저 떠오르는 알고리즘은 **Logistic Regression**입니다. 개별 클래스 집합에 대한 관찰을 예측하는 데 사용되는 지도 학습 분류 알고리즘입니다. 실제로 관측치를 여러 범주로 분류하는 데 사용됩니다. 따라서, 그것의 출력은 본질적으로 별개입니다. **Logistic Regression**은 **Logit Regression**이라고도 합니다. 분류 문제를 해결하는 데 사용되는 가장 단순하고 간단하며 다용도의 분류 알고리즘 중 하나입니다.


# **2. Logistic Regression intuition** <a class="anchor" id="2"></a>



통계학에서 **Logistic Regression Model**은 주로 분류 목적으로 사용되는 널리 사용되는 통계 모델입니다. 즉, 관측치 집합이 주어지면 로지스틱 회귀 알고리즘을 사용하여 관측치를 두 개 이상의 이산 클래스로 분류할 수 있습니다. 따라서 대상 변수는 본질적으로 이산적입니다.





로지스틱 회귀 분석 알고리즘은 다음과 같이 작동합니다


## **선형 방정식 구현**





로지스틱 회귀 분석 알고리즘은 반응 값을 예측하기 위해 독립 변수 또는 설명 변수가 있는 선형 방정식을 구현하는 방식으로 작동합니다. 예를 들어, 우리는 공부한 시간의 수와 시험에 합격할 확률의 예를 고려합니다. 여기서 연구된 시간 수는 설명 변수이며 x1로 표시됩니다. 합격 확률은 반응 변수 또는 목표 변수이며 z로 표시됩니다.





만약 우리가 하나의 설명 변수(x1)와 하나의 반응 변수(z)를 가지고 있다면, 선형 방정식은 다음과 같은 방정식으로 수학적으로 주어질 것입니다



z = β0 + β1x1



여기서 계수 β0과 β1은 모형의 모수입니다.





설명 변수가 여러 개인 경우, 위의 방정식은 다음과 같이 확장될 수 있습니다



z = β0 + β1x1 + β2x2+….+ βnxn



여기서 계수 β0, β1, β2 및 βn은 모델의 매개변수입니다.



따라서 예측 반응 값은 위의 방정식에 의해 주어지며 z로 표시됩니다.


## **Sigmoid 함수**



z로 표시된 이 예측 반응 값은 0과 1 사이에 있는 확률 값으로 변환됩니다. 우리는 예측 값을 확률 값에 매핑하기 위해 시그모이드 함수를 사용합니다. 그런 다음 이 시그모이드 함수는 실제 값을 0과 1 사이의 확률 값으로 매핑합니다.



기계 학습에서 시그모이드 함수는 예측을 확률에 매핑하는 데 사용됩니다. 시그모이드 함수는 S자형 곡선을 가지고 있습니다. 그것은 시그모이드 곡선이라고도 불립니다.



Sigmoid 함수는 로지스틱 함수의 특수한 경우입니다. 그것은 다음과 같은 수학 공식에 의해 주어집니다.


### Sigmoid Function



![Sigmoid Function](https://miro.medium.com/max/970/1*Xu7B5y9gp0iL5ooBj7LtWw.png)


## **Decision boundary**



시그모이드 함수는 0과 1 사이의 확률 값을 반환합니다. 그런 다음 이 확률 값은 "0" 또는 "1"인 이산 클래스에 매핑됩니다. 이 확률 값을 이산 클래스(통과/실패, 예/아니오, 참/거짓)에 매핑하기 위해 임계값을 선택합니다. 이 임계값을 의사결정 경계라고 합니다. 이 임계값을 초과하면 확률 값을 클래스 1에 매핑하고 클래스 0에 매핑합니다.



수학적으로 다음과 같이 표현할 수 있습니다



p ◦ 0.5 => 클래스 = 1



p < 0.5 => 클래스 = 0



일반적으로 의사 결정 경계는 0.5로 설정됩니다. 따라서 확률 값이 0.8(> 0.5)이면 이 관측치를 클래스 1에 매핑합니다. 마찬가지로 확률 값이 0.2(< 0.5)이면 이 관측치를 클래스 0에 매핑합니다. 이것은 아래 그래프에 나와 있습니다


![Decision boundary in sigmoid function](https://ml-cheatsheet.readthedocs.io/en/latest/_images/logistic_regression_sigmoid_w_threshold.png)


## **예측하기**



이제 우리는 로지스틱 회귀 분석에서 시그모이드 함수와 결정 경계에 대해 알고 있습니다. 우리는 시그모이드 함수와 결정 경계에 대한 지식을 사용하여 예측 함수를 작성할 수 있습니다. 로지스틱 회귀 분석의 예측 함수는 관측치가 양수, 예 또는 참일 확률을 반환합니다. 이를 클래스 1이라고 하며 P(클래스 = 1)로 표시합니다. 확률이 1에 가까우면 관측치가 클래스 1에 있고 그렇지 않으면 클래스 0에 있다는 것을 모형에 대해 더 확신할 수 있습니다.


# **3. Assumptions of Logistic Regression** <a class="anchor" id="3"></a>



로지스틱 회귀 분석 모형에는 몇 가지 주요 가정이 필요합니다. 다음은 다음과 같습니다



1. 로지스틱 회귀 모형에서는 종속 변수가 이항, 다항식 또는 순서형이어야 합니다.



2. 관측치가 서로 독립적이어야 합니다. 따라서 관측치는 반복적인 측정에서 나와서는 안 됩니다.



3. 로지스틱 회귀 분석 알고리즘에는 독립 변수 간의 다중 공선성이 거의 또는 전혀 필요하지 않습니다. 즉, 독립 변수들이 서로 너무 높은 상관 관계를 맺어서는 안 됩니다.



4. 로지스틱 회귀 모형은 독립 변수와 로그 승산의 선형성을 가정합니다.



5. 로지스틱 회귀 분석 모형의 성공 여부는 표본 크기에 따라 달라집니다. 일반적으로 높은 정확도를 얻으려면 큰 표본 크기가 필요합니다.


# **4. Types of Logistic Regression** <a class="anchor" id="4"></a>



로지스틱 회귀 분석 모형은 대상 변수 범주를 기준으로 세 그룹으로 분류할 수 있습니다. 이 세 그룹은 아래에 설명되어 있습니다



### 1. 이항 로지스틱 회귀 분석



이항 로지스틱 회귀 분석에서 대상 변수에는 두 가지 범주가 있습니다. 범주의 일반적인 예는 예 또는 아니오, 양호 또는 불량, 참 또는 거짓, 스팸 또는 스팸 없음, 통과 또는 실패입니다.





### 2. 다항 로지스틱 회귀 분석



다항 로지스틱 회귀 분석에서 대상 변수에는 특정 순서가 아닌 세 개 이상의 범주가 있습니다. 따라서 세 개 이상의 공칭 범주가 있습니다. 그 예들은 사과, 망고, 오렌지 그리고 바나나와 같은 과일의 종류를 포함합니다.





### 3. 순서형 로지스틱 회귀 분석



순서형 로지스틱 회귀 분석에서 대상 변수에는 세 개 이상의 순서형 범주가 있습니다. 그래서, 범주와 관련된 본질적인 순서가 있습니다. 예를 들어, 학생들의 성적은 불량, 평균, 양호, 우수로 분류될 수 있습니다.


# **5. Import libraries** <a class="anchor" id="5"></a>



```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load in 

import io
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
import matplotlib.pyplot as plt # data visualization
import seaborn as sns # statistical data visualization
%matplotlib inline

# Input data files are available in the "../input/" directory.
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

# Any results you write to the current directory are saved as output.
```


```python
import warnings

warnings.filterwarnings('ignore')
```



# **6. Import dataset** <a class="anchor" id="6"></a>



```python
from google.colab import files
myfile = files.upload()
```

```python
df = pd.read_csv(io.BytesIO(myfile['weatherAUS.csv']))
```

# **7. Exploratory data analysis** <a class="anchor" id="7"></a>



데이터를 탐색해보겠습니다.


```python
# view dimensions of dataset

df.shape
```

<pre>
(145460, 23)
</pre>

```python
# preview the dataset

df.head()
```

<pre>
         Date Location  MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine  \
0  2008-12-01   Albury     13.4     22.9       0.6          NaN       NaN   
1  2008-12-02   Albury      7.4     25.1       0.0          NaN       NaN   
2  2008-12-03   Albury     12.9     25.7       0.0          NaN       NaN   
3  2008-12-04   Albury      9.2     28.0       0.0          NaN       NaN   
4  2008-12-05   Albury     17.5     32.3       1.0          NaN       NaN   

  WindGustDir  WindGustSpeed WindDir9am  ... Humidity9am  Humidity3pm  \
0           W           44.0          W  ...        71.0         22.0   
1         WNW           44.0        NNW  ...        44.0         25.0   
2         WSW           46.0          W  ...        38.0         30.0   
3          NE           24.0         SE  ...        45.0         16.0   
4           W           41.0        ENE  ...        82.0         33.0   

   Pressure9am  Pressure3pm  Cloud9am  Cloud3pm  Temp9am  Temp3pm  RainToday  \
0       1007.7       1007.1       8.0       NaN     16.9     21.8         No   
1       1010.6       1007.8       NaN       NaN     17.2     24.3         No   
2       1007.6       1008.7       NaN       2.0     21.0     23.2         No   
3       1017.6       1012.8       NaN       NaN     18.1     26.5         No   
4       1010.8       1006.0       7.0       8.0     17.8     29.7         No   

   RainTomorrow  
0            No  
1            No  
2            No  
3            No  
4            No  

[5 rows x 23 columns]
</pre>

```python
col_names = df.columns

col_names
```

<pre>
Index(['Date', 'Location', 'MinTemp', 'MaxTemp', 'Rainfall', 'Evaporation',
       'Sunshine', 'WindGustDir', 'WindGustSpeed', 'WindDir9am', 'WindDir3pm',
       'WindSpeed9am', 'WindSpeed3pm', 'Humidity9am', 'Humidity3pm',
       'Pressure9am', 'Pressure3pm', 'Cloud9am', 'Cloud3pm', 'Temp9am',
       'Temp3pm', 'RainToday', 'RainTomorrow'],
      dtype='object')
</pre>

```python
# view summary of dataset

df.info()
```

<pre>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 145460 entries, 0 to 145459
Data columns (total 23 columns):
 #   Column         Non-Null Count   Dtype  
---  ------         --------------   -----  
 0   Date           145460 non-null  object 
 1   Location       145460 non-null  object 
 2   MinTemp        143975 non-null  float64
 3   MaxTemp        144199 non-null  float64
 4   Rainfall       142199 non-null  float64
 5   Evaporation    82670 non-null   float64
 6   Sunshine       75625 non-null   float64
 7   WindGustDir    135134 non-null  object 
 8   WindGustSpeed  135197 non-null  float64
 9   WindDir9am     134894 non-null  object 
 10  WindDir3pm     141232 non-null  object 
 11  WindSpeed9am   143693 non-null  float64
 12  WindSpeed3pm   142398 non-null  float64
 13  Humidity9am    142806 non-null  float64
 14  Humidity3pm    140953 non-null  float64
 15  Pressure9am    130395 non-null  float64
 16  Pressure3pm    130432 non-null  float64
 17  Cloud9am       89572 non-null   float64
 18  Cloud3pm       86102 non-null   float64
 19  Temp9am        143693 non-null  float64
 20  Temp3pm        141851 non-null  float64
 21  RainToday      142199 non-null  object 
 22  RainTomorrow   142193 non-null  object 
dtypes: float64(16), object(7)
memory usage: 25.5+ MB
</pre>
### Types of variables



이 섹션에서는 데이터 세트를 범주형 변수와 숫자 변수로 분리합니다. 데이터 집합에는 범주형 변수와 숫자 변수가 혼합되어 있습니다. 범주형 변수에는 데이터 유형 개체가 있습니다. 숫자 변수의 데이터 유형은 float64입니다.





우선 범주형 변수를 찾아보겠습니다.



```python
# find categorical variables

categorical = [var for var in df.columns if df[var].dtype=='O']

print('There are {} categorical variables\n'.format(len(categorical)))

print('The categorical variables are :', categorical)
```

<pre>
There are 7 categorical variables

The categorical variables are : ['Date', 'Location', 'WindGustDir', 'WindDir9am', 'WindDir3pm', 'RainToday', 'RainTomorrow']
</pre>

```python
# view the categorical variables

df[categorical].head()
```

<pre>
         Date Location WindGustDir WindDir9am WindDir3pm RainToday  \
0  2008-12-01   Albury           W          W        WNW        No   
1  2008-12-02   Albury         WNW        NNW        WSW        No   
2  2008-12-03   Albury         WSW          W        WSW        No   
3  2008-12-04   Albury          NE         SE          E        No   
4  2008-12-05   Albury           W        ENE         NW        No   

  RainTomorrow  
0           No  
1           No  
2           No  
3           No  
4           No  
</pre>
### 범주형 변수 요약





- 날짜 변수가 있습니다. 날짜 열로 표시됩니다.





- 6개의 범주형 변수가 있습니다. `Location`, `WindGustDir`, `WindDir9am`, `WindDir3pm`, `RainToday`, `RainTomorrow`.





- `RainToday`, `RainTomorrow`라는 이분법적인 범주형 변수가 있습니다.





- Rain Tomorrow가 타깃 변수.


## 범주형 변수 내의 문제 탐색





먼저 범주형 변수에 대해 알아보겠습니다.





### 범주형 변수의 결측값



```python
# check missing values in categorical variables

df[categorical].isnull().sum()
```

<pre>
Date                0
Location            0
WindGustDir     10326
WindDir9am      10566
WindDir3pm       4228
RainToday        3261
RainTomorrow     3267
dtype: int64
</pre>

```python
# print categorical variables containing missing values

cat1 = [var for var in categorical if df[var].isnull().sum()!=0]

print(df[cat1].isnull().sum())
```

<pre>
WindGustDir     10326
WindDir9am      10566
WindDir3pm       4228
RainToday        3261
RainTomorrow     3267
dtype: int64
</pre>
데이터 세트에 결측값이 포함된 범주형 변수는 4개뿐임을 알 수 있습니다. 



`WindGustDir`, `WindDir9am`, `WindDir3pm`,`RainToday`.


### 범주형 변수의 빈도 카운트





이제 범주형 변수의 빈도 수를 확인하겠습니다.



```python
# view frequency of categorical variables

for var in categorical: 
    
    print(df[var].value_counts())
```

<pre>
2013-11-12    49
2014-09-01    49
2014-08-23    49
2014-08-24    49
2014-08-25    49
              ..
2007-11-29     1
2007-11-28     1
2007-11-27     1
2007-11-26     1
2008-01-31     1
Name: Date, Length: 3436, dtype: int64
Canberra            3436
Sydney              3344
Darwin              3193
Melbourne           3193
Brisbane            3193
Adelaide            3193
Perth               3193
Hobart              3193
Albany              3040
MountGambier        3040
Ballarat            3040
Townsville          3040
GoldCoast           3040
Cairns              3040
Launceston          3040
AliceSprings        3040
Bendigo             3040
Albury              3040
MountGinini         3040
Wollongong          3040
Newcastle           3039
Tuggeranong         3039
Penrith             3039
Woomera             3009
Nuriootpa           3009
Cobar               3009
CoffsHarbour        3009
Moree               3009
Sale                3009
PerthAirport        3009
PearceRAAF          3009
Witchcliffe         3009
BadgerysCreek       3009
Mildura             3009
NorfolkIsland       3009
MelbourneAirport    3009
Richmond            3009
SydneyAirport       3009
WaggaWagga          3009
Williamtown         3009
Dartmoor            3009
Watsonia            3009
Portland            3009
Walpole             3006
NorahHead           3004
SalmonGums          3001
Katherine           1578
Nhil                1578
Uluru               1578
Name: Location, dtype: int64
W      9915
SE     9418
N      9313
SSE    9216
E      9181
S      9168
WSW    9069
SW     8967
SSW    8736
WNW    8252
NW     8122
ENE    8104
ESE    7372
NE     7133
NNW    6620
NNE    6548
Name: WindGustDir, dtype: int64
N      11758
SE      9287
E       9176
SSE     9112
NW      8749
S       8659
W       8459
SW      8423
NNE     8129
NNW     7980
ENE     7836
NE      7671
ESE     7630
SSW     7587
WNW     7414
WSW     7024
Name: WindDir9am, dtype: int64
SE     10838
W      10110
S       9926
WSW     9518
SSE     9399
SW      9354
N       8890
WNW     8874
NW      8610
ESE     8505
E       8472
NE      8263
SSW     8156
NNW     7870
ENE     7857
NNE     6590
Name: WindDir3pm, dtype: int64
No     110319
Yes     31880
Name: RainToday, dtype: int64
No     110316
Yes     31877
Name: RainTomorrow, dtype: int64
</pre>

```python
# view frequency distribution of categorical variables

for var in categorical: 
    
    print(df[var].value_counts()/np.float(len(df)))
```

<pre>
2013-11-12    0.000337
2014-09-01    0.000337
2014-08-23    0.000337
2014-08-24    0.000337
2014-08-25    0.000337
                ...   
2007-11-29    0.000007
2007-11-28    0.000007
2007-11-27    0.000007
2007-11-26    0.000007
2008-01-31    0.000007
Name: Date, Length: 3436, dtype: float64
Canberra            0.023622
Sydney              0.022989
Darwin              0.021951
Melbourne           0.021951
Brisbane            0.021951
Adelaide            0.021951
Perth               0.021951
Hobart              0.021951
Albany              0.020899
MountGambier        0.020899
Ballarat            0.020899
Townsville          0.020899
GoldCoast           0.020899
Cairns              0.020899
Launceston          0.020899
AliceSprings        0.020899
Bendigo             0.020899
Albury              0.020899
MountGinini         0.020899
Wollongong          0.020899
Newcastle           0.020892
Tuggeranong         0.020892
Penrith             0.020892
Woomera             0.020686
Nuriootpa           0.020686
Cobar               0.020686
CoffsHarbour        0.020686
Moree               0.020686
Sale                0.020686
PerthAirport        0.020686
PearceRAAF          0.020686
Witchcliffe         0.020686
BadgerysCreek       0.020686
Mildura             0.020686
NorfolkIsland       0.020686
MelbourneAirport    0.020686
Richmond            0.020686
SydneyAirport       0.020686
WaggaWagga          0.020686
Williamtown         0.020686
Dartmoor            0.020686
Watsonia            0.020686
Portland            0.020686
Walpole             0.020665
NorahHead           0.020652
SalmonGums          0.020631
Katherine           0.010848
Nhil                0.010848
Uluru               0.010848
Name: Location, dtype: float64
W      0.068163
SE     0.064746
N      0.064024
SSE    0.063358
E      0.063117
S      0.063028
WSW    0.062347
SW     0.061646
SSW    0.060058
WNW    0.056730
NW     0.055837
ENE    0.055713
ESE    0.050681
NE     0.049038
NNW    0.045511
NNE    0.045016
Name: WindGustDir, dtype: float64
N      0.080833
SE     0.063846
E      0.063083
SSE    0.062643
NW     0.060147
S      0.059528
W      0.058153
SW     0.057906
NNE    0.055885
NNW    0.054860
ENE    0.053870
NE     0.052736
ESE    0.052454
SSW    0.052159
WNW    0.050969
WSW    0.048288
Name: WindDir9am, dtype: float64
SE     0.074508
W      0.069504
S      0.068239
WSW    0.065434
SSE    0.064616
SW     0.064306
N      0.061116
WNW    0.061006
NW     0.059192
ESE    0.058470
E      0.058243
NE     0.056806
SSW    0.056070
NNW    0.054104
ENE    0.054015
NNE    0.045305
Name: WindDir3pm, dtype: float64
No     0.758415
Yes    0.219167
Name: RainToday, dtype: float64
No     0.758394
Yes    0.219146
Name: RainTomorrow, dtype: float64
</pre>
### Number of labels: cardinality



범주형 변수 내의 레이블 수는 **cardinality**라고 합니다. 변수 내의 레이블 수가 많은 경우를 **high cardinality**라고 합니다. high cardinality는 기계 학습 모델에서 몇 가지 심각한 문제를 일으킬 수 있습니다. 그래서 카디널리티가 높은지 확인해보겠습니다.



```python
# check for cardinality in categorical variables

for var in categorical:
    
    print(var, ' contains ', len(df[var].unique()), ' labels')
```

<pre>
Date  contains  3436  labels
Location  contains  49  labels
WindGustDir  contains  17  labels
WindDir9am  contains  17  labels
WindDir3pm  contains  17  labels
RainToday  contains  3  labels
RainTomorrow  contains  3  labels
</pre>
사전 처리가 필요한 '날짜' 변수가 있음을 알 수 있습니다. 저는 다음 섹션에서 전처리를 할 것입니다.





다른 모든 변수에는 상대적으로 적은 수의 변수가 포함되어 있습니다.


### Feature Engineering of Date Variable



```python
df['Date'].dtypes
```

<pre>
dtype('O')
</pre>

```python
# parse the dates, currently coded as strings, into datetime format

df['Date'] = pd.to_datetime(df['Date'])
```


```python
# extract year from date

df['Year'] = df['Date'].dt.year

df['Year'].head()
```

<pre>
0    2008
1    2008
2    2008
3    2008
4    2008
Name: Year, dtype: int64
</pre>

```python
# extract month from date

df['Month'] = df['Date'].dt.month

df['Month'].head()
```

<pre>
0    12
1    12
2    12
3    12
4    12
Name: Month, dtype: int64
</pre>

```python
# extract day from date

df['Day'] = df['Date'].dt.day

df['Day'].head()
```

<pre>
0    1
1    2
2    3
3    4
4    5
Name: Day, dtype: int64
</pre>

```python
# again view the summary of dataset

df.info()
```

<pre>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 145460 entries, 0 to 145459
Data columns (total 26 columns):
 #   Column         Non-Null Count   Dtype         
---  ------         --------------   -----         
 0   Date           145460 non-null  datetime64[ns]
 1   Location       145460 non-null  object        
 2   MinTemp        143975 non-null  float64       
 3   MaxTemp        144199 non-null  float64       
 4   Rainfall       142199 non-null  float64       
 5   Evaporation    82670 non-null   float64       
 6   Sunshine       75625 non-null   float64       
 7   WindGustDir    135134 non-null  object        
 8   WindGustSpeed  135197 non-null  float64       
 9   WindDir9am     134894 non-null  object        
 10  WindDir3pm     141232 non-null  object        
 11  WindSpeed9am   143693 non-null  float64       
 12  WindSpeed3pm   142398 non-null  float64       
 13  Humidity9am    142806 non-null  float64       
 14  Humidity3pm    140953 non-null  float64       
 15  Pressure9am    130395 non-null  float64       
 16  Pressure3pm    130432 non-null  float64       
 17  Cloud9am       89572 non-null   float64       
 18  Cloud3pm       86102 non-null   float64       
 19  Temp9am        143693 non-null  float64       
 20  Temp3pm        141851 non-null  float64       
 21  RainToday      142199 non-null  object        
 22  RainTomorrow   142193 non-null  object        
 23  Year           145460 non-null  int64         
 24  Month          145460 non-null  int64         
 25  Day            145460 non-null  int64         
dtypes: datetime64[ns](1), float64(16), int64(3), object(6)
memory usage: 28.9+ MB
</pre>
날짜 변수에서 추가로 생성된 열이 세 개 있음을 알 수 있습니다. 이제 데이터 집합에서 원래의 "날짜" 변수를 삭제하겠습니다.



```python
# drop the original Date variable

df.drop('Date', axis=1, inplace = True)
```


```python
# preview the dataset again

df.head()
```

<pre>
  Location  MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine WindGustDir  \
0   Albury     13.4     22.9       0.6          NaN       NaN           W   
1   Albury      7.4     25.1       0.0          NaN       NaN         WNW   
2   Albury     12.9     25.7       0.0          NaN       NaN         WSW   
3   Albury      9.2     28.0       0.0          NaN       NaN          NE   
4   Albury     17.5     32.3       1.0          NaN       NaN           W   

   WindGustSpeed WindDir9am WindDir3pm  ...  Pressure3pm  Cloud9am  Cloud3pm  \
0           44.0          W        WNW  ...       1007.1       8.0       NaN   
1           44.0        NNW        WSW  ...       1007.8       NaN       NaN   
2           46.0          W        WSW  ...       1008.7       NaN       2.0   
3           24.0         SE          E  ...       1012.8       NaN       NaN   
4           41.0        ENE         NW  ...       1006.0       7.0       8.0   

   Temp9am  Temp3pm  RainToday  RainTomorrow  Year  Month  Day  
0     16.9     21.8         No            No  2008     12    1  
1     17.2     24.3         No            No  2008     12    2  
2     21.0     23.2         No            No  2008     12    3  
3     18.1     26.5         No            No  2008     12    4  
4     17.8     29.7         No            No  2008     12    5  

[5 rows x 25 columns]
</pre>
이제 데이터 집합에서 날짜 변수가 제거되었음을 확인할 수 있습니다.


### 범주형 변수 탐색





이제 범주형 변수를 하나씩 살펴보도록 하겠습니다



```python
# find categorical variables

categorical = [var for var in df.columns if df[var].dtype=='O']

print('There are {} categorical variables\n'.format(len(categorical)))

print('The categorical variables are :', categorical)
```

<pre>
There are 6 categorical variables

The categorical variables are : ['Location', 'WindGustDir', 'WindDir9am', 'WindDir3pm', 'RainToday', 'RainTomorrow']
</pre>
우리는 데이터 세트에 6개의 범주형 변수가 있다는 것을 알 수 있습니다. 날짜 변수가 제거되었습니다. 먼저 범주형 변수의 결측값을 확인하겠습니다.



```python
# check for missing values in categorical variables 

df[categorical].isnull().sum()
```

<pre>
Location            0
WindGustDir     10326
WindDir9am      10566
WindDir3pm       4228
RainToday        3261
RainTomorrow     3267
dtype: int64
</pre>
`WindGustDir`, `WindDir9am`, `WindDir3pm`, `RainToday`, `RainTomorrow` 변수에 결측값이 포함되어 있음을 알 수 있습니다. 저는 이 변수들을 하나씩 탐색할 것입니다.


### Explore `Location` variable



```python
# print number of labels in Location variable

print('Location contains', len(df.Location.unique()), 'labels')
```

<pre>
Location contains 49 labels
</pre>

```python
# check labels in location variable

df.Location.unique()
```

<pre>
array(['Albury', 'BadgerysCreek', 'Cobar', 'CoffsHarbour', 'Moree',
       'Newcastle', 'NorahHead', 'NorfolkIsland', 'Penrith', 'Richmond',
       'Sydney', 'SydneyAirport', 'WaggaWagga', 'Williamtown',
       'Wollongong', 'Canberra', 'Tuggeranong', 'MountGinini', 'Ballarat',
       'Bendigo', 'Sale', 'MelbourneAirport', 'Melbourne', 'Mildura',
       'Nhil', 'Portland', 'Watsonia', 'Dartmoor', 'Brisbane', 'Cairns',
       'GoldCoast', 'Townsville', 'Adelaide', 'MountGambier', 'Nuriootpa',
       'Woomera', 'Albany', 'Witchcliffe', 'PearceRAAF', 'PerthAirport',
       'Perth', 'SalmonGums', 'Walpole', 'Hobart', 'Launceston',
       'AliceSprings', 'Darwin', 'Katherine', 'Uluru'], dtype=object)
</pre>

```python
# check frequency distribution of values in Location variable

df.Location.value_counts()
```

<pre>
Canberra            3436
Sydney              3344
Darwin              3193
Melbourne           3193
Brisbane            3193
Adelaide            3193
Perth               3193
Hobart              3193
Albany              3040
MountGambier        3040
Ballarat            3040
Townsville          3040
GoldCoast           3040
Cairns              3040
Launceston          3040
AliceSprings        3040
Bendigo             3040
Albury              3040
MountGinini         3040
Wollongong          3040
Newcastle           3039
Tuggeranong         3039
Penrith             3039
Woomera             3009
Nuriootpa           3009
Cobar               3009
CoffsHarbour        3009
Moree               3009
Sale                3009
PerthAirport        3009
PearceRAAF          3009
Witchcliffe         3009
BadgerysCreek       3009
Mildura             3009
NorfolkIsland       3009
MelbourneAirport    3009
Richmond            3009
SydneyAirport       3009
WaggaWagga          3009
Williamtown         3009
Dartmoor            3009
Watsonia            3009
Portland            3009
Walpole             3006
NorahHead           3004
SalmonGums          3001
Katherine           1578
Nhil                1578
Uluru               1578
Name: Location, dtype: int64
</pre>

```python
# let's do One Hot Encoding of Location variable
# get k-1 dummy variables after One Hot Encoding 
# preview the dataset with head() method

pd.get_dummies(df.Location, drop_first=True).head()
```

<pre>
   Albany  Albury  AliceSprings  BadgerysCreek  Ballarat  Bendigo  Brisbane  \
0       0       1             0              0         0        0         0   
1       0       1             0              0         0        0         0   
2       0       1             0              0         0        0         0   
3       0       1             0              0         0        0         0   
4       0       1             0              0         0        0         0   

   Cairns  Canberra  Cobar  ...  Townsville  Tuggeranong  Uluru  WaggaWagga  \
0       0         0      0  ...           0            0      0           0   
1       0         0      0  ...           0            0      0           0   
2       0         0      0  ...           0            0      0           0   
3       0         0      0  ...           0            0      0           0   
4       0         0      0  ...           0            0      0           0   

   Walpole  Watsonia  Williamtown  Witchcliffe  Wollongong  Woomera  
0        0         0            0            0           0        0  
1        0         0            0            0           0        0  
2        0         0            0            0           0        0  
3        0         0            0            0           0        0  
4        0         0            0            0           0        0  

[5 rows x 48 columns]
</pre>
### Explore `WindGustDir` variable



```python
# print number of labels in WindGustDir variable

print('WindGustDir contains', len(df['WindGustDir'].unique()), 'labels')
```

<pre>
WindGustDir contains 17 labels
</pre>

```python
# check labels in WindGustDir variable

df['WindGustDir'].unique()
```

<pre>
array(['W', 'WNW', 'WSW', 'NE', 'NNW', 'N', 'NNE', 'SW', nan, 'ENE',
       'SSE', 'S', 'NW', 'SE', 'ESE', 'E', 'SSW'], dtype=object)
</pre>

```python
# check frequency distribution of values in WindGustDir variable

df.WindGustDir.value_counts()
```

<pre>
W      9915
SE     9418
N      9313
SSE    9216
E      9181
S      9168
WSW    9069
SW     8967
SSW    8736
WNW    8252
NW     8122
ENE    8104
ESE    7372
NE     7133
NNW    6620
NNE    6548
Name: WindGustDir, dtype: int64
</pre>

```python
# let's do One Hot Encoding of WindGustDir variable
# get k-1 dummy variables after One Hot Encoding 
# also add an additional dummy variable to indicate there was missing data
# preview the dataset with head() method

pd.get_dummies(df.WindGustDir, drop_first=True, dummy_na=True).head()
```

<pre>
   ENE  ESE  N  NE  NNE  NNW  NW  S  SE  SSE  SSW  SW  W  WNW  WSW  NaN
0    0    0  0   0    0    0   0  0   0    0    0   0  1    0    0    0
1    0    0  0   0    0    0   0  0   0    0    0   0  0    1    0    0
2    0    0  0   0    0    0   0  0   0    0    0   0  0    0    1    0
3    0    0  0   1    0    0   0  0   0    0    0   0  0    0    0    0
4    0    0  0   0    0    0   0  0   0    0    0   0  1    0    0    0
</pre>

```python
# sum the number of 1s per boolean variable over the rows of the dataset
# it will tell us how many observations we have for each category

pd.get_dummies(df.WindGustDir, drop_first=True, dummy_na=True).sum(axis=0)
```

<pre>
ENE     8104
ESE     7372
N       9313
NE      7133
NNE     6548
NNW     6620
NW      8122
S       9168
SE      9418
SSE     9216
SSW     8736
SW      8967
W       9915
WNW     8252
WSW     9069
NaN    10326
dtype: int64
</pre>
### Explore `WindDir9am` variable



```python
# print number of labels in WindDir9am variable

print('WindDir9am contains', len(df['WindDir9am'].unique()), 'labels')
```

<pre>
WindDir9am contains 17 labels
</pre>

```python
# check labels in WindDir9am variable

df['WindDir9am'].unique()
```

<pre>
array(['W', 'NNW', 'SE', 'ENE', 'SW', 'SSE', 'S', 'NE', nan, 'SSW', 'N',
       'WSW', 'ESE', 'E', 'NW', 'WNW', 'NNE'], dtype=object)
</pre>

```python
# check frequency distribution of values in WindDir9am variable

df['WindDir9am'].value_counts()
```

<pre>
N      11758
SE      9287
E       9176
SSE     9112
NW      8749
S       8659
W       8459
SW      8423
NNE     8129
NNW     7980
ENE     7836
NE      7671
ESE     7630
SSW     7587
WNW     7414
WSW     7024
Name: WindDir9am, dtype: int64
</pre>

```python
# let's do One Hot Encoding of WindDir9am variable
# get k-1 dummy variables after One Hot Encoding 
# also add an additional dummy variable to indicate there was missing data
# preview the dataset with head() method

pd.get_dummies(df.WindDir9am, drop_first=True, dummy_na=True).head()
```

<pre>
   ENE  ESE  N  NE  NNE  NNW  NW  S  SE  SSE  SSW  SW  W  WNW  WSW  NaN
0    0    0  0   0    0    0   0  0   0    0    0   0  1    0    0    0
1    0    0  0   0    0    1   0  0   0    0    0   0  0    0    0    0
2    0    0  0   0    0    0   0  0   0    0    0   0  1    0    0    0
3    0    0  0   0    0    0   0  0   1    0    0   0  0    0    0    0
4    1    0  0   0    0    0   0  0   0    0    0   0  0    0    0    0
</pre>

```python
# sum the number of 1s per boolean variable over the rows of the dataset
# it will tell us how many observations we have for each category

pd.get_dummies(df.WindDir9am, drop_first=True, dummy_na=True).sum(axis=0)
```

<pre>
ENE     7836
ESE     7630
N      11758
NE      7671
NNE     8129
NNW     7980
NW      8749
S       8659
SE      9287
SSE     9112
SSW     7587
SW      8423
W       8459
WNW     7414
WSW     7024
NaN    10566
dtype: int64
</pre>
WindDir9am 변수에 결측값이 10566개 있음을 알 수 있습니다.


### Explore `WindDir3pm` variable



```python
# print number of labels in WindDir3pm variable

print('WindDir3pm contains', len(df['WindDir3pm'].unique()), 'labels')
```

<pre>
WindDir3pm contains 17 labels
</pre>

```python
# check labels in WindDir3pm variable

df['WindDir3pm'].unique()
```

<pre>
array(['WNW', 'WSW', 'E', 'NW', 'W', 'SSE', 'ESE', 'ENE', 'NNW', 'SSW',
       'SW', 'SE', 'N', 'S', 'NNE', nan, 'NE'], dtype=object)
</pre>

```python
# check frequency distribution of values in WindDir3pm variable

df['WindDir3pm'].value_counts()
```

<pre>
SE     10838
W      10110
S       9926
WSW     9518
SSE     9399
SW      9354
N       8890
WNW     8874
NW      8610
ESE     8505
E       8472
NE      8263
SSW     8156
NNW     7870
ENE     7857
NNE     6590
Name: WindDir3pm, dtype: int64
</pre>

```python
# let's do One Hot Encoding of WindDir3pm variable
# get k-1 dummy variables after One Hot Encoding 
# also add an additional dummy variable to indicate there was missing data
# preview the dataset with head() method

pd.get_dummies(df.WindDir3pm, drop_first=True, dummy_na=True).head()
```

<pre>
   ENE  ESE  N  NE  NNE  NNW  NW  S  SE  SSE  SSW  SW  W  WNW  WSW  NaN
0    0    0  0   0    0    0   0  0   0    0    0   0  0    1    0    0
1    0    0  0   0    0    0   0  0   0    0    0   0  0    0    1    0
2    0    0  0   0    0    0   0  0   0    0    0   0  0    0    1    0
3    0    0  0   0    0    0   0  0   0    0    0   0  0    0    0    0
4    0    0  0   0    0    0   1  0   0    0    0   0  0    0    0    0
</pre>

```python
# sum the number of 1s per boolean variable over the rows of the dataset
# it will tell us how many observations we have for each category

pd.get_dummies(df.WindDir3pm, drop_first=True, dummy_na=True).sum(axis=0)
```

<pre>
ENE     7857
ESE     8505
N       8890
NE      8263
NNE     6590
NNW     7870
NW      8610
S       9926
SE     10838
SSE     9399
SSW     8156
SW      9354
W      10110
WNW     8874
WSW     9518
NaN     4228
dtype: int64
</pre>
WindDir3pm 변수에는 4228개의 결측값이 있습니다.


### Explore `RainToday` variable



```python
# print number of labels in RainToday variable

print('RainToday contains', len(df['RainToday'].unique()), 'labels')
```

<pre>
RainToday contains 3 labels
</pre>

```python
# check labels in WindGustDir variable

df['RainToday'].unique()
```

<pre>
array(['No', 'Yes', nan], dtype=object)
</pre>

```python
# check frequency distribution of values in WindGustDir variable

df.RainToday.value_counts()
```

<pre>
No     110319
Yes     31880
Name: RainToday, dtype: int64
</pre>

```python
# let's do One Hot Encoding of RainToday variable
# get k-1 dummy variables after One Hot Encoding 
# also add an additional dummy variable to indicate there was missing data
# preview the dataset with head() method

pd.get_dummies(df.RainToday, drop_first=True, dummy_na=True).head()
```

<pre>
   Yes  NaN
0    0    0
1    0    0
2    0    0
3    0    0
4    0    0
</pre>

```python
# sum the number of 1s per boolean variable over the rows of the dataset
# it will tell us how many observations we have for each category

pd.get_dummies(df.RainToday, drop_first=True, dummy_na=True).sum(axis=0)
```

<pre>
Yes    31880
NaN     3261
dtype: int64
</pre>
Rain Today 변수에는 3261개의 결측값이 있습니다.


### Explore `RainTomorrow` variable



```python
# print number of labels in RainTomorrow variable

print('RainTomorrow contains', len(df['RainTomorrow'].unique()), 'labels')
```

<pre>
RainTomorrow contains 3 labels
</pre>

```python
# check labels in RainTomorrow variable

df['RainTomorrow'].unique()
```

<pre>
array(['No', 'Yes', nan], dtype=object)
</pre>

```python
# check frequency distribution of values in RainTomorrow variable

df['RainTomorrow'].value_counts()
```

<pre>
No     110316
Yes     31877
Name: RainTomorrow, dtype: int64
</pre>

```python
# let's do One Hot Encoding of RainTomorrow variable
# get k-1 dummy variables after One Hot Encoding 
# also add an additional dummy variable to indicate there was missing data
# preview the dataset with head() method

pd.get_dummies(df.RainTomorrow, drop_first=True, dummy_na=True).head()
```

<pre>
   Yes  NaN
0    0    0
1    0    0
2    0    0
3    0    0
4    0    0
</pre>

```python
# sum the number of 1s per boolean variable over the rows of the dataset
# it will tell us how many observations we have for each category

pd.get_dummies(df.RainTomorrow, drop_first=True, dummy_na=True).sum(axis=0)
```

<pre>
Yes    31877
NaN     3267
dtype: int64
</pre>
RainTomorrow 변수에는 3267개의 결측값이 있습니다.


### Explore Numerical Variables



```python
# find numerical variables

numerical = [var for var in df.columns if df[var].dtype!='O']

print('There are {} numerical variables\n'.format(len(numerical)))

print('The numerical variables are :', numerical)
```

<pre>
There are 19 numerical variables

The numerical variables are : ['MinTemp', 'MaxTemp', 'Rainfall', 'Evaporation', 'Sunshine', 'WindGustSpeed', 'WindSpeed9am', 'WindSpeed3pm', 'Humidity9am', 'Humidity3pm', 'Pressure9am', 'Pressure3pm', 'Cloud9am', 'Cloud3pm', 'Temp9am', 'Temp3pm', 'Year', 'Month', 'Day']
</pre>

```python
# view the numerical variables

df[numerical].head()
```

<pre>
   MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine  WindGustSpeed  \
0     13.4     22.9       0.6          NaN       NaN           44.0   
1      7.4     25.1       0.0          NaN       NaN           44.0   
2     12.9     25.7       0.0          NaN       NaN           46.0   
3      9.2     28.0       0.0          NaN       NaN           24.0   
4     17.5     32.3       1.0          NaN       NaN           41.0   

   WindSpeed9am  WindSpeed3pm  Humidity9am  Humidity3pm  Pressure9am  \
0          20.0          24.0         71.0         22.0       1007.7   
1           4.0          22.0         44.0         25.0       1010.6   
2          19.0          26.0         38.0         30.0       1007.6   
3          11.0           9.0         45.0         16.0       1017.6   
4           7.0          20.0         82.0         33.0       1010.8   

   Pressure3pm  Cloud9am  Cloud3pm  Temp9am  Temp3pm  Year  Month  Day  
0       1007.1       8.0       NaN     16.9     21.8  2008     12    1  
1       1007.8       NaN       NaN     17.2     24.3  2008     12    2  
2       1008.7       NaN       2.0     21.0     23.2  2008     12    3  
3       1012.8       NaN       NaN     18.1     26.5  2008     12    4  
4       1006.0       7.0       8.0     17.8     29.7  2008     12    5  
</pre>
### 수치 변수 요약





- 16개의 숫자 변수가 있습니다.





- `MinTemp`, `MaxTemp`, `Rainfall`, `Evaporation`, `Sunshine`, `WindGustSpeed`, `WindSpeed9am`, `WindSpeed3pm`, `Humidity9am`, `Humidity3pm`, `Pressure9am`, `Pressure3pm`, `Cloud9am`, `Cloud3pm`, `Temp9am` and `Temp3pm`.등이 이를 알려줍니다.





- 모든 숫자 변수는 연속형입니다.


## 수치 변수 내의 문제 탐색





이제 수치 변수를 살펴보겠습니다.





### 숫자 변수의 결측값



```python
# check missing values in numerical variables

df[numerical].isnull().sum()
```

<pre>
MinTemp           1485
MaxTemp           1261
Rainfall          3261
Evaporation      62790
Sunshine         69835
WindGustSpeed    10263
WindSpeed9am      1767
WindSpeed3pm      3062
Humidity9am       2654
Humidity3pm       4507
Pressure9am      15065
Pressure3pm      15028
Cloud9am         55888
Cloud3pm         59358
Temp9am           1767
Temp3pm           3609
Year                 0
Month                0
Day                  0
dtype: int64
</pre>
16개의 수치 변수에 결측값이 모두 포함되어 있음을 알 수 있습니다.


### Outliers in numerical variables



```python
# view summary statistics in numerical variables

print(round(df[numerical].describe()),2)
```

<pre>
        MinTemp   MaxTemp  Rainfall  Evaporation  Sunshine  WindGustSpeed  \
count  143975.0  144199.0  142199.0      82670.0   75625.0       135197.0   
mean       12.0      23.0       2.0          5.0       8.0           40.0   
std         6.0       7.0       8.0          4.0       4.0           14.0   
min        -8.0      -5.0       0.0          0.0       0.0            6.0   
25%         8.0      18.0       0.0          3.0       5.0           31.0   
50%        12.0      23.0       0.0          5.0       8.0           39.0   
75%        17.0      28.0       1.0          7.0      11.0           48.0   
max        34.0      48.0     371.0        145.0      14.0          135.0   

       WindSpeed9am  WindSpeed3pm  Humidity9am  Humidity3pm  Pressure9am  \
count      143693.0      142398.0     142806.0     140953.0     130395.0   
mean           14.0          19.0         69.0         52.0       1018.0   
std             9.0           9.0         19.0         21.0          7.0   
min             0.0           0.0          0.0          0.0        980.0   
25%             7.0          13.0         57.0         37.0       1013.0   
50%            13.0          19.0         70.0         52.0       1018.0   
75%            19.0          24.0         83.0         66.0       1022.0   
max           130.0          87.0        100.0        100.0       1041.0   

       Pressure3pm  Cloud9am  Cloud3pm   Temp9am   Temp3pm      Year  \
count     130432.0   89572.0   86102.0  143693.0  141851.0  145460.0   
mean        1015.0       4.0       5.0      17.0      22.0    2013.0   
std            7.0       3.0       3.0       6.0       7.0       3.0   
min          977.0       0.0       0.0      -7.0      -5.0    2007.0   
25%         1010.0       1.0       2.0      12.0      17.0    2011.0   
50%         1015.0       5.0       5.0      17.0      21.0    2013.0   
75%         1020.0       7.0       7.0      22.0      26.0    2015.0   
max         1040.0       9.0       9.0      40.0      47.0    2017.0   

          Month       Day  
count  145460.0  145460.0  
mean        6.0      16.0  
std         3.0       9.0  
min         1.0       1.0  
25%         3.0       8.0  
50%         6.0      16.0  
75%         9.0      23.0  
max        12.0      31.0   2
</pre>
자세히 살펴보면, "Rainfall", "Evaporation", "WindSpeed9am", "WindSpeed3pm" 열에 특이치가 포함되어 있을 수 있음을 알 수 있습니다.





상자 그림을 그려 위 변수의 특이치를 시각화합니다.



```python
# draw boxplots to visualize outliers

plt.figure(figsize=(15,10))


plt.subplot(2, 2, 1)
fig = df.boxplot(column='Rainfall')
fig.set_title('')
fig.set_ylabel('Rainfall')


plt.subplot(2, 2, 2)
fig = df.boxplot(column='Evaporation')
fig.set_title('')
fig.set_ylabel('Evaporation')


plt.subplot(2, 2, 3)
fig = df.boxplot(column='WindSpeed9am')
fig.set_title('')
fig.set_ylabel('WindSpeed9am')


plt.subplot(2, 2, 4)
fig = df.boxplot(column='WindSpeed3pm')
fig.set_title('')
fig.set_ylabel('WindSpeed3pm')
```

<pre>
Text(0, 0.5, 'WindSpeed3pm')
</pre>
<pre>
<Figure size 1500x1000 with 4 Axes>
</pre>
위의 상자 그림은 이러한 변수에 특이치가 많다는 것을 확인합니다.


### 변수 분포 확인





이제 히스토그램을 그려 분포가 정규 분포인지 치우쳐 있는지 확인합니다. 변수가 정규 분포를 따르는 경우에는 극단값 분석을 수행하고, 그렇지 않은 경우에는 IQR(InterQuantile Range)을 찾습니다.



```python
# plot histogram to check distribution

plt.figure(figsize=(15,10))


plt.subplot(2, 2, 1)
fig = df.Rainfall.hist(bins=10)
fig.set_xlabel('Rainfall')
fig.set_ylabel('RainTomorrow')


plt.subplot(2, 2, 2)
fig = df.Evaporation.hist(bins=10)
fig.set_xlabel('Evaporation')
fig.set_ylabel('RainTomorrow')


plt.subplot(2, 2, 3)
fig = df.WindSpeed9am.hist(bins=10)
fig.set_xlabel('WindSpeed9am')
fig.set_ylabel('RainTomorrow')


plt.subplot(2, 2, 4)
fig = df.WindSpeed3pm.hist(bins=10)
fig.set_xlabel('WindSpeed3pm')
fig.set_ylabel('RainTomorrow')
```

<pre>
Text(0, 0.5, 'RainTomorrow')
</pre>
<pre>
<Figure size 1500x1000 with 4 Axes>
</pre>
네 가지 변수가 모두 치우쳐 있음을 알 수 있습니다. 따라서 특이치를 찾기 위해 분위수 범위를 사용합니다.



```python
# find outliers for Rainfall variable

IQR = df.Rainfall.quantile(0.75) - df.Rainfall.quantile(0.25)
Lower_fence = df.Rainfall.quantile(0.25) - (IQR * 3)
Upper_fence = df.Rainfall.quantile(0.75) + (IQR * 3)
print('Rainfall outliers are values < {lowerboundary} or > {upperboundary}'.format(lowerboundary=Lower_fence, upperboundary=Upper_fence))
```

<pre>
Rainfall outliers are values < -2.4000000000000004 or > 3.2
</pre>
'Rainfall'의 경우 최소값과 최대값은 0.0과 371.0입니다. 따라서 특이치는 3.2보다 큰 값입니다.



```python
# find outliers for Evaporation variable

IQR = df.Evaporation.quantile(0.75) - df.Evaporation.quantile(0.25)
Lower_fence = df.Evaporation.quantile(0.25) - (IQR * 3)
Upper_fence = df.Evaporation.quantile(0.75) + (IQR * 3)
print('Evaporation outliers are values < {lowerboundary} or > {upperboundary}'.format(lowerboundary=Lower_fence, upperboundary=Upper_fence))
```

<pre>
Evaporation outliers are values < -11.800000000000002 or > 21.800000000000004
</pre>
`Evaporation`의 경우 최소값과 최대값은 0.0과 145.0입니다. 따라서 특이치는 21.8보다 큰 값입니다.



```python
# find outliers for WindSpeed9am variable

IQR = df.WindSpeed9am.quantile(0.75) - df.WindSpeed9am.quantile(0.25)
Lower_fence = df.WindSpeed9am.quantile(0.25) - (IQR * 3)
Upper_fence = df.WindSpeed9am.quantile(0.75) + (IQR * 3)
print('WindSpeed9am outliers are values < {lowerboundary} or > {upperboundary}'.format(lowerboundary=Lower_fence, upperboundary=Upper_fence))
```

<pre>
WindSpeed9am outliers are values < -29.0 or > 55.0
</pre>
WindSpeed9am`의 경우 최소값과 최대값은 0.0과 130.0입니다. 따라서 특이치는 55.0보다 큰 값입니다.



```python
# find outliers for WindSpeed3pm variable

IQR = df.WindSpeed3pm.quantile(0.75) - df.WindSpeed3pm.quantile(0.25)
Lower_fence = df.WindSpeed3pm.quantile(0.25) - (IQR * 3)
Upper_fence = df.WindSpeed3pm.quantile(0.75) + (IQR * 3)
print('WindSpeed3pm outliers are values < {lowerboundary} or > {upperboundary}'.format(lowerboundary=Lower_fence, upperboundary=Upper_fence))
```

<pre>
WindSpeed3pm outliers are values < -20.0 or > 57.0
</pre>
`WindSpeed3pm`의 경우 최소값과 최대값은 0.0과 87.0입니다. 따라서 특이치는 57.0보다 큰 값입니다.


# **8. Declare feature vector and target variable** <a class="anchor" id="8"></a>



```python
X = df.drop(['RainTomorrow'], axis=1)

y = df['RainTomorrow'].fillna('No')

# 'RainTomorrow'의 null 값은 'NO'로 채우겠습니다.
```

# **9. Split data into separate training and test set** <a class="anchor" id="9"></a>



```python
# split X and y into training and testing sets

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
```


```python
# check the shape of X_train and X_test

X_train.shape, X_test.shape
```

<pre>
((116368, 24), (29092, 24))
</pre>
# **10. Feature Engineering** <a class="anchor" id="10"></a>


**Feature Engineering**은 원시 데이터를 유용한 기능으로 변환하여 모델을 더 잘 이해하고 예측력을 높이는 과정입니다. 저는 다양한 유형의 변수에 대해 피쳐 엔지니어링을 수행할 것입니다.





먼저 범주형 변수와 숫자형 변수를 다시 별도로 표시하겠습니다.



```python
# check data types in X_train

X_train.dtypes
```

<pre>
Location          object
MinTemp          float64
MaxTemp          float64
Rainfall         float64
Evaporation      float64
Sunshine         float64
WindGustDir       object
WindGustSpeed    float64
WindDir9am        object
WindDir3pm        object
WindSpeed9am     float64
WindSpeed3pm     float64
Humidity9am      float64
Humidity3pm      float64
Pressure9am      float64
Pressure3pm      float64
Cloud9am         float64
Cloud3pm         float64
Temp9am          float64
Temp3pm          float64
RainToday         object
Year               int64
Month              int64
Day                int64
dtype: object
</pre>

```python
# display categorical variables

categorical = [col for col in X_train.columns if X_train[col].dtypes == 'O']

categorical
```

<pre>
['Location', 'WindGustDir', 'WindDir9am', 'WindDir3pm', 'RainToday']
</pre>

```python
# display numerical variables

numerical = [col for col in X_train.columns if X_train[col].dtypes != 'O']

numerical
```

<pre>
['MinTemp',
 'MaxTemp',
 'Rainfall',
 'Evaporation',
 'Sunshine',
 'WindGustSpeed',
 'WindSpeed9am',
 'WindSpeed3pm',
 'Humidity9am',
 'Humidity3pm',
 'Pressure9am',
 'Pressure3pm',
 'Cloud9am',
 'Cloud3pm',
 'Temp9am',
 'Temp3pm',
 'Year',
 'Month',
 'Day']
</pre>
### Engineering missing values in numerical variables






```python
# check missing values in numerical variables in X_train

X_train[numerical].isnull().sum()
```

<pre>
MinTemp           1183
MaxTemp           1019
Rainfall          2617
Evaporation      50355
Sunshine         55899
WindGustSpeed     8218
WindSpeed9am      1409
WindSpeed3pm      2456
Humidity9am       2147
Humidity3pm       3598
Pressure9am      12091
Pressure3pm      12064
Cloud9am         44796
Cloud3pm         47557
Temp9am           1415
Temp3pm           2865
Year                 0
Month                0
Day                  0
dtype: int64
</pre>

```python
# check missing values in numerical variables in X_test

X_test[numerical].isnull().sum()
```

<pre>
MinTemp            302
MaxTemp            242
Rainfall           644
Evaporation      12435
Sunshine         13936
WindGustSpeed     2045
WindSpeed9am       358
WindSpeed3pm       606
Humidity9am        507
Humidity3pm        909
Pressure9am       2974
Pressure3pm       2964
Cloud9am         11092
Cloud3pm         11801
Temp9am            352
Temp3pm            744
Year                 0
Month                0
Day                  0
dtype: int64
</pre>

```python
# print percentage of missing values in the numerical variables in training set

for col in numerical:
    if X_train[col].isnull().mean()>0:
        print(col, round(X_train[col].isnull().mean(),4))
```

<pre>
MinTemp 0.0102
MaxTemp 0.0088
Rainfall 0.0225
Evaporation 0.4327
Sunshine 0.4804
WindGustSpeed 0.0706
WindSpeed9am 0.0121
WindSpeed3pm 0.0211
Humidity9am 0.0185
Humidity3pm 0.0309
Pressure9am 0.1039
Pressure3pm 0.1037
Cloud9am 0.385
Cloud3pm 0.4087
Temp9am 0.0122
Temp3pm 0.0246
</pre>
### 가정





데이터가 랜덤으로 완전히 누락되었다고 가정합니다(MCAR). 결측값을 귀속시키는 데 사용할 수 있는 두 가지 방법이 있습니다. 하나는 평균 또는 중위수 귀책이고 다른 하나는 랜덤 표본 귀책입니다. 데이터 집합에 특이치가 있을 경우 중위수 귀책을 사용해야 합니다. 중위수 귀인은 특이치에 강하므로 중위수 귀인을 사용합니다.





결측값을 데이터의 적절한 통계적 측도(이 경우 중위수)로 귀속시킵니다. 귀속은 교육 세트에 대해 수행된 다음 테스트 세트에 전파되어야 합니다. 즉, 트레인과 테스트 세트 모두에서 결측값을 채우기 위해 사용되는 통계적 측정값은 트레인 세트에서만 추출되어야 합니다. 이는 과적합을 방지하기 위한 것입니다.



```python
# impute missing values in X_train and X_test with respective column median in X_train

for df1 in [X_train, X_test]:
    for col in numerical:
        col_median=X_train[col].median()
        df1[col].fillna(col_median, inplace=True)
```


```python
# check again missing values in numerical variables in X_train

X_train[numerical].isnull().sum()
```

<pre>
MinTemp          0
MaxTemp          0
Rainfall         0
Evaporation      0
Sunshine         0
WindGustSpeed    0
WindSpeed9am     0
WindSpeed3pm     0
Humidity9am      0
Humidity3pm      0
Pressure9am      0
Pressure3pm      0
Cloud9am         0
Cloud3pm         0
Temp9am          0
Temp3pm          0
Year             0
Month            0
Day              0
dtype: int64
</pre>

```python
# check missing values in numerical variables in X_test

X_test[numerical].isnull().sum()
```

<pre>
MinTemp          0
MaxTemp          0
Rainfall         0
Evaporation      0
Sunshine         0
WindGustSpeed    0
WindSpeed9am     0
WindSpeed3pm     0
Humidity9am      0
Humidity3pm      0
Pressure9am      0
Pressure3pm      0
Cloud9am         0
Cloud3pm         0
Temp9am          0
Temp3pm          0
Year             0
Month            0
Day              0
dtype: int64
</pre>
이제 훈련 및 테스트 세트의 숫자 열에 결측값이 없음을 알 수 있습니다.


### 범주형 변수의 결측값 엔지니어링



```python
# print percentage of missing values in the categorical variables in training set

X_train[categorical].isnull().mean()
```

<pre>
Location       0.000000
WindGustDir    0.071068
WindDir9am     0.072597
WindDir3pm     0.028951
RainToday      0.022489
dtype: float64
</pre>

```python
# print categorical variables with missing data

for col in categorical:
    if X_train[col].isnull().mean()>0:
        print(col, (X_train[col].isnull().mean()))
```

<pre>
WindGustDir 0.07106764746322013
WindDir9am 0.07259727760208992
WindDir3pm 0.028951258077822083
RainToday 0.02248900041248453
</pre>

```python
# impute missing categorical variables with most frequent value

for df2 in [X_train, X_test]:
    df2['WindGustDir'].fillna(X_train['WindGustDir'].mode()[0], inplace=True)
    df2['WindDir9am'].fillna(X_train['WindDir9am'].mode()[0], inplace=True)
    df2['WindDir3pm'].fillna(X_train['WindDir3pm'].mode()[0], inplace=True)
    df2['RainToday'].fillna(X_train['RainToday'].mode()[0], inplace=True)
```


```python
# check missing values in categorical variables in X_train

X_train[categorical].isnull().sum()
```

<pre>
Location       0
WindGustDir    0
WindDir9am     0
WindDir3pm     0
RainToday      0
dtype: int64
</pre>

```python
# check missing values in categorical variables in X_test

X_test[categorical].isnull().sum()
```

<pre>
Location       0
WindGustDir    0
WindDir9am     0
WindDir3pm     0
RainToday      0
dtype: int64
</pre>
마지막으로 X_train과 X_test에서 결측값을 확인하겠습니다.



```python
# check missing values in X_train

X_train.isnull().sum()
```

<pre>
Location         0
MinTemp          0
MaxTemp          0
Rainfall         0
Evaporation      0
Sunshine         0
WindGustDir      0
WindGustSpeed    0
WindDir9am       0
WindDir3pm       0
WindSpeed9am     0
WindSpeed3pm     0
Humidity9am      0
Humidity3pm      0
Pressure9am      0
Pressure3pm      0
Cloud9am         0
Cloud3pm         0
Temp9am          0
Temp3pm          0
RainToday        0
Year             0
Month            0
Day              0
dtype: int64
</pre>

```python
# check missing values in X_test

X_test.isnull().sum()
```

<pre>
Location         0
MinTemp          0
MaxTemp          0
Rainfall         0
Evaporation      0
Sunshine         0
WindGustDir      0
WindGustSpeed    0
WindDir9am       0
WindDir3pm       0
WindSpeed9am     0
WindSpeed3pm     0
Humidity9am      0
Humidity3pm      0
Pressure9am      0
Pressure3pm      0
Cloud9am         0
Cloud3pm         0
Temp9am          0
Temp3pm          0
RainToday        0
Year             0
Month            0
Day              0
dtype: int64
</pre>
X_train 및 X_test에서 결측값이 없음을 알 수 있습니다.


### 숫자 변수의 공학적 특이치





우리는 'Rainfall', 'Evaporation', 'WindSpeed9am', 'WindSpeed3pm' 열에 특이치가 포함되어 있는 것을 보았습니다. 최상위 코드화 방법을 사용하여 최대값을 상한으로 설정하고 위 변수에서 특이치를 제거합니다.



```python
def max_value(df3, variable, top):
    return np.where(df3[variable]>top, top, df3[variable])

for df3 in [X_train, X_test]:
    df3['Rainfall'] = max_value(df3, 'Rainfall', 3.2)
    df3['Evaporation'] = max_value(df3, 'Evaporation', 21.8)
    df3['WindSpeed9am'] = max_value(df3, 'WindSpeed9am', 55)
    df3['WindSpeed3pm'] = max_value(df3, 'WindSpeed3pm', 57)
```


```python
X_train.Rainfall.max(), X_test.Rainfall.max()
```

<pre>
(3.2, 3.2)
</pre>

```python
X_train.Evaporation.max(), X_test.Evaporation.max()
```

<pre>
(21.8, 21.8)
</pre>

```python
X_train.WindSpeed9am.max(), X_test.WindSpeed9am.max()
```

<pre>
(55.0, 55.0)
</pre>

```python
X_train.WindSpeed3pm.max(), X_test.WindSpeed3pm.max()
```

<pre>
(57.0, 57.0)
</pre>

```python
X_train[numerical].describe()
```

<pre>
             MinTemp        MaxTemp       Rainfall    Evaporation  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean       12.190189      23.203107       0.670800       5.093362   
std         6.366893       7.085408       1.181512       2.800200   
min        -8.500000      -4.800000       0.000000       0.000000   
25%         7.700000      18.000000       0.000000       4.000000   
50%        12.000000      22.600000       0.000000       4.700000   
75%        16.800000      28.200000       0.600000       5.200000   
max        31.900000      48.100000       3.200000      21.800000   

            Sunshine  WindGustSpeed   WindSpeed9am   WindSpeed3pm  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        7.982476      39.982091      14.029381      18.687466   
std         2.761639      13.127953       8.835596       8.700618   
min         0.000000       6.000000       0.000000       0.000000   
25%         8.200000      31.000000       7.000000      13.000000   
50%         8.400000      39.000000      13.000000      19.000000   
75%         8.600000      46.000000      19.000000      24.000000   
max        14.500000     135.000000      55.000000      57.000000   

         Humidity9am    Humidity3pm    Pressure9am    Pressure3pm  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean       68.950691      51.605828    1017.639891    1015.244946   
std        18.811437      20.439999       6.728234       6.661517   
min         0.000000       0.000000     980.500000     977.100000   
25%        57.000000      37.000000    1013.500000    1011.100000   
50%        70.000000      52.000000    1017.600000    1015.200000   
75%        83.000000      65.000000    1021.800000    1019.400000   
max       100.000000     100.000000    1041.000000    1039.600000   

            Cloud9am       Cloud3pm        Temp9am        Temp3pm  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        4.664092       4.710728      16.979454      21.657195   
std         2.280687       2.106040       6.449641       6.848293   
min         0.000000       0.000000      -7.200000      -5.400000   
25%         3.000000       4.000000      12.300000      16.700000   
50%         5.000000       5.000000      16.700000      21.100000   
75%         6.000000       6.000000      21.500000      26.200000   
max         9.000000       8.000000      40.200000      46.700000   

                Year          Month            Day  
count  116368.000000  116368.000000  116368.000000  
mean     2012.767058       6.395091      15.731954  
std         2.538401       3.425451       8.796931  
min      2007.000000       1.000000       1.000000  
25%      2011.000000       3.000000       8.000000  
50%      2013.000000       6.000000      16.000000  
75%      2015.000000       9.000000      23.000000  
max      2017.000000      12.000000      31.000000  
</pre>
이제 우리는 "Rainfall", "Evapation", "WindSpeed9am", "WindSpeed3pm" 열의 특이치가 상한선임을 알 수 있습니다.


### 범주형 변수 인코딩



```python
categorical
```

<pre>
['Location', 'WindGustDir', 'WindDir9am', 'WindDir3pm', 'RainToday']
</pre>

```python
X_train[categorical].head()
```

<pre>
             Location WindGustDir WindDir9am WindDir3pm RainToday
22926   NorfolkIsland         ESE        ESE        ESE        No
80735        Watsonia          NE        NNW        NNE        No
121764          Perth          SW          N         SW       Yes
139821         Darwin         ESE        ESE          E        No
1867           Albury           E        ESE          E       Yes
</pre>

```python
# encode RainToday variable

!pip install category_encoders
```

<pre>
Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
Requirement already satisfied: category_encoders in /usr/local/lib/python3.9/dist-packages (2.6.0)
Requirement already satisfied: pandas>=1.0.5 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (1.5.3)
Requirement already satisfied: statsmodels>=0.9.0 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (0.13.5)
Requirement already satisfied: numpy>=1.14.0 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (1.22.4)
Requirement already satisfied: scikit-learn>=0.20.0 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (1.2.2)
Requirement already satisfied: patsy>=0.5.1 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (0.5.3)
Requirement already satisfied: scipy>=1.0.0 in /usr/local/lib/python3.9/dist-packages (from category_encoders) (1.10.1)
Requirement already satisfied: python-dateutil>=2.8.1 in /usr/local/lib/python3.9/dist-packages (from pandas>=1.0.5->category_encoders) (2.8.2)
Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.9/dist-packages (from pandas>=1.0.5->category_encoders) (2022.7.1)
Requirement already satisfied: six in /usr/local/lib/python3.9/dist-packages (from patsy>=0.5.1->category_encoders) (1.16.0)
Requirement already satisfied: threadpoolctl>=2.0.0 in /usr/local/lib/python3.9/dist-packages (from scikit-learn>=0.20.0->category_encoders) (3.1.0)
Requirement already satisfied: joblib>=1.1.1 in /usr/local/lib/python3.9/dist-packages (from scikit-learn>=0.20.0->category_encoders) (1.2.0)
Requirement already satisfied: packaging>=21.3 in /usr/local/lib/python3.9/dist-packages (from statsmodels>=0.9.0->category_encoders) (23.0)
</pre>

```python
import category_encoders as ce

encoder = ce.BinaryEncoder(cols=['RainToday'])

X_train = encoder.fit_transform(X_train)

X_test = encoder.transform(X_test)
```


```python
X_train.head()
```

<pre>
             Location  MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine  \
22926   NorfolkIsland     18.8     23.7       0.2          5.0       7.3   
80735        Watsonia      9.3     24.0       0.2          1.6      10.9   
121764          Perth     10.9     22.2       1.4          1.2       9.6   
139821         Darwin     19.3     29.9       0.0          9.2      11.0   
1867           Albury     15.7     17.6       3.2          4.7       8.4   

       WindGustDir  WindGustSpeed WindDir9am WindDir3pm  ...  Pressure3pm  \
22926          ESE           52.0        ESE        ESE  ...       1013.9   
80735           NE           48.0        NNW        NNE  ...       1014.6   
121764          SW           26.0          N         SW  ...       1014.9   
139821         ESE           43.0        ESE          E  ...       1012.1   
1867             E           20.0        ESE          E  ...       1010.5   

        Cloud9am  Cloud3pm  Temp9am  Temp3pm  RainToday_0  RainToday_1  Year  \
22926        5.0       7.0     21.4     22.2            0            1  2014   
80735        3.0       5.0     14.3     23.2            0            1  2016   
121764       1.0       2.0     16.6     21.5            1            0  2011   
139821       1.0       1.0     23.2     29.1            0            1  2010   
1867         8.0       8.0     16.5     17.3            1            0  2014   

        Month  Day  
22926       3   12  
80735      10    6  
121764      8   31  
139821      6   11  
1867        4   10  

[5 rows x 25 columns]
</pre>
RainToday_0 변수와 RainToday_1 변수가 RainToday 변수에서 추가로 생성됨을 알 수 있습니다.



이제 X_train 훈련 세트를 만들겠습니다.



```python
X_train = pd.concat([X_train[numerical], X_train[['RainToday_0', 'RainToday_1']],
                     pd.get_dummies(X_train.Location), 
                     pd.get_dummies(X_train.WindGustDir),
                     pd.get_dummies(X_train.WindDir9am),
                     pd.get_dummies(X_train.WindDir3pm)], axis=1)
```


```python
X_train.head()
```

<pre>
        MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine  WindGustSpeed  \
22926      18.8     23.7       0.2          5.0       7.3           52.0   
80735       9.3     24.0       0.2          1.6      10.9           48.0   
121764     10.9     22.2       1.4          1.2       9.6           26.0   
139821     19.3     29.9       0.0          9.2      11.0           43.0   
1867       15.7     17.6       3.2          4.7       8.4           20.0   

        WindSpeed9am  WindSpeed3pm  Humidity9am  Humidity3pm  ...  NNW  NW  S  \
22926           31.0          28.0         74.0         73.0  ...    0   0  0   
80735           13.0          24.0         74.0         55.0  ...    0   0  0   
121764           0.0          11.0         85.0         47.0  ...    0   0  0   
139821          26.0          17.0         44.0         37.0  ...    0   0  0   
1867            11.0          13.0        100.0        100.0  ...    0   0  0   

        SE  SSE  SSW  SW  W  WNW  WSW  
22926    0    0    0   0  0    0    0  
80735    0    0    0   0  0    0    0  
121764   0    0    0   1  0    0    0  
139821   0    0    0   0  0    0    0  
1867     0    0    0   0  0    0    0  

[5 rows x 118 columns]
</pre>
마찬가지로 'X_test' 테스트 세트를 만들 것입니다.



```python
X_test = pd.concat([X_test[numerical], X_test[['RainToday_0', 'RainToday_1']],
                     pd.get_dummies(X_test.Location), 
                     pd.get_dummies(X_test.WindGustDir),
                     pd.get_dummies(X_test.WindDir9am),
                     pd.get_dummies(X_test.WindDir3pm)], axis=1)
```


```python
X_test.head()
```

<pre>
        MinTemp  MaxTemp  Rainfall  Evaporation  Sunshine  WindGustSpeed  \
138175     21.9     39.4       1.6         11.2      11.5           57.0   
38638      20.5     37.5       0.0          9.2       8.4           59.0   
124058      5.1     17.2       0.2          4.7       8.4           50.0   
99214      11.9     16.8       1.0          4.7       8.4           28.0   
25097       7.5     21.3       0.0          4.7       8.4           15.0   

        WindSpeed9am  WindSpeed3pm  Humidity9am  Humidity3pm  ...  NNW  NW  S  \
138175          20.0          33.0         50.0         26.0  ...    0   0  0   
38638           17.0          20.0         47.0         22.0  ...    0   0  0   
124058          28.0          22.0         68.0         51.0  ...    0   0  0   
99214           11.0          13.0         80.0         79.0  ...    0   0  0   
25097            2.0           7.0         88.0         52.0  ...    0   0  0   

        SE  SSE  SSW  SW  W  WNW  WSW  
138175   0    0    0   0  0    0    0  
38638    0    0    0   0  0    0    0  
124058   0    0    0   0  1    0    0  
99214    0    0    0   1  0    0    0  
25097    0    0    0   0  0    0    0  

[5 rows x 118 columns]
</pre>
이제 모델 구축을 위한 교육 및 테스트가 준비되었습니다. 그 전에 모든 형상 변수를 동일한 척도에 매핑해야 합니다. 그것은 '기능 확장'이라고 불립니다. 다음과 같이 하겠습니다.


# **11. Feature Scaling** <a class="anchor" id="11"></a>



```python
X_train.describe()
```

<pre>
             MinTemp        MaxTemp       Rainfall    Evaporation  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean       12.190189      23.203107       0.670800       5.093362   
std         6.366893       7.085408       1.181512       2.800200   
min        -8.500000      -4.800000       0.000000       0.000000   
25%         7.700000      18.000000       0.000000       4.000000   
50%        12.000000      22.600000       0.000000       4.700000   
75%        16.800000      28.200000       0.600000       5.200000   
max        31.900000      48.100000       3.200000      21.800000   

            Sunshine  WindGustSpeed   WindSpeed9am   WindSpeed3pm  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        7.982476      39.982091      14.029381      18.687466   
std         2.761639      13.127953       8.835596       8.700618   
min         0.000000       6.000000       0.000000       0.000000   
25%         8.200000      31.000000       7.000000      13.000000   
50%         8.400000      39.000000      13.000000      19.000000   
75%         8.600000      46.000000      19.000000      24.000000   
max        14.500000     135.000000      55.000000      57.000000   

         Humidity9am    Humidity3pm  ...            NNW             NW  \
count  116368.000000  116368.000000  ...  116368.000000  116368.000000   
mean       68.950691      51.605828  ...       0.054078       0.059123   
std        18.811437      20.439999  ...       0.226173       0.235855   
min         0.000000       0.000000  ...       0.000000       0.000000   
25%        57.000000      37.000000  ...       0.000000       0.000000   
50%        70.000000      52.000000  ...       0.000000       0.000000   
75%        83.000000      65.000000  ...       0.000000       0.000000   
max       100.000000     100.000000  ...       1.000000       1.000000   

                   S             SE            SSE            SSW  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        0.068447       0.103723       0.065224       0.056055   
std         0.252512       0.304902       0.246922       0.230029   
min         0.000000       0.000000       0.000000       0.000000   
25%         0.000000       0.000000       0.000000       0.000000   
50%         0.000000       0.000000       0.000000       0.000000   
75%         0.000000       0.000000       0.000000       0.000000   
max         1.000000       1.000000       1.000000       1.000000   

                  SW              W            WNW            WSW  
count  116368.000000  116368.000000  116368.000000  116368.000000  
mean        0.064786       0.069323       0.060309       0.064958  
std         0.246149       0.254004       0.238059       0.246452  
min         0.000000       0.000000       0.000000       0.000000  
25%         0.000000       0.000000       0.000000       0.000000  
50%         0.000000       0.000000       0.000000       0.000000  
75%         0.000000       0.000000       0.000000       0.000000  
max         1.000000       1.000000       1.000000       1.000000  

[8 rows x 118 columns]
</pre>

```python
cols = X_train.columns
```


```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_train = scaler.fit_transform(X_train)

X_test = scaler.transform(X_test)
```


```python
X_train = pd.DataFrame(X_train, columns=[cols])
```


```python
X_test = pd.DataFrame(X_test, columns=[cols])
```


```python
X_train.describe()
```

<pre>
             MinTemp        MaxTemp       Rainfall    Evaporation  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        0.512133       0.529359       0.209625       0.233640   
std         0.157596       0.133940       0.369223       0.128450   
min         0.000000       0.000000       0.000000       0.000000   
25%         0.400990       0.431002       0.000000       0.183486   
50%         0.507426       0.517958       0.000000       0.215596   
75%         0.626238       0.623819       0.187500       0.238532   
max         1.000000       1.000000       1.000000       1.000000   

            Sunshine  WindGustSpeed   WindSpeed9am   WindSpeed3pm  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        0.550516       0.263427       0.255080       0.327850   
std         0.190458       0.101767       0.160647       0.152642   
min         0.000000       0.000000       0.000000       0.000000   
25%         0.565517       0.193798       0.127273       0.228070   
50%         0.579310       0.255814       0.236364       0.333333   
75%         0.593103       0.310078       0.345455       0.421053   
max         1.000000       1.000000       1.000000       1.000000   

         Humidity9am    Humidity3pm  ...            NNW             NW  \
count  116368.000000  116368.000000  ...  116368.000000  116368.000000   
mean        0.689507       0.516058  ...       0.054078       0.059123   
std         0.188114       0.204400  ...       0.226173       0.235855   
min         0.000000       0.000000  ...       0.000000       0.000000   
25%         0.570000       0.370000  ...       0.000000       0.000000   
50%         0.700000       0.520000  ...       0.000000       0.000000   
75%         0.830000       0.650000  ...       0.000000       0.000000   
max         1.000000       1.000000  ...       1.000000       1.000000   

                   S             SE            SSE            SSW  \
count  116368.000000  116368.000000  116368.000000  116368.000000   
mean        0.068447       0.103723       0.065224       0.056055   
std         0.252512       0.304902       0.246922       0.230029   
min         0.000000       0.000000       0.000000       0.000000   
25%         0.000000       0.000000       0.000000       0.000000   
50%         0.000000       0.000000       0.000000       0.000000   
75%         0.000000       0.000000       0.000000       0.000000   
max         1.000000       1.000000       1.000000       1.000000   

                  SW              W            WNW            WSW  
count  116368.000000  116368.000000  116368.000000  116368.000000  
mean        0.064786       0.069323       0.060309       0.064958  
std         0.246149       0.254004       0.238059       0.246452  
min         0.000000       0.000000       0.000000       0.000000  
25%         0.000000       0.000000       0.000000       0.000000  
50%         0.000000       0.000000       0.000000       0.000000  
75%         0.000000       0.000000       0.000000       0.000000  
max         1.000000       1.000000       1.000000       1.000000  

[8 rows x 118 columns]
</pre>
이제 'X_train' 데이터 세트를 로지스틱 회귀 분류기에 입력할 준비가 되었습니다. 다음과 같이 하겠습니다.


# **12. Model training** <a class="anchor" id="12"></a>



```python
# train a logistic regression model on the training set
from sklearn.linear_model import LogisticRegression


# instantiate the model
logreg = LogisticRegression(solver='liblinear', random_state=0)


# fit the model
logreg.fit(X_train, y_train)
```

<pre>
LogisticRegression(random_state=0, solver='liblinear')
</pre>
# **13. Predict results** <a class="anchor" id="13"></a>



```python
y_pred_test = logreg.predict(X_test)

y_pred_test
```

<pre>
array(['No', 'No', 'No', ..., 'Yes', 'No', 'No'], dtype=object)
</pre>
### predict_proba 방법





**http_proba** 메서드는 이 경우 대상 변수(0 및 1)에 대한 확률을 배열 형식으로 제공합니다.



0은 비가 오지 않을 확률이고 1은 비가 올 확률입니다.`



```python
# probability of getting output as 0 - no rain

logreg.predict_proba(X_test)[:,0]
```

<pre>
array([0.83215823, 0.74551076, 0.79860387, ..., 0.42025444, 0.6575465 ,
       0.96954917])
</pre>

```python
# probability of getting output as 1 - rain

logreg.predict_proba(X_test)[:,1]
```

<pre>
array([0.16784177, 0.25448924, 0.20139613, ..., 0.57974556, 0.3424535 ,
       0.03045083])
</pre>
# **14. Check accuracy score** <a class="anchor" id="14"></a>



```python
from sklearn.metrics import accuracy_score

print('Model accuracy score: {0:0.4f}'. format(accuracy_score(y_test, y_pred_test)))
```

<pre>
Model accuracy score: 0.8484
</pre>
### train-set와 test-set 정확도 비교





이제 train-set와 test-set 정확도를 비교하여 과적합 여부를 확인하겠습니다.



```python
y_pred_train = logreg.predict(X_train)

y_pred_train
```

<pre>
array(['No', 'No', 'No', ..., 'No', 'No', 'No'], dtype=object)
</pre>

```python
print('Training-set accuracy score: {0:0.4f}'. format(accuracy_score(y_train, y_pred_train)))
```

<pre>
Training-set accuracy score: 0.8488
</pre>
### 과적합 및 과소적합 여부 점검



```python
# print the scores on training and test set

print('Training set score: {:.4f}'.format(logreg.score(X_train, y_train)))

print('Test set score: {:.4f}'.format(logreg.score(X_test, y_test)))
```

<pre>
Training set score: 0.8488
Test set score: 0.8484
</pre>
교육 세트 정확도 점수는 0.8488인 반면 테스트 세트 정확도는 0.8484입니다. 이 두 값은 상당히 비슷합니다. 따라서 과적합의 문제는 없습니다.


로지스틱 회귀 분석에서는 C = 1의 기본값을 사용합니다. 교육 및 테스트 세트 모두에서 약 84%의 정확도로 우수한 성능을 제공합니다. 그러나 교육 및 테스트 세트의 모델 성능은 매우 유사합니다. 그것은 아마도 부족한 경우일 것입니다.



저는 C를 늘리고 좀 더 유연한 모델을 맞출 것입니다.



```python
# fit the Logsitic Regression model with C=100

# instantiate the model
logreg100 = LogisticRegression(C=100, solver='liblinear', random_state=0)


# fit the model
logreg100.fit(X_train, y_train)
```

<pre>
LogisticRegression(C=100, random_state=0, solver='liblinear')
</pre>

```python
# print the scores on training and test set

print('Training set score: {:.4f}'.format(logreg100.score(X_train, y_train)))

print('Test set score: {:.4f}'.format(logreg100.score(X_test, y_test)))
```

<pre>
Training set score: 0.8489
Test set score: 0.8491
</pre>
C=100이 테스트 세트 정확도를 높이고 교육 세트 정확도를 약간 높인다는 것을 알 수 있습니다. 따라서 더 복잡한 모델이 더 나은 성능을 발휘해야 한다는 결론을 내릴 수 있습니다.


이제 C=0.01을 설정하여 기본값인 C=1보다 정규화된 모델을 사용하면 어떻게 되는지 알아보겠습니다.



```python
# fit the Logsitic Regression model with C=001

# instantiate the model
logreg001 = LogisticRegression(C=0.01, solver='liblinear', random_state=0)


# fit the model
logreg001.fit(X_train, y_train)
```

<pre>
LogisticRegression(C=0.01, random_state=0, solver='liblinear')
</pre>

```python
# print the scores on training and test set

print('Training set score: {:.4f}'.format(logreg001.score(X_train, y_train)))

print('Test set score: {:.4f}'.format(logreg001.score(X_test, y_test)))
```

<pre>
Training set score: 0.8427
Test set score: 0.8418
</pre>
C=0.01을 설정하여 보다 정규화된 모델을 사용하면 교육 및 테스트 세트 정확도가 기본 매개 변수에 비해 모두 감소합니다.


### 모델 정확도와 null 정확도 비교



모형 정확도는 0.8491입니다. 그러나 위의 정확도에 근거하여 우리의 모델이 매우 좋다고 말할 수는 없습니다. **null 정확도**와 비교해야 합니다. Null 정확도는 항상 가장 빈도가 높은 클래스를 예측하여 얻을 수 있는 정확도입니다.



그래서 우리는 먼저 테스트 세트의 클래스 분포를 확인해야 합니다.



```python
# check class distribution in test set

y_test.value_counts()
```

<pre>
No     22726
Yes     6366
Name: RainTomorrow, dtype: int64
</pre>
가장 빈번하게 발생하는 클래스는 22726로 나타났습니다. 따라서 22726을 총 발생 횟수로 나누면 null 정확도를 계산할 수 있습니다.



```python
# check null accuracy score

null_accuracy = (22726/(22726+6366))

print('Null accuracy score: {0:0.4f}'. format(null_accuracy))
```

<pre>
Null accuracy score: 0.7812
</pre>
우리의 모델 정확도 점수는 0.8491이지만 null 정확도 점수는 0.7812임을 알 수 있습니다. 따라서 로지스틱 회귀 분석 모형이 클래스 레이블을 예측하는 데 매우 효과적이라는 결론을 내릴 수 있습니다.


이제 위의 분석을 바탕으로 분류 모델 정확도가 매우 우수하다는 결론을 내릴 수 있습니다. 우리 모델은 클래스 레이블을 예측하는 측면에서 매우 잘 수행하고 있습니다.





그러나 기본적인 값 분포는 제공하지 않습니다. 또한, 그것은 우리 반 학생들이 저지르는 오류의 유형에 대해서는 아무 것도 말해주지 않습니다.





우리에게는 `Confusion matrix`라는 또 다른 도구가 있습니다.


# **15. Confusion matrix** <a class="filename" id="15"></a>



Confusion matrix은 분류 알고리즘의 성능을 요약하는 도구입니다. Confusion matrix은 분류 모델 성능과 모델에 의해 생성되는 오류 유형에 대한 명확한 그림을 제공합니다. 각 범주별로 분류된 정확한 예측과 잘못된 예측의 요약을 제공합니다. 요약은 표 형식으로 표시됩니다.





분류 모델 성능을 평가하는 동안 네 가지 유형의 결과가 가능합니다. 이 네 가지 결과는 아래에 설명되어 있습니다





**True Positives (TP)** – True Positives은 관측치가 특정 클래스에 속하고 관측치가 실제로 해당 클래스에 속한다고 예측할 때 발생합니다.





**True Negatives(TN)** – True Negatives은 관측치가 특정 클래스에 속하지 않고 실제로 관측치가 해당 클래스에 속하지 않는다고 예측할 때 발생합니다.





**False Positives(FP)** – False Positives는 관측치가 특정 클래스에 속하지만 실제로는 해당 클래스에 속하지 않는다고 예측할 때 발생합니다. 이러한 유형의 오류를 **Type I 오류라고 합니다.**







**False Negatives(FN)** – False Negatives는 관측치가 특정 클래스에 속하지 않지만 실제로는 해당 클래스에 속한다고 예측할 때 발생합니다. 이는 매우 심각한 오류이며 **Type II 오류라고 합니다.**







이 네 가지 결과는 아래에 제시된 Confusion matrix로 요약됩니다.



```python
# Print the Confusion Matrix and slice it into four pieces

from sklearn.metrics import confusion_matrix

cm = confusion_matrix(y_test, y_pred_test)

print('Confusion matrix\n\n', cm)

print('\nTrue Positives(TP) = ', cm[0,0])

print('\nTrue Negatives(TN) = ', cm[1,1])

print('\nFalse Positives(FP) = ', cm[0,1])

print('\nFalse Negatives(FN) = ', cm[1,0])
```

<pre>
Confusion matrix

 [[21543  1183]
 [ 3227  3139]]

True Positives(TP) =  21543

True Negatives(TN) =  3139

False Positives(FP) =  1183

False Negatives(FN) =  3227
</pre>
혼동 매트릭스는 '20892 + 3285 = 24177 정확한 예측'과 '3087 + 1175 = 4262 부정확한 예측'을 보여줍니다.





이 경우, 우리는





- `True Positives` (실제 긍정:1, 예측 긍정:1) - 20892





- `True Negatives` (실제 네거티브:0 및 예측 네거티브:0) - 3285





- `False Positives`(실제 음성: 0이지만 예측 양성: 1) - 1175'(Type I 오류)`





- `False Negatives` (실제 긍정:1이지만 예측 부정:0) - 3087(타입 II 오류)



```python
# visualize confusion matrix with seaborn heatmap

cm_matrix = pd.DataFrame(data=cm, columns=['Actual Positive:1', 'Actual Negative:0'], 
                                 index=['Predict Positive:1', 'Predict Negative:0'])

sns.heatmap(cm_matrix, annot=True, fmt='d', cmap='YlGnBu')
```

<pre>
<Axes: >
</pre>

![image](https://user-images.githubusercontent.com/122151981/232208702-b27a266b-87df-4c72-b078-ce64b76a5673.png)

# **16. Classification metrices** <a class="anchor" id="16"></a>


## Classification Report





**Classification Report**는 분류 모델의 성능을 평가하는 또 다른 방법입니다. 모델에 대한 **precision**, **recall**, **f1** 와와 **support** 점수가 표시됩니다. 저는 이 용어들을 나중에 설명하겠습습니다.



다음과 같이 분류 보고서를 인쇄할 수 있습니다



```python
from sklearn.metrics import classification_report

print(classification_report(y_test, y_pred_test))
```

<pre>
              precision    recall  f1-score   support

          No       0.87      0.95      0.91     22726
         Yes       0.73      0.49      0.59      6366

    accuracy                           0.85     29092
   macro avg       0.80      0.72      0.75     29092
weighted avg       0.84      0.85      0.84     29092

</pre>

```python
TP = cm[0,0]
TN = cm[1,1]
FP = cm[0,1]
FN = cm[1,0]
```


```python
# print classification accuracy

classification_accuracy = (TP + TN) / float(TP + TN + FP + FN)

print('Classification accuracy : {0:0.4f}'.format(classification_accuracy))
```

<pre>
Classification accuracy : 0.8484
</pre>
## 분류 오류



```python
# print classification error

classification_error = (FP + FN) / float(TP + TN + FP + FN)

print('Classification error : {0:0.4f}'.format(classification_error))
```

<pre>
Classification error : 0.1516
</pre>
## 정밀도





**정밀도**는 모든 예측된 긍정적 결과 중 정확하게 예측된 긍정적 결과의 비율로 정의할 수 있습니다. 참 및 거짓 양성의 합계에 대한 참 양성(TP + FP)의 비율로 지정할 수 있습니다.





따라서 **정밀도**는 정확하게 예측된 양성 결과의 비율을 나타냅니다. 그것은 부정적인 계층보다 긍정적인 계층에 더 관심이 있습니다.







수학적으로 정밀도는 'TP 대 (TP + FP)의 비율로 정의할 수 있습니다.`



```python
# print precision score

precision = TP / float(TP + FP)


print('Precision : {0:0.4f}'.format(precision))
```

<pre>
Precision : 0.9479
</pre>
## 재현율





재현율은 모든 실제 긍정적 결과 중 정확하게 예측된 긍정적 결과의 비율로 정의할 수 있습니다.

참 양성과 거짓 음성의 합(TP + FN)에 대한 참 양성(TP)의 비율로 지정할 수 있습니다. **Recall**은(는) **Sensitivity**라고도 합니다.





**Recall**은 정확하게 예측된 실제 긍정의 비율을 나타냅니다.





수학적으로 재현율은 'TP 대 (TP + FN)의 비율로 지정할 수 있습니다.`



```python
recall = TP / float(TP + FN)

print('Recall or Sensitivity : {0:0.4f}'.format(recall))
```

<pre>
Recall or Sensitivity : 0.8697
</pre>
## True Positive Rate





**True Positive Rate**은 **Recall**과 동의어입니다.




```python
true_positive_rate = TP / float(TP + FN)


print('True Positive Rate : {0:0.4f}'.format(true_positive_rate))
```

<pre>
True Positive Rate : 0.8697
</pre>
## False Positive Rate



```python
false_positive_rate = FP / float(FP + TN)


print('False Positive Rate : {0:0.4f}'.format(false_positive_rate))
```

<pre>
False Positive Rate : 0.2737
</pre>
## Specificity



```python
specificity = TN / (TN + FP)

print('Specificity : {0:0.4f}'.format(specificity))
```

<pre>
Specificity : 0.7263
</pre>
## f1-score





**f1-score**는 정밀도와 호출의 가중 조화 평균입니다. 가능한 가장 좋은 **f1-score**는 1.0이고 가장 나쁜 **f1-score**입니다

0.0이 됩니다. **f1-score**는 정밀도와 호출의 조화 평균입니다. 따라서 **f1-score**는 정확도와 리콜을 계산에 포함시키기 때문에 항상 정확도 측도보다 낮습니다. "f1-score"의 가중 평균은 다음과 같이 사용되어야 합니다

전역 정확도가 아닌 분류기 모델을 비교합니다.


## Support





**Support**은 데이터셋에서 클래스의 실제 발생 횟수입니다.


# **17. Adjusting the threshold level** <a class="anchor" id="17"></a>



```python
# print the first 10 predicted probabilities of two classes- 0 and 1

y_pred_prob = logreg.predict_proba(X_test)[0:10]

y_pred_prob
```

<pre>
array([[0.83215823, 0.16784177],
       [0.74551076, 0.25448924],
       [0.79860387, 0.20139613],
       [0.58506698, 0.41493302],
       [0.92162932, 0.07837068],
       [0.95626552, 0.04373448],
       [0.57882815, 0.42117185],
       [0.50297879, 0.49702121],
       [0.80278163, 0.19721837],
       [0.72342592, 0.27657408]])
</pre>
### 관찰





- 각 행에서 숫자는 1이 됩니다.





- 2개의 클래스(0 및 1)에 해당하는 2개의 열이 있습니다.



- 클래스 0 - 내일 비가 오지 않을 확률을 예측합니다.



- 클래스 1 - 내일 비가 올 확률을 예측합니다.





- 예측 확률의 중요성



- 비가 오거나 오지 않을 확률로 관측치의 순위를 매길 수 있습니다.





- predict_proba 공정



- 확률을 예측합니다



- 확률이 가장 높은 클래스 선택





- 분류 임계값 레벨



- 분류 임계값 레벨은 0.5입니다.



- 클래스 1 - 확률이 0.5 이상일 경우 비가 올 확률이 예측됩니다.



- 클래스 0 - 확률이 0.5 미만일 경우 비가 오지 않을 확률이 예측됩니다.



```python
# store the probabilities in dataframe

y_pred_prob_df = pd.DataFrame(data=y_pred_prob, columns=['Prob of - No rain tomorrow (0)', 'Prob of - Rain tomorrow (1)'])

y_pred_prob_df
```

<pre>
   Prob of - No rain tomorrow (0)  Prob of - Rain tomorrow (1)
0                        0.832158                     0.167842
1                        0.745511                     0.254489
2                        0.798604                     0.201396
3                        0.585067                     0.414933
4                        0.921629                     0.078371
5                        0.956266                     0.043734
6                        0.578828                     0.421172
7                        0.502979                     0.497021
8                        0.802782                     0.197218
9                        0.723426                     0.276574
</pre>

```python
# print the first 10 predicted probabilities for class 1 - Probability of rain

logreg.predict_proba(X_test)[0:10, 1]
```

<pre>
array([0.16784177, 0.25448924, 0.20139613, 0.41493302, 0.07837068,
       0.04373448, 0.42117185, 0.49702121, 0.19721837, 0.27657408])
</pre>

```python
# store the predicted probabilities for class 1 - Probability of rain

y_pred1 = logreg.predict_proba(X_test)[:, 1]
```


```python
# plot histogram of predicted probabilities


# adjust the font size 
plt.rcParams['font.size'] = 12


# plot histogram with 10 bins
plt.hist(y_pred1, bins = 10)


# set the title of predicted probabilities
plt.title('Histogram of predicted probabilities of rain')


# set the x-axis limit
plt.xlim(0,1)


# set the title
plt.xlabel('Predicted probabilities of rain')
plt.ylabel('Frequency')
```

<pre>
Text(0, 0.5, 'Frequency')
</pre>

![image](https://user-images.githubusercontent.com/122151981/232208942-7195cd71-bb14-407e-a200-f1cb171862c3.png)

### 관찰





- 위의 히스토그램이 매우 양으로 치우쳐 있음을 알 수 있습니다.





- 첫 번째 열은 확률이 0.0과 0.1 사이인 관측치가 약 15,000개임을 나타냅니다.





- 확률이 0.5보다 작은 관측치가 있습니다.





- 그래서 이 소수의 관측치들은 내일 비가 올 것이라고 예측하고 있습니다.





- 내일은 비가 오지 않을 것이라는 관측이 대다수입니다.


# **18. ROC - AUC** <a class="message" id="18"></a>



## ROC 곡선





분류 모델 성능을 시각적으로 측정하는 또 다른 도구는 **ROC 곡선**입니다. ROC 곡선은 **Receiver Operating Characteric Curve**의 약자입니다. **ROC 곡선**은 다양한 수준에서 분류 모델의 성능을 보여주는 그림입니다

분류 임계값 레벨입니다.







**ROC 곡선**은 다양한 임계값 레벨에서 **False Positive Rate(FPR)**에 대한 **True Positive Rate(TPR)**를 표시합니다.







**실제 양성률(TPR)**은 **호출**이라고도 합니다. 'TP 대 (TP + FN)의 비율로 정의됩니다.`







**False Positive Rate(FPR)**는 'FP 대 (FP + TN)의 비율로 정의됩니다.`









ROC 곡선에서는 단일 지점의 TPR(True Positive Rate)과 FPR(False Positive Rate)에 초점을 맞출 것입니다. 이를 통해 다양한 임계값 레벨에서 TPR과 FPR로 구성된 ROC 곡선의 일반적인 성능을 얻을 수 있습니다. 따라서 ROC 곡선은 여러 분류 임계값 수준에서 TPR 대 FPR을 표시합니다. 임계값 레벨을 낮추면 더 많은 항목이 포지티브로 분류될 수 있습니다. 그러면 True Positives(TP)와 False Positives(FP)가 모두 증가합니다.



```python
# plot ROC Curve

from sklearn.metrics import roc_curve

fpr, tpr, thresholds = roc_curve(y_test, y_pred1, pos_label = 'Yes')

plt.figure(figsize=(6,4))

plt.plot(fpr, tpr, linewidth=2)

plt.plot([0,1], [0,1], 'k--' )

plt.rcParams['font.size'] = 12

plt.title('ROC curve for RainTomorrow classifier')

plt.xlabel('False Positive Rate (1 - Specificity)')

plt.ylabel('True Positive Rate (Sensitivity)')

plt.show()
```

![image](https://user-images.githubusercontent.com/122151981/232208994-436d0899-5bb7-4bc5-a287-2f9ce101bda8.png)

ROC 곡선은 특정 컨텍스트에 대한 민감도와 특수성의 균형을 맞추는 임계값 레벨을 선택하는 데 도움이 됩니다.


## ROC-AUC





**ROC-AUC**는 **수신기 작동 특성 - 곡선 아래 영역**의 약자입니다. 분류기 성능을 비교하는 기술입니다. 이 기술에서 우리는 곡선 아래의 영역(AUC)을 측정합니다. 완벽한 분류기는 ROC AUC가 1인 반면, 순수한 무작위 분류기는 ROC AUC가 0.5입니다.





따라서 **ROC AUC**는 곡선 아래에 있는 ROC 그림의 백분율입니다.



```python
# compute ROC AUC

from sklearn.metrics import roc_auc_score

ROC_AUC = roc_auc_score(y_test, y_pred1)

print('ROC AUC : {:.4f}'.format(ROC_AUC))
```

<pre>
ROC AUC : 0.8671
</pre>
### comment





- ROC AUC는 분류기 성능의 단일 숫자 요약입니다. 값이 높을수록 분류기가 더 좋습니다.



- 우리 모델의 ROCAUC는 1에 접근합니다. 그래서, 우리는 우리의 분류기가 내일 비가 올지 안 올지 예측하는 것을 잘한다는 결론을 내릴 수 있습니다.



```python
# calculate cross-validated ROC AUC 

from sklearn.model_selection import cross_val_score

Cross_validated_ROC_AUC = cross_val_score(logreg, X_train, y_train, cv=5, scoring='roc_auc').mean()

print('Cross validated ROC AUC : {:.4f}'.format(Cross_validated_ROC_AUC))
```

<pre>
Cross validated ROC AUC : 0.8675
</pre>
# **19. k-Fold Cross Validation** <a class="anchor" id="19"></a>



```python
# Applying 5-Fold Cross Validation

from sklearn.model_selection import cross_val_score

scores = cross_val_score(logreg, X_train, y_train, cv = 5, scoring='accuracy')

print('Cross-validation scores:{}'.format(scores))
```

<pre>
Cross-validation scores:[0.84802784 0.84927387 0.84940277 0.8450565  0.84879474]
</pre>

```python
# compute Average cross-validation score

print('Average cross-validation score: {:.4f}'.format(scores.mean()))
```

<pre>
Average cross-validation score: 0.8481
</pre>
우리의 원래 모델 점수는 0.887입니다. 교차 검증 평균 점수는 0.8481입니다. 따라서 교차 검증을 통해 성능이 향상되지 않는다는 결론을 내릴 수 있습니다.


# **20. Hyperparameter Optimization using GridSearch CV** <a class="anchor" id="20"></a>



```python
from sklearn.model_selection import GridSearchCV


parameters = [{'penalty':['l1','l2']}, 
              {'C':[1, 10, 100, 1000]}]



grid_search = GridSearchCV(estimator = logreg,  
                           param_grid = parameters,
                           scoring = 'accuracy',
                           cv = 5,
                           verbose=0)


grid_search.fit(X_train, y_train)
```

<pre>
GridSearchCV(cv=5,
             estimator=LogisticRegression(random_state=0, solver='liblinear'),
             param_grid=[{'penalty': ['l1', 'l2']}, {'C': [1, 10, 100, 1000]}],
             scoring='accuracy')
</pre>

```python
# examine the best model

# best score achieved during the GridSearchCV
print('GridSearch CV best score : {:.4f}\n\n'.format(grid_search.best_score_))

# print parameters that give the best results
print('Parameters that give the best results :','\n\n', (grid_search.best_params_))

# print estimator that was chosen by the GridSearch
print('\n\nEstimator that was chosen by the search :','\n\n', (grid_search.best_estimator_))
```

<pre>
GridSearch CV best score : 0.8483


Parameters that give the best results : 

 {'penalty': 'l1'}


Estimator that was chosen by the search : 

 LogisticRegression(penalty='l1', random_state=0, solver='liblinear')
</pre>

```python
# calculate GridSearch CV score on test set

print('GridSearch CV score on test set: {0:0.4f}'.format(grid_search.score(X_test, y_test)))
```

<pre>
GridSearch CV score on test set: 0.8488
</pre>
### Comments



- 우리의 원래 모델 테스트 정확도는 0.8501인 반면 그리드 검색 CV 정확도는 0.8507입니다.





- 그리드 검색 CV가 이 특정 모델의 성능을 향상시킨다는 것을 알 수 있습니다.


# **21. Results and conclusion** <a class="anchor" id="21"></a>


1. 로지스틱 회귀 모형 정확도 점수는 0.8501입니다. 그래서, 이 모델은 호주에 내일 비가 올지 안 올지 예측하는 데 매우 좋은 역할을 합니다.



2. 내일 비가 올 것이라는 관측은 소수입니다. 내일은 비가 오지 않을 것이라는 관측이 대다수입니다.



3. 이 모델은 과적합의 징후가 없습니다.



4. C 값을 증가시키면 테스트 세트 정확도가 높아지고 교육 세트 정확도가 약간 증가합니다. 따라서 더 복잡한 모델이 더 나은 성능을 발휘해야 한다는 결론을 내릴 수 있습니다.



5. 임계값 레벨을 높이면 정확도가 높아집니다.



6. 우리 모델의 ROCAUC는 1에 접근합니다. 그래서, 우리는 우리의 분류기가 내일 비가 올지 안 올지 예측하는 것을 잘한다는 결론을 내릴 수 있습니다.



7. 원래 모델 정확도 점수는 0.8501인 반면 RFECV 이후 정확도 점수는 0.8500입니다. 따라서 기능 집합을 줄이면 거의 유사한 정확도를 얻을 수 있습니다.



8. 원래 모델에서는 FP = 1175인 반면 FP1 = 1174입니다. 그래서 우리는 대략 같은 수의 오검출을 얻습니다. 또한 FN = 3087인 반면 FN1 = 3091입니다. 그래서 우리는 약간 더 높은 거짓 음성을 얻습니다.



9. 우리의 원래 모델 점수는 0.8476입니다. 교차 검증 평균 점수는 0.8474입니다. 따라서 교차 검증을 통해 성능이 향상되지 않는다는 결론을 내릴 수 있습니다.



10. 당사의 원래 모델 테스트 정확도는 0.8501인 반면 그리드 검색 CV 정확도는 0.8507입니다. 그리드 검색 CV가 이 특정 모델의 성능을 향상시킨다는 것을 알 수 있습니다.


이제 블로그 글을 마치도록 하겠습니다. 감사합니다.

