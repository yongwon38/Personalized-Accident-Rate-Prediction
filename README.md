# **자동차보험 사고율 예측에 기반한 고객 맞춤 특약 추천 마케팅 제안**

### **제 2회 데이터 기반 리스크 관리 경진대회 대상 수상**

- 구성 : 팀 프로젝트 (4인)
- 사용 언어 및 도구 : `Python`(`Jupyter Notebook`), `R`

  
- 실험 설계 및 검증
    - 문자열 범주형 자료 Encoding
    - 일부 특약에 대한 가입 여부, 운전 경력 등에 대해 이진 파생변수 생성
- 모델 설계
    - 설명변수가 모두 범주형 변수로 이뤄져 있어 기존의 통계 모형으로는 높은 설명력을 보장할 수 없음.
    - 사고율이 정규분포를 따르지 않으며, 현업의 다양한 데이터에 대한 가변성을 확보하기 위한 목적에는 머신러닝 모델이 더욱 적합할 것으로 판단.
    - 총 3단계의 모델링으로 진행.
      - 사고율 1을 기준으로 사고율 1 이상의 고위험 고객 및 사고율 1 미만의 저위험 고객으로 나누어 이진분류 모델링 진행
        - 사고율이 높은 구간에 대해 떨어지는 예측력을 보완하기 위함
        - 평가 지표로는 Macro F1 Score와 AUC를 종합적으로 고려
        - 로지스틱 회귀분석으로 고위험 및 저위험 고객 분류 모델 진행
          - 타 모델과 성능 차이가 크지 않고, 높은 설명력을 보장하기 때문.
      - 사고율 예측을 위한 머신러닝 모델링 진행 및 앙상블(Ensemble) 진행
        - 분류된 고위험 및 저위험 고객에 대해 개별적인 사고율 예측 모델링 진행
        - 평가지표 : MSE (Mean Squared Error)
        - 개별 모델의 분산이 높아 측정 성능이 감소하는 문제 발생
        - Decision Tree, Gradient Boosting, Random Forest 등 3개의 머신러닝 모델의 결과를 종합(평균)하여 예측 모델 구성 (Voting)
      - 머신러닝 모델링 결과를 바탕으로 연관규칙 분석 및 T-test 실시
        - 특약 가입 여부와 사고율간의 연관성이 파악되어 특약 가입 고객군의 특성을 파악하기 위해 연관규칙분석 실시.
        - 향상도(Lift) 1.5 이상을 기준으로 특약 가입과 연관성이 높은 변수 선정.
        - 선정된 특성을 지닌 고객군 대상으로, 특약 미가입자를 가입으로 변환 전후의 기대 사고율을 T-test로 그 차이의 유의성을 검정.

- 결과
    - 모델 성능 및 해석
        - 이진분류 모델
            - Macro F1 score : 0.5232, AUC : 0.66 (Cut-off point : 0.1393)
            - 피보험자 연령대가 높고, 직전 3년간 사고건수가 많을수록 고위험군에 속할 확률 증가.
            - 가입경력이 길고, 할인 특약에 가입할수록 저위험군에 속할 확률 증가.
        - 머신러닝 모델
            - 최종 MSE : 0.61 (저위험군 : 0.60 , 고위험군 : 0.73)
            - 고위험군 : 가입경력, 피보험자연령대, 직전3년 사고건수가 높으면 사고율도 높게 나타남, 할인 특약 가입자는 낮은 사고율을 보임.
            - 저위험군 : 국산차 가입자가 낮은 사고율을 보임
            - 피보험자연령대, 가입경력, 차종, 차량경과년수, 할인특약 가입여부가 공통적으로 사고율 예측에 중요하게 작용.
            - 저위험군에 비해, 고위험군 고객에서 직전3년간 사고건수가 상대적으로 사고율 예측에 중요하게 작용.
        - 연관규칙 분석 및 T-test
            - 마일리지 할인특약 가입 특성 : 피보험자 연령 50대, 배기량 2000cc 초과, 차량 경과년수 10년 초과 고객
            - 블랙박스특약 가입 특성 : 피보험자 연령대 5-60대, 차량 경과년수 10년 이하 고객
            - 마일리지 할인특약과 블랙박스 특약의 가입 특성을 가진 미가입 고객 대상으로 가입 후 예상 시고율이 유의미하게 감소 (유의수준 0.1)
            - 이같은 분석 결과를 신규 계약 인사자가 신속히 파악 가능하도록 대시보드 구성
            - Tableau를 활용하여 고객 별 예상 사고율과 사고 위험요인을 제시하는 동적 대시보드 구성
- 의의 및 기대효과
    - 사고율 감소를 유도하는 할인 특약을 가입 의사가 높을 것으로 예상되는 고객군에 추천하는 고객 맞춤 특약 추천 마케팅 실시 가능
    - 신규 고객 대상 언더라이팅 고도화
    - 사고율 예측에 활용되는 변수의 다양화로 마케팅 아이디어 도출 및 예측 성능 개선 기대
    - AI 기술 등으로 수집한 영상 데이터 등을 언더라이팅에 활용하며 3세대 보험으로의 전환 기대
