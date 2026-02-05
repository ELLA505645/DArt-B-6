Spotify 데이터로 인기도를 결정하는 요소를 분석해서,
새로운 노래의 인기도를 예측해보자!


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



