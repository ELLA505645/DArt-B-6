## 🎶 Spotify 데이터로 인기도를 결정하는 요소를 분석해서, 새로운 노래의 인기도를 예측해보자!🎵

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

~~~python
import matplotlib.pyplot as plt
~~~
- matplotlib에는 히스토그램, 선 그래프 등이 포함
~~~python
%matplotlib inline
~~~
- 그래프를 새 창이 아니라 노트북 셀 안에 바로 출력<br>
<br>
[columns:21] [rows:114000]

| 컬럼명 | 의미 |
|------|------|
| popularity | Spotify 인기도 점수 (0~100) | 
| track_name | 곡 제목 | 
| artists | 아티스트 이름 | 
| album_name | 앨범 이름 | 
| track_genre | 곡 장르 | 
| duration_ms | 곡 길이 (밀리초) | 
| explicit | 욕설·성적 표현 포함 여부 | 
| danceability | 춤추기 적합한 정도 | 
| energy | 곡의 에너지 수준 | 
| loudness | 평균 음량 (dB) | 
| speechiness | 말하는 비중 | 
| acousticness | 어쿠스틱 사운드 비중 | 
| instrumentalness | 연주곡일 확률 | 
| liveness | 라이브 공연 느낌 | 
| valence | 감정 톤 (밝음/우울) | 
| tempo | 곡의 속도 (BPM) |
| key | 곡의 조성 | 
| mode | 장조/단조 | 
| time_signature | 박자 구조 |

*explicit이 True면 청소년 유해 콘텐츠(19세), 스포티파이에서 E마크가 붙는다고 함
~~~python
data.drop(data.columns[0], axis=1, inplace=True)
~~~
- 첫번째 컬럼을 열(axis=1)기준으로 삭제, 원본데이터 직접 수정(inplace=True)
* 이 경우에는 index를 삭제한듯
~~~python
data.describe()
~~~
- 수치형 컬럼들의 통계를 요약
<img width="65" height="255" alt="image" src="https://github.com/user-attachments/assets/49a7a125-a6c7-4fd0-a449-bba7db5be758" />

*여기서 확인하면 좋을 것
1) 평균 값
2) 수치형 컬럼들의 각 범위
3) 이상치 존재 여부

~~~python
data.info()
~~~
- Dtype이 bool은 자료형 이름
- bool -> 값이 딱 두개뿐인 자료형 (ex. True/False) (이진변수를 말하는듯)

~~~python
data.popularity.min()
data.popularity.max()
~~~
- 예측 목표인 popularity의 범위와 대략적인 분포값을 확인
- min이 0이고, max가 100이니까 -> 연속적인 분포겠다 -> 회귀모델을 적용해야겠다! 고 생각할 수 있음.

  
## 2. 데이터 EDA

[EDA 순서]
1. Missing Values - 결측치 확인
2. Duplicate values - 중복값 확인
3. All the numerical variables - 수치형 변수 확인
4. Distribution of the numerical variables: Check the skewness of the features. - 수치형 변수 분포 확인(치우침 skewness 확인)
5. Categorical variables -  범주형 변수 확인
6. Cardinality of Categorical variables - 범주형 변수 카테고리 수 확인 
7. Outliers - 이상치 확인
8. Relationship between independent and dependent features - 독립변수와 종속변수의 상관관계 분석

* EDA 방식, 순서 잘 익혀놓기!!

~~~python
features_with_nan=[feature for feature in data.columns if data[feature].isna().sum()>0]
features_with_nan
~~~
- 결측치가 하나라도 있는 컬럼 골라서 새리스트로 저장하기
~~~python
data = data.dropna()
~~~
> 확인 결과: 결측치가 세가지 칼럼에 각 하나씩밖에 없어서 drop하는걸로 결정!
*결측치 수도 전체에 비해 매우 적어서 맞는 판단이라고 생각함.
~~~python
data=data.drop_duplicates()
~~~
- 중복값도 걸러줌
- row가 114000에서 113549로 줄어듦!

