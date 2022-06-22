







# Numpy (Numeric python)



![image-20220422092456291](/Users/krc/Library/Application Support/typora-user-images/image-20220422092456291.png)

![image-20220422092707028](/Users/krc/Library/Application Support/typora-user-images/image-20220422092707028.png)

스칼라 :숫자

벡터: (순서가있음) ,리스트, 문자열 ,row, column

매트릭스: 2차원 행렬

텐서: 3차원(이상) , 이미지 데이터

~~~
#벡터
list=[1,2,3,4] #벡터 
#리스트는 순서가있다.
list ='안녕하세요' #벡터
list[0] 
~~~



### 데이터 형태에 따른 사칙연산



\> 스칼라 +, -, *, / -> 결과도 스칼라  

벡터 +, -, 내적 -> +, - 결과는 벡터, 내적 결과는 스칼라  

매트릭스 +, -, *, /  

텐서 +, -, *, /  



#### 데이터분석에 자주 사용하는 특수한 연산

벡터와 벡터의 내적

  <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422092941465.png" alt="image-20220422092941465" style="zoom: 50%;" />

  

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093242391.png" alt="image-20220422093242391" style="zoom:50%;" />

**: 6과6과 내적 되고 형태는 (4,5) 로 된다**



#### 벡터와 벡터의 내적이 이루어지려면

​    

​    \1. 벡터가 마주하는 shape의 갯수(길이)가 같아야 합니다.

​    \2. 연산 앞에 위치한 벡터는 전치(transpose) 되어야 합니다.



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093442032.png" alt="image-20220422093442032" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093505051.png" alt="image-20220422093505051" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093530195.png" alt="image-20220422093530195" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422095259706.png" alt="image-20220422095259706" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422095323567.png" alt="image-20220422095323567" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422095405217.png" alt="image-20220422095405217" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093020940.png" alt="image-20220422093020940" style="zoom:50%;" />





#### 벡터 내적으로 방정식 구현

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093415572.png" alt="image-20220422093415572" style="zoom:50%;" />





## 브로드캐스팅

\> 파이썬 넘파이 연산은 브로드캐스팅을 지원합니다.  

벡터연산 시 shape이 큰 벡터의 길이만큼 shape이 작은 벡터가 연장되어 연산됩니다.

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093653373.png" alt="image-20220422093653373" style="zoom:50%;" />



~~~~

~~~~



**## Numpy function(유니버셜 함수)**

\> `numpy`는 컴퓨팅연산을 위한 다양한 연산함수를 제공합니다.  

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422093910460.png" alt="image-20220422093910460" style="zoom:50%;" />



#### 주로 쓰는 함수요약

### 수리연산
- **`prod()`** :곱연산
- **`dot()`**
- **`sum()`**
- **`cumprod()`**    :누적곱연산
- **`cumsum()`** :누적합연산 ,누적수
- **`abs()`**
- **`sqaure()`**
- **`sqrt()`**
- **`exp()`**
- **`log()`**

### 통계연산
- **`mean()`**
- **`std()`**
- **`var()`**
- **`max()`**
- **`min()`**
- **`argmax()`** : 최대값이 존재하고 있는 인덱스 넘버를 리턴, 출력값이 인덱스, 
- **`argmin()`** : 최소값 인덱스

### 로직연산
- **`arange()`** : 범위설정 (시작,끝+1,스텝수) , range()함수와 동일
- **`isnan()`**: 범위데이터 생성, (시작,끝+1,데이터갯수)
- **`isinf()`**
- **`unique()`**

### 기하
- **`shape()`**
- **`reshape()`**
- **`ndim()`**
- **`transpose()`**

------



np. unique(): 고유값확인

Np.reshape( list,(2,2,2,3) ): 형태변경

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422094958835.png" alt="image-20220422094958835" style="zoom:50%;" />



Np.ndim() :차원확인



## Numpy array (배열, 행렬)
> - numpy 연산의 기본이 되는 데이터 구조입니다.  
> - 리스트보다 간편하게 만들 수 있으며 **연산이 빠른** 장점이 있습니다.  
> - **브로드캐스팅 연산을 지원**합니다.  
> - 단, **같은 type**의 데이터만 저장 가능합니다.  
> - array 또한 numpy의 기본 함수로서 생성 가능합니다.  

