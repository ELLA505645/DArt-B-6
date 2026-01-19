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


















  