### 수치형 변수
~~~python
feature_numerical=[feature for feature in data.columns if data[feature].dtype!='O']
~~~
- dtype = 'O'(알파벳 대문자) -> 문자형(str)
- dtype != 'O' -> 숫자형을 지칭 (int, float, bool(이진변수) 포함)
~~~python
len(feature_numerical)
~~~
- length를 나타냄 -> feature_numerical이 몇개인지 셀 수 있음
~~~python
feature_discrete_numerical=[feature for feature in feature_numerical if data[feature].nunique()<50]
~~~
- ⭐️ 숫자형 컬럼 중에서 연속형말고, 이산형(discrete)을 보겠다는 뜻
-> 범주형 성격의 숫자(숫자에 의미가 있는게 아니라 숫자로 category 분류를 해놓은 컬럼들을
  골라내서 보겠다

~~~python
nunique()
~~~
- 서로 다른 값의 개수
> nunique < 50
- 서로 다른 값의 개수가 50개 미만인 것 골라내기
~~~
❓ 왜 기준이 50개일까?
gpt 답변: 보통 EDA에서 사용하는 경험적, 일반적 기준이라고함
~~~
> ['explicit', 'key', 'mode', 'time_signature'] : 이산형 컬럼 4가지가 추출됨

### 비연속형 수치형 변수 - popularity 관계 분석

~~~python
for feature in feature_discrete_numerical:
    dataset = data.copy()
    sns.barplot(
        x=feature,
        y=dataset['popularity'],
        data=dataset,
        estimator=np.median
    )
    plt.show()
~~~
- 위에서 추출한 이산형 컬럼 4가지를 x축으로, popularity를 y축으로 설정
-  estimator=np.median: 중앙값을 사용 -> 이상치 영향을 최대한 줄이고, 범주별로 대표적인 인기 수준을 보는 그래프 
<img width="706" height="533" alt="image" src="https://github.com/user-attachments/assets/195f1483-ba6e-4a83-9e9d-58c14c119398" />
<img width="715" height="537" alt="image" src="https://github.com/user-attachments/assets/d34f8193-b023-4321-b3de-7134d9c7da66" />

> 해석방법:
각 x 항목에 포함되는 곡들이 얼만큼의 popularity를 가장 보편적으로 나타내고 있는가

-> 이 그래프를 보고 차이가 많이 나는 함수는 나중에 모델에 넣을 수 있음

### 연속형 수치형 변수 - popularity 관계 분석

~~~python
features_continuous_numerical=[features for features in feature_numerical if features not in feature_discrete_numerical]
~~~
- feature_numerical 이면서 이산형(feature_discrete_numerical)이 아닌 변수
-> 연속형 수치형 변수만 골라서 추출할 수 있음

> ['popularity', 'duration_ms', 'danceability', 'energy', 'loudness', 'speechiness',
 'acousticness', 'instrumentalness', 'liveness', 'valence', 'tempo'] 칼럼 추출
  
~~~python
from scipy.stats import skew

for feature in features_continuous_numerical:
    dataset = data.copy()
    print(feature, 'skewness is :', skew(dataset[feature]))
    sns.histplot(x=feature, data=dataset, bins=25, kde=True)
    plt.show()
~~~
- skew(dataset[feature])) : 분포의 비대칭 정도를 숫자로 계산
*bins= 막대그래프의 개수, kde=True 곡선 표시

<img width="709" height="536" alt="image" src="https://github.com/user-attachments/assets/27fca901-ab43-4c57-ab72-56b0b323ecf4" />

> 음향 데이터에는 표준 음량대 (보편적인 범위)가 존재해서 loudness가 0아래에 거의 몰려있음.

<img width="720" height="524" alt="image" src="https://github.com/user-attachments/assets/623bd30c-e148-48ae-a723-8c8ccb121d58" />

>예:acousticness -> 오른쪽으로 치우친 분포
*대부분 값이 작고 일부만 큰 값을 가짐
*이런 유형처럼 치우쳐있는 분포는 로그변환, 제곱근 변환 등의 수치변환을 해줄 수 있음.

<img width="733" height="537" alt="image" src="https://github.com/user-attachments/assets/28aeead9-8857-4e83-8584-a75fda521840" />
>정규분포 모양 -> 선형회귀 등에 적합하다

