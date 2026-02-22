
🚕하루 시간대별로 택시 이동(유입/유출)이 어떻게 달라질까?🚖
- 
NYC를 위치 기반으로 몇 개의 구역으로 나눈 뒤, 시간대별로 각 구역에 들어오고 나가는 택시 수를 파악하자

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

~~~python
import numpy as np
~~~
- 수치 계산, 연산 라이브러리
~~~python
from matplotlib import animation
~~~
- 애니메이션(시간에 따라 변하는 시각화) 만들 때 사용하는 라이브러리
~~~python
from matplotlib import cm
~~~
- 컬러맵(colormap) 모음
- 클러스터별 색칠, 히트맵 등에서 자주 사용
~~~python
from sklearn.cluster import KMeans
~~~
- k-means 군집화 알고리즘 사용할 때
(위경도 좌표를 넣어서 k개 구역으로 쪼갤 때 사용)
~~~python
from sklearn.neighbors import KNeighborsClassifier
~~~
- KKN 분류기
- 새로운 좌표가 들어왔을 때 어느 클러스터에 속하는지 빠르게 예측할 때 사용

### 컬럼 확인

| 컬럼명                | 타입     | 설명                          |
| ------------------ | ------ | --------------------------- | 
| id                 | object | 택시별 고유 ID         | 
| vendor_id          | int    | 택시 회사 구분 ID (주로 1, 2)       | 
| pickup_datetime    | object | 승차 시각                       | 
| dropoff_datetime   | object | 하차 시각                       | 
| passenger_count    | int    | 탑승 인원 수                     | 
| pickup_longitude   | float  | 승차 지점 경도                    |
| pickup_latitude    | float  | 승차 지점 위도                    | 
| dropoff_longitude  | float  | 하차 지점 경도                    | 
| dropoff_latitude   | float  | 하차 지점 위도                    | 
| store_and_fwd_flag | object | 데이터가 서버로 즉시 전송되었는지 여부 (Y/N) | 
| trip_duration      | int    | 이동 시간 (초 단위)                | 

### 포함할 위치 범위 좁히기

~~~python
xlim = [-74.03, -73.77]
ylim = [40.63, 40.85]
df = df[(df.pickup_longitude> xlim[0]) & (df.pickup_longitude < xlim[1])]
df = df[(df.dropoff_longitude> xlim[0]) & (df.dropoff_longitude < xlim[1])]
df = df[(df.pickup_latitude> ylim[0]) & (df.pickup_latitude < ylim[1])]
df = df[(df.dropoff_latitude> ylim[0]) & (df.dropoff_latitude < ylim[1])]
~~~
- xlim[0] -> 왼쪽 경계 (-74.03)
- xlim[1] -> 오른쪽 경계 (-73.77)
- ylim[0] -> 아래쪽 경계 (40.63)
- ylim[1] -> 위쪽 경계 (40.85)


### 승하차 위치 지도 위에 시각화

~~~python
longitude = list(df.pickup_longitude) + list(df.dropoff_longitude)
latitude = list(df.pickup_latitude) + list(df.dropoff_latitude)
~~~
1) 승차 경도와 하차 경도를 하나의 리스트로 합침
2) 승차 위도와 하차 위도를 하나의 리스트로 합침

~~~python
plt.plot(longitude, latitude, '.', alpha = 0.4, markersize = 0.05)
~~~
- 경도, 위도를 '.'으로 표시
- alpha = 0.4 -> 점을 0.4만큼 투명하게 한다는 뜻 (밀집도를 더 잘 볼 수 있게)
- markersize = 0.05 -> 점을 엄청 작게 찍어서 겹쳐도 잘 구분할 수 있게 <br>
<br>

> **❓ 승차 경도랑 승차 위도, 하차 경도랑 하차 위도로 각각 리스트화 하면 안될까?**
~~~
plt.plot(longitude,latitude)
이기 때문에 

(longitude[0], latitude[0])  → pickup 1
(longitude[1], latitude[1])  → pickup 2
...
(longitude[n], latitude[n])  → dropoff 1
(longitude[n+1], latitude[n+1]) → dropoff 2

