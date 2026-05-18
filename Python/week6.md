# Python 7주차 정규 과제 

📌Python 정규과제는 매주 정해진 분량의 『*파이썬 라이브러리를 활용한 데이터 분석*』 을 읽고 학습하는 것입니다. 이번주는 아래의 **Python_7th_TIL**에 나열된 분량을 읽고 공부하시면 됩니다.

아래의 문제를 풀어보며 학습 내용을 점검하세요. 문제를 해결하는 과정에서 개념을 스스로 정리하고, 필요한 경우 참고 자료를 통해 보완하는 것이 좋습니다.

**교재 실습 예제 파일은 07_Python_Template 레포지토리의 notebooks 폴더에 업로드되어 있습니다.**

**👀(수행 인증샷은 필수입니다.)** 

## Python_7th_TIL

### 9장 그래프와 시각화 
#### 1. 맷플롯립 API 간략하게 살펴보기
#### 2. 판다스에서 시본으로 그래프 그리기 
#### 3. 다른 파이썬 시각화 도구
#### 4. 마치며
### 10장 데이터 집계와 그룹 연산
#### 1. 그룹 연산에 대한 고찰
#### 2. 데이터 집계
#### 3. apply 메서드: 일반적인 분리-적용-병합
#### 4. 그룹 변환과 래핑되지 않은 groupby
#### 5. 피벗 테이블과 교차표
#### 6. 마치며 


## Study Schedule

| 주차  | 공부 범위     | 완료 여부 |
| ----- | ------------- | --------- |
| 1주차 | p.25~82    | ✅         |
| 2주차 | p.83~129   | ✅         |
| 3주차 | p.131~179  | ✅         |
| 4주차 | p.181~246 | ✅         |
| 5주차 | p.247~309 | ✅         |
| 6주차 | p.310~379 | ✅         |
| 7주차 | p.381~465 | ✅         |


<br>

<!-- 여기까진 그대로 둬 주세요-->

---

# 1️⃣ 학습 내용 정리

## 1. 맷플롯립 API 간략하게 살펴보기

### 개념정리

맷플롯립은 주로 2D 그래프를 위한 데스크톱 패키지로 출판물 수준의 그래프를 만든다.
맷플롯립은 모든 운영체제의 다양한 GUI 백엔드를 지원하며 일반적으로 널리 사용하는 벡터와 래스터 형식으로 그래프를 저장할 수 있다.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="623" height="826" alt="image" src="https://github.com/user-attachments/assets/7f7650df-c822-4b32-a429-ba507e2893db" />
<img width="583" height="815" alt="image" src="https://github.com/user-attachments/assets/3046b5b4-a83f-41d9-a90e-c76bc7ea837c" />
<img width="535" height="917" alt="image" src="https://github.com/user-attachments/assets/a72b5b6d-5dba-4880-bec2-78468a96be15" />



## 2. 판다스에서 시본으로 그래프 그리기 

### 개념정리

*판다스를 사용하다 보면 행과 열 레이블을 가진 다양한 열 데이터를 다루게 된다.
시본: 맷플롯립 기반의 고차원 통계 그래픽 라이브러리

### 실습 인증

<img width="539" height="841" alt="image" src="https://github.com/user-attachments/assets/3e55bbe6-9b44-417d-b7be-09382044261b" />
<img width="556" height="917" alt="image" src="https://github.com/user-attachments/assets/f363ea9f-70e8-4374-8451-433b4b4f9c70" />
<img width="546" height="848" alt="image" src="https://github.com/user-attachments/assets/42ed20f6-99bb-41fe-b363-58658c9d8a02" />


<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->


## 3. 다른 파이썬 시각화 도구

### 개념정리

파이썬으로 동적 대화형 그래프를 그릴 수 있음.
예시: 웹이나 출판을 위한 정적 그래프를 생성한다면 맷플롯립과 판다스, 사본처럼 맷플롯립 기반의 라이브러리를 사용할 수 있다. 


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="553" height="874" alt="image" src="https://github.com/user-attachments/assets/67fbe602-0927-4a18-b248-ec757c84a247" />
<img width="528" height="742" alt="image" src="https://github.com/user-attachments/assets/168e0f45-b739-48fd-b7e5-46e140519bc5" />
<img width="519" height="547" alt="image" src="https://github.com/user-attachments/assets/4b5bf356-bff4-403f-80a9-adfd925c1383" />