>> array 함수 호출 기본구조  
ex) **`np.array(배열변환 대상 데이터)`**  
ex) **`np.arange(start, end, step_forward)`**



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422100929730.png" alt="image-20220422100929730" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422100950153.png" alt="image-20220422100950153" style="zoom:50%;" />



### 특수한 형태의 array를 함수로 생성
함수 호출의 기본구조
> ex) **`np.ones([자료구조 shape])`**  
> 자료구조 shape은 정수, **[ ]**리스트, **( )** 튜플 로만 입력가능합니다.

>> - ones()  
>> - zeros()  
>> - empty()  
>> - eye()





~~~
# 1로 초기화한 array 생성
np.ones((5,5))
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422101038755.png" alt="image-20220422101038755" style="zoom:50%;" />



**### array 속성 및 내장함수**

`np.array` 에는 유용한 수리, 통계 연산을 위한 함수가 갖추어져 있습니다. 다차원 기하학적 연산을 위한 함수도 함께 살펴보겠습니다.  

​    

\> array 내장 속성 호출 기본구조  

ex) ***\*****`test_array.ndim`*****\***  

자주 사용하는 속성 `shape`, `dtype`, `ndim`

​    

\> array 내장함수 호출 기본구조  

ex) ***\*****`test_array.prod()`*****\***

​    

위에 학습한 np.sum() 과는 달리 array 변수의 인자를 받아 그대로 사용합니다.





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422102828065.png" alt="image-20220422102828065" style="zoom:50%;" />



~~~python
# 벡터의 가중합 연습문제
# 삼성전자, 셀트리온, 카카오로 포트폴리오를 구성하려한다. 
# 각 종목의 가격은 80,000원, 270,000원, 160,000원이다.
# 삼성전자 100주, 셀트리온 30주, 카카오 50주로 구성하기 위한 매수금액을 구하시오
#(80000 * 100) + (270000 * 30) + (160000 * 50)
stock = np.array([100, 30, 50])
price = np.array([80000, 270000, 160000])
stock @ price
***
24100000
~~~



## array 인덱싱, 슬라이싱(매우중요)

\> 기본적으로 자료구조란 데이터의 묶음, 그 묶음을 관리 할 수 있는 바구니를 이야기 합니다.  

데이터 분석을 위해 자료구조를 사용하지만 자료구조안 내용에 접근을 해야 할 경우도 있습니다.

 \>>  **인덱싱이란?**  

데이터 바구니 안에 든 내용 하나에 접근하는 명령, 인덱스는 내용의 순번  

\>> ***\*슬라이싱\****이란?  

데이터 바구니 안에 든 내용 여러가지에 접근 하는 명령  



기본적으로 인덱싱과 슬라이싱의 색인은 리스트와 동일합니다.



## 팬시인덱싱

numpy에서 벡터연산을 통해 bool 형태의 벡터를 기준으로 데이터를 선별하는 방법



~~~python
# 테스트 array 생성
pet = np.array(['개', '고양이', '고양이', '햄스터', '개', '햄스터'])
num = np.array([1, 2, 3, 4, 5, 6])
indexing_test = np.random.randn(6, 5)
indexing_test
***
array([[-0.87655047,  0.55287212, -1.22329249,  0.53230273,  1.06007179],
       [-0.21831787, -0.29615533,  0.96977998,  0.37232531, -0.43324773],
       [ 0.16334586,  0.44705092,  1.28762124, -0.86595101,  0.0247562 ],
       [-1.18436252,  0.49544793,  0.18898468, -2.30499082,  0.1624554 ],
       [ 0.80554653, -2.71359896, -0.50455748, -1.17437155, -1.02644625],
       [-0.49753891,  0.62784511, -1.62567488,  0.0339107 ,  0.04971904]])
~~~



~~~
# num array 조건연산
num>3
pet=='cat'
***
array([False,False,...])
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422104820005.png" alt="image-20220422104820005" style="zoom:50%;" />

