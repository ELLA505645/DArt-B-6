👶📈 태아의 건강을 예측해보자
-
태아에 대한 다양한 정보를 통해 태아의 건강상태를 분류해보자!

# 1️⃣ 분석 흐름 따라잡기

## 1. 데이터 불러오기 및 확인하기

~~~python
from sklearn.model_selection import train_test_split, GridSearchCV, cross_val_score
~~~
- 모델 학습/검증 관련 도구들
- 테스트와 실제 모델들의 성능을 높여준다

~~~python
from sklearn.preprocessing import StandardScaler
~~~
- 값들의 스케일(범위)를 맞춰준다

~~~python
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
~~~
- 모델이 얼마나 잘 맞추는지 평가하는 도구

~~~python
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier, VotingClassifier
~~~
- 앙상블 계열 모델링 도구들을 불러옴
- RandomForest : 여러 트리를 랜덤하게 만들어서 그중에서 결정
- GradientBoosting : 이전 모델의 실수를 다음 모델이 보완하는 방식
- VotingClassifier : 서로 다른 모델들을 같이 돌려서 다수결 혹은 확률 평균으로 결론 내리는 방식

### 컬럼 확인

| Column Name                                            | 설명                                      |
| ------------------------------------------------------ | --------------------------------------- |
| baseline value                                         | 태아 심박수의 기본선(baseline) 값 (bpm 단위 평균 심박수) |
| accelerations                                          | 심박수 가속 횟수 (단위 시간당 상승 빈도)                |
| fetal_movement                                         | 태아 움직임 횟수                               |
| uterine_contractions                                   | 자궁 수축 횟수                                |
| light_decelerations                                    | 가벼운 심박수 감소 횟수                           |
| severe_decelerations                                   | 심각한 심박수 감소 횟수                           |
| prolongued_decelerations                               | 장시간 지속되는 심박수 감소 횟수                      |
| abnormal_short_term_variability                        | 단기 심박 변이 중 비정상 비율                       |
| mean_value_of_short_term_variability                   | 단기 심박 변이 평균값                            |
| percentage_of_time_with_abnormal_long_term_variability | 장기 심박 변이 중 비정상 상태가 차지하는 시간 비율           |
| mean_value_of_long_term_variability                    | 장기 심박 변이 평균값                            |
| histogram_width                                        | 심박수 히스토그램의 폭                            |
| histogram_min                                          | 심박수 히스토그램의 최소값                          |
| histogram_max                                          | 심박수 히스토그램의 최대값                          |
| histogram_number_of_peaks                              | 히스토그램의 피크(봉우리) 개수                       |
| histogram_number_of_zeroes                             | 히스토그램에서 0 빈도 구간 수                       |
| histogram_mode                                         | 히스토그램의 최빈값                              |
| histogram_mean                                         | 히스토그램 평균값                               |
| histogram_median                                       | 히스토그램 중앙값                               |
| histogram_variance                                     | 히스토그램 분산                                |
| histogram_tendency                                     | 히스토그램의 경향성 (상승/하락/안정 등)                 |
| fetal_health                                           | 타겟 변수 (1: 정상, 2: 의심, 3: 병리적 상태)         |

총 22개 컬럼

### 상관관계 확인

~~~python
print(df.isnull().sum())
print(df.describe())
~~~
- 결측치 개수 확인
- 수치형 변수들의 요약 통계량 확인

~~~python
plt.figure(figsize=(12, 10))
sns.heatmap(df.corr(), annot=True, cmap='coolwarm', fmt='.2f')
plt.title('Correlation Matrix')
plt.show()
~~~
- 수치형 변수들끼리의 heatmap 확인

<img width="690" height="648" alt="image" src="https://github.com/user-attachments/assets/f5788962-f693-47d8-8963-1e7148ea5812" />

~~~python
X = df.drop(columns=['fetal_health'])
y = df['fetal_health']
~~~
- fetal_health를 제외한 모든 변수 X로, fetal_health는 y로 분리