이렇게 앞부분은 앞부분끼리, 뒷부분은 뒷부분끼리 자동으로 짝이 맞아짐
~~~
<img width="731" height="668" alt="image" src="https://github.com/user-attachments/assets/87685cd9-8e8b-4e2f-b806-d96657d487eb" />

1️⃣ 점 하나 = 택시 승차 or 하차 위치 하나

2️⃣ 점이 진하게 겹친 곳 = 택시 활동이 많은 지역

3️⃣ 클러스터링을 할 수 있는 느낌이 온다

~~~python
loc_df['longitude'] = longitude
loc_df['latitude'] = latitude
kmeans = KMeans(n_clusters=15, random_state=2, n_init=10).fit(loc_df)
~~~
- n_clusters = 15 -> 구역을 15개로 쪼갠다
- random_state=2 -> 방식을 숫자하나(2)로 고정시켜서 동일한 결과값이 나오도록 한다
- n_init=10 -> 초기 중심점을 10번 다르게 해서 그중에서 가장 좋은 결과를 선택
  
**10이 보통 자주 사용하는 값이라고 함*

~~~python
loc_df['label'] = kmeans.labels_
~~~
- 각점이 어느 클러스터에 속했는지 저장 -> 각 점마다 구역번호가 붙게 됨.

~~~python
loc_df = loc_df.sample(200000)
plt.figure(figsize = (10,10))
for label in loc_df.label.unique():
plt.plot(loc_df.longitude[loc_df.label == label],loc_df.latitude[loc_df.label == label],'.', alpha = 0.3, markersize = 0.3)
~~~
- 점이 너무 많아서 200000개만 뽑아서 진행
- 각 클러스터 구역별로 plt 시각화를 진행한다
  
**색상을 별도로 지정하지 않아도 여러번 plot을 호출하면 자동으로 색을 순서대로 바꿔준다*

<img width="703" height="675" alt="image" src="https://github.com/user-attachments/assets/c23c5c17-925f-49d9-be75-88796c71d5b2" />

> 클러스터링 결과가 실제 뉴욕의 지역 구분과 비슷하게 나타남

~~~python
ax.plot(kmeans.cluster_centers_[label,0],kmeans.cluster_centers_[label,1],'o', color = 'r')
~~~
- kmeans.cluster_centers : 클러스터의 평균을 자동으로 계산함
- kmeans.cluster_centers_[label,0] -> 해당하는 클러스터 라벨의 0번째(longitude) 변수들의 평균
- kmeans.cluster_centers_[label,1] -> 해당하는 클러스터 라벨의 1번째(latitude) 변수들의 평균

~~~python
ax.annotate(label, (kmeans.cluster_centers_[label,0],kmeans.cluster_centers_[label,1]), color = 'b', fontsize = 20)
ax.set_title('Cluster Centers')
plt.show()
~~~
- annotate() : 그래프 위에 텍스트를 특정 좌표에 붙이는 함수
- ax.annotate(label, (x, y)) : (x,y) 위치에 label이라는 글자를 써라
<img width="702" height="676" alt="image" src="https://github.com/user-attachments/assets/c8761ee6-1bac-4fed-8c93-5bcbfed5795e" />

### 택시 이동 양상 지도 위에 시각화 - (1) 중심점 찍어주기
~~~python
df['pickup_cluster'] = kmeans.predict(df[['pickup_longitude','pickup_latitude']])
~~~
- 각 승차 위치가 어느 클러스터 구역에 속하는지 예측하고 판단 -> pickup_cluster에 저장

~~~python
df['dropoff_cluster'] = kmeans.predict(df[['dropoff_longitude','dropoff_latitude']])
~~~
- 각 하차 위치가 어느 클러스터 구역에 속하는지 예측하고 판단

-> dropoff_cluster에 저장

~~~python
df['pickup_hour'] = df.pickup_datetime.apply(lambda x: parser.parse(x).hour )
~~~
- parser.parse(x) : 문자열인 pickup_datetime을 datetime형태로 변환
- .hour : 시(hour)만 추출
- pickup_hour 라는 새로운 칼럼으로 저장


> **❓왜 출발시간만 처리하고 도착시간은 처리를 안할까?**
~~~
이 노트북의 분석 목적은 수요 발생 시점 기준으로, 시간대별 수요 분석 / 출발 지역별 혼잡 분석
을 알아보는거라서 pickup시간 중심으로 본다
~~~

