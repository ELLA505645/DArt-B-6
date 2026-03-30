# SQL_ADVANCED 4주차 정규 과제 

📌SQL_ADVANCED 정규과제는 매주 정해진 분량의 『*혼자 공부하는 SQL*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **SQL_ADVANCED_4th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 제시된 강의를 참고하여 보완하는 것이 좋습니다.

<!-- 강의 링크는 아래와 같습니다.
https://www.youtube.com/watch?v=DMNpkj_bZIs&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=13
https://www.youtube.com/watch?v=BUHj-behLyc&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=14
https://www.youtube.com/watch?v=JrXWxku7ZIM&list=PLVsNizTWUw7GCfy5RH27cQL5MeKYnl8Pm&index=15
-->

**교재 실습 예제 파일은 07_SQL_ADVANCED_Template 레포지토리의 src 폴더에 업로드되어 있습니다. market_db 파일도 해당 폴더에 함께 포함되어 있으니 참고하시기 바랍니다.**

**👀(수행 인증샷은 필수입니다.)** 

## SQL_ADVANCED_4th_TIL

### 5장 테이블과 뷰
#### 01. 테이블 만들기
#### 02. 제약조건으로 테이블을 견고하게
#### 03. SQL 가상의 테이블: 뷰 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.24~99    | ✅         |
| 2주차 | p.102~155   | ✅         |
| 3주차 | p.158~213  | ✅         |
| 4주차 | p.216~271 | ✅         |
| 5주차 | p.274~327 | 🍽️         |
| 6주차 | p.330~369 | 🍽️         |
| 7주차 | p.372~407 | 🍽️         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 테이블 만들기 

<!-- 이번 챕터에서 제시된 실습을 흐름에 맞게 진행한 후, 실습 과정이 보일 수 있도록 인증 사진을 2장 이상 제출해 주세요. -->

<img width="1399" height="799" alt="image" src="https://github.com/user-attachments/assets/e88d0b8e-01f5-4e5f-90ef-b8f350b8c51f" />

<img width="1181" height="697" alt="image" src="https://github.com/user-attachments/assets/214c6b48-7dc4-4fb3-8ab5-f719df2820be" />


## 2. 제약조건으로 테이블을 견고하게 

**테이블을 만들 때는 테이블의 구조에 필요한 제약조건을 설정해줘야 한다**

<대표적인 제약조건>

1) PRIMARY KEY 기본 키 (학번, 아이디, 사번 등과 같은 고유한 번호를 의미하는 열에 지정)
3) FOEIGN KEY 외래 키 (기본 키와 연결되는 열에 지정)
4) UNIQUE
5) CHECK
6) DEFAULT
7) NULL

> 제약조건이 많은 테이블은 데이터의 오류가 적고 튼튼해짐



> **확인문제: 다음 보기 중에서 각 문항이 설명하는 것을 고르세요.**

보기는 아래와 같습니다.
```
CHECK / DEFAULT / PRIMAY KEY / UNIQUE / NOT NULL / FOREIGN KEY
```

```
여기에 답과 그 이유를 적어주세요!
1. 입력되는 데이터가 조건에 맞는지 검사하는 기능: CHECK
2. 값을 입력하지 않으면 자동으로 들어갈 값: DEFAULT
3. 빈 값을 입력하는 것을 허용하지 않음: NOT NULL
```


## 3. 가상의 테이블: 뷰 

<!-- 뷰에 관해 배우게 된 점을 적어주세요. -->
**뷰는 데이터베이스 개체 중의 하나로, 사용자들의 입장에서는 테이블과 거의 동일한 개체로 취급됨**

<뷰의 종류>
1) 단순 뷰 (하나의 테이블과 연관된 뷰)
2) 복합 뷰 (2개 이상의 테이블과 연관된 뷰)

> 뷰를 사용하는 이유는 1) 보안에 도움이 된다 2) 복잡한 SQL을 단순하게 만들 수 있다 등이 있다.


> **확인문제: 다음은 뷰의 특징입니다. 거리가 먼 것을 하나 고르세요.**

보기는 아래와 같습니다.
```
1️⃣ 뷰에는 테이블의 모든 열을 포함시켜야 합니다.
2️⃣ 뷰는 복잡한 SQL을 단순하게 만드는 효과가 있습니다.
3️⃣ 뷰는 보안에 도움이 됩니다.
4️⃣ 일부 사용자가 테이블에는 접근하지 못하게 하고, 뷰에만 접근하도록 설정할 수 있습니다.
```

```
1️⃣번, 뷰는 사용자가 필요한 컬럼만 골라서 만들 수 있는 가상의 테이블로서,

업무에 필요한 열만 선택적으로 포함하는 것이 일반적이다.

```


---

# 2️⃣ 실습과제

## 1. 데이터베이스 구축

아래 코드를 MySQL Workbench에 붙여넣은 후,  
**전체 드래그 → 실행 (Ctrl + Shift + Enter)** 하여 데이터베이스를 생성하세요.

```sql
CREATE DATABASE IF NOT EXISTS week4_db;
USE week4_db;
```
<img width="1153" height="608" alt="image" src="https://github.com/user-attachments/assets/fcc141cb-1a8b-4833-add5-c2c3fc7585e1" />

## 2. 실습문제

1. 다음 조건을 만족하는 `users` 테이블을 생성하시오.
```
- user_id는 INT이며 **기본키(Primary Key)**로 설정합니다.
- name은 VARCHAR(20)이며 NULL을 허용하지 않습니다.
- email은 VARCHAR(50)이며 중복을 허용하지 않습니다.
- signup_date는 DATE 타입으로 설정합니다.
- grade는 INT이며 기본값(Default)을 1로 설정합니다.
```


2. 다음 조건을 만족하는 orders 테이블을 생성하시오.
```
- order_id는 INT이며 기본키(Primary Key)로 설정합니다.
- user_id는 INT이며 NULL을 허용하지 않습니다.
- amount는 INT이며 0보다 커야 합니다.
- order_date는 DATE 타입으로 설정합니다.
```


3. 다음 조건을 만족하여 데이터를 삽입하시오.
```
- users 테이블에 3명 이상의 데이터를 직접 INSERT 하시오.
- orders 테이블에 3건 이상의 데이터를 직접 INSERT 하시오.
```


4. users와 orders 테이블을 활용하여 다음 컬럼을 보여주는 뷰 user_order_view를 생성하시오.
```
- user_id
- name
- amount
```



5. 생성한 user_order_view를 조회하시오.


## 3. 제출 방법

1. 각 문제의 실행 결과가 보이도록 화면을 캡처합니다.
2. 테이블 생성 결과, 데이터 삽입 결과, 뷰 생성 및 조회 결과가 모두 보이도록 제출합니다.

<img width="248" height="317" alt="image" src="https://github.com/user-attachments/assets/ab2152d4-77e0-4ac5-afdd-6f62132e49a5" />

<img width="241" height="233" alt="image" src="https://github.com/user-attachments/assets/412d0f37-7650-48c2-b3a8-51552533f54c" />

<img width="1166" height="339" alt="image" src="https://github.com/user-attachments/assets/035a3401-64ae-428a-a19d-892b9ea437ec" />

<img width="955" height="403" alt="image" src="https://github.com/user-attachments/assets/c2fba21e-c8e2-4a0d-a4cb-1612d77a1177" />

<img width="1170" height="549" alt="image" src="https://github.com/user-attachments/assets/0ad57bd8-cf5e-4e06-8b5b-be1fd44f598c" />

### 🎉 수고하셨습니다.






