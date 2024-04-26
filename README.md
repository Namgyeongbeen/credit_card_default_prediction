# credit_card_default_prediction
개인 데이터분석 프로젝트. 신용카드 연체고객 예측

- **개요** : 신용카드 상환을 연체할 것으로 예상되는 고객을 예측하는 모델 개발 및 상환 연체와 주요 변수들 간의 영향도 파악.
- **성과** : EDA/비즈니스 인사이트 도출 역량 향상 및 목적에 맞는 가설 설정과 변수간 상관관계 파악하는 역량, 분류 문제에 적절한 ML기법을 사용하는 역량 향상.
- **데이터** : 대만 카드업체의 1년간의 고객 데이터 from kaggle([링크](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset))
- **`Language / Tool` : `Python` `Pandas` `Numpy` `Google colab` `Matplotlib` `Seaborn`**
- **과정 및 결과 :** 1️⃣EDA, 전처리  ⇒  2️⃣모델링, 성능평가  ⇒  3️⃣Hyperparameter tuning  ⇒  4️⃣재 모델링, 성능평가, 활용방안 제안
    - **주요 전처리 및 EDA :** 연체 여부와 다른 컬럼간의 상관관계를 찾는 것과 연체 여부에 따른 고객들의 특성을 파악하는 것을 중심으로 진행.
        - **전처리** : EDA 결과로부터 문제 해결에 필요 없는 컬럼은 버리거나 모델링에 사용하기 좋은 형태로 바꾸는 등의 feature engineering, target 데이터의 불균형을 해소하기 위한 SMOTE.
    - **모델링과 그 결과 및 해석, 활용방안**
        - **모델링** : XGBoost 사용. 데이터가 많지 않고 categorical한 데이터도 많지 않아, lightgbm이나 catboost 대신 XGBoost 사용. 과적합 방지 기능도 있으며 hyperparameter tuning으로 성능을 올리기도 좋다.
        - **성능 평가** : ROC AUC score 사용. 이진분류 문제이고 연체자가 비 연체자에 비해 적은, 불균형한 데이터이기 때문이다.
        - **Hyperparameter tuning** : Optuna 사용. Hyperparameter의 범위만 지정하면 빠르게 최적의 parameter를 찾기 때문에 GridsearchCV보다 효율적이고 빠르다.
        - **해석** : 해당 모델은 상환 연체자를 분류해내는 데에 좋은 성능을 보인다. 또한,  연체 여부와 관련이 높은 feature는 AGE, LIMIT_BAL(신용한도), bill_09(가장 최근의 상환상태)… 인 것으로 보여진다. 예를 들어 LIMIT_BAL의 경우 신용 한도가 낮을수록 연체율이 뚜렷하게 높은 경향을 보이고, 가장 낮은 신용한도 고객들의 연체율은 50%에 가깝다.
        - **활용 방안** : 물론 다른 feature들도 모니터 해야 하겠지만 이 feature들을 위주로 세심하게 관리한다면 1️⃣고객들의 상환 연체를 미연에 감지할 수 있을 것이고 궁극적으로는 2️⃣연체 관련 대응책을 마련하거나 대출 전략을 재조정 하는 등의 효과적인 신용 관리 전략을 수립하거나, 3️⃣연체 예상 고객 목록을 만들어 집중관리할 수도 있을 것이다.
        
        ![Untitled](https://github.com/Namgyeongbeen/credit_card_default_prediction/assets/152850843/a68433ed-2bb3-48ad-93cb-9ebbeb41c974)
        EDA내용 중 하나. 신용한도 별 연체율. 한도가 높아질수록 연체율이 낮아지는 것을 볼 수 있다.
 


        ![Untitled (1)](https://github.com/Namgyeongbeen/credit_card_default_prediction/assets/152850843/3fae1cda-b553-4514-b619-f6a2ef102ad7)
        모델의 성능을 측정하는 ROC AUC 곡선. ROC AUC score는 0.92가 나왔다. 불균형 개선 전의 score는 0.78이었다.
 


        ![Untitled (2)](https://github.com/Namgyeongbeen/credit_card_default_prediction/assets/152850843/20ca0767-bbd1-427b-b146-5d40d84c249d)        
        Feature importance. 연체 여부와 연관성이 높은 parameter 탐색.


        
- **Lesson Learned**
    - EDA는 단변량 분석에서 그쳤고 feature engineering도 그다지 만족스럽지 못했는데, 도메인 지식이 풍부했다면
    EDA나 feature engineering에 있어 좀 더 효율적이고 더 나은 인사이트를 얻었을 것 같다.
    - label이 불균형한 데이터는 균형을 맞춰주는 것이 모델의 성능 차이에 큰 영향을 미치는 것을 확인했고, 불균형을 개선해 모델의 성능을 향상시켰다.
    현업에선 이런 경우 데이터의 불균형을 반드시 개선해주어야 원하는 성능의 모델을 얻을 수 있을 것이다.
    데이터 분석가의 업무 중 가장 중요한 것은 모델링이 아닌 Data preprocessing임을 알 수 있었다.
    - 모델 성능을 개선하는 다른 방법인 Hyperparameter tuning 기법 중 GridsearchCV와 Optuna를