~~~python
## 조건연산 추가 sum, any, all
(num>3).sum() true 만 더해진다
(num>3).any() any 하나라도 true면 true다
(num>3).all()


~~~



# Pandas
> - 데이터 과학자를 위해 **테이블형태**로 데이터를 다룰 수 있게 해주는 패키지(python용 엑셀)
> - 기존 데이터처리 라이브러리인 numpy 대신 주로 사용
> - 일반인이 데이터분석을 접하기 쉽게 만들어준 결정적인 라이브러리
> - pandas만으로도 충분히 데이터 분석이 가능할 정도로 고수준의 함수들을 내장
> - 앞으로 진행하는 데이터분석 과정에서 주로 사용하게 될 데이터구조



## pandas 설치 및 import

​    

* 콘솔창에서 실행 시  

~~~
**`pip install pandas`**  
**`conda install pandas`**

~~~

​    

* 주피터 노트북으로 실행 시  

***\*****`!pip install pandas`*****\***

​    

아나콘다 환경으로 python 환경설정 시 기본적으로 설치가 되어있음

~~~python
# numpy import
import numpy as np

# pandas import
import pandas as pd
# pd라는 닉네임은 많은 파이썬 유저들이 사용하고 있는 닉네임, 분석을 위한 필수는 아니지만 되도록이면 위와 같이 사용을 해줍시다.

pd.options.display.max_columns = 200
# 불러들이는 데이터에 맞춰 모든 컬럼을 확인 가능하도록 옵션값을 주었습니다.
pd.options.display.max_info_columns =200
# 그냥 실행 시키시고 지금 이해 못하셔도 좋습니다.
~~~



## DataFrame

* 엑셀에 익숙한 사용자를 위해 제작 된 ***\*테이블형태의 데이터 구조\****  

* 다양한 형태의 데이터를 받아 사용할 수 있으며 다양한 ***\*통계, 시각화 함수를 제공\****한다.  



실제 데이터를 불러들이고 값을 확인 해 보며 기본적인 pandas 사용법을 익혀보도록 하겠습니다.



**### 데이터 불러오기**

pandas는 다양한 데이터 파일 형태를 지원하며 주로 csv, xlsx, sql, json을 사용합니다.

​    

**`read_csv()`**  

**`read_excel()`**

**`read_sql()`**  

**`read_json()`**

**`json_normalize()`**



~~~
pwd
~~~

~~~
# DataFrame 의 약자로서 형식적으로 df 변수명을 사용한다.
# pandas패키지의 read_csv() 함수를 사용하여 loan1.csv 파일을 불러들여 데이터프레임을 만들고 df 이름의 변수로 저장
df=pd.read_csv('csv파일주소복사')
~~~

~~~
df=pd.read_excel('excel파일주소복사')
~~~

~~~
!pip install xlrd, openpyxl, pyxlsb 
#하나하나 따로 설치해야 에러안남
~~~

~~~python
# 엑셀파일에 시트에 따라 데이터 구분이 지어진 경우 시트별로 데이터프레임 제작 가능
# 다른 엑셀파일형식을 가져올 때 engine파라메터 추가해주시면 됩니다.
df1 = pd.read_excel('./data/loan1.xlsb', 
                    sheet_name='구매영수증상세+상품마스터포함', 
                    engine='pyxlsb',
                    encoding='utf-8') # 윈도우의 경우 cp949
~~~



### 데이터프레임 확인

~~~python
# 참고! 실습은 하지 않습니다만 쿼리를 사용하여 데이터베이스로부터 데이터프레임을 만드는 것도 가능합니다.
# 데이터베이스로 부터 자료 읽기

# 필요한 모듈 추가 설치 - 각 데이터베이스 별로 다릅니다.
# !pip install pymysql

# sql 모듈 로드하기
# import pymysql
# mysql, mariadb, sqlite, postgresql, ms-sql, oracle, mongodb

# 접속하기
# 접속방법 또한 DB 종류에 따라 다릅니다.
# con = pymysql.connect(host='db서버주소', port=3306, user='id', passwd='pwd', db='dbname')