~~~python
clusters['x'] = kmeans.cluster_centers_[:,0]
clusters['y'] = kmeans.cluster_centers_[:,1]
clusters['label'] = range(len(clusters))
~~~
- [:,0] -> :(모든 행), 0(0번째 열)
- [:,1] -> :(모든 행), 1(1번째 열)
- 모든 클러스터의 위도와 경도의 평균을 가지고 온 것

~~~python
clusters['label'] = range(len(clusters))
~~~
- 클러스터 번호를 붙여줌 (0~14)
- 클러스터의 len 길이(개수)에 맞게 번호를 붙여서 'label'로 정의해줌

예시)

| x (경도) | y (위도) | label |
| ------ | ------ | ----- |
| -73.98 | 40.75  | 0     |
| -73.99 | 40.76  | 1     |
| ...    | ...    | ...   |


### 택시 이동 양상 지도 위에 시각화 - (2) 시간대별 이동 흐름 애니메이션

~~~python
def animate(hour):
~~~
- 특정 시간에 대한 프레임 설정. 애니메이션은 이 함수를 시간대별로 반복 실행 

~~~python
ax.plot(loc_df.longitude[loc_df.label == label],
loc_df.latitude[loc_df.label == label],
'.', alpha = 1, markersize = 2, color = 'gray');
~~~
- 배경으로 모든 점을 gray 컬러로 찍어준다

~~~python
ax.plot(kmeans.cluster_centers_[label,0],
kmeans.cluster_centers_[label,1],
'o', color = 'r');
~~~
- 각 클러스터 중심을 빨간점으로 표시해준다


[출발 클러스터, 도착 클러스터 정의]
~~~python
num_of_rides = len(
    df[(df.pickup_cluster == label) &
       (df.dropoff_cluster == dest_label) &
       (df.pickup_hour == hour)]
)
~~~
- pickup_cluster == label로
- dropoff_cluster == dest_label
- pickup_hour == hour 정의
- num_of_rides = len() -> label → dest_label로 이동한 ride 개수

~~~python
dist_x = clusters.x[...] - clusters.x[...]
dist_y = clusters.y[...] - clusters.y[...]
~~~
- 출발 -> 도착 x좌표 거리 차이 계산
- 출발 -> 도착 y좌표 거리 차이 계산

~~~python
pct = np.true_divide(num_of_rides, len(df))
~~~
- 전체 ride 중 해당 이동이 차지하는 비율
* 이동이 많을수록 선이 굵게 표시됨)

~~~python
arr = Arrow(start_x, start_y, -dist_x, -dist_y, width=15*pct)
~~~
- 벡터함수 /  start_x, start_y 에서 -dist_x, -dist_y 방향으로 화살표 생성
- dist_x : 출발 - 도착 -> -dist_x : 도착- 출발
  
  ex) 출발 = (1,1) / 도착 = (4,5)
  
  dx = 4 - 1 = 3, dy = 5 - 1 = 4
  
  -> Arrow(1,1,3,4)

~~~python
ax.add_patch(arr)
~~~
- 화살표를 그래프에 추가

~~~python
ani = animation.FuncAnimation(
    fig,
    animate,
    sorted(df.pickup_hour.unique()),
    interval = 1000
)
~~~
- hour (0~23시) 순서대로 animate실행
- 1000ms 단위 간격 -> 1초 간격으로 애니메이션 변

~~~python
ani.save('animation.gif', writer='imagemagick', fps=2)
~~~
- GIF 파일로 저장
- fps=2 -> 초당 2프레임
<img width="636" height="601" alt="image" src="https://github.com/user-attachments/assets/f1de7210-25f4-4e7a-bb33-da7687ed2ca7" />

1️⃣ 각 시간대별로 출발 구역 -> 도착 구역 이동을 나타냄

2️⃣ 이동량이 많을수록 화살표가 두꺼움

### 택시 이동 양상 지도 위에 시각화 - (2) 시간대별 가중치 적용 이동 흐름 애니메이션

