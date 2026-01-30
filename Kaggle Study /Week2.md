**📊AMAZON SALES raw data를 파헤쳐보자!**

시작하기 전,
데이터 설명:
> 1) 1000개 넘는 실제 상품이 존재
> 2) 각 상품에는 식별 번호(ID)가 있음
> 3) 인도 지역의 상품

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

- **import numpy as np**
- **import pandas as pd**
- **import matplotlib.pyplot as plt**
- **import seaborn as sns**


**[Columns]- 16개**
~~~
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

**[Rows]: 1465**

## 2. 데이터 전처리하기

- **df['discounted_price'] = df['discounted_price'].str.replace("₹",'')**
  
  **df['discounted_price'] = df['discounted_price'].str.replace(",",'')**
 
  **df['discounted_price'] = df['discounted_price'].astype('float64')**

할인율 칼럼에서 ₹,기호를 공백으로 바꿔서 제거해주고, float64타입으로 바꿔준다

*df['discount_percentage'] = df['discount_percentage'].str.replace('%','').astype('float64')

-> 이런식으로 합쳐서 쓸 수도 있음

- **df['rating'].value_counts()**

  ' ' 컬럼에 대해 개수 세는 코드

- **df.query('rating == "|"')**
  
평점에서 '|'인 문자를 찾음 

> 보통 리뷰 크롤링 같은걸 하면 평점이 없는 경우에 |로 출력되어서 나오는 경우가 많다고 함

- **df['rating'] = df['rating'].str.replace('|', '4.0').astype('float64')**

  rating 컬럼에서 |에 해당하는 것을 4.0으로 바꿔준다 (아마존에서 실제로 확인 후)

- **df['rating_count'] = df['rating_count'].str.replace(',', '').astype('float64')**

rating_count에서 1,234처럼 ,가 들어있을 수 있어서 1234.0인 float형식으로 바꿔준다

- **duplicates = df.duplicated()**

> df[duplicates]

df.duplicated()가 중복값을 찾아주는 함수

- **df.isna().sum()**

isna()에서 결측치면 True, 아니면 False 

-> Sum()을 통해서 True값을 1로 계산해서 컬럼별로 결측치 개수를 세어준다

- **df1 = df[['product_id', 'product_name', 'category', 'discounted_price', 'actual_price', 'discount_percentage', 'rating', 'rating_count']].copy()**

필요한 컬럼만 골라서 df1이라는 새로운 데이터프레임 만들어줌

> .copy()를 붙이는 이유: df1 수정할때 혹시나 df에 영향이 갈 수도 있어서 예방차원에서 처리하는 방식

- **catsplit = df['category'].str.split('|', expand=True)**

~~~
⭐️ split

(*실제 카테고리 형태: Home&Kitchen|Kitchen&HomeAppliances|Vacuum)

카테고리 처리를 |를 기준으로 split해달라. expand=True는 split된 것들을 리스트로 두지말고 컬럼 여러개로 펼쳐달라는 뜻. 

(expand=False면 결과값을 리스트 형태로 한 셀에 유지시켜줌)
~~~

- **catsplit = catsplit.rename(columns={0:'category_1', 1:'category_2', 2:'category_3'})**

대분류 -> category_1

중분류 -> category_2

소분류 -> category_3

로 구분하였음. 

- **df1['category_1'] = catsplit['category_1']
df1['category_2'] = catsplit['category_2']**

이후에 df1에 추가해줌

<img width="402" height="161" alt="image" src="https://github.com/user-attachments/assets/9d407b19-c982-499e-b623-67026b754be8" />

> 결과
<img width="1599" height="147" alt="image" src="https://github.com/user-attachments/assets/0aca9fd1-6cc6-4c22-9ec3-dce4a96cb2d8" />
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ



- **df1['product_id'].str.strip()**

strip()함수는 문자열 앞뒤 공백을 제거해주는 역할

- **score에 대한 분류 네이밍을 별도로 해줌**
~~~
rating_score = []

