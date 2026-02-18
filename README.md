📌 프로젝트 소개
과거 7일간의 대기오염 물질 농도 및 기상 데이터를 기반으로 다음 날의 미세먼지(PM10)와 초미세먼지(PM2.5) 농도를 예측하는 시계열 예측(Time-Series Forecasting) 프로젝트입니다.
단순한 예측 모델 구현을 넘어, 통계적 검정(쌍체 표본 T-검정)을 통한 객관적인 모델 비교, 하이퍼파라미터 최적화, 그리고 시계열 데이터의 특성을 고려한 교차 검증(TimeSeriesSplit) 등 탄탄한 머신러닝/딥러닝 파이프라인 구축에 초점을 맞추었습니다.

🛠️ 기술 스택
Language: Python

Deep Learning / Machine Learning: TensorFlow (Keras), Scikit-learn, keras-tcn

Data Analysis & Optimization: Pandas, NumPy, SciPy, Optuna

📊 사용 데이터 및 피처 엔지니어링
입력 데이터 (과거 7일치 시퀀스 데이터 적용):

대기오염 지표: PM10, PM2.5, 오존, 이산화질소, 일산화탄소, 아황산가스

기상 지표: 평균기온, 평균 풍속, 상대습도, 현지기압

파생 및 계절 변수: 이동평균선(MA7, MA30), 전일 농도(lag), 계절 원핫인코딩(Spring, Summer, Autumn, Winter)

Feature Importance 파악: RandomForestRegressor의 Gini Importance를 활용하여 예측에 큰 영향을 미치는 주요 피처를 선별하고 모델의 설명력을 확보했습니다.

🚀 핵심 파이프라인 및 수행 과정
1. 다양한 알고리즘 비교 및 통계적 검증 (Model Selection)
베이스라인 모델인 SVR과 딥러닝 모델들(LSTM, 1D-CNN, TCN)의 성능을 10-Fold 교차 검증으로 1차 비교했습니다.

단순한 R2 Score 수치 비교에 그치지 않고, SciPy를 활용한 **쌍체 표본 T-검정(Paired T-test)**을 수행하여 각 모델 간의 성능 차이가 통계적으로 유의미한지(P-value 검토) 수학적으로 검증했습니다.

2. Optuna를 활용한 하이퍼파라미터 최적화
LSTM 모델의 성능을 극대화하기 위해 Optuna 프레임워크를 도입했습니다.

은닉층 노드 수(16~64), 활성화 함수(relu vs tanh), 옵티마이저(adam vs rmsprop), 배치 사이즈(8, 16, 32) 등의 탐색 공간을 설정하고, 검증 손실(Validation Loss)을 최소화하는 최적의 파라미터를 자동 탐색했습니다.

3. 시계열 특성을 고려한 평가 방법론 적용 (TimeSeriesSplit)
미래의 데이터가 과거를 학습하는 데이터 누수(Data Leakage)를 방지하기 위해 TimeSeriesSplit을 활용한 중첩 교차 검증을 수행했습니다.

전체 데이터의 80%를 훈련/검증용으로 사용하여 SVR과 LSTM을 최종 경쟁시켰으며, 두 모델의 평균 성능을 비교한 결과 LSTM을 최종 모델로 선정하였습니다.

4. 최종 모델 학습 및 평가
선정된 LSTM 모델을 80%의 데이터로 재학습(Early Stopping 적용) 시킨 후, 학습 과정에 전혀 노출되지 않은 나머지 20%의 Unseen Data(최종 테스트 셋)로 평가를 진행했습니다.

📈 최종 예측 결과 (Test 데이터 기준)
[PM10 (미세먼지) 예측 성능]

R² Score: 0.4480

RMSE: 0.09

MAE: 0.07

[PM2.5 (초미세먼지) 예측 성능]

R² Score: 0.4664

RMSE: 0.12

MAE: 0.10
