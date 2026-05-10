# 머신러닝 프로젝트 제안서

### Title | ENERGY STAR Score와 예측 배출량 잔차 기반 NYC 건물 탄소 배출 괴리 분석
2026년 4월 14일(Updated), 최은혜 김준서

### Subject

에너지 효율 점수(ENERGY STAR Score)와 실제 탄소 배출량 간의 괴리 분석을 통한 데이터 신뢰도 검증 및 이상 탐지

### DataSet | NYC Building Energy and Water Data Disclosure (LL84) & PLUTO (건물 물리적 특성)

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Property Id</th>
      <th>NYC Borough, Block and Lot (BBL)</th>
      <th>Largest Property Use Type - Gross Floor Area (ft²)</th>
      <th>Year Built</th>
      <th>ENERGY STAR Score</th>
      <th>Site EUI (kBtu/ft²)</th>
      <th>Natural Gas Use (therms)</th>
      <th>Electricity Use - Grid Purchase (kWh)</th>
      <th>Total GHG Emissions (Metric Tons CO2e)</th>
      <th>Primary Property Type - Self Selected_Ambulatory Surgical Center</th>
      <th>...</th>
      <th>Primary Property Type - Self Selected_Vocational School</th>
      <th>Primary Property Type - Self Selected_Wastewater Treatment Plant</th>
      <th>Primary Property Type - Self Selected_Wholesale Club/Supercenter</th>
      <th>Primary Property Type - Self Selected_Worship Facility</th>
      <th>Primary Property Type - Self Selected_Zoo</th>
      <th>Borough_BROOKLYN</th>
      <th>Borough_MANHATTAN</th>
      <th>Borough_QUEENS</th>
      <th>Borough_STATEN IS</th>
      <th>Borough_Unknown</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21205224</td>
      <td>4006520042</td>
      <td>25000</td>
      <td>2010</td>
      <td>71</td>
      <td>66.7</td>
      <td>10625.27867</td>
      <td>176982.1</td>
      <td>107.5</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2665352</td>
      <td>1-01206-0001</td>
      <td>260780</td>
      <td>1970</td>
      <td>100</td>
      <td>19.2</td>
      <td>8273.198321</td>
      <td>1229531.2</td>
      <td>398.6</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2665400</td>
      <td>1-01832-0043</td>
      <td>324378</td>
      <td>1943</td>
      <td>84</td>
      <td>66.9</td>
      <td>179358.0772</td>
      <td>1283733.2</td>
      <td>1323</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2665405</td>
      <td>1-00142-0025</td>
      <td>1039841</td>
      <td>1975</td>
      <td>3</td>
      <td>113.8</td>
      <td>992843.9804</td>
      <td>7797471.9</td>
      <td>7582.1</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2669439</td>
      <td>4-09649-0053</td>
      <td>85020</td>
      <td>1953</td>
      <td>37</td>
      <td>102.1</td>
      <td>73241.99945</td>
      <td>377020.1</td>
      <td>518.3</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 92 columns</p>
</div>

**Resource**: NYC Open Data

**Calumn...**

- **건물 식별**

Property Id, BBL(Borough, Block, Lot)

- **물리적 스펙**

Primary Property Type, Gross Floor Area (ft²), Year Built, Number of Buildings

- **에너지/환경**

ENERGY STAR Score, Site EUI (kBtu/ft²), Natural Gas/Electricity Use, Total GHG Emissions

### 배경 및 필요성 | '무늬만 친환경' 건물 식별의 시급성

- 뉴욕시 탄소 배출의 약 70%가 건물에서 발생하며, 강력한 규제(LL97)가 시행됨에 따라 건물의 에너지 효율 점수 부풀리기나 실제 배출량과의 불일치 사례가 보고됨.

- 에너지 점수라는 '상대적 지표'에 의존하는 대신, 물리적 스펙 기반의 '절대적 배출 기댓값'을 예측하여 데이터의 진위 여부를 가릴 수 있는 객관적 진단 모델이 필요함.

### 프로젝트 목표

- **고정밀 탄소 배출 예측 모델 구축**

건물의 면적, 용도, 연식, 에너지 사용 구성을 입력값으로 하여 해당 건물의 정상적인 '총 온실가스 배출량(GHG Emissions)'을 예측.

- **그린워싱 탐지 알고리즘 개발**

에너지 스타 점수는 높지만, 예측치 대비 실제 배출량이 비정상적으로 높은 '괴리 구간' 건물을 통계적(Z-Score 등)으로 식별하는 시스템 구축.

### 주요 기능 및 특징

- **Physical-Operational Hybrid Learning**