### 연속형 수치형 변수들간의 상관관계 확인

- 히트맵 활용
-> 변수들끼리 서로 얼마나 비슷하게 움직이냐
  
1) 변수 간 중복 정보 확인 (상관계수가 너무 높으면 둘 중 하나만 써도 됨)
2) 모델 안정성 (다중공선성 방지)

-> 어떤 연속형 수치형 변수들을 사용할지 골라내는 과정

### 연속형 수치형 변수들간의 이상치 확인

- boxplot 확인
<img width="709" height="528" alt="image" src="https://github.com/user-attachments/assets/f7f74a6e-932a-4526-9a72-f428be94c36a" />

이런식의 이상치가 많은 그래프가 많이 나옴
but, 오디오 데이터 특성 자체가 한쪽에 몰리고 극단값이 소수로 존재하는 구조임
-> 당연한 그래프 모양이기 때문에 여기서는 별도의 처리 진행X -> 신중하게 판단

### 범주형 변수

~~~python
feature_categorical=[feature for feature in data.columns if data[feature].dtypes=='O']
print('Number of categorical features:', len(feature_categorical))
data[feature_categorical].head()
~~~
- dtypes=='O' : 문자열 기반 컬럼들만 지칭
- 'Number of categorical features: 5' 결과 추출 -> 범주형 변수는 5개

~~~python
for feature in feature_categorical:
    dataset=data.copy()
    print(feature, ': Number of unique entries:', dataset[feature].nunique())
~~~
- Number of unique entries : 서로 다른 고유한 값이 몇개인지 추출
<img width="494" height="160" alt="image" src="https://github.com/user-attachments/assets/b96b562e-fb8e-43a3-9b0d-58e9d38b9559" />

- 대부분 몇만개의 고유값을 가짐
- track_genre -> 범주 개수가 114개로, 그룹별로 특성을 파악할 수 있을 것 같이 보인다!


### track_genre(장르)와 popularity의 관계 시각화

~~~python
dataset=data.copy()
plt.figure(figsize=(16,12))
sns.lineplot(x='track_genre', y='popularity', data=dataset)
plt.xticks(rotation=90)
plt.show()
~~~
- plt.xticks(rotation=90) : x축의 글자를 90도 회전시켜서 가독성을 좋게 만듦
*x변수가 너무 많을 때 사용하면 좋을듯

## 3. Feature Engineering - 모델 적용 위한 데이터 가공 단계

[데이터들을 모델에 적용할 수 있도록 처리]
1) speechiness의 의미를 더 분명하게 변환
2) 연속형 변수의 skewness(치우침) 정도를 완화
3) 범주형 변수를 숫자로 변환
4) 변수 스케일을 같은 범위로 맞추기


### 3-(1) speechiness 처리
- 원래 컬럼 의미는 '말소리의 비중'
- 이 사람은 이 컬럼을 더 구체적으로 분석해서 음악/랩/말 위주 로 분류하고자 했음
- 연속형 변수 -> 이산형(범주형) 변수로 변환

~~~python
speechiness_type=[]
for i in data.speechiness:
    if i<0.33:
        speechiness_type.append('Low')
    elif 0.33<=i<=0.66:
        speechiness_type.append('Medium')
    else:
        speechiness_type.append('High')

~~~
- speechiness_type 라는 파생변수를 따로 만들어줌 (분류 수치는 spotify의 공식 설명 활용)
- 범위에 따라 라벨링을 해줌

| 라벨명 | 수치 |
|------|------|
| Low | 109947 | 
| Medium | 2726 | 
| High |  876 | 

-> 그래도 꽤 의미있는 분류인 것 같다


### 3-(2) skewness 처리

- 연속형 수치형 변수들과 popularity의 관계를 더 정확하게 보기 위해서 4가지의
  정규분포 방식을 시도해준다