# query 만들기
# query = 'select * from samples'

# 자료 불러오기
# data = pd.read_sql(query, con=con)
~~~



### 데이터 저장하기

불러들인 혹은 작업을 마친 데이터프레임을 다양한 파일형태로 저장이 가능합니다.

**`to_csv()`**

**`to_excel()`**

**`to_sql()`**



~~~python
# index=False 파라메터는 기존 데이터프레임의 인덱스를 무시하고 저장
#개꿀팁 (회사 DRM 안걸림)
#.gz 압축
df.to_cvs('경로.gz',index=False)
#불러오기
df_test=pd.read_csv('경로.gz')
~~~



### 사용 데이터 간략 설명

>  미국 핀테크 회사인 lending club의 대출 데이터베이스  클라우드펀딩과 대출을 결합한 핀테크의 시초라고 부를 수 있는 회사  
>
> 방대한 양의 대출정보를 공개하면서 금융정보분석에도 기여한 공이 큰 데이터  2007 ~ 2015 년 대출정보 및 개인정보를 담고 있음  226만건, 145항목 정보를 담고있음  
>
> 실습데이터는 이 중 4만건을 추출한 데이터를 사용합니다.



데이터살펴보기

~~~
# 데이터를 불러들인 후 가장 처음 하는 작업
# 데이터의 구조, 형태 파악하기
# 데이터의 첫 5개 데이터 하나(샘플, 인스턴스) 확인하기
df.head()
# 10개를 확인하려면?
df.head(10)
~~~



~~~
# 데이터의 마지막 5개 샘플 확인하기
# 데이터가 잘 가져왔는지 확인 할 때 보통 씁니다.
df.tail()
~~~



~~~
# 데이터의 갯수를 살펴봅니다
len(df)
~~~



~~~
# 데이터의 전반적인 정보를 확인합니다.
df.info()
# dtype 정보에서는 각 컬럼별 데이터 타입을 확인 할 수 있습니다.
# object == str 이라고 생각하셔도 무방합니다.
# verbose, null_counts
~~~



~~~
# 데이터의 기초통계량을 확인합니다.
df.describe()
~~~



~~~
# numpy 함수로 데이터 shape 확인
df.shape
# 인덱스
df.index
# 컬럼
df.columns[:25] #한꺼번에 선택하기
~~~



~~~py
#컬럼 고유값 출력
for col_nm in df.coumns:
  print([''])

~~~



### 데이터접근 (인덱싱, 슬라이싱, 샘플링)



~~~python
# 첫 샘플 혹은 레코드(대출건)에 대한 데이터를 살펴보겠습니다.
# 인덱스넘버로 데이터에 접근하는 .iloc[색인]
# 각 컬럼이나, 행단위 접근했을 때 출력되는 벡터 데이터를 Serise(시리즈) 라고 하는 자료구조
# index, values로 각각의 속성에 접근 가능
df.iloc[0]
df.iloc[0].index
df.iloc[0].values #array로 묶임 #brodcasting 가능 # 빠르다
한번씩 찍어보고 확인해보자
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422113918141.png" alt="image-20220422113918141" style="zoom:50%;" />



~~~
# 10번 인덱스 부터 20번 인덱스 샘플 접근
# start, end+1, step
df.iloc[10:20:2] #짝수
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422114221760.png" alt="image-20220422114221760" style="zoom:50%;" />



~~~
# 컬럼 단위 샘플 접근
df['emp_title']
#딕셔너리 #키 - 값 :출력
# df[텍스트형태의 컬럼명]
# 인덱싱이나 슬라이싱으로 데이터에 접근을 할 때 큰 단위를 선택하고 그 결과에서 인덱싱 혹은 슬라이싱을 하면
# 조금 더 편하게, 쉽게 데이터 접근이 가능하다.
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422114323749.png" alt="image-20220422114323749" style="zoom:50%;" />



~~~
# 여러 컬럼 동시 접근
df.[['grade','sub_grade']]
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422114445957.png" alt="image-20220422114445957" style="zoom:50%;" />



