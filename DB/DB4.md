# 4일차

## Database 4일차

- 조건문 관련 함수 2 : CASE 함수

  - 다양한 비교 연산자를 이용해 조건을 제시할 수 있기 때문에 범위 지정도 가능

  - 프로그래밍 언어의 IF - ELSE IF - ELSE문과 유사한 구조

  - 형식(표현식, expression)

  ```
  CASE WHEN 조건1 THEN 결과1
       WHEN 조건2 THEN 결과2
       WHEN 조건3 THEN 결과3
       ELSE 결과 N
  END
  ```

  <br />

  - 중첩 CASE 문

  ```
  CASE WHEN 조건1 THEN 결과1
            CASE
                    WHEN 조건1 THEN 결과1
                    WHEN 조건2 THEN 결과2
            END
       WHEN 조건2 THEN 결과2
            CASE
              WHEN 조건1 THEN 결과1
              WHEN 조건2 THEN 결과2
            END
       ELSE
  END
  ```

  ```
  CASE WHEN 조건1 THEN 결과1
       ELSE CASE WHEN 조건1 THEN 결과1
                 WHEN 조건2 THEN 결과2
            END
  END
  ```

<br />

- DECODE vs CASE

  - DECODE

    - 오라클 전용 함수

    - 조건의 수가 많으면 가독성이 떨어짐

  - CASE

    - SQL 표준으로 다양한 DBMS에서 사용 가능

    - 조건을 더 유연하게 사용가능

    - 가독성이 좋음

<br />

- CASE 활용

  - 전체 선수들의 포지션별 평균 키 및 전체 평균키

  ```sql
  SELECT ROUND(AVG(CASE WHEN POSITION = 'MF' THEN HEIGHT END), 2) 미드필더
    , ROUND(AVG(CASE WHEN POSITION = 'GK' THEN HEIGHT END), 2) 골키퍼
    , ROUND(AVG(CASE WHEN POSITION = 'DF' THEN HEIGHT END), 2) 디펜더
    , ROUND(AVG(CASE WHEN POSITION = 'FW' THEN HEIGHT END), 2) 포워드
    , ROUND(AVG(HEIGHT), 2) 전체평균키
  FROM PLAYER;
  ```

<br />

- 그룹 함수

  - 하나 이상의 행을 그룹으로 묶어 연산하여 총합, 평균 등을 하나의 결과로 나타냄

  - COUNT(): 입력되는 데이터의 총 건수(행)를 출력

  - SUM(): 입력되는 데이터의 누적값 구해서 출력

  - AVG(): 입력되는 데이터의 평균값 구해서 출력

  - MAX(): 입력되는 데이터 중 가장 큰 값 출력

  - MIN(): 입력되는 데이터 중 가장 작은 값 출력

  > ROLLUP(), PIVOT(), RANK(), DENSE_RANK() 오라클 전용 함수이되 알 필요 있음

<br />

- 그룹 함수 특징

  - 여러 행의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 함수

  - GROUP BY절은 행들을 소그룹화함

  - SELECT, HAVING, ORDER BY절에 사용 가능

  > 테이블에 아무리 행이 많아도 단 한 개의 결과 값만을 산출

  > NULL을 포함하지 않음

<br />

- 그룹 함수 문제 예시 및 풀이

  ```sql
  SELECT MAX(HEIGHT) 큰키
    , MIN(HEIGHT) 작은키
    , AVG(HEIGHT) 평균키
    , COUNT(PLAYER_ID) "선수들의 총수"
  FROM PLAYER;
  ```

  <br />

  - Player 테이블에서 전체 행수, 그룹 함수에서 적용되는 키건수, 최대키, 최소키, 총합키, 평균키, 전체 행수로 적용한 평균키, 문제가 되는 선수 수를 조회

    - 키건수: 그룹함수로 실제 적용되는 데이터들

  ```sql
  SELECT COUNT(PLAYER_ID) "전체 행수"
    , COUNT(HEIGHT) "그룹함수 적용 키건수"
    , MAX(HEIGHT) 최대키
    , MIN(HEIGHT) 최소키
    , SUM(HEIGHT) 총합키
    , ROUND(AVG(HEIGHT), 2) 평균키
    , ROUND((SUM(HEIGHT) / COUNT(PLAYER_ID)), 2) "전체 행수 적용 평균키"
    , COUNT(PLAYER_ID) - COUNT(HEIGHT) "문제가 되는 선수 수"
  FROM PLAYER:
  ```