~~~python
dataset_log[feature] = np.log(dataset_log[feature] + 1)
~~~
- 오른쪽 꼬리를 줄여주는 방식
~~~python
dataset_reci[feature] = 1 / (dataset_reci[feature] + 1)
~~~
- 큰 값 압축, 작은 값을 강조해줌
*분포를 뒤집는 경우도 있다
~~~python
dataset_sqrt[feature] = dataset_sqrt[feature] ** (1/2)
~~~
- 완만한 왜도를 완화
~~~python
dataset_expo[feature] = dataset_expo[feature] ** (1/5)
~~~
- dataset_sqrt보다 더 강하게 압축
*강한 skew일때 사용

### 3-(2) skewness 처리 - 변환 방법별 비교 (어떤 방식이 정규분포에 가장 가까워지는지)

- skewness ≈ 0 → 정규분포에 가까움 (이상적)

- |skewness| ≈ 0 → 정규분포에 가까움 (이상적)

[Q-Q plot]
*불러오는 도구는 probplot 으로 사

<img width="865" height="263" alt="image" src="https://github.com/user-attachments/assets/4fe99b33-41af-4c51-8d04-745a78e96a00" />

- 빨간 선: 이 데이터가 완벽한 정규분포라면 그려져야 할 기준선
1) 점들이 빨간 선을 잘 따라갈 때: 정규분포에 가갑다
2) 왼쪽이나 오른쪽에서 휘어진다: skewness 존재
3) 계단처럼 뭉쳐있다 : 값이 특정 구간에 많이 몰려있음


| 변수명 |  적용한 변환  |
| ---------------- | -------- |
| popularity        | 변환 없음               | 
| danceability     | | 변환 없음               |
| energy           | 변환 없음               | 
| valence           | 변환 없음               | 
| tempo            | 변환 없음               |
| loudness         |  변환 없음               | 
| duration_ms      |  1/5 power (x^(1/5)) | 
| speechiness      |  1/5 power (x^(1/5)) | 
| instrumentalness |  1/5 power (x^(1/5)) | 
| liveness         | 1/5 power (x^(1/5)) | 
| acousticness     |  제곱근 (x^(1/2))       | 

> 구분기준에 따라 각 변수별로 적합한 변환방법 채택

- 이후에 한번 더 히트맵을 그려서 변화사항을 확인한다

### 3-(3) 범주형 컬럼 인코딩

*범주형 컬럼의 고유값(cardinality)가 너무 커서 원-핫 인코딩은 어려움

> BaseN encoding
- 범주를 N진법 기반 숫자로 변환
- 고유값이 많은 변수에 적합한 인코딩 방법

~~~python
pip install category_encoders
import category_encoders as ce
~~~
- category_encoders : 높은 cardinality 범주형 변수를 위한 라이브러리

⭐️
~~~python
encoder1 = ce.BaseNEncoder(
    cols=['track_genre','album_name', 'track_name','artists'],
    base=10,
    return_df=True
)
data = encoder1.fit_transform(data)
data.head()
~~~
- BASE-10 진법으로 만들어준다
⭐️
~~~
ex. 54928번째 고유값의 taylor swift를 표현해보자<br>
<br>

*최댓값이 몇만. -> 총 다섯자리의 수
-> 자릿수 컬럼이 
artists_0 / artists_1 / artists_2 / artists_3 / artists_4

이렇게 생성됨 <br>
<br>
<img width="761" height="314" alt="image" src="https://github.com/user-attachments/assets/0120fc3e-5664-446e-a243-8be5c49bc66f" />

표현해보면)

| artists_0 | artists_1 | artists_2 | artists_3 | artists_4 |
| --------- | --------- | --------- | --------- | --------- |
| 8         | 2         | 9         | 4         | 5         | <br>
<br>
- one-hot 인코딩
범주 50,000개
컬럼 50,000개<br>
<br>
- BASEN
범주 50,000개
컬럼 5개 <-!!
~~~

### 3-(4) Feature 스케일링

~~~python
data['explicit'] = np.where(data['explicit']==False, 0, 1)
~~~
- 19세 (True / False) -> 0 / 1 로 변환

~~~python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
~~~
- 지금까지 처리해준 변수들에 대해서 표준화를 진행해준다 (범위 맞춰주려고)

- y변수인 'popularity'랑 이진변수인 'mode'는 스케일에서 제외

❓
근데 왜 이진변수인 explicit는 제외안할까?