~~~python
# row와 columns을 동시에 슬라이싱 하는 속성
# df.loc[인덱스, 컬럼명]
#10~20까지 인덱스 접근
# grade > sub_grade

df.loc[10:20, ['grade','sub_grade']] # 여러개 꼭 리스트로 묶어서
~~~



~~~python
# df의 컬럼명을 순환하면서 컬럼단위로 접근하고 각 컬럼의 고유값을 출력해주는 코드
for col_nm in df.columns:
  print(col_nm, df[col_nm].unique())
df['id'] # 컬럼명
~~~



~~~python
# 고윳값 갯수 출력
for col_nm in df.columns:
  print(col_nm, df[col_nm].nunique()) #nunipue
~~~



## 팬시인덱싱

> ***\*****`bool`*****\*** 형태의 array를 조건을 전달하여 다차원 배열을 인덱싱하는 방법.  
>
> 조건식을 사용하여 분석에 필요한 데이터샘플을 추출하기 용이합니다.



## 데이터프레임 병합



> 실제 분석업무를 진행하다보면 데이터가 여기저기 분산되어 있을 경우가 더 많습니다.  
>
> 조각난 데이터를 분석에 필요한 데이터셋으로 만들기 위해 데이터프레임 병합을 많이 사용합니다.  
>
> 한개 이상의 데이터프레임을 병합 할 때 주로 사용하는 함수 2가지를 알아보겠습니다.    



### 데이터 병합에 사용가능한 key(병합할 기준이 되는 행 or 열)값이 있는경우

**`pd.merge`**(베이스데이터프레임, 병합할데이터프레임)  

> 사용 가능 한 파라메터
- `how` : 'left', 'right', 'inner', 'outer'
- `left_on` : key값이 다를 경우 베이스데이터프레임의 key 설정
- `right_on` : key값이 다를 경우 병합데이터프레임의 key 설정
  
### 단순 데이터 연결
**`pd.concat`**([베이스데이터프레임, 병합할데이터프레임], axis=0 or 1)
> 사용 가능 한 파라메터  
- `axis` : 축 방향 설정





### merge 예시

~~~python
merge_df1 = pd.DataFrame({
    '이름': ['원영', '사쿠라', '유리', '예나', '유진', '나코', '은비', '혜원', '히토미', '채원', '민주', '째욘'],
    '국어': [100, 70, 70, 70, 60, 90, 90, 70, 70, 80, 100, 100],
    '영어': [100, 90, 80, 50, 70, 100, 70, 90, 100, 100, 80, 100]
    }, columns=['이름', '국어', '영어']) 

merge_df2 = pd.DataFrame({
    '일어': [80, 100, 100, 90, 70, 50, 100],
    '수학': [90, 70, 100, 80, 70, 80, 90],
    '이름': ['원영', '사쿠라', '나코', '히토미', '예나', '은비', '째욘'],
    }, columns=['일어', '수학', '이름'])
~~~

~~~python
# 데이터프레임 확인
merge_df1
~~~

~~~python
# 병합 테스트
pd.merge(merge_df1, merge_df2)
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422134421211.png" alt="image-20220422134421211" style="zoom:50%;" />

~~~python
# 병합 테스트
pd.merge(merge_df1, merge_df2, how='outer')
pd.merge(merge_df1, merge_df2, how='left')
pd.merge(merge_df1, merge_df2, how='right')

~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422134534673.png" alt="image-20220422134534673" style="zoom:50%;" />

~~~python
# 병합 테스트
pd.merge(merge_df1, merge_df2, left_on:'이름', right:'이름')
~~~



###  concat 예시

현재 df에 저장되어있는 데이터에 추가로 2만개의 데이터를 이어붙여보겠습니다. df1이라는 변수에 이어붙일 데이터를 불러들여 병합을 진행해보겠습니다.





**## 인덱스 편집**

방금 전 concat으로 병합한 데이터프레임의 이상한 점을 찾으셨나요?  

데이터 자체는 잘 붙였지만 인덱스가 꼬여있습니다. 인덱스 편집은 데이터분석을 위해 필요한 인덱스를 설정하기 위해 필요합니다.



