**🍿🎬 Movie Recommendation 시스템을 만들어보자!**
> 넷플릭스에서 추천 서비스는 user에게는 그들이 원하는 컨텐츠를 추천받고,
 provider들은 이익을 얻을 수 있어 win-win solution임!
 
> 넷플릭스에는 75%의 영화가 추천 서비스로 인해 시청된다!

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기

- **import re**
  
  정규표현식 도구. 텍스트에서 특정 패턴을 찾거나 (예:특수문자 제거), 치환할 때 사용

- **import tensorflow as tf**
  
  딥러닝 모델 만들고 학습시킬 때 사용
 (tensorflow는 머신러닝과 딥러닝 모델을 만들고 서비스에 사용할 수 있도록 도와주는 소프트웨어 도구라고함)

- **import tensorflow_recommenders**
  
  ranking 모델 구조 만들 때 자주 사용됨

- **from collections import counter**
  
  단어 빈도 세기 등의 카운트 작업에 많이 씀 -> 리뷰 분석 등 할 때 사용하는 듯!!

- **from datetime import datetime**
  
  날짜, 시간 처리용. 영화 개봉일을 datetime 형태로 바꾸거나 연도를 추출할 때 사용

- **from wordcloud import WordCloud**
  
  단어 구름 시각화. 

- **from sklearn.feature_extraction.text import TfidfVectorizer**
  
  텍스트를 벡터로 변환하느는 도구 (영화 설명/키워드/장르 등을 벡터화)

- **from sklearn.metrics.pairwise import cosine_similarity**
  
  벡터 간 유사도를 계산하는 함수
  > 영화-영화 유사도 계산할 때 사용

- **axis=1**
  
  열을 지운다는 뜻 ','를 뒤에 써주면 ,앞에 지정한 값을 지워서 나타나게 함

- **df['~~~'] = df['~~~'].fillna()**
  
  df['컬럼명'] = df['컬럼명'].fillna(채울값)
  해당하는 컬럼명의 결측치를 채울값으로 채워달라는 뜻

- **dropna()**
  
  결측치를 기준으로 데이터를 삭제하는 함수


## 2. 데이터 전처리하기

- **fi len(text) == 1:
  else:**

  만약에 텍스트(장르)가 하나일 경우에는 이렇게, 텍스트가 하나가 아닐 경우(장르가 여러개일 경우)에는 이렇게 처리한다는 뜻

- **get_text**

  텍스트로 변환시키는 코드

- **reset_index**

  삭제된 행들이 있으니 중간중간이 비어서 보일 것임.
  인덱스를 0부터 다시 매긴다


## 3. 데이터 요약보기

데이터 간략하게 확인해서 앞에서 진행한 전처리가 모두 잘 적용됐는지 확인

- **df.info()**

행 개수, 컬럼 개수, 컬럼명, non-null값 개수, 데이터 타입(dtype), 전체 메모리 사용량 을 나타냄!



## 4. 시각화

### (1) 시각화 - 1
- **matplotlib**
  
  수치 데이터를 그래프로 그리기 위한 도구를 불러내는 코드

- **plt.scatter(x=[0.5, 1.5], y=[1,1]**
  
  두개의 큰 점의 위치를 찍어줌

- **plt.axis('off')**

  축(x,y), 눈금, 테두리 전부 숨김 처리

> 결과물
  
<img width="594" height="313" alt="image" src="https://github.com/user-attachments/assets/99599e31-b236-4646-bb2c-42f53c7a5c38" />


### (2) 시각화 - 2

> 예산과 수익이 인기에 얼만큼 영향을 미치는지 분석

- df_plot

  ~~~
  ❓ 시각화 - 1 에서는 데이터프레임을 따로 안만들었는데 시각화 - 2 에서는 만든이유?

  시각화1에서는 NaN이나 0값이 있어도 관계없음.
  but, 시각화 2에서는 값들 간에 '미치는 영향'을 분석하는 것이기 때문에 0값을 제거하고
  수치를 분석해야 함.

- ⭐️ regplot 함수

  회귀 관계를 그려주는 함수
  -> x.y 데이터 추출, 결측치 계산, 선형 회귀 계산, 점으로 데이터 시각화 

  > 결과물
  <img width="779" height="391" alt="image" src="https://github.com/user-attachments/assets/8e283c45-36ff-4996-86f6-de96cd582027" />

(1) budget과 revenue가 약하게 영향을 미친다
(2) 점 하나가 영화 1편을 의미
(3) 노란 선이 넓어지는 지점은, 그 구간에서 신뢰도가 떨어진다는 뜻
(4) 예산보다는 수익이 더 크게 관계가 있다









  
