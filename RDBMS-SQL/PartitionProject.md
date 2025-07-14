# US Accidents Partitioning Performance Comparison

대용량 데이터셋으로 **파티셔닝 후 성능 비교**를 실습을 위한 프로젝트이다.

## 프로젝트 목적

- 대용량 데이터 파티셔닝을 통해 **쿼리 속도, 스캔량, 처리 비용 비교**
- 다양한 파티셔닝 방식(RANGE, LIST)을 실제 데이터에 적용한 뒤 쿼리 처리 속도와 효율성 비교

---

## 팀원

| <img src="https://avatars.githubusercontent.com/LeeJoEun-01" width="140"/> | <img src="https://avatars.githubusercontent.com/shinjunsuuu" width="140"/> | <img src="https://avatars.githubusercontent.com/yunkihong-dev" width="140"/> |
| :------------------------------------------------------------------------: | :------------------------------------------------------------------------: | :--------------------------------------------------------------------------: |
|                                   이조은                                   |                                   신준수                                   |                                    홍윤기                                    |

---

## 데이터셋

🚑 **[US Accidents (2016 - 2023) - Kaggle](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents/data)**

- **설명:**
  - 2016년 2월 ~ 2023년 3월, 미국 49개 주 **약 770만 건의 사고 데이터**
  - 교통 센서, 카메라, 도로교통부 API 기반 수집
  - **사고 예측, 지역·날씨·시간별 분석 가능**
- **파이썬을 사용한 전처리:**
  - `TRUE/FALSE` → SQL용 `1/0` 변환
  - 분석 목적에 맞지 않는 컬럼 15개를 제거하여 데이터 크기를 줄이고 처리 효율성을 향상시켰으며, 이를 통해 본격 분석 전 테스트 및 쿼리 성능 검증을 효과적으로 수행할 수 있도록 전처리를 진행

### 데이터 속성명

| 컬럼명            | 설명                                | 컬럼명            | 설명                         | 컬럼명         | 설명                          |
| ----------------- | ----------------------------------- | ----------------- | ---------------------------- | -------------- | ----------------------------- |
| ID                | 사고 고유 식별자                    | City              | 사고 발생 도시               | Bump           | 과속 방지턱 여부 (0/1)        |
| Severity          | 사고 심각도 (1: 낮음, 4: 매우 높음) | State             | 사고 발생 주(State)          | Give_Way       | 양보 표지 여부 (0/1)          |
| Start_Time        | 사고 발생 시각                      | Timezone          | 사고 발생 지역의 표준 시간대 | No_Exit        | 출구 없음 구역 여부 (0/1)     |
| End_Time          | 사고 종료 시각                      | Weather_Timestamp | 날씨 데이터 기록 시각        | Roundabout     | 로터리 여부 (0/1)             |
| Country           | 사고 발생 국가                      | Visibility(mi)    | 가시거리(마일)               | Stop           | 정지 표지 여부 (0/1)          |
| Airport_Code      | 인근 공항 코드                      | Wind_Speed(mph)   | 바람 속도(마일/시간)         | Traffic_Signal | 신호등 여부 (0/1)             |
| Temperature(F)    | 기온(화씨)                          | Weather_Condition | 날씨 상태 (Rain, Snow 등)    | Amenity        | 사고 인근 편의시설 여부 (0/1) |
| Wind_Direction    | 바람 방향                           | Railway           | 철도 교차 여부 (0/1)         | Crossing       | 횡단보도 여부 (0/1)           |
| Precipitation(in) | 강수량(인치)                        | Station           | 역 근처 여부 (0/1)           | Junction       | 교차로 여부 (0/1)             |
| Source            | 데이터 수집 소스                    | Traffic_Calming   | 교통 진정 시설 여부 (0/1)    | Turning_Loop   | 회전 구간 여부 (0/1)          |
| County            | 사고 발생 카운티                    |


<details>
<summary><span style="font-size: 24px; font-weight: bold;">DBeaver에 CSV 파일 Import하는 방법</span></summary>

1. database 생성
2. database 우클릭 → `Import Data`
3. `Input File` > `Browse` > `.csv` 파일 선택
4. `Tables Mapping` > `Configure`
   - `REAL`-> `FLOAT`와 같이
   - <span style="color: #E6B800; font-weight: bold;">자료 타입이 다르면 여기서 직접 매핑 필요❗</span>

예시:  
<img width="700" alt="image" src="https://github.com/user-attachments/assets/83d82405-b80b-46dd-ad91-ea2bb5713aae" />

</details>

---

## 📊 파티션 방식