~~~
# 인덱스리셋
concat_df.reset_index(drop=Ture) #필요없는것 지웟음 drop=Ture 
# 살려할 케이스가 있다 ,인덱스가 날짜 ,시간 일경우 살려야함
concat_df.reset_index(drop=Ture, inplace=Ture)

~~~

~~~
ignor_index() #병합된다
~~~



## 컬럼편집

인덱스편집과 마찬가지로 데이터프레임의 컬럼을 변경해야 할 경우도 있습니다. 데이터프레임은 컬럼단위 샘플링 및 인덱싱, 이름변경이 가능합니다.



~~~
# df 컬럼명 접근
df['id']
~~~

~~~~
# columns 속성도 인덱싱 및 슬라이싱이 가능합니다.
df.columns[:25]
~~~~

~~~
#df의 개인정보에 관한 컬럼만을 색인으로 df를 슬라이싱하고 person_df 변수에 할당
concat_df[concat_df.columns[:25]]
~~~



~~~
df.to_csv('경로',index=False) #저장 #필요없는 데이터 삭제
~~~



 ### 컬럼삭제

현재 데이터셋에는 개인식별정보가 지워져서 데이터가 존재하지 않습니다. 불필요한 데이터 column을 지우도록 하겠습니다.



~~~
# 지울 column의 데이터값이 모두 NaN인지 확인
df['id'].isna().all()
~~~

~~~python
# 컬럼 삭제 (drop, del, pop)
df.drop('id',axis=1,inplace=True)
~~~

~~~
del df ['member_id']
~~~

~~~
df.pop('desc')
~~~

~~~
df.info()
~~~





### 컬럼명 변경



​    경우에 따라서는 데이터셋 제작 중 컬럼명을 변경해야 할 경우도 있습니다.

​    국내 수집 데이터 사용 시 컬럼이 한글일 경우 영어로 변경을 많이 합니다.

~~~
# home_ownership을 간략하게 home으로 변경
# 한글도 가능합니다만 권장하지는 않습니다.
df.rename(columns={'home_ownership':'home'},inplace=True)# 컬럼명 home으로 바꿈
~~~



**## 데이터 샘플링 및 분석**

> 데이터병합, 인덱스편집, 컬럼선택만으로도 불필요한 정보를 삭제하고 새롭게 데이터셋을 만들 수 있는것을 확인했습니다.  

> 위에 학습한 내용도 데이터 샘플링에 속한 내용이지만 지금부터는 데이터셋의 데이터를 살펴보면서 의미있는 데이터를 추려보도록 하겠습니다.  
>
> 

>  **데이터프레임의 기본적인 인덱싱, 슬라이싱, 조건부 샘플링을 조합하면 데이터의 샘플을 확인 하는 과정만으로도 데이터분석이 가능해집니다.**



~~~
# emp_title 접근
df['emp_title']
~~~

~~~
# 값을 카운트 하는 함수 value_counts()
df['emp_title'].value_counts().head(20)
~~~



### 데이터프레임 형변환

~~~python
# Owner, owner 같은 직업이지만 대소문자 구분에 따라 다른 값으로 취급되는 문제가 있네요.
# 대소문자 구분을 없애기 위해 모두 소문자로 데이터값을 변경하겠습니다.
# 소문자 변환 전 혹시모를 int, float 데이터가 있을지 모를 상황에 대비해서 모두 문자열로 변경해주겠습니다.
# 형변환 함수 astype(데이터타입)
NaN값 문자열로 변환해야함
df['emp_title']=df['emp_title'].astype(str)
~~~

~~~python
%%time
# 반복문을 사용한 데이터 변경도 가능
# 하지만 파이썬의 강점을 살리지 못한 코드
for index,item in enumerate( df ['emp_title']):
  df.loc[index,'emp_title'] = item.upper()  # loc 대신 at 사용하면 빨라짐 #하나하나 접근할때만
  
df.head()
~~~



### 배운사람들의 코드, 고오급 python 스킬

