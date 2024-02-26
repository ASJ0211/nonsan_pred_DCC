프로젝트 팀 농스트라다무스

## 머신러닝 및 딥러닝을 활용한 기상 데이터 기반 농작물 가격 예측 서비스 제안
- 2023 데이터 청년 캠퍼스 팀 프로젝트
- 기간: ‘23년 07월 ~ ‘23년 08월(1개월)
- 2023 한국정책착회 빅데이터 분석 분야 프로젝트 평가 한국정책학회장상(우수상) 수상작

- Tools: python , R , hivesql
- 주요 내용
  -물리한계검사와 기후범위검사 기준에 따라 월별 이상치 결측치로 처리 <br>
  -관측 지점/월별 선형/스플라인/지수평활/칼만필터 등 다양한 대체 방법 사용 <br>
  -계절성을 고려하기 위해 sin-cos 변수 및 월별 온도 변수 생성 <br>
  -주요 기상변수(온도, 압력, 강수량, 풍속)의 7일 평균 및 14일 평균이 가격과 상관이 높기 때문에 누적된 기상을 반영하는 변수 생성 <br>
  -각 기상변수에 지연 효과 반영하는 1시차~7시차, 14시차, 21시차, 28시차 변수 생성 <br>
  -지역별 생산량 가중치 적용 및 음수값과 왜곡 분포 보정하는 log변환 <br>
- 활용 데이터
  - 기상청 종관 기상 관측 데이터(AWS) (2016.01~2021.12)
  - 기상청 방재 기상 관측 데이터(ASOS) (2016.01~2021.12)
  - 통계청 농작물 생산 조사 데이터 (2016.01~2021.12)
  - 농식품 빅데이터 거래소 도매시장 품목별 거래 현황 데이터 (2016.01~2021.12)
- 주요결과
  -기존 연구에서 다루어지지 않은 변수로 30일 이상의 중장기 일별 가격 예측 시도 및 품종별 차별화된 가격 예측 모델 전략 제시
  -시계열 데이터의 특성을 살린 총 100개 파생변수 생성으로 초기모델 대비 오차 감소율 94.1% 최종 모델 구축
  -22개 품목별 모델 선정 및 양파 품목 21년 기준 예측 오차값 RMSE 55 달성 
<br>


## 1️⃣ 개요
### 1.1 데이터 전처리
1) 데이터 수집 2) 결측치/이상치 파악 3) 이상치 대체 4) 변수간 관계 파악 5) 파생변수 생성 
### 1.2 모델링
1. 목표 2. 품목별 모델링 3. 앙상블 4. 결과
<br>

## 2️⃣ 주제 정의
기후 변화에 따른 농작물 가격 상승은 전반적인 물가 상승을 발생시키는 애그플레이션 현상을 유발하고 
이러한 변화는 소득이 한정적인 65세 이상 가구에게 치명적으로 작용한다.
한국은 빠르게 고령화되고 있기 때문에 이를 해결할 대책이 필요하다.
-지난달 7월 집중호우 발생으로 애호박 도매가격이 전일대비 63.3%상승하는 등 농산물 가격 급등으로 애그플레이션(Agriculture) 확산
-OECD 2019년 기준 한국은 고령자 상대적 빈곤율이 40% 초과하는 유일한 국가
-‘제4차 저출산 제4차 저출산·고령사회 기본계획(2021~2025)'에 따르면 4차 산업혁명 기술 기반 노인 복지 서비스 확대 예정
<br>

## 3️⃣ 과정 