~~~python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
~~~
- train 데이터, test 데이터 분리
- 20%를 테스트 데이터로 사용
- random_state는 같은 방식으로 고정시켜주기

-> 나중에 사용할건데 미리 나눈듯하다..


~~~python
sns.countplot(x='fetal_health', data=df)
~~~
- 정상/의심/병리 상태가 각각 몇개씩 있는지 확인
<img width="721" height="477" alt="image" src="https://github.com/user-attachments/assets/f907e1ee-e369-4b59-91e5-1f86a0a2a25a" />

> 클래스 불균형을 확인하기 위함
1번 클래스가 90% 이상이면 모델이 그냥 1만 찍어도 정확도가 높아질 수 있음

~~~python
df.hist(bins=15, figsize=(20, 15), edgecolor='black')
~~~
- 모든 수치형 변수의 히스토그램을 그려줌
- 각 변수의 분포(치우침, 이상치 여부)를 확인
<img width="705" height="543" alt="image" src="https://github.com/user-attachments/assets/8656db33-567a-4077-8f96-642598aa18f6" />


## 2. 모델링
### 모델 선정
~~~python
models = {
    'Random Forest': RandomForestClassifier(random_state=42),
    'Support Vector Machine': SVC(random_state=42),
    'K-Nearest Neighbors': KNeighborsClassifier(),
    'Decision Tree': DecisionTreeClassifier(random_state=42),
    'Gradient Boosting': GradientBoostingClassifier(random_state=42)
}
~~~
- 랜덤포레스트 분류 모델, SVM 분류 모델, KNN 모델, 결정트리 모델, 그레디언트 부스팅 모델 생성
- 5개 모델을 포함한 딕셔너리를 만듦. 모델들을 이름으로 꺼내서 반복문으로 돌릴 수 있다 