numpy를 학습하면서 브로드캐스팅에 관하여 잠깐 언급했었습니다. 그렇다면 그 파워풀하다던 브로드캐스팅은 어떻게 사용해야할까요?

​    

\>기타 언어에서는 지원하지 않는 기능이니만큼 파이썬의 특징을 가장 잘 살리는 코드  

***\*****`apply`*****\*** 함수를 사용하여 인자로 받는 모든 데이터에 함수를 적용



#### apply 함수로 컬럼에 적용시키는 코드 구조

​    df['컬럼명'] = df['컬럼명'].apply(lambda x: func(x) if 조건문)

​    df['컬럼명'] = df['컬럼명'].apply(func_nm)





## 데이터 재구조화

~~~python
# 각 직업별 평균연봉이 궁금하다 groupby
# 엑셀의 pivol table 과 비슷한 기능
df.groupby('emp_title').mean() #평균값으로 묶는다 #데이터프레임
df.groupby('emp_title').mean()['annual_inc'].sort_values(ascending=False).head(20) #역순정렬
~~~



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422150812800.png" alt="image-20220422150812800" style="zoom:50%;" />

~~~python
#piov_table
pd.pivot_table(
	data=df,
	values='loan_amnt',
	index='home',
  columns='grade',
  aggfunc:np.mean #함수
	)
~~~

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422151217974.png" alt="image-20220422151217974" style="zoom:50%;" />



## 결측치 처리

> 데이터 분석을 위해서는 데이터셋 내에 빈 값이 있는 경우 분석에 방해가 될 수 있는 여지가 많습니다.  
>
> 모든 결측치를 없애야 하는 것은 아니지만 되도록이면 결측치를 채우는 방법, 
>
> 혹은 없애는 방법등으로 결측치를 처리합니다.  
>
> <img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422151707713.png" alt="image-20220422151707713" style="zoom:50%;" />

~~~
df.info()
~~~

>  확인결과 emp_title, emp_length, dti에 결측치가 존재합니다.
>
> 해당 컬럼의 결측치 샘플들을 살펴보고 결측치를 처리해 보겠습니다.

~~~python
# 컬럼별 결측치 확인을 위한 isnull()함수 리턴값이 bool 형태로 반환되어 조건부 샘플링이 가능합니다.
df['emp_length'].isnull() # boolean
df[df['emp_length'].isnull()]
~~~

~~~python
# dti 컬럼에 결측치가 존재하는 샘플 확인
df[df['dti'].isnull()]
~~~

>   직업과 근속연수에 관한 부분은 데이터를 통한 유추나 계산값을 통해 채워넣을 수 있는 항목은 아닌 것 같습니다.

>    다만 dti의 경우 실수로 채워져 있는 부분이니 수업을 위해 평균값 혹은 근사치를 계산하여 채워보도록 하겠습니다.



### 결측치 채우기

~~~python
# fillna() 함수로 NaN 값을 dti 컬럼의 평균으로 채우기
df['dti'].fillna(df['dti'].mean(),inlpace=T)
#평균,최빈값 (범주형카테고리컬한 데이터)
# fillna() 함수의 다양한 채우기 방법 파라메터 확인해보기
df['dti'].fillna(df['dti'].mean(),method='ffill',inlpace=True)
df['dti'].fillna(df['dti'].mean(),method='bfill',inlpace=True)
~~~

### 결측치 제거

~~~python
# view값으로 dropna 결과값 확인
df.dropna() #결측치 있는행 날림
df.dropna(inplace=True)
df.info() #제거확인
~~~



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422154805329.png" alt="image-20220422154805329" style="zoom:50%;" />





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422155049704.png" alt="image-20220422155049704" style="zoom:50%;" />

=90





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422160850347.png" alt="image-20220422160850347" style="zoom:50%;" />

<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422160908060.png" alt="image-20220422160908060" style="zoom:50%;" />





<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422161117284.png" alt="image-20220422161117284" style="zoom:50%;" />



<img src="/Users/krc/Library/Application Support/typora-user-images/image-20220422161134864.png" alt="image-20220422161134864" style="zoom:50%;" />