~~~python
pct = np.true_divide(num_of_rides, len(df[df.pickup_hour == hour]))
~~~
- 위의 애니메이션은 전체 시간을 기준으로 흐름을 나타낸 것임.
- 이번 애니메이션은 hour을 기준으로 그 시간대에서 가장 특징적인 흐름을 파악할 수 있다.
- 
  ex)
  
  <img width="391" height="657" alt="image" src="https://github.com/user-attachments/assets/8e0bc059-bbb0-4a5d-bc5a-f75658327373" />


<img width="614" height="593" alt="image" src="https://github.com/user-attachments/assets/9b6baa82-574c-45a2-9579-94ff25ac8f4a" />

1️⃣ 아침 시간대는 택시 이동 대부분이 맨해튼 섬 내부에서 발생

2️⃣ 늦은 저녁이 되면 브루클린 지역, 특히 윌리엄스버그 쪽으로 이동하는 택시의 비중이 훨씬 커짐


### 택시 이동 양상 그래프로 시각화

(동네에 이름붙여주고, 히트맵 그려봤는데 동네별 이동량이 크게 격차가 안나서 생략할게요)

~~~python
rides_df
~~~

| From \ To | Chelsea | Queens | JFK | ... |
| --------- | ------- | ------ | --- | --- |
| Chelsea   | 1200    | 300    | 50  | ... |
| Queens    | 200     | 800    | 70  | ... |
| JFK       | 40      | 60     | 500 | ... |

~~~python
fig, ax = plt.subplots(figsize = (12,12))

for i in range(len(rides_df)):  
ax.plot(rides_df.sum(axis = 1)[i], rides_df.sum(axis = 0)[i], 'o', color = 'b')
~~~
- 산점도 캔버스 생성
- rides_df.sum(axis = 1)[i] : i 동네에서 출발해서 다른 동네로 간 총량 -> x축
- rides_df.sum(axis = 0)[i] : 다른 동네에서 i동네로 들어온 총량 -> y축

~~~python
ax.plot([0,250000],[0,250000], color = 'r', linewidth = 1)
~~~
- 유입 = 유출이 같은 지점이 되는 기준선을 생성

<img width="700" height="657" alt="image" src="https://github.com/user-attachments/assets/afe263ce-d93b-4a31-9b1c-897a3878e820" />


1️⃣ 선 위쪽에 있는 동네는 유입>유출 (들어오는 택시가 더 많다)

2️⃣ 선 아래쪽에 있는 동네는 유출>유입 (나가는 택시가 더 많다)

3️⃣ 퀸즈, 브루클린, 할렘 같은 주거 지역은 유입이 많고 / 상업 관광 성격의 지역은 유출이 더 많다 / 두개의 공항은 보통 택시들이 줄을 서서 승객을 태워서 나오는게 흔하기 때문에 유출이 더 많다


### 택시 이동 양상 그래프로 시각화 - 계절별 패턴 분석

~~~python
df['pickup_month'] = df.pickup_datetime.apply(lambda x: parser.parse(x).month)
~~~
- pickup_datetime에서 month 만 뽑아서 pickup_month 칼럼으로 저장

[6월 vs 1월 비교]

~~~python
for col in rides_df.columns[:-1]:
    rides_df[col] = rides_df.name.apply(
        lambda x: len(df[(df.pickup_neighborhood == x) &
                         (df.dropoff_neighborhood == col) &
                         (df.pickup_month == 6)])
    )
~~~
- df.pickup_neighborhood == x -> 출발 동네(x)랑 df.dropoff_neighborhood == col -> 도착 동네(y)
  의 건수 반복해서 카운트
- df.pickup_month == 6 -> 6월의 이동 건수

* 나머지 모든 과정은 위에서 한 과정 반복
* 1월도 똑같이 반복
<img width="706" height="650" alt="image" src="https://github.com/user-attachments/assets/17c724a8-0d9f-4bc9-a40b-c43be0535e87" />


-> 전혀 큰 차이 없음..



# 2️⃣ 느낀점

지도 시각화 방법을 꼭 공부해보고 싶었는데 방식을 알게 되어서 좋았다

인사이트를 뽑아낸다기보다는, 그 초석을 마련해주는 분석까지인 것 같은데 이런걸 활용해서
더 깊은 분석과 인사이트를 뽑아내는 노트북도 공부해보고 싶다.

코드가 낯선게 많아서 어려웠다!! 시계열 시각화라 그런가..








