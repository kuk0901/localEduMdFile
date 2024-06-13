# 3일차

## Database 3일차

- 변환형 함수 예시

  ```sql
  SELECT SYSDATE, TO_CHAR(SYSDATE, 'YYYY-MM-DD DAY')
  FROM DUAL;

  SELECT SYSDATE, TO_CHAR(SYSDATE, 'YYYY-MM-DD DY')
  FROM DUAL;

  SELECT SYSDATE, TO_CHAR(SYSDATE, 'YYYY-MM-DD, HH24 : MI : SS')
  FROM DUAL;

  SELECT HIREDATE, TO_CHAR(HIREDATE, 'YYYY-MM-DD DAY')
  FROM EMP;
  ```

<br />

- 날짜형 함수 중요 부분

  - 모호한, 애매한, 헷갈리는 코드 작성 X

  ```sql
  SELECT *
  FROM EMP
  WHERE HIREDATE = '80/12/17'; -- 암시적 형 변환

  SELECT *
  FROM EMP
  WHERE HIREDATE = '1980/12/17'; -- 암시적 형 변환
  ```

  > 암시적 형 변환 사용 시 에러날 수 있음

  <br />

  - 날짜와 문자 비교: 문자형을 날짜형으로 변환, 날짜형을 문자형으로 변환

  ```sql
  SELECT SYSDATE, TO_DATE('1980/12/17', 'YYYY/MM/DD')
    , TO_DATE('20800612', 'YYYYMMDD')
  FROM DUAL;

  SELECT SYSDATE, TO_DATE('1980/12/17 125011', 'YYYY/MM/DD HHMISS')
    , TO_DATE('20800612 13:50:11', 'YYYYMMDD HH24:MI:SS')
  FROM DUAL;

  -- date를 내가 원하는 형태로 보는 방법: TO_CHAR 사용
  SELECT SYSDATE
    , TO_CHAR(TO_DATE('1980/12/17 125011', 'YYYY/MM/DD HHMISS'), 'HH:MI:SS')
    , TO_CHAR(TO_DATE('20800612 13:50:11', 'YYYYMMDD HH24:MI:SS'), 'YYYYMMDD HH24:MI:SS')
  FROM DUAL;
  ```

  > 날짜의 끝은 문자

<br />

- 날짜형 함수 문제 풀이

  - 현재 날짜에서 2024년 5월 13일 사이의 일수를 구함

  ```sql
  SELECT SYSDATE - TO_DATE('2024/05/13', 'YYYY/MM/DD')
  FROM DUAL;

  SELECT TRUNC(SYSDATE - TO_DATE('2024/05/13', 'YYYY/MM/DD'))
  FROM DUAL;
  ```

  <br />

  - "2,592,000" 이 값이 초라면 분, 시, 일로 변환

  ```sql
  SELECT 2592000 초
    , (2592000 / 60) 분
    , (2592000 / 60) 60 시
    , ((2592000 / 60) / 60) / 24 일
  ```

  <br />

  - emp 테이블에서 사원들 년차 구하기, 경력이 높은 사원부터 출력하되 경력이 같은 경우 입사 날짜가 빠른 순으로 정렬

  - 사원번호, 이름, 입사날짜, 경력년차

  ```sql
  SELECT EMPNO 사원번호, ENAME 이름, HIREDATE 입사날짜
    , TO_NUMBER(TO_CHAR(SYSDATE, 'YYYY')) - TO_NUMBER(TO_CHAR(HIREDATE, 'YYYY')) 경력년차
  FROM EMP
  ORDER BY 경력년차 DESC, HIREDATE;

  SELECT EMPNO 사원번호, ENAME 이름, HIREDATE 입사날짜
    , ROUND((SYSDATE - HIREDATE) / 365) 경력년차
  FROM EMP
  ORDER BY 경력년차 DESC, HIREDATE;
  ```

<br />

- 두 날짜 사이의 달수를 구하는 MONTHS_BETWEEN()

  ```sql
  SELECT SYSDATE, SYSDATE + 365
    , MONTHS_BETWEEN(SYSDATE + 365, SYSDATE) 월차이
  FROM DUAL;
  ```

<br />

- 숫자 출력 형식

  | 구분 | 설명                                                 |
  | ---- | ---------------------------------------------------- |
  | 0    | 자릿수를 나타내며 자릿수가 맞지 않을 경우 0으로 채움 |
  | 9    | 자릿수를 나타내며 자릿수가 맞지 않아도 채우지 않음   |
  | L    | 각 지역별 통화 기호를 앞에 표시                      |
  | .    | 소수점                                               |
  | ,    | 천 단위 자리 구분                                    |

<br />

- 숫자형을 문자형으로 변형

  ```sql
  SELECT 123000, TO_CHAR(123000)
  FROM DUAL;

  SELECT 123000, TO_CHAR(123000, 'L999,999') , TO_CHAR(123000, 'L000,000')
  FROM DUAL;

  SELECT 123000, TO_CHAR(123000, 'L9,999,999') , TO_CHAR(123000, 'L0,000,000')
  FROM DUAL;
  ```