| 기준                             | 방식  | 설명                                       |
| -------------------------------- | ----- | ------------------------------------------ |
| 📆 **년도 (Start_Time - YEAR)**  | RANGE | 2016~2023 연도별 분리                      |
| 🍂 **계절 (Start_Time - MONTH)** | LIST  | 월 기준 계절별 분리 (겨울, 봄, 여름, 가을) |
| 🚦 **사고 수준 (Severity)**      | LIST  | 심각도 기준 분리 (1~4)                     |

> ### 년도(start_time-**year**) 파티셔닝 SQL

```sql
CREATE TABLE US_Accidents_patitioned_year (
    -- 기존 컬럼들...
    Accident_Year INT GENERATED ALWAYS AS (YEAR(Start_Time)) STORED,
    PRIMARY KEY (ID, Accident_Year)
)
PARTITION BY RANGE (Accident_Year) (
    PARTITION p2016 VALUES LESS THAN (2017),
    PARTITION p2017 VALUES LESS THAN (2018),
    PARTITION p2018 VALUES LESS THAN (2019),
    PARTITION p2019 VALUES LESS THAN (2020),
    PARTITION p2020 VALUES LESS THAN (2021),
    PARTITION p2021 VALUES LESS THAN (2022),
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION pMax   VALUES LESS THAN MAXVALUE
);
```

> ### 계절(start_time-**month**) 파티셔닝 SQL

```sql
CREATE TABLE US_Accidents2_season_partitioned (
    -- 기존 컬럼들...
    Start_Month INT GENERATED ALWAYS AS (MONTH(Start_Time)) STORED,
        PRIMARY KEY (ID, Start_Month)

)
PARTITION BY LIST (Start_Month)(
PARTITION p_winter VALUES IN (12, 1, 2),
PARTITION p_spring VALUES IN (3, 4, 5),
PARTITION p_summer VALUES IN (6, 7, 8),
PARTITION p_fall VALUES IN (9, 10, 11)
);
```

> ### 사고 수준(**severity**) 파티셔닝 SQL

```sql
ALTER TABLE severity_partitioned_data
PARTITION BY RANGE (Severity) (
    PARTITION p01 VALUES LESS THAN (2),
    PARTITION p02 VALUES LESS THAN (3),
    PARTITION p03 VALUES LESS THAN (4),
    PARTITION p04 VALUES LESS THAN (5),
    PARTITION p_max VALUES LESS THAN MAXVALUE
);
```

---

# 쿼리별 다양한 파티션 성능 비교

<details>
<summary>DBeaver 사용해서 쿼리별 데이터 성능 비교하는 방법</summary>
 - Window > Show View > Query Manager
<img width="658" height="462" alt="image" src="https://github.com/user-attachments/assets/06923af3-d336-47ea-addf-721d6c47ceeb" />

</details>

## 날씨별 심각한 사고(Severtiry>=3) 건수 쿼리 비교

```sql
SELECT
    Weather_Condition,
    COUNT(*) AS accident_count
FROM
    severity_partitioned_data
WHERE
    Severity >= 3
GROUP BY
    Weather_Condition
ORDER BY
    accident_count DESC;
```

| 파티셔닝 기준           | 실행 시각        | 실행 시간 | 결과 건수 | 증감 |
| ----------------- | ------------ | ----- | ----- |------|
| ❌ 원본 데이터 (파티셔닝 X) | Jul-11 15:19 | 26s   | 127   | 기준 |
| ⚙️ `Severity` 기준  | Jul-11 15:21 | 14s   | 127   | <span style="color=#0000FF"> ⬇️ 12s </span>|
| 📅 `년도` 기준        | Jul-11 15:25 | 23s   | 127   | <span style="color=red"> ⬆️ 3s </span>|
| 🍂 `계절` 기준        | Jul-11 15:28 | 27s   | 127   | <span style="color=red"> ⬆️ 1s </span>|


#### 결과 고찰 🔍

> 맑음, 구름 많음 등에서 사고 건수가 높았으며 이는 해당 날씨 조건에서 교통량이 많아 사고가 자주 발생했을 가능성을 시사한다. 비, 눈, 안개에서도 심각한 사고가 다수 발생해 도로 미끄러움과 시야 제한이 사고에 영향을 주었음을 보여주고 있어 악천후 시 사고 건수는 적어도 심각도는 높을 수 있어 안전 운전이 중요하다. 이를 통해 날씨별 사고 대응 정책 수립 및 사고 예방 방안을 마련하는 기반 자료로 활용이 가능하다.

<br>
----

## 코로나 이전, 이후 도시별 사고 건수 쿼리 비교