for score in df1['rating']:
    if score < 2.0 : rating_score.append('Poor')
    elif score < 3.0 : rating_score.append('Below Average')
    elif score < 4.0 : rating_score.append('Average')
    elif score < 5.0 : rating_score.append('Above Average')
    elif score == 5.0 : rating_score.append('Excellent')
~~~

- **df1['rating_score'] = df1['rating_score'].astype('category')**

category는 범주형 타입을 말함. 

*위의 분류처럼 정해진 라벨 집합을 다룰때 좋다!

- **df1['rating_score'] = df1['rating_score'].cat.reorder_categories(['Below Average', 'Average', 'Above Average', 'Excellent'], ordered=True)**

~~~
⭐️cat.reorder_categories() 함수

category(범주형) 데이터에 순서를 부여해주는 함수

ordered = True로 두면 아래->위 순서
~~~


 - **df1['difference_price'] = df1['actual_price'] - df1['discounted_price']**

   (실제가 - 할인가) 컬럼을 새로 만들어줌

> 결과
<img width="1615" height="139" alt="image" src="https://github.com/user-attachments/assets/daa00e47-0af2-47a0-9c20-a82bfe854dd5" />

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
- **reviewer_id_exp = reviewer_id_split.explode()**


**⭐️explode() 함수: 리스트를 행 방향으로 풀어버리는 함수**
<img width="683" height="89" alt="image" src="https://github.com/user-attachments/assets/a2d72b48-24c7-44f0-b69f-1b31a7314d00" />
  
> 결과
<img width="708" height="204" alt="image" src="https://github.com/user-attachments/assets/dfec1c98-5a14-4667-a6fc-671af1929ce7" />

-> 여기서 reset_index(drop=True) 활용하여 인덱싱 다시 해줌

- **df21 = pd.DataFrame(data=reviewer_id_clean)
df22 = pd.DataFrame(data=reviewer_name_clean)**

새로 정리된 데이터들 new 데이터프레임으로 받아준다

~~~
❓ 데이터를 그냥 받는거랑 DataFrame으로 받는거랑 차이점이 뭘까?

- 그냥 받을때는, Series로 타입이 유지되는 경우
reviewer_id_exp = reviewer_id_split.explode()
*따로 감쌀 필요없이 그대로 출력해도 됨

- 데이터프레임으로 받을때는, Series를 DataFrame으로 받고 싶은 경우
df21 = pd.DataFrame(data=reviewer_id_clean)
*전체를 감싸서 바꾸고 싶을 경우 사용

두개를 잘 구분하는 것이 중요할 것 같다!
~~~

- **df2 = pd.merge(df21, df22, left_index=True, right_index=True)**

user_id랑 user_name이랑 merge 처리해준다 (앞에서 자르고 split하고 했지만 순서는 그대로 유지되기 때문에 merge가능)

> 결과값
<img width="482" height="240" alt="image" src="https://github.com/user-attachments/assets/a8c5bc5f-6fd9-4e02-b488-2b250e30c2cf" />

## 3. 시각화
> 데이터를 살펴보고 이해하는 과정

- **sns.set_palette(palette="icefire")**

  icefire 색상 팔레트는 대비가 강한 색상 조합이라서 barplot같은 시각화에 잘 어울림

### <주제1: Product Category>
*category_1과 category_2의 관계를 파악하는 것이 목적

- **main_sub = df1[['category_1', 'category_2', 'product_id']]**
- **main_sub_piv = pd.pivot_table(main_sub, index=['Main Category', 'Sub-Category'], aggfunc='count')**

피벗 테이블을 생성하겠다

<img width="703" height="714" alt="image" src="https://github.com/user-attachments/assets/5bfc8987-e56c-4151-a20a-5116a5b455b2" />

⭐️
1) 메인 카테고리 아래에
2) 해당하는 서브 카테고리들이 계층적으로 정렬됨
   -> 멀티 인덱스
3) 인덱스로 사용되지 않고 남은 product_id의 개수를 count해서 적어줌

