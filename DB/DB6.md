# 6일차

## Database 6일차

- 계층형(Hierarchical)

  - 계층 데이터: 동일 테이블에 계층적으로 상위와 하위 데이터가 포함된 데이터

  ```
  ex)

  1. 사원들 사이에 상사 사원과 하위 사원의 관계 존재

  2. 조직 테이블 중에 상위 조직과 하위 조직 존재

  ...
  ```

  <br />

  - Oracle의 계층형 질의 구문

  > LEVEL 가상 칼럼

  ```sql
  LEVEL 상위관계 -- 루트 데이터이면 1, 그 하위 데이터이면 2 (리프(LEAF) 데이터까지 1씩 증가)
  CONNECT_BY_ISLEAF AS 리프데이터 -- 전재 과정에서 해당 데이터가 리프 데이터이면 1, 아니면 0

  START WITH MGR IS NULL -- 최상위 관리자를 찾음, 일반적으로 NULL 값
  CONNECT BY PRIOR EMPNO = MGR -- 계층 구조를 만듦, 하위와 상위로 관련된 칼럼 연결
  ORDER SIBLINGS BY ENAME; -- 같은 레벨의 항목을 이름순으로 정렬(기준을 다르게 사용 가능)
  ```

  <br />

  - Oracle의 계층 구조 사용법

  ```sql
  SELECT LPAD(' ', 2 * (LEVEL - 1)) || ENAME AS EMP_NAME
    , EMPNO
    , MGR
    , LEVEL 상위관계
    , CONNECT_BY_ISLEAF AS 리프데이터
  FROM EMP
  START WITH MGR IS NULL
  CONNECT BY PRIOR EMPNO = MGR
  ORDER SIBLINGS BY ENAME;
  ```

<br />

- 서브쿼리(subquery)

  - 하나의 sql문 안에 포함되어 있는 또 다른 sql문

  - 메인 쿼리와 서브쿼리를 포함하는 종속적인 관계

<br />

- 조인(JOIN) VS 서브쿼리(subquery)

  - 조인: 참여하는 모든 테이블이 대등한 관계에 있기 때문에 조인에 참여하는 모든 테이블의 칼럼을 어느 위치에라도 자유롭게 사용 가능

  - 서브쿼리: 메인 쿼리의 칼럼을 모두 사용할 수 있지만 메인 쿼리는 서브쿼리의 칼럼 사용 불가

<br />

- 서브쿼리 사용 시 주의 사항

  1. 서브쿼리를 괄호로 감싸서 사용

  2. 서브쿼리는 단일 행(single row) 또는 복수 행(multiple row) 비교 연산자와 함께 사용 가능

     > 단일 행 비교 연산자는 서브쿼리의 결과가 반드시 1건 이하여야 하고, <br /> 복수 행 비교 연산자는 서브쿼리의 결과 건수와 상관없음

  3. 서브쿼리에서는 order by절 사용 불가

     > order by절은 select절에서 오직 한 개만 올 수 있기 때문에 메인 쿼리의 마지막 문장에 위치

<br />

- 서브쿼리 사용 가능 위치

  - select, from, where, having, order by, insert문의 values 절, update문의 set절

<br />

- 서브쿼리 종류

  | 종류            | 설명                                                                                                                                                                 |
  | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | 비연관 서브쿼리 | 서브쿼리가 메인쿼리 칼럼을 갖고 있지 않는 형태의 서브쿼리, <br /> 메인쿼리에 값(서브쿼리가 실행된 결과)을 제공하기 위한 목적으로 주로 사용                           |
  | 연관 서브쿼리   | 서브쿼리가 메인쿼리 칼럼을 갖고 있는 형태의 서브쿼리, <br /> 일반적으로 메인쿼리가 먼저 수행되어 읽힌 데이터를 서브쿼리에서 조건이 맞는지 확인하고자 할 때 주로 사용 |

<br />

- 단일 행 서브쿼리에서 사용되는 연산자

  - 비교 연산자: =, <, <=, <> 등

<br />

- 다중 행 서브쿼리에서 사용되는 연산자

  - in, all, any 등

<br />

- 다중 칼럼 서브쿼리

  - 서브쿼리와 메인쿼리에서 비교하고자 하는 칼럼 개수와 칼럼의 위치가 동일해야 함

<br />

- 단일 행 서브쿼리 표현식

  ```sql
  SELECT 칼럼명1
  FROM TABLE
  WHERE 칼럼명 = (SELECT 칼럼명1
                FROM TABLE2);
  ```

<br />

