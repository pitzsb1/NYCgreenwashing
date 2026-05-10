# 프로젝트 중간보고 | Building Energy Efficiency Analysis

## 1. 프로젝트 개요

본 프로젝트는 London EPC (Energy Performance Certificate) 데이터를 활용하여 건물의 물리적/구조적 특성을 기반으로 **에너지 효율 점수(CURRENT_ENERGY_EFFICIENCY)**를 예측하고,
실제 값과 모델 예측 간의 차이를 통해 **비정상적인 건물(anomaly)**을 탐지하는 것을 목표로 함.

## 2. 중간 결과

### 데이터 수집 및 구조 이해

* London EPC 데이터셋 (domestic certificates) 로드
  
* 약 **4,459,813 rows × 93 columns** 규모의 대용량 데이터 확인
  
* 데이터가 borough 단위 폴더 구조로 분산되어 있어 병합 처리 수행

```text
all-domestic-certificates/
 ├── domestic-E09xxxx/
      └── certificates.csv
```

### 데이터 탐색 (EDA)

#### 데이터 타입 구성 확인:

* Numeric: 약 35개
    
* Categorical: 약 58개

#### 주요 변수 확인:

* Target: `CURRENT_ENERGY_EFFICIENCY`
    
* 구조적 변수: 면적, 방 개수, 건축 연도 등
    
* 에너지 관련 변수: 조명, 창호, 난방 등

#### 분포 분석

* Target은 대체로 **정규분포 형태 (약 50~80 구간 집중)**

* 일부 극단값 존재 (낮은 효율 건물)

#### 데이터 이슈 발견

* 음수 값 존재 (예: 에너지 소비량)
  
* 이상치 존재 (비정상적으로 큰 면적, 방 개수)
  
* 결측값 다수 존재
  
* 일부 컬럼은 의미 없는 ID 정보 포함

### 데이터 정제 (핵심 작업)

#### 샘플링 전략 적용

* 전체 데이터 대신 약 **100,000 샘플**로 실험 진행
  
* 이유: 계산 효율 및 빠른 iteration

#### Feature Selection (중요)

초기 93개 컬럼 → 의미 있는 feature만 선택

```python
use_cols = [
    "TOTAL_FLOOR_AREA",
    "NUMBER_HABITABLE_ROOMS",
    "NUMBER_HEATED_ROOMS",
    "LOW_ENERGY_LIGHTING",
    "MULTI_GLAZE_PROPORTION",
    "PROPERTY_TYPE",
    "BUILT_FORM",
    "TENURE",
    "CONSTRUCTION_AGE_BAND"
]
```

#### 불필요 / 문제 feature 제거

* ID 계열: `LMK_KEY`, `UPRN` 등
  
* Leakage 변수:

  * CO2_EMISSIONS_CURRENT
  * ENERGY_CONSUMPTION_CURRENT
  
* 결측 과다 컬럼 제거

### 카테고리 처리

초기 문제:

> One-hot encoding 이후 feature 수가 600개 이상으로 폭발

해결 방법:

* 문자열 정규화 (예: tenure 소문자 처리)
  
* 상위 빈도 category만 유지 (`top-k`)
  
* 나머지는 `"other"`로 묶음

결과:

```text
Encoded X shape: (99176, 33)
```

→ 차원 축소 성공 (600+ → 33)

## 3. 중간 결과 요약

| 항목             | 결과                  |
| -------------- | ------------------- |
| 데이터 규모         | 약 445만 행            |
| 샘플 데이터         | 약 10만 행             |
| 초기 feature 수   | 93                  |
| 정제 후 feature 수 | 33                  |
| 데이터 상태         | 모델 학습 가능 수준으로 정리 완료 |

→ 프로젝트 목표에 맞게 정상적으로 정제 및 구조화

## 4. 추후 일정 (Week 2 ~ Final)

### 4.1 모델 구축

* Baseline 모델 구축 (RandomForest / Gradient Boosting)
  
* 성능 평가 (MAE, R²)
  
* Feature Importance 분석

### 4.2 성능 개선

* 모델 개선 (Hyperparameter tuning)
  
* 추가 feature 실험
  
* 이상치 탐지 로직 설계

### 4.3 EDA

* Anomaly Detection 구현
  
* 결과 시각화 (예측 vs 실제 비교)
  
* 사례 분석 (비정상 건물 해석)

### 4.4 인사이트 도출

* 전체 데이터 적용
  
* 예측 결과 기반 지역 단위 분석 (Borough-level analysis) 수행

* 구별 평균 실제 효율 vs 예측 효율 비교

→ 특정 구에서 과대평가(과장) 여부 탐지

* Residual (실제 - 예측) 기반 지도 시각화

→ 에너지 효율이 비정상적으로 높은/낮은 지역 식별

* Top anomaly 건물 사례 분석

→ 개별 건물 단위에서 구조적 특징 및 문제 해석

## 5. 현재 진행 상황 평가

✓ 데이터 구조 이해 완료

✓ 핵심 feature 선정 완료

✓ 전처리 파이프라인 구축 완료