<br />

- GROUP BY절

  - 소그룹에 대한 항목별로 통계 정보를 얻을 때 추가로 사용 => 중복 제거

  - 표현식

  ```
  SELECT[DISTINCT] 칼럼명 [별칭]
  FROM 테이블명
  [WHERE 조건식]
  [GROUP BY column이나 표현식]
  [HAVING 그룹조건식]
  ```

<br />

- GROUP BY절과 HAVING절의 특성

  ```
  - GROUP BY절을 통해 소그룹별 기준을 정한 후, SELECT 절에 집계 함수를 사용

  - 집계 함수의 통계 정보는 NULL 값을 가진 행을 제외하고 수행

  - GROUP BY절에서는 SELECT절과는 달리 ALIAS 명 사용 불가

  - 집계 함수는 WHERE절에 올 수 없음(WHERE절은 집계 함수를 사용할 수 있는 GROUP BY절보다 먼저 수행됨)

  - WHERE절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거

  - HAVING절은 GROUP BY절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건 표시 가능

  - GROUP BY절에 의한 소그룹별로 만들어진 집계 데이터 중, HAVING절에서 제한 조건을 두어
    조건을 만족하는 내용만 출력

  - HAVING절은 일반적으로 GROUP BY절 뒤에 위치
  ```

<br />

- GROUP BY 예시

  - 부서 별로 평균 급여를 구함

  ```sql
  -- 에러 발생 코드
  SELECT DEPTNO, AVG(SAL)
  FROM EMP;

  SELECT DEPTNO, AVG(SAL)
  FROM EMP
  GROUP BY DEPTNO;
  ```

  > DEPTNO는 여러 개의 row(행), AVG(SAL)은 단일 row(행)를 가짐

<br />

- GROUP BY 문제 풀이

  - 부서별 최대 급여와 최소 급여를 구함

  - 부서번호 최대급여 최소급여

  ```sql
  SELECT DEPTNO 부서번호
    , MAX(SAL) 최대급여, MIN(SAL) 최소급여
  FROM EMP
  GROUP BY DEPTNO;
  ```

  <br />

  - 직업별 입사일이 가장 이른 날짜를 출력

  - 직업 입사일

  ```sql
  SELECT JOB 직업, MIN(HIREDATE) 입사일
  FROM EMP
  GROUP BY JOB;
  ```

<br />

- HAVING절

  - SELECT절에 조건을 사용하여 결과를 제한할 때는 WHERE절을 사용

  - 그룹의 결과를 제한할 때는 HAVING절 사용

  > GROUP BY절 전용 조건절이라고 이해하면 편함

<br />

- HAVING절 예시

  - 급여 평균이 2000 이상인 부서만 출력

  ```sql
  SELECT DEPTNO, AVG(SAL)
  FROM EMP
  GROUP BY DEPTNO
  HAVING AVG(SAL) >= 2000;
  ```

<br />

