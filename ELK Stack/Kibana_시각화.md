## 프로젝트명: "우리" 어떤 카드 쓸까?

## **📊 주제 : 카드 소비 데이터 기반 ELK 분석 및 맞춤 카드 추천**

- 본 프로젝트는 카드 소비 데이터를 기반으로 다양한 연령대 및 회원등급별 사용자 소비 패턴을 분석
- Kibana 시각화를 통해 **데이터 기반 카드 추천** 인사이트 도출을 목적으로 함

## 👥 팀 소개

| <img width="150px" src="https://avatars.githubusercontent.com/u/45265805?v=4"/> | <img width="150px" src="https://avatars.githubusercontent.com/u/112679148?v=4"/> | <img width="150px" src="https://avatars.githubusercontent.com/u/176018876?v=4"/> | <img width="150px" src="https://avatars.githubusercontent.com/u/78733700?v=4"/> |
| :---: | :---: | :---: | :---: |
| **박지원** | **신기범** | **임채준** | **이조은** |
| [@bbo9866](https://github.com/bbo9866) | [@shin-kibeom](https://github.com/shin-kibeom) | [@dlacowns21](https://github.com/dlacowns21) | [@LeeJoEun-01](https://github.com/LeeJoEun-01) |

## 🔍 프로젝트 개요

- **분석 대상**: 연령대, 카테고리, 회원등급, 카드유형(신용/체크)
- **사용 기술**: Elasticsearch, Logstash, Kibana (ELK Stack)
- **주요 내용**:
    - 사용자군별 소비 패턴 시각화
    - 소비 증가/감소 트렌형
    - 데이터 기반 맞춤형 카드 추천 로직

### 🎯 **프로젝트 목적**

- 카드 소비 데이터를 기반으로 **사용자 맞춤형 카드 추천 인사이트 제공**
- **Kibana 시각화**를 통해 연령대 및 회원등급별 **소비 패턴 분석**
- **ELK Stack (Elasticsearch, Logstash, Kibana)** 기반 데이터 수집·저장·시각화 파이프라인 구축을 통해 **실무 수준의 데이터 처리 및 시각화 기술을 체득**

## 📊 전연령대별 소비 그래프

<img width="700" alt="image" src="https://github.com/user-attachments/assets/92327d32-59ac-4318-bd06-91bebc424365" />


**[ 그래프 소개 ]**

- X축: 연령 (20~85세까지, 간격 5)
- Y축: 각 대분류 분야별 소비량 평균(단위: 천원, 만원단위 절사)
- 특징
    - 유통의 범위가 넓어서 유통이 차지하는 비율이 전 연령대가 다 높음
        
        (ex. 대형마트, 편의점, 온라인 쇼핑몰 etc..)
        
    - 전체적으로 20살때부터 40세 미만까지 계속 소비가 증가하다가 40세부터 조금씩 전체적인 소비가 감소
    - 이 와중에도 보험/병원비는 계속 증가

### 신용카드/체크카드 사용
| area 그래프 | stackbar 그래프|
| ---- | ----|
|<img width="500" alt="image" src="https://github.com/user-attachments/assets/32e84d61-bdf8-4e4d-bc8b-746054a43463" />|    <img width="500" alt="image" src="https://github.com/user-attachments/assets/bb5351a6-ae40-4e47-8c1f-d56b991a9ba4" /> |

---

## 🧓 65세 이상 소비 패턴 분석

### 🕸️ 모든 소비량을 포함한 그래프

<img width="700" alt="image" src="https://github.com/user-attachments/assets/1183d2ea-cd84-4356-87e2-038860ea1b7b" />


### ✅ 주요 소비 카테고리

- 의료기관
- 유통업영리
- 음식료품
- 일반/휴게음식
    
### 📊 65세 이상 주요 소비 그래프
<img width="700" alt="image" src="https://github.com/user-attachments/assets/dcc15d6e-f87b-4909-9089-a508bda841e3" />
    

**그래프 특징**

- 일반/휴게음식, 음식료품 소비량과 전체적으로 소비가 감소
- 의료기관 소비량이 점진적으로 계속 증가

<img width="700" alt="image" src="https://github.com/user-attachments/assets/dade8161-fbe2-4adb-a716-37734f599b42" />


### ✅ 카드 추천 기준

1. 병원비, 약국, 의료기관 할인 혜택 포함
2. (소비 감소 고려) 연회비가 낮거나 없는 카드
3. 현금처럼 쓸 수 있는 할인형 카드 (체크카드 또는 할인형 신용카드)
4. 교통/통신/마트처럼 필수생활 영역 혜택 포함 시 우선 고려

### 🔖 카드 추천

- **우리카드 카드의정석 EVERYDAY CHECK :**
    
    https://pc.wooricard.com/dcpc/yh1/crd/crd01/H1CRD101S02.do
    

![image.png](attachment:35ce2734-a407-42d0-8698-2937ff3e4d66:image.png)

**혜택**

- **국내/해외 가맹점 0.2% 적립**
- **마트/슈퍼/커피/주유 0.**1% 특별 적립
- 전월 실적 조건, 적립한도 제한 없음

- **우리카드 7CORE :**
    
    https://pc.wooricard.com/dcpc/yh1/crd/crd01/H1CRD101S02.do?cdPrdCd=103755
    

![image.png](attachment:5af3da63-dd2a-4114-8737-3e24d37e7fca:image.png)

**혜택**

- 온라인쇼핑, 대형마트, 배달앱 10% 청구할인
- 교육, 병원, 주유 10% 청구 할인

---

## 👨‍💼 35세 ~ 45세 소비 패턴 분석

### ✅ 주요 소비 카테고리

- 학원, 서적/문구 등 교육·자기계발
- 사무/통신기기
- 자동차/연료/정비

### 📊 35~45세 이상 주요 소비 그래프

- **그래프 특징** : **서적/문구** 카테고리에서 타 연령대 대비 높은 비율 차지

<img width="700" alt="image" src="https://github.com/user-attachments/assets/fce91db2-69c9-4e36-a48c-51669c985aac" />


- **그래프 특징** : **차량 카테고리**에서 50세까지 지속 상승 후, 감소 추세 확인
  - (대/중분류 기준)

<img width="700" alt="image" src="https://github.com/user-attachments/assets/1248b2b6-d176-491b-b65f-27431c83c557" />


### ✅ 카드 추천 기준

- 자녀 교육 또는 자기계발 혜택 포함
- 서점/온라인강의/문구점 할인
- 주유소/정비소 할인, 차량 유지비 절감

### 🔖 카드 추천

| 카드 이미지 | 카드명 | 주요 혜택 |
|-------------|--------|-----------|
| <img width="400" alt="체크카드" src="https://github.com/user-attachments/assets/017cf3b3-8a1a-4b9e-9f5b-05edfb7ac0f7" />| **카드의정석 화물복지카드** | **정유3사 할인 혜택 및 온라인 서점 할인 혜택**<br>• 리터 당 최대 70원 할인<br>• Yes24, 교보문고, 영풍문고 3,000원 캐시백 할인 |
| <img width="400" alt="신용카드" src="https://github.com/user-attachments/assets/0bba084d-022c-43bc-aee4-550a112a895c" /> |**우리카드 7CORE** | • 교육, 병원, 주유 10% 청구할인 혜택<br>• 온라인쇼핑, 대형마트, 배달앱 10% 청구할인 |

---

## 🧑‍🎓 20세 ~ 30세 소비 패턴 분석

### ✅ 주요 소비 카테고리

- 음식점
- 여행 / 교통수단

### 📊 20세 ~30세 주요 소비 그래프
- (중분류 기준)

| 요식업 | 숙박업|
| ------- | ------|
|<img width="500" alt="image" src="https://github.com/user-attachments/assets/ad7a081d-07b3-4b42-8a98-0424a77bcc51" />|<img width="500" alt="image" src="https://github.com/user-attachments/assets/c1c86b69-60a6-4117-9e58-446d91a9dcd1"/> |

- 그래프 특징 :
    음식점, 프랜차이즈 외식 지출, 여행 지출 비율이 가장 높음

### ✅ 카드 추천 기준

- 프랜차이즈 음식점, 간편식 할인
- 식사 시간대 캐시백
- 항공권/숙박 통합 혜택

### 🔖 카드 추천


| 카드 이미지 | 카드명 | 주요 혜택 |
|-------------|--------|-----------|
|<img width="400" alt="오하카드" src="https://github.com/user-attachments/assets/6cc636e0-922f-4bc9-a373-af663e690313" />| ***우리카드 카드의정석 오하 CHECK** | **EAT 5% 캐시백**<br>•배달의민족, 쿠팡이츠 5% 캐시백 <br> <br>**LIFE 5% 캐시백**<br>• 대중교통, 이동통신 5% 캐시백 |
|<img width="400" alt="위비트래블" src="https://github.com/user-attachments/assets/8e5a79fe-5688-4783-8cb3-0625c139063a" />| **우리카드 위비트래블 체크카드** | **해외 가맹점 이용수수료 면제**<br><br>**해외, 간편결제 5% 캐시백**|


## **💎 회원 등급별 소비 패턴**

- 카드사 입장에서는 고객 유입시 ‘인당 평균 카드 사용량’이 가장 많은 VVIP 고객 유입이 중요하다고 판단
- 따라서 VVIP 회원 등급의 소비 패턴을 분석하여 카드 추천을 하고자 함

### 📊 회원 등급별 분포
- (카드 전체 사용량 기준)

- 회원 등급별 인원수를 보았을 때 VVIP 회원은 전체 인원의 약 1%에 해당함
    - 21: VVIP
    - 22: VIP
    - 23: 플래티넘
    - 24: 골드
    - 25: 해당없음음

<img width="700" alt="image" src="https://github.com/user-attachments/assets/80803e76-8764-4a7b-8fba-1f15578e9827" />

### 📊 회원 등급별 평균 카드 사용량
- (카드 전체 사용량 기준)

- 회원 등급별 평균 카드 사용량을 보면 VVIP 등급이 가장 높은 것을 볼 수 있음
- 등급별 카드 사용량이 일정하게 증가하는 것을 보아 등급 선정 기준을 잘 잡은 듯 함

<img width="700" alt="image" src="https://github.com/user-attachments/assets/b2e5f007-26c5-43f6-9902-5fa4692240da" />


### 📊 VVIP 회원 등급의 주요 소비 그래프
- (대분류기준)

- 회원 등급별 대분류 카드 사용량을 보면 대부분의 분류들에 있어 균등하게 카드 사용량이 증가하는데 특히 유통, 요식업, 보험/병원 분류에서 카드 사용량이 가장 눈에 띄게 늘어나는 것을 볼 수 있음

<img width="700" alt="image" src="https://github.com/user-attachments/assets/6039474e-4455-42fa-81c4-2adef4c76665" />


### ✅ 주요 소비 카테고리
- 유통업
- 요식업
- 보험/병원

### 📊 VVIP 회원 등급의 유통업 분류별 소비 그래프
- (중분류기업 분류별 소비 그래프)

- 유통업 카드 사용량 중 회원 등급이 올라갈수록 상대적으로 유통업 영리 분류보다 유통업 비영리에 카드 사용량이 더 많음

<img width="700" alt="image" src="https://github.com/user-attachments/assets/cb52a65e-6ab9-4feb-8f12-4a377dd00196" />

- 영리 유통업보다 비영리 유통업에 돈을 더 소비
    - 금전적으로 여유가 있어 비영리 유통업(ex.학술, 종교, 자선) 소비 추구

### 📊 VVIP 회원 등급의 요식업 분류별 소비 그래프
- (중분류기준)

- 요식업 분류 중 일반/휴게 음식과 음식료품 중 회원 등급이 올라갈수록 음식료품에 카드 사용량이 더 많음

<img width="700" alt="image" src="https://github.com/user-attachments/assets/7755339f-52eb-44bb-a540-bd6e18debcaf" />


- 음식점에서의 카드 사용량은  다른 등급의 회원들과 비슷하지만 음식료품에 돈을 더 많이 소비
    - 기본적인 음식료품을 더 고가의 상품을 사용

### 📊 VVIP 회원 등급의 보험/의료 분류별 소비 그래프
- (중분류기준)

- 보험/의료 분류 중 보건/위생, 보험, 의료기관 모두 등급이 올라갈수록 카드 사용량이 많지만 의료기관에 카드 사용량이 가장 눈에 띄게 증가함

<img width="700" alt="image" src="https://github.com/user-attachments/assets/dbcaec0c-3d42-498f-a411-ebecec1e43d8" />


- 의료기관에서 다른 등급의 회원들보다 돈을 더 많이 소비
    - 병원에 돈을 아끼지 않음

→ VVIP 회원들은 어느 분야의 카드 혜택보다는 카드 전체 실적을 기반한 더 큰 혜택을 선호할 수 있음

### 🔖 카드 추천

- **TWO CHAIRS W :**
    
    https://pc.wooricard.com/dcpc/yh1/crd/crd01/H1CRD101S02.do
    

![image.png](attachment:1c810337-d07a-4232-ab88-2d62b084b031:image.png)

**혜택**

- 프리미엄 기프트 2종
    - 초년도 1천만원, 차년도 이후 5천만원 이상 이용 시 제공
- **국내외 2% 적립**
    - 전월실적조건 없이 월 이용금액 1억원까지 적립 제공

- **로얄블루1000카드 [SKYPASS]:**
    
    https://pc.wooricard.com/dcpc/yh1/crd/crd01/H1CRD101S02.do
    

![image.png](attachment:4d851526-0ed9-48fb-b0a3-8eaa51547f79:image.png)

혜택

- **의전혜택**
    - 명품점 점장 Escort, 미술관 Docent 서비스 등차별화 된 의전혜택 제공
- **100만원 +**
    - 100만원 상당의 GIFT는 기본,연간 이용실적에 따른 추가 GIFT 제공

---

## 💡 인사이트

- **ELK Stack**을 활용해 **연령대·회원등급별** 소비 데이터를 시각화함으로써, 데이터 기반의 카드 추천 논리를 실제 카드 상품과 효과적으로 연결할 수 있었음
- 각 연령층의 주요 소비 카테고리를 중심으로 **실제 생활과 밀접한 혜택** 설계 기준을 도출함
- 회원 등급별 소비 성향 분석을 통해 프리미엄 등급 고객 대상 마케팅 인사이트를 확보할 수 있었음
- 20~30대의 소비 금액 증가 추이를 통해, **“돈을 절약하자”** 는 생활 인사이트를 얻게 됨
  
## ⚠️ 트러블슈팅

- ELK Stack 사용 미흡으로, 시각화 구성에 많은 시간 소요
- 원천 데이터의 카테고리별 대/중분류 기준 모호
    
    → 자체 기준 설정을 통해 분석 실시(ex. 유통, 요식업 - 음식료품 등)
    
- 원래 AGE 데이터 타입이 문자이어서 Logstash를 사용해서 숫자형으로 바꾸고 싶었지만, 시간 부족으로 타입 변환을 하지 못하고 Kibana의 Filter 기능을 사용해서 해결함

## ⭐ 향후 계획

- **데이터 자동 적재 파이프라인 구축**
    
    → Logstash + cron 또는 Python 스크립트를 활용해 정기적인 데이터 수집 및 Elasticsearch 자동 적재 실습 예정
    
- **AGE 데이터 타입 변환 방법 습득**
    
    → **string → integer 타입으로 변환**하여 연령 필터링 및 집계 분석이 가능하도록 전처리