-> GPT한테 물어보니까 제외해주면 더 좋다고함.

~~~python
for feature in features_scaling:
    data[feature] = data_to_replace[feature].values
~~~
- 원본 데이터에 스케일링 한 값 반영
- 마지막으로 한번 더 결측치 체크


### 3-(5) Feature selection

~~~python
X = data.drop(['popularity'], axis=1)
y = data['popularity']
X.head()
~~~ 
1) 타깃 분리 - y변수를 popularity로 두고 나머지 컬럼들은 모두 x로
~~~python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=7
)
~~~ 
2) train/test 분리 - 전체 데이터의 30%를 테스트로 사용
* random_state=7 : 생성 방식 규칙을 7로 고정한다 (숫자는 크게 의미없고, 방식을 고정한다는 뜻)

~~~python
def correlation(dataset,threshold):
    correlated_columns=set()
    correlation_matrix=dataset.corr()
    for i in range(len(correlation_matrix.columns)):
        for j in range(i):
            if abs(correlation_matrix.iloc[i,j])>threshold:
                colname=correlation_matrix.columns[i]
                correlated_columns.add(colname)
    return correlated_columns
~~~ 
4) 상관 높은 변수 제거
- dataset.corr() : 입력 변수들끼리의 상관계수 행렬 (X들 서로 간의 상관 파악)
- 서로 너무 비슷해서 굳이 2번 넣을 필요없는 변수들을 찾아줌

5) 학습용 데이터 재구성
- 변동사항 train이랑 test 데이터 똑같이 반영
- 최종적으로 결측값 있는지 확인


## 4. 모델링

*서로 다른 회귀 모델들을 비교
~~~python
from sklearn.linear_model import LinearRegression, Lasso, Ridge
from xgboost import XGBRegressor, XGBRFRegressor
from sklearn.tree import DecisionTreeRegressor
from sklearn.linear_model import BayesianRidge
~~~ 

[해석방법]

~~~python
plt.subplot(1,2,1)
plt.scatter(y_test,prediction)
    
plt.subplot(1,2,2)
sns.distplot(residual, hist=False, kde=True)
plt.show()
~~~ 
scatter: 점들이 y=x대각선 주변에 모여있을수록 예측이 정확하다는 뜻
residual: 실제값-예측값 간의 잔차를 나타냄. 중심이 0근처-> 평균적으로 편향이 없음 / 한쪽으로 쏠림 -> 과대, 과소 예측



x축: 실제 popularity
y축: 모델에 예측한 popularity <br>
<br>
[좋은 모델]
<img width="714" height="390" alt="image" src="https://github.com/user-attachments/assets/ad36b769-0c6a-4b0d-bc8f-f8e95885a06f" />

[안좋은 모델]
<img width="706" height="407" alt="image" src="https://github.com/user-attachments/assets/fadf3cb9-8b43-4b2e-8b2b-6259b8580cff" />


## 4. 모델링 - 모델 성능 확인

~~~python
from sklearn.metrics import mean_squared_error, mean_absolute_error
from sklearn.metrics import r2_score
~~~ 
- 휘귀 모델에서 성능 확인할 때 쓰는 성능 지표들

<img width="505" height="273" alt="image" src="https://github.com/user-attachments/assets/b9758734-d43a-4191-a950-065a92935337" />

[봐야하는 순서]
1) R² score (설명력) -> 클수록 좋다
   
2) MAE (ABMSE) (평균 오차 크기) -> 작을수록 좋다

3) MSE (오차 제곱) -> 작을수록 좋다
* 더 세밀하게 오차를 확인할 수 있다

<img width="715" height="404" alt="image" src="https://github.com/user-attachments/assets/6cfed3b1-972e-4a49-8b30-9dd7f0bbbf83" />

-> XGBOOST가 제일 성능이 좋음!!!<br>
<br>

**⭐️ 이렇게 결과가 나온 이유**

popularity를 결정하는 요인은 굉장히 조건적이고, 비선형적이다.
ex. 같은 장르라도 아티스트에 따라 영향 다름 / danceability가 높아도 energy가 낮으면 인기 없음

-> 트리 모델이 유리 