- DB test exam(8문제)

  > 키의 반올림은 셋째 자리에서 반올림

  1. 가장 최근에 입사한 사원의 입사일과 입사한 지 가장 오래된 사원의 입사일을 출력

  - EX: 1980년4월27일 00시00분00초 형태

  - 입사일 사일

  ```sql
  SELECT TO_CHAR(MAX(HIREDATE), 'YYYY"년"MM"월"DD"일" HH24"시"MI"분"') AS 입사일
    , TO_CHAR(MIN(HIREDATE), 'YYYY"년"MM"월"DD"일" HH24"시"MI"분"') AS 입사일
  FROM EMP;
  ```

  <br />

  2. 10번 부서 소속의 사원 중에서 커미션을 받는 사원의 수를 구함

  ```sql
  SELECT COUNT(COMM) 사원수
  FROM EMP
  WHERE DEPTNO = 10
  GROUP BY DEPTNO;
  ```

  <br />

  3. 선수 테이블을 아래의 조건에 맞게 구함

  - 포지션, 인원수, 키인원수, 최대키, 최소키, 평균키

  - 포지션이 없는 선수들의 인원수도 출력

  - 포지션, 인원수, 키인원수, 최대키, 최소키, 평균키에서 NULL이 있는 경우 0으로 표시

  - 키의 값이 NULL이면 170cm로 변경 => 키인원수 등 모두 변경돼야 함

  ```sql
  SELECT POSITION 포지션
    , COUNT(PLAYER_ID) 인원수
    , COUNT(NVL(HEIGHT, 170)) 키인원수
    , MAX(NVL(HEIGHT, 170)) 최대키
    , MIN(NVL(HEIGHT, 170)) 최소키
    , ROUND(AVG(NVL(HEIGHT, 170)), 2) 평균키
  FROM PLAYER
  GROUP BY POSITION;
  ```

  <br />

  4. K-리그 선수들의 포지션별 평균키를 구하는데 평균키가 180cm 이상인 정보만 출력

  - 포지션 평균키

  ```sql
  SELECT POSITION 포지션
    , ROUND(AVG(HEIGHT), 2) 평균키
  FROM PLAYER
  GROUP BY POSITION
  HAVING AVG(HEIGHT) >= 180;
  ```

  <br />

  5. 부서의 최댓값과 최솟값을 구하되 최대 급여가 2900 이상인 부서를 부서 번호가 빠른 순으로 출력

  - DEPTNO 최대급여 최소급여

  ```sql
  SELECT DEPTNO, MAX(SAL) 최대급여, MIN(SAL) 최소급여
  FROM EMP
  GROUP BY DEPTNO
  HAVING MAX(SAL) >= 2900
  ORDER BY DEPTNO;
  ```

  <br />

  6. 삼성 브루윙즈와 FC서울의 인원수 조회

  - 팀ID 인원수

  ```sql
  SELECT TEAM_ID 팀ID, COUNT(PLAYER_ID)
  FROM PLAYER
  GROUP BY TEAM_ID
  HAVING TEAM_ID = 'K02' OR TEAM_ID = 'K09';

  SELECT TEAM_ID 팀ID, COUNT(PLAYER_ID)
  FROM PLAYER
  WHERE TEAM_ID = 'K02' OR TEAM_ID = 'K09'
  GROUP BY TEAM_ID;
  ```

  <br />

  7. 포지션별 평균키만 출력하되 최대키가 190cm 이상인 선수를 가지고 있는 포지션의 정보를 포지션별 평균 키가 높은 순으로 출력

  - 포지션 평균키

  ```sql
  SELECT POSITION 포지션
    , ROUND(AVG(HEIGHT), 2) 평균키
  FROM PLAYER
  GROUP BY POSITION
  HAVING MAX(HEIGHT) >= 190
  ORDER BY ROUND(AVG(HEIGHT), 2) DESC;
  ```

  <br />

  8. 같은 달에 입사한 사원들 별 최고 급여를 구하되 2000보다 높은 급여만 출력, 단 입사한 달이 빠른 순으로 정렬

  - 입사한 달 최고급여

  ```sql
  SELECT TO_CHAR(HIREDATE, 'MM') "입사한 달"
    , MAX(SAL)
  FROM EMP
  GROUP BY TO_CHAR(HIREDATE, 'MM')
  HAVING MAX(SAL) > 2000
  ORDER BY "입사한 달";
  ```