- 서브쿼리 문제풀이

  - SMITH와 같은 부서에서 근무하는 사원의 정보 조회

  - 모든 칼럼

  ```sql
  SELECT *
  FROM EMP
  WHERE DEPTNO = (SELECT DEPTNO
                  FROM EMP
                  WHERE ENAME = 'SMITH');
  ```

  <br />

  - SMITH와 동일한 직급을 가진 사원 조회

  - 모든 칼럼

  ```sql
  SELECT *
  FROM EMP
  WHERE JOB = (SELECT JOB
                  FROM EMP
                  WHERE ENAME = 'SMITH');
  ```

  <br />

  - SMITH의 급여와 동일하거나 더 많이 받는 사원명과 급여 조회

  - 사원명 급여

  ```sql
  SELECT ENAME 사원명
    , SAL 급여
  FROM EMP
  WHERE SAL >= (SELECT SAL
                  FROM EMP
                  WHERE ENAME = 'SMITH')
  AND ENAME <> 'SMITH';
  ```

  <br />

  - DALLAS에서 근무하는 사원의 이름, 부서번호 조회하되 사원명을 내림차순

  - 사원명 부서번호

  ```sql
  SELECT ENAME 사원명
    , DEPTNO 부서번호
  FROM EMP
  WHERE DEPTNO = (SELECT DEPTNO
                  FROM DEPT
                  WHERE LOC = 'DALLAS')
  ORDER BY ENAME DESC;
  ```

  <br />

  - 영업부에서 근무하는 모든 사원의 이름과 급여 조회하되 급여가 적은 순

  - 사원명 급여

  ```sql
  SELECT ENAME 사원명
    , SAL 급여
  FROM EMP
  WHERE DEPTNO = (SELECT DEPTNO
                  FROM DEPT
                  WHERE DNAME = 'SALES')
  ORDER BY SAL;
  ```

  <br />

  - 자신의 직속상관이 KING인 사원이면서 급여가 2500 이상인 사원의 이름과 급여 조회

  - 사원명 급여

  ```sql
  SELECT ENAME 사원명
    , SAL 급여
  FROM EMP
  WHERE MGR = (SELECT EMPNO
              FROM EMP
              WHERE ENAME = 'KING')
  AND SAL >= 25000;
  ```

  <br />

  - 선수들 중에서 키가 평균 이하인 선수들의 정보를 선수명이 빠른 순으로 조회

  - 선수면 포지션 키 백넘버

  ```sql
  SELECT PLAYER_NAME 선수명
    , POSITION 포지션
    , HEIGHT 키
    , BACK_NO 백넘버
  FROM PLAYER
  WHERE HEIGHT <= (SELECT AVG(HEIGHT)
                  FROM PLAYER)
  ORDER BY PLAYER_NAME;
  ```

<br />

- 다중 행 서브쿼리

  - IN(OR 연산자와 같은 의미): 메인 쿼리의 비교 조건이 서브 쿼리의 결과 중 "하나라도 일치"하면 참

  - ANY, SOME(ANY와 동일): 메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과와 "하나 이상 일치"하면 참

  - ALL: 메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과의 "모든 값과 일치"하면 참

  - EXIST: 메인 쿼리의 비교 조건이 서브 쿼리의 결과 중에서 "만족하는 값이 하나라도 존재"하면 참

<br />

- 다중 행 서브쿼리 문제 풀이

  - 부서별로 가장 급여를 많이 받는 사원의 정보 조회

  - EMPNO ENAME SAL DEPTNO

  ```sql
  SELECT EMPNO
    , ENAME
    , SAL
    , DEPTNO
  FROM EMP
  WHERE SAL IN (SELECT MAX(SAL)
                FROM EMP
                GROUP BY DEPTNO)
  ORDER BY SAL DESC;
  ```

<br />

- ALL 연산자

  - 30번 소속 사원들 중에서 급여를 가장 많이 받는 사원보다 더 많은 급여를 받는 사람의 급여 조회

  ```sql
  SELECT MAX(SAL)
  FROM EMP
  GROUP BY DEPTNO
  HAVING DEPTNO = 30;

  SELECT ENAME, SAL
  FROM EMP
  WHERE SAL >= (SELECT MAX(SAL)
               FROM EMP
               GROUP BY DEPTNO
               HAVING DEPTNO = 30);

  SELECT ENAME, SAL
  FROM EMP
  WHERE SAL >= ALL(SELECT MAX(SAL)
                FROM EMP
                WHERE DEPTNO = 30);
  ```

<br />

- ANY 연산자

  ```sql
  SELECT ENAME, SAL
  FROM EMP
  WHERE SAL > (SELECT MIN(SAL)
               FROM EMP
               GROUP BY DEPTNO
               HAVING DEPTNO = 30);

  SELECT ENAME, SAL
  FROM EMP
  WHERE SAL > ANY(SELECT SAL
                FROM EMP
                WHERE DEPTNO = 30);
  ```

<br />

- ANY 연산자, ALL 연산자 문제 풀이

  - 영업 사원들보다 급여를 많이 받는 사원들의 이름과 급여를 출력하되 영어 사원 미출력

  - ENAME SAL

  ```sql
  SELECT *
  FROM EMP;

  SELECT SAL
  FROM EMP
  WHERE JOB = 'SALESMAN';

  SELECT ENAME, SAL
  FROM EMP
  WHERE SAL > ALL(SELECT SAL
                  FROM EMP)
  ```

  <br />

  - 영업 사원들의 최소 급여보다 많이 받는 사원들의 이름과 직급을 출력하되 영업 사원은 출력하지 않음

  - ENAME SAL JOB

  ```sql
  SELECT *
  FROM EMP;

  SELECT ENAME
    , SAL
    , JOB
  FROM EMP
  WHERE SAL > ANY(SELECT SAL
                  FROM EMP
                  WHERE JOB = 'SALESMAN')
  AND JOB <> 'SALESMAN';
  ```

  <br />

  - 실제로 현업에서 사용하는 방식

  ```sql
  SELECT ENAME, SAL, JOB
  FROM EMP
  WHERE SAL > (SELECT MIN(SAL)
               FROM EMP
               WHERE JOB = 'SALESMAN')
  AND JOB <> 'SALESMAN';
  ```
