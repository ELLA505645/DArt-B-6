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

> 추천 시스템을 만들기 전에 영화들이 각각 어떤 특징들을 나타내고 있는지 확인해보자

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

- **df_plot**

  ~~~
  ❓ 시각화 - 1 에서는 데이터프레임을 따로 안만들었는데 시각화 - 2 에서는 만든이유?

  시각화1에서는 NaN이나 0값이 있어도 관계없음.
  but, 시각화 2에서는 값들 간에 '미치는 영향'을 분석하는 것이기 때문에 0값을 제거하고
  수치를 분석해야 함.
  ~~~

- **⭐️ regplot 함수**

  회귀 관계를 그려주는 함수
  -> x.y 데이터 추출, 결측치 계산, 선형 회귀 계산, 점으로 데이터 시각화 

  > 결과물
  <img width="779" height="391" alt="image" src="https://github.com/user-attachments/assets/8e283c45-36ff-4996-86f6-de96cd582027" />

~~~
Insight

(1) budget과 revenue가 약하게 영향을 미친다

(2) 점 하나가 영화 1편을 의미

(3) 노란 선이 넓어지는 지점은, 그 구간에서 신뢰도가 떨어진다는 뜻

(4) 예산보다는 수익이 더 크게 관계가 있다
~~~

### (3) 시각화 - 3

- **jointplot**

두 변수의 관계를 분포를 포함하여 확인하기 위함

> 결과물

<img width="546" height="542" alt="image" src="https://github.com/user-attachments/assets/0f6b617e-f3df-4745-b165-54b068db8a9e" />

~~~
Insight

(1) 대부분의 영화가 budget보다 revenue가 크다
~~~

**⭐️ 이런 관계를 시각화로 확인하는 이유가 뭘까**

-> 추천 시스템을 만드는 것인데, 우리가 가지고 있는 영화들의 대부분이 흥행작들이라는 것을 파악하기 위해서.

마이너한 영화는 왜 추천되지 않을까? 에 대한 참고사항과 보완사항이 될 수 있음.

### (4) 시각화 - 4

- **df_plot = pd.DataFrame(Counter(genres_list).most_common(5), columns=['genre', 'total'])**

 - counter(genres_list): 장르별 등장 횟수 계산
 - most_common(5): 상위 5개만 추출

> 결과물
<img width="371" height="354" alt="image" src="https://github.com/user-attachments/assets/44acb6f6-ac83-42ce-832e-c87eaddbe68e" />


- **df_plot.loc[len(df_plot)] = { 'genre': 'Others', 'total': df_plot_full[6:].sum()[0] }**

 바로 전 df_plot을 상위 5개만 추출해달라고 입력, 
 나머지 6개부터는 'others'라는 이름으로 합쳐서 추출해라

> 결과물
<img width="371" height="347" alt="image" src="https://github.com/user-attachments/assets/25be2fdb-aac2-4ae2-9441-837bd4baa605" />

~~~
Insight
  
(1) 드라마 장르가 가장 수가 많다

(2) 상위 다섯개 장르 외에도, 38.67%를 차지하는 다른 장르들이 많다
~~~

### (5) 시각화 - 5

- displot
- kind= 'hist'

  displot을 만들꺼고, 히스토그램도 같이 만든다

  > 결과물

  <img width="738" height="280" alt="image" src="https://github.com/user-attachments/assets/1780981f-a613-4409-88b1-3c8698b7159b" />

~~~
Insight

(1) 시간 흐름에 따른 분포를 보기 위한 과정

(2) 데이터가 일부 시기에 편향이 있다

(3) 추천 시스템이 최근(2020년 이후) 영화에 약할 수 있다는 것을 확인

(4) 데이터의 한계를 알 수 있음
~~~

### (6) 시각화 - 6

- relplot

관계형 (relational) 시각화 함수

*regplot과 relplot의 차이점?
-> regplot은 회귀선을 나타내주고, 추세를 알 수 있음
-> relplot은 그룹 간의 관계를 비교할 수 있음

- vote_average와 popularity의 관계

> 결과물
<img width="746" height="379" alt="image" src="https://github.com/user-attachments/assets/f072495f-911d-42bf-abc8-7b1b55c92741" />

~~~
Inisght

(1) 투표 수가 많을 수록 동그라미의 크기가 커진다

(2) 0점이나 10점 같은 극단적 점수로 갈수록 투표수가 적어진다

(3) 1명이 평가한 10점보다 50명이 평가한 8점이 더욱 타당성이 있기 때문에 추후에 이러한 것들을
조정할 수 있는 판단 기준이 됨
( 평점 + 투표 수를 함께 고려 or 가중평균 사용 or 투표수가 너무 적은 것은 배제)
~~~



## 5. 추천 시스템 만들기

**(1) 평점 및 수치 기반 추천 점수 만들기**

- **가중평균 공식 사용**

- **m = df['vote_count'].quantile(0.8)**

투표수가 80% 넘는 것만 사용 -> 상위 20%에 해당하는 것만 사용

- **df['weighted_average'] = (R*v + C*m)/(v+m)**

*R * v: 영화 자신의 평점 X 투표 수 (이 영화에 대한 실제 평가 총량)*
*C * m: 전체 평균 평점 X 기준 투표 수 ( 전체 평균을 기준으로 값이 튀지 않게 보정하는 과정)*

- **MinMaxScaler를 사용해서 정규화**

  추천 점수를 만들기 위해서, popularity와 vote_average를 사용할 것임.
  단위가 달라서 정규화 진행

- **weighted_df['score'] = weighted_df['weighted_average']*0.4 + weighted_df['popularity'].astype('float64')*0.6**

weighted_average에 0.4의 가중치, popularity에 0.6의 가중치를 두고 두개를 합쳐서
최종 추천 점수를 만듦
(*popularity는 데이터셋에서 제공)


**(2) 컨텐츠 (텍스트) 기반 추천 서비스 만들기**

- 텍스트 정보 선택

내용/설명/인물/키워드 등의 텍스트 정보 고려 -> 텍스트 유사도 기반 추천을 하려고

- separate 함수

  괄호 안 내용 제거, 숫자 제거, 공백 제거, 문장부호 제거, 소문자 변환 처리


- content_df[''] = content_df[''].apply(remove_punc)

모든 컬럼들을 소문자화, 특수문자 제거

- bag_of_words

 bag_of_words컬럼에다가 정제된 모든 텍스트를 합쳐준다

 