PLUTO 데이터의 물리적 특성(외벽, 층수 등)과 LL84의 운영 데이터를 결합하여 건축학적 관점의 배출량 베이스라인 수립.

- **Multivariate Anomaly Detection**

단순 배출량 비교를 넘어, 에너지 스타 점수와 배출량 간의 상관계수를 기반으로 한 잔차(Residual) 분석 기법 적용.

- **Borough-Specific Weighting**

맨해튼의 고층 상업용 빌딩과 브루클린의 저층 주거용 건물의 에너지 소비 패턴 차이를 반영한 지역별 가중치 적용.

### 일정 및 계획

- **1주차 | 데이터 통합 및 정제**

LL84와 PLUTO 데이터 매칭(BBL 기준), 단위 변환(kBtu to Metric Tons) 및 결측치 처리.

- **2주차 | 탐색적 데이터 분석(EDA)**

에너지 점수 상위권 건물들의 실제 배출량 분포 시각화, 그린워싱 의심 샘플 추출 및 가설 검정.

- **3주차 | 머신러닝 모델링**

XGBoost, LightGBM을 활용한 배출량 예측 모델 구축 및 SHAP 분석을 통한 주요 변수 영향도 파악.

- **4주차 | 이상 탐지 및 보고서 마무리**

예측 잔차 기반 그린워싱 지수(Dissonance Index) 산출 및 시애틀 모델 대비 NYC 특수성(고밀도) 반영 결과 정리.

### 기대효과

- **정책적 가이드라인 제공**

단순 고효율 점수 획득보다 실질적인 탄소 감축 활동(에너지 믹스 개선 등)이 필요한 건물을 타겟팅하여 컨설팅 우선순위 도출.

- **지속 가능한 도시 관리**

'그린워싱'을 방지함으로써 뉴욕시의 2050 탄소 중립 로드맵이 실질적인 수치에 기반하여 실행될 수 있도록 지원.

### 결론

본 프로젝트는 에너지 효율 점수라는 지표 뒤에 숨겨진 실제 탄소 배출의 실태를 머신러닝으로 파헤치는 데 목적이 있음. 이는 단순히 배출량을 맞추는 '예측'의 단계를 넘어, 데이터 간의 모순을 찾아내는 '검증'의 도구로서 기능을 수행함. 이를 통해 뉴욕시 건물들의 진정한 친환경 성능을 판별하고, 실질적인 탄소 저감을 이끌어내는 데이터 기반의 정책 의사결정 모델을 제시하고자 함.

### 기존 프로젝트의 문제점 개선

- **지표 간 독립성 결여 해결**

기존 프로젝트는 상관관계가 높은 변수들을 단순히 예측에 활용하지만, 본 프로젝트는 'ENERGY STAR Score'와 '실제 배출량' 사이의 불일치에 집중ㅇ함. 지표가 좋음에도 배출량이 높은 '모순'을 해결하지 못하는 기존 분석의 한계를 개선함.

- **물리 데이터 결합의 부재 보완**

시애틀 프로젝트가 제공된 운영 데이터에 의존했다면, 본 프로젝트는 NYC PLUTO(필지 수준의 물리적 특성) 데이터를 결합. 건물의 구조적 특성(외벽 재질, 용적률 등)을 반영하여 "왜 이 건물은 효율이 낮은가?"에 대한 근거를 더 구체적으로 제시함.


### 기존 프로젝트의 성능 개선 (기술적 차이)

- **앙상블 기반의 비선형 관계 학습**

시애틀 프로젝트에서 사용된 기법을 고도화하여, Year Built나 Gross Floor Area 등 에너지 소비와 비선형적 관계를 가진 변수들을 처리하기 위해 XGBoost 및 LightGBM의 하이퍼파라미터 최적화 수행.

- **이상치 처리 프로세스 정립**

단순히 이상치를 제거하는 것이 아니라, 이를 '그린워싱 후보군'으로 레이블링하여 별도의 분류(Classification) 모델로 확장 가능한 파이프라인을 구축.

### 실제 환경 개선 및 반영 제안

- **데이터 무결성 검증 시스템**

에너지 점수를 자체 보고하는 과정에서 발생할 수 있는 오류나 의도적 수치 조작을 머신러닝 모델이 상시 모니터링하여, 공공 데이터의 투명성을 획기적으로 높일 수 있습니다.

- **리모델링 우선순위 최적화**

예산이 한정된 상황에서, '그린워싱 지수'가 높거나 효율 개선 잠재력이 큰 노후 건물을 데이터 기반으로 선정하여 행정 자원의 최적 배분을 제안합니다.