~~~python
param_grids = {
~~~
- 모델별로 어떤 파라미터 조합을 시험할지를 담는 딕셔너리 실행

~~~python
'Random Forest': {
'n_estimators': [100, 200],
'max_depth': [10, 20, None]    },
~~~
- 트리 개수를 100개 혹은 200개로 시험 -> 보통 많을수록 성능은 좋아지지만 느려짐
- 트리의 최대 깊이를 10,20,제한없음(None)으로 시험 -> 깊을수록 과적합 위험이 커질 수 있음

~~~python
'Support Vector Machine': {
'C': [0.1, 1, 10],
'kernel': ['linear', 'rbf']    },
~~~
- C: 오차를 얼마나 허용할지 조절 -> C가 커질수록 오차를 덜 허용해서 경계가 빡세질 수 있음
- kernel: 결정 경계를 어떤 형태로 만들지 선택 linear는 직선/평면, rbf는 비선형으로 더 유연하게 경계를 만들 수 있음

~~~python
'K-Nearest Neighbors': {
'n_neighbors': [3, 5, 7],
'weights': ['uniform', 'distance']    },
~~~
- n_neighbors: 몇명의 이웃을 보고 투표할지 결정 -> 작으면 민감하고 크면 안정적임(but 둔해질 수 있다)
- uniform은 모든 이웃을 똑같이 1표로, distance는 가까운 이웃일수록 더 큰 영향력을 준다

~~~python
'Decision Tree': {
'max_depth': [10, 20, None],
'min_samples_split': [2, 10, 20]    },
~~~
- 트리 깊이 설정 (*None일 때 과적합 주의)
- min_samples_split: 필요한 최소 샘플 개수 -> 값이 커지면 트리가 덜 복잡해지고 과적합을 줄일 수 있음

~~~python
'Gradient Boosting': {
'n_estimators': [100, 200],
'learning_rate': [0.01, 0.1, 0.2]    }
~~~
- n_estimators: 트리 모델을 몇개 쌓을지 설정 -> 숫자가 클수록 성능이 높아지지만 속도가 느려지고 과적합 위험이 커짐
- learning_rate: 한 번 업데이트할 때 얼마나 조금씩 배울지 설정 -> 작을수록 천천히 학습해서 안정적이지만 더 많은 트리가 필요할 수 있음

~~~python
for name, model in models.items():
~~~
- model 딕셔너리를 돌면서 하나씩 꺼내서 반복

### 모델 학습
~~~python
grid_search = GridSearchCV(model, param_grids[name], cv=5, scoring='accuracy', n_jobs=-1)
grid_search.fit(X_train_scaled, y_train)
~~~
- cv=5로 해서 5겹 교차검증으로 평균 성능 확인
- scoring='accuracy'은 정확도 기준으로 최적으로 고름

~~~python
grid_search.fit(X_train_scaled, y_train)
~~~
- 학습 데이터로 여러 파라미터 조합 교차검증하면서 성능 비교

-> 최적의 조합을 찾는다

### 모델 성능 평가

~~~python
print(classification_report(y_test, y_pred))
~~~
- precision/recall/f1-score를 클래스별로 출력
- precision: 위험하다고 예측한 것 중에 실제 위험한게 얼만큼인지
- recall: 실제로 위험한 사람중에 위험하다고 예측한게 얼마만큼인지
- f1-score:  precision/recall을 함께 고려한 점수


~~~python
print(confusion_matrix(y_test, y_pred))
~~~
어떤 클래스가 어떤 클래스로 많이 틀렸는지 표로 출력

<img width="243" height="128" alt="image" src="https://github.com/user-attachments/assets/5ed9c4d5-5040-4530-b8bd-699539e72703" />

    

<img width="706" height="523" alt="image" src="https://github.com/user-attachments/assets/45379ef8-dffd-4e9c-872a-c2b4d3ea8e96" />


### 앙상블
> 여러 모델을 동시에 활용해서 최종 예측을 하겠다

~~~python
voting_clf = VotingClassifier(estimators=[
    ('rf', best_models['Random Forest']),
    ('svc', best_models['Support Vector Machine']),
    ('knn', best_models['K-Nearest Neighbors']),
    ('dt', best_models['Decision Tree']),
    ('gb', best_models['Gradient Boosting'])
], voting='hard')
~~~
- voting='hard': 각기 다른 모델의 평가 결과를 다수결로 결정하는 방식
(가장 의견이 많은 결론으로 띠라감)

~~~python
print('Ensemble Model Classification Report:\n')
print(classification_report(y_test, y_pred))
~~~
- 앙상블의 리포트 출력
- 앙상블의 precision/recall/f1 출


### Feature Importance 확인 (랜덤 포레스트 기준)

~~~python
feature_importances = rf_model.feature_importances_
features = X.columns
sns.barplot(x=feature_importances, y=features)
~~~
- 랜덤포레스트가 계산한 변수 중요도를 가져와서 barplot으로 나타내기
<img width="684" height="334" alt="image" src="https://github.com/user-attachments/assets/e1c8f318-7f44-433d-87f9-7335bdff0cf9" />

### 결론

~~~python
print(f'{name} Accuracy: {accuracies[name]:.2f}')
~~~
- 각 모델의 정확도를 소수 둘째 자리까지 출력

~~~python
ensemble_accuracy = accuracy_score(y_test, voting_clf.predict(X_test_scaled))
~~~
- 앙상블 모델의 테스트 정확도를 계산해서 저장

~~~python
sns.barplot(x=list(accuracies.keys()) + ['Ensemble'], y=list(accuracies.values()) + [ensemble_accuracy])
~~~
- 각 모델의 정확도와 앙상블 정확도를 막대그래프로 비교

<img width="699" height="483" alt="image" src="https://github.com/user-attachments/assets/4bfbdf0a-7ad6-44bb-a8fa-8fa99308df70" />

1️⃣ 랜덤 포레스트와 그래디언트 부스팅 모델은 높은 정확도를 보임
2️⃣ 변수 중요도 분석을 통해서 태아 건강 예측에 영향을 미치는 주요 요인을 확인함



# 2️⃣ 느낀점
모델링을 통해서 예측을 하고자하는 경우에는 바로 모델링으로 들어가서 결론을 낼 수 있구나

방학 캐글 필사 스터디 끝!! 수고하셨습니다



