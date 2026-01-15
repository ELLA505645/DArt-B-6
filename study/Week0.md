~~~
집값을 예측해보자!
~~~

# 분석 흐름 따라잡기

## 1. 데이터 불러오기

- import 00 as 00

  숫자, 표, 시각화, 그래프 등을 불러올껀데, 앞으로 이 이름들을 줄여서 편하게 부르겠다고 지정하는 것
- warning 관련

  경고를 무시하거나 화면에 안띄우게 설정할 수 있음
- plt.rcParams[000] = 000

  그래프의 스타일을 설정하는 것
  그래프 색상, 테두리, 크기 등을 설정할 수 있음
- house = pd.read_csv("000")

  000의 데이터를 읽어서 house라는 데이터프레임에 저장하겠다고 지정
- checking dataset, house.head()

  데이터셋을 잘 불러왔는지 확인, 데이터의 첫5행을 나타내줌 -> 데이터 대략적으로 확인할 수 있음

## 2. 데이터 요약보기

- T 적용해서 시각화
 
    행이랑 열을 T(전치)해서 보기 좋게 정렬하겠다는 뜻

- .bar(subset=['mean'], color=colors[3])

    mean에 대해서 바 형대로 시각화해서 강조해준다
- .background_gradient(subset=['std','50%','max'])
 
    std(표준편차), 50%(중앙값), max(최댓값) 열에 배경 색 그라데이션을 넣어준다
    -> 색깔이 달라서 비교를 쉽게 할 수 있음

- ns.distplot(house['SalePrice'],color=colors[7])
- plt.axvline(x=house['SalePrice'].mean(), color=colors[7], linestyle='--', linewidth=2)
 
    데이터의 분포를 distlot 형식으로 시각화해서 보겠다. 평균값 위치에 세로 점선을 표시해달라.
    -> 평균이 어디쯤인지, 분포가 어떤 모양으로 되어있는지 한눈에 파악할 수 있다.

~~~
❓ 분포를 보는 이유!

    - 값이 한쪽으로 치우쳐 있는지 확인
    - 이상치가 있는지 확인
    - 로그 변환이 필요할지 확인
~~~

 ## 3. 결측치 및 상관관계 확인

- missing = house.isnull().sum()

  컬럼별로 결측치가 몇개있는지 확인 isnull() -> 셀이 비어있다면 TRUE로 체크한다, TRUE의 개수를 SUM한다

- missing = missing[missing > 0], missing = missing.sort_values(ascending = False)

  결측치가 하나라도 있는거 체크, *ascending은 오름차순을 나타내는데 False라고 하면 내림차순을 나타냄.

- plot.bar

  막대그래프로 나타내겠다

- figure, heatmap
     
  새로운 틀 안에 히트맵을 만들겠다.
  * house.corr() : house 데이터프레임의 수치형 변수들끼리의 상관계수를 구해보자
  * sns.heatmap(000): 히트맵을 색으로 표현하자 -> 제일 처음 sns로 줄여말하기로 했던 것
  * annot = True: 각 칸에 숫자를 써주겠다  (annot의 뜻은 주석, 표시)

 ## 4. 전처리 (분석할 수 있는 형태로 데이터를 바꾸는 과정)
 
 - 설명변수(x)랑 타깃 변수(y)를 설정

  타깃변수는 제외시키고, 결측치가 50% 이상인 변수들을 드랍, train데이터랑 test데이터를 일치시키기 위해 test데이터에서도 컬럼 삭제

  - 수치형 데이터(float, int), 범주형 데이터 골라내기
  - SimpleImputer: 결측치를 채워주는 도구 사용
  - MinMaxScaler()

    수치형 컬럼들을 0~1 사이 값으로 스케일링

    ~~~
    ❓ 수치형 컬럼을 스케일링 하는 이유?

    예를 들어,
    - GrLivArea(거주 면적): 500 ~ 4000

    - LotFrontage(도로 접면): 20 ~ 300

    - OverallQual(품질 점수): 1 ~ 10

    - YearBuilt(건축 연도): 1870 ~ 2010

     숫자 크기가 완전히 다름.
     스케일링을 안할 경우
     > 단순히 숫자가 큰 변수를 더 중요한 변수로 착각할 수 있음!!!!
     > 때문에 MinMaxScaler()는 각 컬럼을 0~1범위로 압축해주는 역할 수행
     ~~~

 - 특정 범주형 컬럼 처리하기

   지하실(Bsmt...), 차고(Garage...), 벽난로(FireplaceQu) 와 같은 컬럼들은 값이 비어있는게
   데이터 누락x, 그 시설 자체가 없는 경우가 많음
   > 비어있는 값을 'none'과 같이 '없음'이라는 범주로 만들어준다

   ⭐️특정 칼럼들의 특성을 파악해서 적절하게 결측치를 채워주고, 값을 변환시켜주는 것도 중요한 능력!


### (1) 범주형을 숫자로 바꾸는 결측치 처리

- One-hot encode
~~~
⭐️ 원핫인코딩이란?
범주형을 단순히 수치형으로 변환시키게 되면 무엇을 의미하는지 알 수 없고, 모델은 숫자 크기에 따라 관계를 해석하게 됨.

 그래서, 
  
  Color
  -----
  Red
  Blue
  Green
  Red
  
 라는 데이터가 있을 때,
 
 
 Color_Red   Color_Blue   Color_Green
 -----------------------------------
 1              0              0
 0              1              0
 0              0              1
 1              0              0
 
 이렇게 해주면 행별로 1값이 하나이기 때문에, 어떤 항목에 해당하는지 알 수 있음.
~~~

- 원-핫 인코딩으로 만들어진 칼럼들에 이름을 붙여서 리스트로 뽑기

  encoded_cols = list(encoder.get_feature_names(cat_cols))
- 이 칼럼들을 house에 추가

 house[encoded_cols] = encoder.transform(house[cat_cols])
  

## 5. 학습용 데이터, 검증용 데이터 나누기 (모델 만들기 전 시행)

- train_test_split

  데이터를 학습용, 검증용으로 나눠줌

- X_train: 학습용 입력 / y_train: 학습용 정답 / X_test: 검증용 입력 / y_test: 검증용 정답
*y는 정답인 SalePrice임

- test_Size = 0.25

  25%를 검증용(test)으로 사용하겠다.
  75%는 학습용(train)으로 사용하겠다.







    
