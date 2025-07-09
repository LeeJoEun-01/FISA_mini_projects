# 📘 RDBMS SQL 학습 프로젝트

## ✨ 학습 목표

- **Oracle과 MySQL**을 다루며 각 DBMS의 차이점과 공통점을 이해

- 실습은 Oracle과 MySQL 둘 다 실습하기 위해 **DBeaver** 사용

- 더 깊은 탐구와 자율적인 복습을 위해, 팀원들과 함께 배운 내용을 적극적으로 활용하고, **서로 문제를 만들어 풀며 자기주도적으로 학습**을 진행

---

## 👥 함께 학습한 사람

| 이제현  | 임유진     | 이정이              | 이조은                                                   |
|:---:|:--------------:|:------------------:|:--------------------------------:|
| <img src="https://github.com/lyjh98.png" width="80">      | <img src="https://github.com/imewuzin.png" width="80">    | <img src="https://github.com/2jeong2.png" width="80">     | <img src="https://github.com/LeeJoEun-01.png" width="80"> |
| [@lyjh98](https://github.com/lyjh98)  |  [@imewuzin](https://github.com/imewuzin)       | [@2jeong2](https://github.com/2jeong2)   | [@LeeJoEun-01](https://github.com/LeeJoEun-01) | 

## ⚙️ 실습 테이블

| 테이블<br>이름      | 속성                                                          | 데이터                     |
| ---------------- | ------------------------------------------------------------- | --------------------------------- |
| **`emp`:<br>사원**  | 사번, 이름, 직업, 상사번호, 급여, 커미션, 입사일, 부서번호 등 | ![image](https://github.com/user-attachments/assets/4bd6b58f-dc9e-4b20-8f56-7968d21166ab)|
| **`dept`:<br>부서** | 부서번호, 부서명, 위치                                        | ![image](https://github.com/user-attachments/assets/9627ff00-cd2a-41b2-9438-57ec7dc01b27)|


    
## 📄 학습 문제 & SQL 예시

아래 9가지 문제를 Oracle, MySQL로 각각 작성해보며 실습했습니다.

---

<details>
<summary> 1️⃣ 돈 많이 버는 사람 이름 TOP 5 출력</summary>
<div markdown="1">

**Oracle**

```sql
SELECT ename
FROM (
  SELECT ename
  FROM emp
  ORDER BY sal DESC
)
WHERE ROWNUM <= 5;
```

**MySQL**

```sql
SELECT ename
FROM emp
ORDER BY sal DESC
LIMIT 5;
```

</div> </details>

<details>
<summary> 2️⃣ 직업에 A가 2번 들어가는 사람 이름</summary>
<div markdown="2">

**Oracle/MySQL (동일)**

```sql
SELECT ename
FROM emp
WHERE job LIKE '%A%A%';
```

</div> </details>

<details>
<summary> 3️⃣ 복합 조건 검색</summary>
<div markdown="3">

> 사원번호 7900 이상이며 두번째 글자가 A
> 또는 부서번호 30이면서 1981-09-01 이후 입사자

**Oracle**

```sql
SELECT empno, ename, deptno, hiredate
FROM emp
WHERE (empno >= 7900 AND ename LIKE '_A%')
   OR (deptno = 30 AND hiredate >= TO_DATE('1981-09-01', 'YYYY-MM-DD'));
```

**MySQL**

```sql
SELECT empno, ename, deptno, hiredate
FROM emp
WHERE (empno >= 7900 AND ename LIKE '_A%')
   OR (deptno = 30 AND hiredate >= '1981-09-01');
```

</div> </details>

<details>
<summary> 4️⃣ 1981년대 입사자 이름 내림차순</summary>
<div markdown="4">

**Oracle**

```sql
SELECT ename
FROM emp
WHERE hiredate BETWEEN TO_DATE('1981-01-01', 'YYYY-MM-DD')
                   AND TO_DATE('1981-12-31', 'YYYY-MM-DD')
ORDER BY ename DESC;
```

**MySQL**

```sql
SELECT ename
FROM emp
WHERE hiredate BETWEEN '1981-01-01' AND '1981-12-31'
ORDER BY ename DESC;
```
</div> </details>

> ##### 1981년대 입사자 찾기
> `BETWEEN -- AND ` 대신에 `EXTRACT()`사용 가능
> WHERE EXTRACT(YEAR FROM hiredate) = 1981

<details>
<summary> 5️⃣ 1981년 입사자 급여순 상세 조회</summary>
<div markdown="5">

**Oracle**

```sql
SELECT ename, job, sal, hiredate
FROM emp
WHERE hiredate BETWEEN TO_DATE('1981-01-01', 'YYYY-MM-DD')
                   AND TO_DATE('1981-12-31', 'YYYY-MM-DD')
ORDER BY sal DESC;
```

**MySQL**

```sql
SELECT ename, job, sal, hiredate
FROM emp
WHERE hiredate BETWEEN '1981-01-01' AND '1981-12-31'
ORDER BY sal DESC;
```

</div> </details>

<details>
<summary> 6️⃣ 월급 1000 이상인 사원 이름, 직업, 월급</summary>
<div markdown="6">
### 6️⃣ 월급 1000 이상인 사원 이름, 직업, 월급

**Oracle/MySQL (동일)**

```sql
SELECT ename, job, sal
FROM emp
WHERE sal >= 1000;
```

</div> </details>

<details>
<summary> 7️⃣ 직업이 SALESMAN인 사원 이름, 직업, 연봉 내림차순</summary>
<div markdown="7">

**Oracle**

```sql
SELECT ename, job, sal*12 AS 연봉
FROM emp
WHERE job = 'SALESMAN'
ORDER BY sal*12 DESC;
```

**MySQL**

```sql
SELECT ename, job, sal*12 AS 연봉
FROM emp
WHERE job = 'SALESMAN'
ORDER BY 연봉 DESC;
```

</div> </details>

<details>
<summary> 8️⃣ 사원 이름, 번호, 직업, 연봉을 부서번호로 정렬</summary>
<div markdown="8">

**Oracle/MySQL (동일)**

```sql
SELECT ename, empno, job, sal*12 AS 연봉
FROM emp
ORDER BY deptno;
```

</div> </details>

<details>
<summary> 9️⃣ 이름, 직업, 고용일, 연봉 (고용일 오름차순, 연봉 내림차순)</summary>
<div markdown="9">

**Oracle**

```sql
SELECT ename AS 이름, job AS 직업, hiredate AS 고용일, sal*12 AS 연봉
FROM emp
ORDER BY 고용일 ASC, 연봉 DESC;
```

**MySQL**

```sql
SELECT ename AS 이름, job AS 직업, hiredate AS 고용일, sal*12 AS 연봉
FROM emp
ORDER BY 고용일 ASC, 연봉 DESC;
```

</div> </details>

## 🔍 MySQL vs Oracle 비교

| 구분               | Oracle             | MySQL                           |
| ------------------ | ------------------ | ------------------------------- |
| **TOP N 조회**     | `ROWNUM` 사용      | `LIMIT` 사용                    |
| **날짜 포맷**      | `TO_DATE()`로 변환 | `'YYYY-MM-DD'` 문자열 직접 사용 |
| **NULL 처리 함수** | `NVL()`            | `IFNULL()`                      |
| **문자 대소문자**  | 대소문자 구분 엄격 | 기본은 구분 안함 (BINARY 필요)  |
| **서브쿼리/정렬**  | ANSI SQL 호환      | ANSI SQL 호환                   |

---

## ✅ 복습 포인트

- 표준 SQL과 DB별 문법 차이 이해
- 실무에 자주 쓰는 `ORDER BY`, `LIKE`, `BETWEEN`, `IN`, `NOT IN`
- 날짜/숫자 조건 및 문자열 패턴 매칭
- 데이터 조회 로직의 **가독성**, **재사용성** 고려
