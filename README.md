# Criminal_Classification
- 범죄 유형 분류 (5월 월간 dacon이었는데 페이지가 사라져서 더 이상 열람 불가)

## EDA
- 변수 간 상관관계 분석
  -  수치형-수치형 상관관계 (spearman)
    -  ![image](https://github.com/Cheol-Kyu/Criminal_Classification/assets/87174143/18f4b360-97be-4373-b5f5-cb640becaa84)
    - 강설량-강수량-적설량을 제외하고는 그다지 관계가 없음
    - TARGET과도 비상관성을 보임 (TARGET은 비순서-범주형 데이터이기에 이 결과는 유의해야 함)

  -  범주형-범주형 상관관계 (cramer's V: 비순서형 범주형 변수이기 때문에 사용)
    -  ![image](https://github.com/Cheol-Kyu/Criminal_Classification/assets/87174143/73ebc0cf-b16f-4711-85de-e35bc639b84c)
    - 위 그림은 상관계수가 0.1 이상 혹은 -0.1 이하에 True를 반환하는 함수를 적용 (상관관계만 입력하면 seaborn에서 히트맵이 그려지지 않음)
    - 날씨 관련 데이터는 서로 약간 혹은 그 이상 상관관계를 보이지만, TARGET과는 비상관성을 보임
    - 소관경찰서, 소관지역, 범죄발생지는 서로 상관관계를 보이지만, TARGET과도 상관성을 보임

- 랜덤포레스트 활용 변수 선택
  - 여기서도 소관경찰서, 소관지역, 범죄발생지만 특성 중요도 0.1을 상회

## Modeling
- 변수는 TARGET과 상관성을 보이는 소관경찰서, 소관지역, 범죄발생지만 사용
  - 모든 변수를 다 넣고 랜덤포레스트를 사용했을 때, f1 score는 0.5104
- Catboost를 이용해 분류 모델 구성 (f1 score: 0.5270)
- Optuna를 통해 하이퍼파라미터 최적화 (f1 score: 0.5305)

## 함의
- 날씨는 범죄 종류 예측에 그다지 도움이 되지 않는 변수
  - 개인적인 추측으로는, 범죄 발생여부나 범죄 강도에 영향을 끼치는 변수가 아닐까 생각함
- 소관경찰서, 소관지역, 범죄발생지만 TARGET과 상관성을 보이는 것은 아마 특정 지역, 특정 장소에 따라 특정 종류의 범죄가 일어나는게 아닐까 생각함 (즉, 지역적인 요소가 중요)
