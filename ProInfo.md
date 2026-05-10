# 머신러닝 프로젝트 제안서

## Title | 에너지 효율 등급 신뢰성 검증 및 숨겨진 비효율 탐지 (London EPC 기반)

2026년 4월 14일(Updated), 최은혜 김준서

## Subject

건물 에너지 효율 등급(Energy Performance Certificate)이 실제 건물 특성과 일관되게 평가되고 있는지 분석하고, 머신러닝을 활용하여 평가 시스템의 신뢰성과 잠재적 불일치(숨겨진 비효율)를 탐지

## DataSet | UK EPC (Energy Performance Certificate, London Subset)

**Resource**: UK Government Open Data

|    | CURRENT_ENERGY_RATING   |   CURRENT_ENERGY_EFFICIENCY |   TOTAL_FLOOR_AREA |   CO2_EMISSIONS_CURRENT | PROPERTY_TYPE   | BUILT_FORM    | CONSTRUCTION_AGE_BAND        | TENURE          |
|---:|:------------------------|----------------------------:|-------------------:|------------------------:|:----------------|:--------------|:-----------------------------|:----------------|
|  0 | D                       |                          59 |               93   |                     4.6 | House           | Mid-Terrace   | England and Wales: 1950-1966 | Owner-occupied  |
|  1 | C                       |                          70 |               66.5 |                     2.9 | Flat            | Semi-Detached | England and Wales: 1967-1975 | rental (social) |
|  2 | D                       |                          59 |              125   |                     5.8 | House           | Semi-Detached | England and Wales: 1930-1949 | owner-occupied  |
|  3 | D                       |                          57 |              110   |                     5.4 | House           | Mid-Terrace   | England and Wales: 1900-1929 | owner-occupied  |
|  4 | C                       |                          73 |               43   |                     1.4 | Flat            | Mid-Terrace   | England and Wales: 1950-1966 | rental (social) |

### Data Description

* **건물 식별**

LMK_KEY (인증서 ID), UPRN (건물 고유 ID)

* **물리적 스펙**

Property Type, Built Form, Total Floor Area, Construction Age Band

* **에너지/환경**

Current Energy Rating (A–G), Current Energy Efficiency, CO2 Emissions (Current), Main Heating Description, Tenure

* **건물 특성(독립 변수 x)**: CURRENT_ENERGY_EFFICIENCY, TOTAL_FLOOR_AREA, CO2_EMISSIONS_CURRENT, PROPERTY_TYPE, BUILT_FORM, CONSTRUCTION_AGE_BAND, TENURE, MAINHEAT_DESCRIPTION

* **실질적 에너지 효율 등급(종속 변수 y)**: CURRENT_ENERGY_RATING(A, B, C, D, E, F, G)

## 배경 및 필요성 | “평가 지표는 항상 신뢰할 수 있는가?”

* 건물 에너지 효율 등급(EPC)은 거래, 임대, 정책 결정에 중요한 기준으로 활용됨
* 그러나 동일 건물에 대해 시간이나 평가 조건에 따라 **서로 다른 등급이 부여되는 사례** 존재
* 단순히 등급을 활용하는 것을 넘어, **그 등급 자체의 신뢰성을 검증하는 분석**이 필요함

즉, “좋은 등급”이 반드시 “실제 효율이 좋은 건물”을 의미하는가?

## 프로젝트 목표

* **에너지 효율 등급 예측 모델 구축**

건물의 물리적/에너지 특성을 기반으로 정상적인 에너지 등급을 예측하는 모델 구축

* **평가 신뢰성 검증**

동일 건물(UPRN 기준)에 대해 시간에 따른 등급 변화 분석을 통해 평가 시스템의 일관성 검증

* **숨겨진 비효율 탐지 (Hidden Inefficiency)**

모델 예측 결과와 실제 등급 간의 불일치를 기반으로 과대평가 혹은 과소평가된 건물 식별

## 주요 기능 및 특징

* **Model-as-a-Validator 접근 방식**

머신러닝 모델을 단순 예측 도구가 아닌 **데이터 및 평가 시스템 검증 도구로 활용**

* **Consistency-Based Analysis**

동일 건물의 등급 변화 추적을 통해 평가 시스템의 안정성과 신뢰성 분석

* **Residual / Misclassification 기반 탐지**

모델 예측과 실제 등급 간 차이를 활용하여 숨겨진 비효율 및 이상 사례 탐지

## 일정 및 계획

* **1주차 | 데이터 정제 및 구조 이해**

London EPC 데이터 필터링(E09), 주요 변수 선택 및 결측치 처리

* **2주차 | 탐색적 데이터 분석(EDA)**

건물 유형 및 연식에 따른 등급 분포 분석 동일 건물의 등급 변화 패턴 탐색

* **3주차 | 머신러닝 모델링**

CatBoost, XGBoost 기반 등급 예측 모델 구축 SHAP 분석을 통한 주요 변수 영향도 파악

* **4주차 | 신뢰성 분석 및 보고서 작성**

UPRN 기준 일관성 분석 및 hidden inefficiency 사례 도출

평가 시스템의 구조적 문제 해석

## 기대효과

* **평가 시스템 신뢰성 검증**

공공 에너지 평가 데이터의 일관성과 신뢰성을 데이터 기반으로 검증

* **숨겨진 비효율 건물 식별**

표면적으로는 효율적이나 실제 특성과 불일치하는 건물 탐지

* **데이터 기반 정책 개선 기여**

에너지 평가 기준 및 정책 설계에 대한 개선 방향 제시

## 결론

본 프로젝트는 단순한 예측 모델을 넘어 에너지 효율 등급이라는 지표 자체를 분석 대상으로 삼음. 머신러닝을 활용하여 평가 결과와 실제 건물 특성 간의 불일치를 탐지함으로써, 데이터의 신뢰성과 평가 시스템의 구조적 한계를 드러내는 데 목적을 두고 있음.

## 기존 프로젝트의 문제점 개선

* **단순 예측 중심 접근의 한계 극복**

기존 프로젝트가 단순한 예측 정확도에 집중했다면, 본 프로젝트는 예측 결과를 활용하여 **데이터 및 평가 시스템 자체를 검증**

* **지표 중심 분석의 한계 보완**

단일 지표(에너지 점수)에 의존하지 않고, 다양한 건물 특성을 반영한 모델 기반 비교 분석 수행

## 기존 프로젝트의 성능 개선 (기술적 차이)

* **분류 기반 비선형 모델 적용**

에너지 등급(A–G) 예측을 위해 CatBoost 및 XGBoost 활용

범주형 변수 처리 및 비선형 관계 학습 강화

* **오차 기반 해석 확장**

단순 오차를 제거하는 것이 아니라, 이를 분석 대상(비효율/불일치)으로 활용하는 구조로 확장

## 실제 환경 개선 및 반영 제안

* **에너지 평가 품질 모니터링 시스템**

평가 결과의 일관성을 지속적으로 분석하여 데이터 품질 및 평가 신뢰성 확보

* **효율 개선 우선순위 도출**

예측 대비 과소평가된 건물 또는 개선 여지가 큰 건물을 식별하여 정책 및 리모델링 우선순위 제안
