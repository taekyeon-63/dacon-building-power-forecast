# 🏢🔌 DACON Building Power Forecast – README 템플릿

# 프로젝트 개요
대회/과제: [DACON | 건물 전력 사용량 예측]

목표 변수: 전력소비량(kWh)

평가지표: RMSE

기간: [2025-08-15 ~ 2025-08-22]

# 주제선정 이유

<img width="1162" height="650" alt="1" src="https://github.com/user-attachments/assets/9ab2c0ed-18ac-493d-9262-a94886dd74cd" />

# 데이터 전처리
<img width="1162" height="652" alt="2" src="https://github.com/user-attachments/assets/56e350a9-5263-4ccf-ba24-963962577229" />

데이터 배치: DACON 데이터셋을 data/raw/에 저장 (train.csv, test.csv ...)

EDA: notebooks/01_eda.ipynb에서 데이터 확인 및 기본 이상치/결측 파악

피처 엔지니어링: notebooks/02_feature_engineering.ipynb 또는 src/features.py 실행

학습/검증: notebooks/03_model_train.ipynb 또는 src/modeling.py에서 CV 학습

추론/제출: notebooks/04_inference_submit.ipynb 또는 src/inference.py 실행 → submission.csv 생성


<img width="1163" height="654" alt="3" src="https://github.com/user-attachments/assets/409bb8ee-678b-4ae9-8284-2371d6bf2f1f" />


기본 컬럼: [건물번호, 날짜/시간, 기상(선택), 설비(선택) ...]

결측 처리: [중위수/선형보간/전날 동시간 대치 등]

이상치 처리: [IQR/윈저라이징/클리핑]

스케일링: [미적용/표준화/로그변환 등]

시점 파생: hour, dayofweek, weekend, month, is_holiday(공휴일), is_workinghour 등

지연/이동통계 (본 프로젝트 핵심):

lag_1, lag_24, lag_168

roll_3/6/24_mean, roll_3/6/24_std

상호작용(선택): 주말×기온, 업무시간×월 등

외생변수(선택): 기온, 습도, 풍속, 강수량, 체감온도, 휴일/학사/근무 캘린더, 설비 가동 플래그 등

# 모델링
<img width="1168" height="654" alt="4" src="https://github.com/user-attachments/assets/c12f72e6-8881-40e0-9f46-d40756beeaaa" />

베이스라인: LightGBM/XGBoost/CatBoost Regressor

튜닝: RandomizedSearch or Optuna (early stopping, monotone/feature constraints 필요시 적용)

최종 선택: [CatBoost | LightGBM | 앙상블] → 이유: [성능/안정성/추론속도 등]

# 결론 및 한계점
<img width="1168" height="658" alt="5" src="https://github.com/user-attachments/assets/5573317c-97a4-442c-8c6f-7c62caa1cbde" />

외생 변수 결핍: 날씨·캘린더·설비 정보 부재로 피크 예측 취약 → 외부 데이터 연계/상호작용 피처 추가

검증 설계 미흡 위험: 단일 스플릿/리크 가능성 → TimeSeriesSplit·rolling-origin 다중 검증, 홀드아웃 운영

건물 이질성 미반영: 글로벌 단일 모델의 평균화 → 건물ID/용도 카테고리화, 세그먼트별/하이브리드 모델
