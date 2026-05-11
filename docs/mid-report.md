# 프로젝트 중간보고 | Building Energy Efficiency Analysis - London

## 1. 프로젝트 개요

본 프로젝트는 London EPC (Energy Performance Certificate) 데이터를 활용하여 건물의 물리적/구조적 특성과 에너지 효율 간의 관계를 분석하고, 동일 건물에 대한 반복 평가 결과의 일관성 검증.

기존의 단순 예측 중심 접근을 넘어, EPC 평가 결과 자체의 신뢰성을 데이터 기반으로 분석하고, 비정상적인 평가 사례(anomaly)를 탐지하는 것을 주요 목표로 함.

## 2. 중간 결과

### 2.1 데이터 수집 및 구조 이해

* London EPC 데이터셋 (domestic certificates) 로드

* 데이터가 borough 단위 폴더 구조로 분산되어 있어 병합 수행

```text
all-domestic-certificates/
 ├── domestic-E09xxxx/
      └── certificates.csv
```

### 2.2 데이터 정제 및 분석 대상 구성

초기 데이터에서 분석 목적에 맞게 핵심 변수만 선택:

```python
cols = [
    "UPRN",
    "CURRENT_ENERGY_RATING",
    "CURRENT_ENERGY_EFFICIENCY",
    "TOTAL_FLOOR_AREA",
    "CO2_EMISSIONS_CURRENT",
    "PROPERTY_TYPE",
    "BUILT_FORM",
    "CONSTRUCTION_AGE_BAND",
    "TENURE",
    "MAINHEAT_DESCRIPTION",
    "LODGEMENT_DATE"
]
```

#### 전처리 과정

* UPRN 결측 제거 → 동일 건물 식별 가능 데이터만 유지

* 동일 건물 반복 평가 분석을 위해 **UPRN 기준 2회 이상 등장 데이터만 추출**

```text
전체 데이터: 8228 rows
중복 평가 데이터: 4786 rows
실제 건물 수: 약 2200개
```

### 2.3 동일 건물 기준 평가 일관성 분석

각 건물(UPRN)별로 에너지 등급 변화량을 계산:

```python
rating_gap = max(rating) - min(rating)
```

#### 결과

```text
rating_gap 분포:

0 → 1008
1 → 856
2 → 256
3 → 64
4 → 9
5 → 2
```

> 약 **15%의 건물에서 2단계 이상의 등급 변화 발생**

### 등급 변화 분포

<img width="580" height="448" alt="image" src="https://github.com/user-attachments/assets/5013ac68-a0a2-46ed-a61e-77e701803a79" />

* 대부분은 안정적 (0~1)
* 그러나 상당수에서 비정상적 변화 존재

### 2.4 이상 건물 분석 (rating_gap ≥ 2)

이상 건물: **331개**

## 2.5 효율 점수 변동 분석

각 건물의 efficiency 변화량 계산:

```python
eff_gap = max(efficiency) - min(efficiency)
```

#### 결과

```text
평균: 30.9
중앙값: 29
최소: 13
최대: 78
```

### Rating vs Efficiency 변화

<img width="563" height="453" alt="image" src="https://github.com/user-attachments/assets/0f392e6a-8729-48fa-be1c-1f69f8bb959c" />

### Insight

* efficiency 점수 변화가 클수록 등급 변화도 증가
* 등급 변화는 랜덤이 아니라 **efficiency 점수 변화에 의해 결정됨**

## 2.6 물리적 특성 안정성 검증

면적 변화 분석:

```text
median: 2.5
75%: ~7
```

> 대부분의 건물에서 면적 변화는 매우 작음 → 동일 건물로 간주 가능

## Final Conclusion

### 동일 건물에서도 등급 변화 존재

→ 약 **15%에서 2단계 이상 변화**

### 건물 자체는 거의 변하지 않음

→ 면적 안정적

### efficiency 점수는 크게 변동

→ 평균 약 **30 변화**

* 동일 건물에 대해 물리적 특성은 안정적인 반면,
* 에너지 효율 점수는 큰 폭으로 변동하며,
* 이러한 점수 변동이 등급 변화의 주요 원인으로 작용함.

## 3. 현재 프로젝트 방향 변화

기존: 효율 점수 예측 → anomaly 탐지

현재: **평가 시스템 자체의 일관성 검증 + anomaly 탐지**

Model = Validator 역할로 재정의

## 4. 추후 일정

### 4.1 모델 기반 검증

* CatBoost 모델 구축

* 정상적인 등급 예측 수행

* 실제 vs 예측 비교

### 4.2 Hidden Inefficiency 탐지

* 모델과 실제 등급 차이 분석

* 이상 건물 자동 탐지

### 4.3 원인 분석

* 난방 방식 변화
  
* 카테고리 변수 불일치
  
* 평가 입력 변수 변동

### 4.4 결과 해석

* EPC 평가 시스템의 구조적 한계 분석

* 정책적 개선 방향 제시

## 5. 현재 진행 상황 평가

✓ 데이터 구조 이해 완료

✓ 동일 건물 기반 분석 구조 구축 완료

✓ 평가 일관성 문제 발견

✓ 등급 변화 원인 규명 (efficiency 변동)

✓ 핵심 인사이트 도출 완료