❶문제정의 ❷데이터 수집 및 가공 ❸데이터 분석 ❹모델 개발 ❺가격 예측 ❻서비스 제안 및 기대효과.
![image](https://github.com/ASJ0211/nonsan_pred_DCC/assets/118821779/29bd90bf-05dd-40a9-a754-428e22f23a75)


### 👤역할
기획,데이터 수집, 데이터 분석, 데이터 시각화, 모델개발
<br>

### 🧐 Main Issues
AWS, ASOS 날씨 데이터를 지역별로 그룹화하고 지역별 생산량을 가중치로 활용해 가격 예측을 하고자 하였음.
날씨데이터를 전처리하는 과정에서 결측치, 이상치 데이터를 처리하는게 어려운 점이였음.
또한 기존에 시계열 데이터를 활용하면서 롤링변수와 사인,코사인 변환 등을 활용하지 못해 아쉬웠던 점을
이번 프로젝트에서 이러한 기법을 적용해 파생변수들을 생성해 모델의 예측 정확도를 향상시키고자 하였음.

### 🛠️ Resolved
-이상치 처리-
날씨데이터 에서 이상치는 기상청의 물리 한계 검사기준을 활용해 이상치를 제거하였음.
가격이상치는 3시그마 이론 기반 99.7퍼센타일을 초과하는 값을 이상치로 판단해 삭제처리함.
정규성검정.
-타겟(가격)의 경우 음수값, 0, 혹은 시장 가격과 맞지 않는 잠재적 이상치가 존재하는 문제가 발생함
-이를 해결하기 위해 QQ-plot, Anderson-Darling 정규성 검정 후 분포를 헤치지 않는 선에서 이상치로 처리함
![image](https://github.com/ASJ0211/nonsan_pred_DCC/assets/118821779/a3183266-3d1a-4827-9350-33ba030462ad)
<br/>
-결측치 대체-<br/>
1.관측지점별로 결측치를 스플라인 보간을 활용해 대체함(주어진 데이터 내에서 부드러운 곡선이나 고차다항식을 생성해 보간) <br/>
2.지역별 평균값으로 대체해 보간 (ex 경상남도)<br/>
3.IDW 보간: 세종시의 값은 인접지역인 대전,천안,청주의 거리별 가중치를 활용해 역거리 가중보간. <br/>
-파생변수-<br/>
이전 프로젝트에서 해보지 못했던 lag effect를 반영한 변수와, 롤링변수를 활용해 파생변수를 생성하였음.<br/>
월,일별 sin 및 cos 변환을 통해 연속성과 주기성을 보존하도록 처리하였음.<br/>


<br>

### 🎯 Result
##모델부분
-통계적 모델-
??
-머신러닝 모델-
XGBoost와 LGBM 모델을 활용해 앙상블.
auto ml을 통해 확인하였을때 
기존 row 데이터 (rmse: 960) > 전처리,파생변수 생성 후 데이터(rmse: 55)로 
데이터 EDA를 통해 인사이트를 도출하고 만들어낸 파생변수가 유의미하게 작용하여 초기모델에 비해 향상됨을 확인.
또한 shap 변수 중요도를 활용해 변수와 target의 관계를 시각화 하였음.
![image](https://github.com/ASJ0211/nonsan_pred_DCC/assets/118821779/77d481a4-3ae4-43eb-88b9-15888260fa96)


### ⭐ Learnd Lessons
데이터 분석에 있어서 전처리 과정, 파생변수 생성과정에서의 결정에 있어서 매번 논리를 세우고 데이터 시각화를 통해 논리적인 전처리 과정을 만들기 위해 많은 노력을 기울였다.
이러한 과정에서 모든 선택에 데이터로서 얘기하는것이 중요하다는 것을 깨닫게 되었음.
또한 서비스를 만들어나가는데 있어서 고객층과, 비지니스 모델을 만들어 나가는 과정에 대해 배웠음.
아쉬웠던 점은 날씨데이터가 결측치나 이상치가 많아서 전처리 과정에서 많은 시간이 들어갔던 점이였음.

### 💎Expected Effects
![image](https://github.com/ASJ0211/nonsan_pred_DCC/assets/118821779/24bbd844-ff3d-4706-ac39-a093fdce8179)

![image](https://github.com/ASJ0211/nonsan_pred_DCC/assets/118821779/b21e16b0-fd61-4c37-af0b-87a1f89f8c23)