## 4. 그룹 연산에 대한 고찰

### 개념정리

그룹 연산에 대해 분리-적용-결합의 용어가 존재함.


### 실습 인증

<img width="549" height="863" alt="image" src="https://github.com/user-attachments/assets/e165d16d-5877-4407-a5c7-b4cabb8c0c59" />
<img width="549" height="599" alt="image" src="https://github.com/user-attachments/assets/26857649-8f72-4a9b-a886-aa3a3c83c372" />


<!-- 이 부분을 지우고 실행 화면을 제출해주세요. -->


## 5. 데이터 집계

### 개념정리

데이터 집계는 배열로부터 스칼라 값을 만들어내는 모든 데이터 변환 작업을 말한다. 

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="607" height="716" alt="image" src="https://github.com/user-attachments/assets/190e1d98-59b5-4206-8094-74ec354d1bf8" />

<img width="609" height="450" alt="image" src="https://github.com/user-attachments/assets/ef0f200c-bc8c-457e-8506-24e527c0d8da" />


## 6. apply 메서드: 일반적인 분리-적용-병합 

### 개념정리

apply 메서드는 객체를 여러 조각으로 나누고, 전달된 함수를 각 조각에 일괄적으로 적용한 후 이를 다시 합친다.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="603" height="823" alt="image" src="https://github.com/user-attachments/assets/d8ad6936-84b0-4e80-b75c-5ce49e5f4ed1" />

<img width="602" height="553" alt="image" src="https://github.com/user-attachments/assets/48097a67-6103-4b81-939e-4f561a896c40" />


## 7. 그룹 변환과 래핑되지 않은 groupby

### 개념정리

transform 메서드는 apply와 유사하지만 더 많은 제약 사항을 갖는다.

1) 그룹 모양대로 브로드캐스팅할 스칼라 값을 생성할 수 있다.
2) 입력 그룹과 동일한 모양의 객체를 생설할 수 있다.
3) 입력을 변경하면 안된다.


### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="596" height="664" alt="image" src="https://github.com/user-attachments/assets/ec09e7a7-8331-4bf8-8468-d6c6999bb8d0" />
<img width="596" height="542" alt="image" src="https://github.com/user-attachments/assets/61c9c534-f4af-4470-9e92-07489b0215ef" />



## 8. 피벗 테이블과 교차표 

### 개념정리

피벗 테이블은 스프레드시트 프로그램과 다른 데이터 분석 소프트웨어에서 흔히 볼 수 있는 데이터 요약 도구다. 
데이터를 하나 이상의 키로 수집해서 어떤 키는 행에, 어떤 키는 열에 나열해서 데이터를 정리한다.

### 실습 인증

<!-- 예제 실습을 진행한 후, 실행 화면을 2-3장 캡쳐하여 제출해주세요. -->

<img width="602" height="660" alt="image" src="https://github.com/user-attachments/assets/4721f5b6-5ac8-4869-a2d7-eeb5a028428f" />

<img width="583" height="717" alt="image" src="https://github.com/user-attachments/assets/932374ea-8865-40f5-afcc-e86ff6c234d4" />



---

이번 주차는 마지막 주차로, 별도의 실습 과제가 없습니다.

부족한 커리큘럼이었음에도 불구하고 한 학기 동안 과제를 성실하게 수행해주셔서 감사합니다.

파이썬 과제 제작을 마무리하는 시점에 이 글을 작성하고 있는데, 저 또한 파이썬에 능통하거나 익숙하지 않았지만, 과제를 제작하기 위해 책을 찾아보고, 템플릿을 제작하고 검수하면서 많은 배움이 있었던 것 같습니다. 

여러분들께서도 모든 과제를 마친 지금 시점에 파이썬이 아직 낯설게 느껴질 수도 있을 것 같습니다.

하지만 자주 접하고, 직접 코드를 작성해보고, 데이터를 어떻게 다루면 좋을지 고민해보는 시간을 가지다 보면 점점 익숙해질 것이라 생각합니다.

정말 고생 많으셨고, 여러분들께서 앞으로 파이썬을 활용하는 데에 있어 이 과제가 한 줌이라도 보탬이 된다면 정말 감사하겠습니다.

여러분의 앞날을 응원합니다!! 😄



### 🎉 수고하셨습니다.