- **most_main_items = df1['category_1'].value_counts()
    .head(5)
    .rename_axis('category_1')
    .reset_index(name='counts')**

  category_1에서 카테고리별 상품 개수 계산 후 head(5) 사용해서 가장 많은 상위 5개만 선택

  > 결과값
  <img width="738" height="715" alt="image" src="https://github.com/user-attachments/assets/860292d7-4992-4416-b445-0213469861a9" />

-> Electronics 항목이 가장 상품이 많다

-> 그중에서도 acccessories & peripherals 상품들이 가장 많다


### <주제2: Correlation between Features>
*변수들간의 상관관계를 파악하는 것이 목적

- **sns.heatmap(ax=ax[0], data=df1.corr())**

df1.corr() : df1의 수치형 컬럼들만 자동으로 선택
각 컬럼쌍들에 대해서 피어슨 상관계수 계산

- **sns.scatterplot(ax=ax[1], data=df1, y='discounted_price', x='actual_price', color='brown')**

산점도 그리기


<img width="698" height="333" alt="image" src="https://github.com/user-attachments/assets/91859697-acdc-45c2-9bae-648a19cd6cd0" />

히트맵 확인 후 상관관계가 있어보이는 두 변수 discounted_price와 actual price를 선택해서 산점도를 확인함 

(근데 약간 당연한거 아닌가....하는 의문)

-> 다른 변수들간에는 크게 상관관계가 없다는걸로 해석하면 좋을듯

### <주제3: rating>

- **sns.histplot(ax=ax[0], data=df1, x='rating', bins=15, kde=True, color='blue')**

bins= 15 라는 것은 막대그래프가 15개가 되도록 그리는 것

(값이 촘촘한지, 분포가 넓은지에 따라서 bins의 숫자를 조정해줄 수 있다)

kde = True: 분포 곡선 추가

> 결과
<img width="706" height="257" alt="image" src="https://github.com/user-attachments/assets/78efb240-6247-43a8-9445-84d5af281bcc" />
ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
*전형적인 결과

1) 평점이 3.5~4.5가 가장 많음
2) amount of ratings가 인기있는 상품에는 많고, 대부분 리뷰 수가 거의 없음


- **boxplot**

  <img width="736" height="419" alt="image" src="https://github.com/user-attachments/assets/1a89a297-47c5-4069-9b00-11998f62922a" />

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ

> 이런 판매 데이터에서 boxplot을 활용하면 제품별로 중앙값, 최소/최대, 이상치 등등을
한번에 비교할 수 있어서 좋은 것 같다!!

- **rate_main_cat = df1.groupby(['category_1','rating_score']).agg('count').iloc[:,1].rename_axis().reset_index(name='Amount')**

category1과 rating_score를 groupby해서 category1별로 각 rating_score에 해당하는 개수를 셀거다,


### <주제4: Reviewers>

- **top_reviewer = data=df2['user_name'].value_counts()**

- user_name기준으로 숫자 세줌

~~~
❓Reviewers로 barplot을 왜 만들어보냐?

1️⃣ 평점/리뷰를 의사결정에 쓰려고 할 때

어떤 상품이 평점도 높고, 리뷰수가 많은데 소수의 계정에서 나온거라면,

리뷰 데이터의 대표성이 의심됨

2️⃣ 리뷰 조작 가능성 or 이상 패턴 탐지

한 사람이 짧은 기간에 반복 리뷰, 수십개 상품을 리뷰

3️⃣ 익명 리뷰 구조를 이해

“Amazon Customer”, “Kindle Customer" 이름은 실제 사용자명이 아니라 플랫폼에서 자동으로 처리된 공용 표시명

-> 아, user_name은 개인 식별자로 부적절하구나 를 판단할 수 있다
~~~



# 2️⃣ 느낀점

시각화를 어떻게 하느냐에 따라서 데이터에 대해 판단할 수 있는 방법이 무궁무진하다

잘 익혀놓자!!

