```sql
SELECT
  City,
  CASE
    WHEN YEAR(Start_Time) BETWEEN 2016 AND 2019 THEN '2016~2019'
    WHEN YEAR(Start_Time) BETWEEN 2020 AND 2023 THEN '2020~2023'
  END AS 기간구분,
  COUNT(*) AS 사고건수
FROM US_Accidents2
WHERE YEAR(Start_Time) BETWEEN 2016 AND 2023
  AND City IS NOT NULL
GROUP BY City, 기간구분
ORDER BY City, 기간구분;
```

| 파티셔닝 기준           | 실행 시각        | 실행 시간  | 결과 건수 | 증감                                            |
| ----------------- | ------------ | ------ | ----- | --------------------------------------------- |
| ❌ 원본 데이터 (파티셔닝 X) | Jul-11 16:01 | 2m 29s | 200   | 기준                                            |
| 📅 `년도` 기준        | Jul-11 15:57 | 2m 06s | 200   | <span style="color:#1E90FF;">⬇️ 21s 감소</span> |

#### 결과 고찰 🔍

> 코로나 이전(2016-2019)과 이후(2020-2023) 사고 건수를 도시별로 비교한 결과, 일부 지역은 감소했지만 전반적으로는 증가세를 보였다. 특히 2021년 이후 사회적 거리두기 완화와 이동량 증가로 사고가 다시 급증했으며, 대도시를 중심으로 증가 폭이 커졌다. 이는 팬데믹 이후 배달 수요 증가와 자가용 이동 증가 등의 생활 변화가 주요 원인으로 해석된다. 이러한 결과는 교통안전 정책 및 인프라 개선 방향 설정에 활용될 수 있다.

<br>
----

## 계절별 사고 건수와 평균 기온 쿼리 비교

```sql
SELECT
    CASE
        WHEN Start_Month IN (12, 1, 2) THEN 'Winter'
        WHEN Start_Month IN (3, 4, 5) THEN 'Spring'
        WHEN Start_Month IN (6, 7, 8) THEN 'Summer'
        WHEN Start_Month IN (9, 10, 11) THEN 'Fall'
    END AS Season,
    COUNT(*) AS Accident_Count,
    ROUND(AVG(Temperature_F), 2) AS Avg_Temperature_F
FROM US_Accidents2_season_partitioned
GROUP BY Season
ORDER BY FIELD(Season, 'Winter', 'Spring', 'Summer', 'Fall');
```

| 파티셔닝 기준           | 실행 시각        | 실행 시간 | 증감                                            |
| ----------------- | ------------ | ----- | --------------------------------------------- |
| ❌ 원본 데이터 (파티셔닝 X) | Jul-11 16:54 | 45s   | 기준                                            |
| 🍂 `계절` 기준 파티셔닝   | Jul-11 16:39 | 22s   | <span style="color:#1E90FF;">🔽 23s 감소</span> |


#### 결과 고찰 🔍

> 기온이 낮은 겨울과 가을에 사고 건수가 높아져 저온 및 기상 악화가 사고 위험도를 증가시키는 경향이 나타났다. 여름철은 기온이 높고 도로 상태가 안정되어 사고 건수가 가장 낮게 나타났다. 봄철은 기온이 낮음에도 여름보다 사고가 많아 계절별 사고 특성이 뚜렷하게 구분되었다. 파티셔닝과 전과 후 결과가 매우 뚜렷해 조회 기준에 따른 파티셔닝이 필요해 보인다는 생각이 들었다.

<br>

### 트러블슈팅 ☄️
1️⃣데이터셋 크기로 인한 성능 미분별 문제
  : 이전 7만 건 데이터셋으로 테스트했을 때 파티셔닝 전후의 속도 차이가 거의 나타나지 않아 효과를 검증할 수 없었다. 이를 해결하기 위해 770만 건 이상 대용량 데이터셋(US Accidents 2016-2023) 으로 교체하여 실험하였고, 이후 파티셔닝의 성능 차이가 명확히 드러났다.
2️⃣ VARCHAR 타입 필드 크기 문제
  : DBeaver로 CSV Import 시 Description과 같이 길이가 가변적이고 큰 VARCHAR 필드의 크기 측정이 어려워 에러가 발생했다. 해결을 위해 분석에 필요 없는 Description 필드를 삭제하고, 나머지 VARCHAR 필드는 길이를 조정하여 테이블에 적재하였다.
3️⃣ 파티셔닝 기준 및 쿼리 최적화
  : 처음에는 단순히 파티셔닝을 적용하는 것에만 집중했지만, 실험 중 조건에 따라 특정 파티션만 스캔되도록 쿼리를 작성해야 성능 개선이 나타난다는 점을 발견했다. 이후 파티션 키 기반의 조회 조건으로 쿼리를 수정하여 실험의 신뢰성을 높였다.
