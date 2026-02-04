Spotify에서 인기있는 노래는 무엇일까? 예측해보기!


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










