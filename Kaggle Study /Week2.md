AMAZON SALES raw data를 파헤쳐보자!

시작하기 전,
데이터 설명:
1) 1000개 넘는 실제 상품이 존재
2) 각 상품에는 식별 번호(ID)가 있음
3) 인도 지역의 상품

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

- import numpy as np
- import pandas as pd
- import matplotlib.pyplot as plt
- import seaborn as sns

~~~
<Columns> - 16개
product_id - Product ID
product_name - Name of the Product
category - Category of the Product
discounted_price - Discounted Price of the Product
actual_price - Actual Price of the Product
discount_percentage - Percentage of Discount for the Product
rating - Rating of the Product
rating_count - Number of people who voted for the Amazon rating
about_product - Description about the Product
user_id - ID of the user who wrote review for the Product
user_name - Name of the user who wrote review for the Product
review_id - ID of the user review
review_title - Short review
review_content - Long review
img_link - Image Link of the Product
product_link - Official Website Link of the Product
~~~

Rows: 1465

## 2. 데이터 전처리하기

- df['discounted_price'] = df['discounted_price'].str.replace("₹",'')
 df['discounted_price'] = df['discounted_price'].str.replace(",",'')
 df['discounted_price'] = df['discounted_price'].astype('float64')

할인율 칼럼에서 ₹,기호를 공백으로 바꿔서 제거해주고, float64타입으로 바꿔준다
*df['discount_percentage'] = df['discount_percentage'].str.replace('%','').astype('float64')
이런식으로 합쳐서 쓸 수도 있음

- df['rating'].value_counts()

  ' ' 컬럼에 대해 개수 세는 코드

- df.query('rating == "|"')
  
평점에서 '|'인 문자를 찾음 -> 보통 리뷰 크롤링 같은걸 하면 평점이 없는 경우에 |로 출력되어서 나오는 경우가 많다고 함

- df['rating'] = df['rating'].str.replace('|', '4.0').astype('float64')

  rating 컬럼에서 |에 해당하는 것을 4.0으로 바꿔준다 (아마존에서 실제로 확인 후)

-df['rating_count'] = df['rating_count'].str.replace(',', '').astype('float64')

rating_count에서 1,234처럼 ,가 들어있을 수 있어서 1234.0인 float형식으로 바꿔준다

- duplicates = df.duplicated()
df[duplicates]

df.duplicated()가 중복값을 찾아주는 함수

-df.isna().sum()

isna()에서 결측치면 True, 아니면 False -> Sum()을 통해서 True값을 1로 계산해서 컬럼별로 결측치 개수를 세어준다

-df1 = df[['product_id', 'product_name', 'category', 'discounted_price', 'actual_price', 'discount_percentage', 'rating', 'rating_count']].copy()

필요한 컬럼만 골라서 df1이라는 새로운 데이터프레임 만들어줌
> .copy()를 붙이는 이유: df1 수정할때 혹시나 df에 영향이 갈 수도 있어서 예방차원에서 처리하는 방식

- catsplit = df['category'].str.split('|', expand=True)
catsplit
*실제 카테고리 형태: Home&Kitchen|Kitchen&HomeAppliances|Vacuum

카테고리 처리를 |를 기준으로 split해달라. expand=True는 split된 것들을 리스트로 두지말고 컬럼 여러개로 펼쳐달라는 뜻.

- catsplit = catsplit.rename(columns={0:'category_1', 1:'category_2', 2:'category_3'})

대분류 -> category_1
중분류 -> category_2
소분류 -> category_3
로 구분하였음. 

-df1['category_1'] = catsplit['category_1']
df1['category_2'] = catsplit['category_2']

이후에 df1에 추가해줌

<img width="1599" height="147" alt="image" src="https://github.com/user-attachments/assets/0aca9fd1-6cc6-4c22-9ec3-dce4a96cb2d8" />