<br />

- 숫자형으로 변환

  ```sql
  SELECT 123,000 - 20,000
  FROM DUAL;

  SELECT '123,000' - '20,000'
  FROM DUAL;

  -- 숫자형으로 변환
  SELECT TO_NUMBER('123,000', '999,999') - TO_NUMBER('20,000', '999,999')
  FROM DUAL;
  ```

<br />

- 환율 문제 풀이

  - 10,000원의 엔화 환율과 10,000원의 달러 환율 즉, 환율을 계산할 수 있는 코드

  ```sql
  select  TO_CHAR((1 * 10000), 'L999,999') as "한국(원)"
    , TO_CHAR(((1 * 10000) / 8.763500), '999,990.99') as "일본(엔)"
    , TO_CHAR(((1 * 10000) / 1377.40), '999,990.99') as "미국(달러)"
  from DUAL;

  select  TO_CHAR(((1 * 10000)), '999,990.99') as "일본(엔)"
    , TO_CHAR((1 * 10000) / 0.114110, 'L999,999') as "한국(원)",
    , TO_CHAR(((1 * 10000) / 157.174645), '999,990.99') as "미국(달러)"
  from DUAL;
  ```

  > 타 수강생분 코드 인용

<br />

- 부정 연산자

  - ANSI/ISO 표준 : 모든 운영 체제에서 사용 가능

    | 종류             | 연산자 | 의미      |
    | ---------------- | ------ | --------- |
    | 부정 논리 연산자 | !=     | 같지 않다 |
    | 부정 논리 연산자 | ^=     | 같지 않다 |
    | 부정 논리 연산자 | <>     | 같지 않다 |

  <br />

  - 부정 SQL 연산자: 오라클에서만 사용 가능

    | 종류            | 연산자              | 의미                        |
    | --------------- | ------------------- | --------------------------- |
    | 부정 SQL 연산자 | NOT 칼럼명 =        | 같지 않다                   |
    | 부정 SQL 연산자 | NOT 칼럼명 >        | ~ 보다 크지 않다            |
    | 부정 SQL 연산자 | NOT BETWEEN A AND B | A와 B의 값 사이에 있지 않다 |
    | 부정 SQL 연산자 | NOT IN (LIST)       | LIST 값과 일치하지 않다     |
    | 부정 SQL 연산자 | IS NOT NULL         | NULL 값을 갖지 않는다       |

<br />

- NULL을 다루는 함수

  - NVL(조회하고 싶은 칼럼, NULL 데이터를 변경하고 싶은 값)

  - NULLIF(EXPR1, EXPR2): 1이 2와 같으면 NULL을, 아니면 1 리턴

  - COALESCE(EXPR1, EXPR2): 첫 번째 칼럼을 선택 값으로, 두 번째 칼럼을 2차 선택 값으로 선택하되 두 칼럼 모두 NULL인 경우 NULL로 표시

  > 일반적으로 NVL을 가장 많이 사용

<br />

- NULL과 공집합

  - 공집합은 NULL과 다름

  - 공집합: 조건에 맞는 데이터가 한 건도 없는 경우 => NULL 데이터와 다르게 이해

<br />

- NULL 함수 문제 풀이

  - 사원의 연봉을 구함, 커미션이 없는 사원의 경우 전부 자신의 월급의 10%를 적용, 연봉+커미션이 가장 높은 순으로 정렬

  - 사원명, 월급, 연봉, 커미션, 연봉+커미션

  ```sql
  SELECT ENAME 사원명, SAL 월급, (SAL * 12) 연봉
    , NVL(COMM, SAL * 0.1) 커미션
    , (SAL * 12) + NVL(COMM, SAL * 0.1) "연봉+커미션"
  FROM EMP
  ORDER BY "연봉+커미션" DESc;
  ```

  <br />

  - 모든 사원에는 상관(MANAGER) 존재, 상사가 없는 사원만 출력하되 MANAGER에 C E O가 출력되도록 함

  - EMPNO ENAME MANAGER

  ```sql
  -- 더 좋은 코드
  SELECT EMPNO, ENAME, NVL(TO_CHAR(MRG), 'C E O') MANAGER
  FROM EMP
  WHERE MGR IS NULL;

  SELECT EMPNO, ENAME, NVL(NULL, 'C E O') MANAGER
  FROM EMP
  WHERE MGR IS NULL;
  ```

<br />

- 조건문 관련 함수 1 : DECODE 함수

  - switch case문과 같은 기능(조건문)

  - 형식

  ```
  DECODE(표현식 조건 1, 결과 1,
              조건 2, 결과 2
              조건 3, 결과 3
              기본결과 N(else와 비슷)
  )
  ```
