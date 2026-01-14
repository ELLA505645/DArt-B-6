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
* 분포를 보는 이유!

    - 값이 한쪽으로 치우쳐 있는지 확인
    - 이상치가 있는지 확인
    - 로그 변환이 필요할지 확인
~~~

 ### 3. 결측치 및 상관관계 확인

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
     
  
