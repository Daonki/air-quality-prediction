# 🌫️ Air Pollution (PM10) Prediction Project

## 📌 프로젝트 개요
본 프로젝트는 **서울 지역의 미세먼지(PM10) 농도**를 예측하기 위해  
미세먼지 데이터와 기상 데이터를 결합한 **시계열 기반 머신러닝 모델**을 구현한 프로젝트입니다.

특히 **전일 동일 시간의 미세먼지 농도(lag feature)** 와  
시간 정보(`hour`, `day`, `month`)를 활용하여  
→ **1시간 뒤(t+1) PM10 농도**를 예측합니다.

---

## 🎯 문제 정의
### 목표
- 현재 시점의 대기오염 및 기상 정보를 바탕으로  
  **1시간 뒤 PM10 농도 예측**

### 활용 가능성
- 실시간 미세먼지 예보
- 조기 경보 시스템
- 환경 정책 의사결정 지원

---

## 🗂️ 데이터 소개
### 1️⃣ 데이터 구성

| 구분 | 파일명 | 설명 |
|---|---|---|
| 학습 데이터 | air_2024.csv | 2024년 미세먼지 데이터 |
| 학습 데이터 | weather_2024.csv | 2024년 기상 데이터 |
| 테스트 데이터 | air_2025.csv | 2025년 미세먼지 데이터 |
| 테스트 데이터 | weather_2025.csv | 2025년 기상 데이터 |

---

### 2️⃣ 주요 변수
#### 미세먼지 / 대기오염
- PM10
- PM25
- CO
- NO2
- O3

#### 기상 변수
- 기온
- 풍향
- 증기압
- 시정
- 지중온도

---

## ⚙️ 데이터 전처리
### ⏱️ 시간 정합성 처리 (핵심)
- 미세먼지 데이터: **1시 ~ 24시**
- 날씨 데이터: **0시 ~ 23시**

➡️ 미세먼지 측정 시간을 **1시간 감소(-1)** 시켜  
날씨 데이터와 동일한 시간 기준으로 병합

---

### 🔄 전처리 파이프라인
1. `time` 변수 생성 및 `datetime` 변환
2. 시간 기준(inner join) 데이터 병합
3. 불필요 변수 제거
4. 시계열 특성을 고려한 **선형 보간(linear interpolation)** 으로 결측치 처리
5. 파생 변수 생성
   - `month`, `day`, `hour`
   - `PM10_lag1` : 전일 동일 시간 PM10
   - `PM10_1` : 1시간 뒤 PM10 (Target)

---

## 📥 Feature / Target 구성
### 입력 변수 (X)
- 대기오염: `CO`, `O3`, `NO2`, `PM10`, `PM25`
- 기상: `기온`, `풍향`, `증기압`, `시정`, `30cm 지중온도`
- 시간 정보: `hour`, `day`, `month`
- 시계열 변수: `PM10_lag1`

### 예측 변수 (y)
- `PM10_1` (1시간 뒤 PM10 농도)

---

## 🤖 머신러닝 모델링

### 사용 모델
- Linear Regression
- Random Forest Regressor
- Gradient Boosting Regressor
- XGBoost Regressor

### 학습 전략
- **2024년 데이터 → Train**
- **2025년 데이터 → Test**
- Lag feature 생성을 고려하여 **초기 24시간 데이터 제외**

---

## 📊 모델 성능 비교
| 모델 | MSE | R² |
|---|---|---|
| Linear Regression | 51.43 | 0.93161 |
| Random Forest | 58.83 | 0.92178 |
| **Gradient Boosting** | ⭐ 50.81 | ⭐ 0.93244 |
| XGBoost | 76.53 | 0.89824 |

➡️ **Gradient Boosting 모델이 가장 우수한 성능을 기록**

---

## 🔍 Feature Importance 인사이트
- **현재 PM10 값**이 가장 중요한 변수
- `hour` 변수 → 미세먼지의 일중 패턴 반영
- 날씨 변수들은 확산/정체를 보조적으로 설명

---

## 📁 결과물 구조
```text
├── train_x.csv
├── train_y.csv
├── test_x.csv
├── test_y.csv
├── linear_regression_model.pkl
├── random_forest_model.pkl
├── gradient_boost_model.pkl
├── xgboost_model.pkl
