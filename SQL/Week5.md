# SQL_BASIC 6주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_6th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**6주차 과제는 강의 내용을 정리하는 것과 함께, 프로그래머스에서 제공하는 SQL 문제를 직접 풀어보는 실습도 병행합니다.** 강의에서는 **배운 내용을 정리하고 주요 쿼리 예제를 정리**하며, 프로그래머스 문제는 **직접 풀어본 뒤 풀이 과정과 결과, 배운 점을 함께 기록**해주세요. 완성된 과제는 Github에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**(수행 인증샷은 필수입니다.)** 

## SQL_BASIC_6th

### 섹션 6. 다량의 자료를 연결 : JOIN 

### 5-1. Intro

### 5-2. JOIN 이해하기

### 5-3. 다양한 JOIN 방법

### 5-4. JOIN 쿼리 작성하기 

### 5-5. JOIN을 처음 공부할 때 헷갈렸던 부분

### 5-6. JOIN 연습문제 1~2번

### 5-6. JOIN 연습문제 3~5번

### 5-7. 정리



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | ✅         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<!-- 여기까진 그대로 둬 주세요-->

<br>

---

# 1️⃣ 개념정리

## 5-2. JOIN 이해하기

~~~
✅ 학습 목표 :
* JOIN에 대한 정의와 필요성에 대해 설명할 수 있다.
~~~

데이터는 대부분 다량의 자료임
JOIN이라는 것은 연결하는 것 ( 자료를 합치는 과정)

~~~
JOIN
: 다른 데이터 테이블을 연결하는 것
~~~

#### For example
포켓몬과 트레이너
포켓몬은 다양한 곳에서 나타남
* 포켓몬 데이터와 트레이너 데이터를 연결할 수 있는 공통 값이 없음!

~~~
트레이너가 포획한 포켓몬 데이터 = 트레이너와 포켓몬을 연결할 수 있음
~~~
예시:
<img width="906" height="443" alt="image" src="https://github.com/user-attachments/assets/0ef81692-dece-44bc-a7d2-ccd156329757" />

여러 테이블 JOIN이 가능하다!

<특징>
1) 공통적으로 존재하는 칼럼 key가 있다면 가능
2) 보통 id 값을 key로 많이 사용한다
3) 특정범위로도 join이 가능하다
4) 정규화하여 중복을 최소화시킬 수 있다



## 5-3. 다양한 JOIN 방법

~~~
✅ 학습 목표 :
* JOIN 방법들의 종류를 설명할 수 있다. 
* 각 JOIN 방법들의 차이점에 대해서 설명할 수 있다. 
~~~

<JOIN의 종류>
1) INNER : 공통 요소만 연결
2) LEFT / RIGHT : 왼쪽 / 오른쪽 테이블 기준으로 연결
3) FULL : 양쪽 기준으로 연
4) CROSS : 두 테이블 각각의 요소를 곱하기
<img width="908" height="429" alt="image" src="https://github.com/user-attachments/assets/de2198e7-36a9-4025-8f4f-74c249a8e6af" />
<img width="911" height="309" alt="image" src="https://github.com/user-attachments/assets/76b02505-1c22-46ba-bdc9-9e8f6f1ab2c5" />



## 5-4. JOIN 쿼리 작성하기 

~~~
✅ 학습 목표 :
* JOIN을 사용한 문법에 대해 이해하여 적용할 수 있다.
* JOIN 을 활용한 쿼리를 작성할 수 있다. 
~~~

1) 테이블 확인
2) 기준 테이블 정의 ( 가장 많이 참고할 기준 )
3) JOIN KEY 찾기
4) 결과 예상하기

#### SQL JOIN 문법
FROM 하단에 JOIN할 TABLE을 작성하고 ON 뒤에 공통된 컬럼 (key)를 작성
~~~SQL
SELECT
 A. col1,
 A. col2,
 B. col11,
 B. col12
FROM table1 AS A
LEFT JOIN table2 AS B
ON A.key = B.key
~~~

*JOIN을 사용할 때 주의해야할 점
1) 목적에 따라 JOIN을 선택하기
2) 기준이 되는 Table을 왼쪽에 두기 ( 기준에는 기준값, 우측에 데이터 계속 추가 )
3) JOIN 개수에 한계는 없으나, 너무 많이 하고 있지 않은지 확
4) 컬럼 선택은 무엇을 하고자 하는가? 에 따라 다름
5) NULL이 자주 발생할 수 있음


## 5-6. JOIN 연습문제 1~5번 

~~~
✅ 학습 목표 :
* 연습문제(3문제 이상) 푼 것들 정리하기
~~~

#### 1번 문제
트레이너가 보유한 포켓몬들은 얼마나 있는지 알 수 있는 쿼리를 작성해주세요

문제 풀이 흐름
1) trainer_pokemon에서 status가 active, training인 경우만 필터링
2) 필터링한 결과를 pokemon Table과 JOIN
3) 2)의 결과에서 pokemon_name, COUNT(pokemon_id) AS pokemon_cnt
   
#### 3번 문제
트레이너의 고향과 포켓몬을 포획한 위치를 비교하여, 자신의 고향에서 포켓몬을 포획한 트레이너의 수를 계산해주세요

-사용할 테이블: trainer, trainer_pokemon
-Join KEY: trainer_id = trainer_pokemon, trainer_id
-trainer_pokemon을 LEFT로 하기
-trainer를 왼쪽에 두고 쿼리를 작성
-status 상관없이 구하기


#### 5번 문제
인천 출신 트레이너들은 1세대, 2세대 포켓몬을 각각 얼마나 보유하고 있나요?

~~~SQL
FROM(
SELECT
 id,
 trainer_id,
 pokemon_id,
 status
FROM basic.tp
WHERE
 status IN ("Active", "Training")
) AS tp
LEFT JOIN basic.trainer AS t
ON ttp.trainer_id = t.id
LEFT JOIN pokemon AS p
ON tp.pokemon_id = p.id
WHERE
 t.hometown = "Incheon"
~~~



<br>

<br>

---

# 2️⃣ 확인문제 & 문제 인증

## 프로그래머스 문제 

https://school.programmers.co.kr/learn/courses/30/lessons/164673

> 조건에 부합하는 중고거래 댓글 조회하기 (JOIN)

https://school.programmers.co.kr/learn/courses/30/lessons/144854

> 조건에 맞는 도서와 저자 리스트 출력하기 (JOIN)

<img width="678" height="220" alt="image" src="https://github.com/user-attachments/assets/17a6a4c1-f96e-4f27-8a25-67b670431154" />
ㅠㅠ......

<img width="683" height="193" alt="image" src="https://github.com/user-attachments/assets/41c2124b-ae5d-46d7-b194-92e08ce00c56" />




---

# 3️⃣ 참고자료

JOIN 에 대해서 그림으로 쉽게 이해할 수 있는 자료들도 있어서 첨부합니다. 아래의 블로그도 학습할 때 같이 참고해주세요.

1. https://data-marketing-bk.tistory.com/entry/SQL-JOIN-%ED%95%9C-%EB%B0%A9%EC%97%90-%EC%A0%95%EB%A6%AC-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%BD%94%EB%93%9C%EA%B9%8C%EC%A7%80-%EC%9D%B4%EA%B2%83%EB%A7%8C-%EB%B3%B4%EC%9E%90



2. https://velog.io/@wijoonwu/JOIN

<br>

### 🎉 수고하셨습니다.
