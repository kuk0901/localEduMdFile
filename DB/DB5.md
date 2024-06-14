# 5일차

## Database 5일차

- 쿼리 실행 순서

  | 순서 | 구문                     | 내용               |
  | ---- | ------------------------ | ------------------ |
  | 5    | SELECT 칼럼명 [별칭]     | 선택, 출력문       |
  | 1    | FROM 테이블명            |
  | 2    | WHERE 조건식             | 몇 건(ROW)         |
  | 3    | GROUP BY 칼럼이나 표현식 | 그룹화             |
  | 4    | HAVING 그룹조건식        | 그룹화된 값의 조건 |
  | 6    | ORDER BY 칼럼이나 표현식 | 데이터 정렬        |

<br />

- 문제풀이

  - 각 부서의 직무별로 평균 급여 계산

  - 부서가 존재해야 하며 평균 급여가 2000 이상인 경우에만 부서별, 직무별로 오름차순, 평균 급여는 높은 순으로 결과 표시

  ```sql
  SELECT DEPTNO 부서, JOB 직무, AVG(SAL) 평균급여
  FROM EMP
  WHERE DEPTNO IS NOT NULL
  GROUP BY DEPTNO, JOB
  HAVING AVG(SAL) >= 2000
  ORDER BY DEPTNO, JOB, AVG(SAL) DESC;
  ```

  <br />

  - 각 팀의 포지션별 선수 수를 계산

  - 단 선수들의 수가 11명 미만인 경우만 출력하되 팀별, 포지션별로 오름차순 하여 결과 표시

  ```sql
  SELECT TEAM_ID 팀
    , POSITION 포지션
    , COUNT(PLAYER_ID) "팀별 포지션 선수 수"
  FROM PLAYER
  GROUP BY TEAM_ID, POSITION
  ```

<br />

- 조인(JOIN)

  - 여러 테이블을 묶어서 데이터 처리 가능

  ```
  DB에서는 데이터가 중복되면 여러 가지 이상 현상이 발생하기 때문에 데이터가 중복되지 않도록 하기 위해서
  2개 이상의 테이블로 나눠 정보를 저장함

  => 원하는 정보를 얻어오려면 나누어진 테이블을 붙여서 사용해야 함
  => 테이블을 붙여주는 것
  => JOIN
  ```

  ```sql
  SELECT ENAME, DNAME
  FROM EMP, DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO
  AND ENAME = 'SMITH';
  ```

<br />

- JOIN의 종류

  | 이름                       | 설명                                     |
  | -------------------------- | ---------------------------------------- |
  | EQUI JOIN(동등 조인)       | 동일 칼럼을 기준으로 조인                |
  | NON-EQUI JOIN(비동등 조인) | 동일 칼럼 없이 다른 조건을 사용하여 조인 |
  | OUTER JOIN(외부 조인)      | 조인 조건에 만족하지 않는 로우도 나타냄  |
  | SELF JOIN(자체 조인)       | 한 테이블 내에서 조인                    |
  | CROSS JOIN(상호 조인)      | 조인 조건이 없는 조인                    |

  > CROSS JOIN은 주로 과부하 테스트에서 사용됨

  > JOIN 후에는 하나의 테이블로 사용

<br />

- EQUI JOIN(동등 조인) 문제 풀이

  - 조인을 사용해 뉴욕에서 근무하는 사원의 이름과 급여 출력

  - 이름 급여 근무지

  ```sql
  SELECT EMP.ENAME 이름
    , EMP.SAL 급여
    , DEPT.LOC 근무지
  FROM EMP. DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO
  AND DEPT.LOC = 'NEW YORK';
  ```

  <br />

  - ACCOUNTING 부서 소속 사원의 이름과 입사일을 입사일 늦은 사원 순으로 출력

  - 사원명 입사일

  ```sql
  SELECT EMP.ENAME 사원명
    , EMP.HIREDATE 입사일
  FROM EMP. DEPT
  WHERE EMP.DEPTNO = DEPT.DEPTNO
  AND DEPT.DNAME = 'ACCOUNTING'
  ORDER BY EMP.HIREDATE DESC;
  ```

<br />

- NON-EQUI JOIN(비동등 조인) 예시 및 문제 풀이

  - 예시

  ```sql
  SELECT ENAME, SAL, GRADE, LOSAL, HISAL
  FROM EMP E, SALGRADE SG
  WHERE E.SAL >= SG.LOSAL
  AND E.SAL <= SG.HISAL;
  ```

  > 테이블도 Alias 가능

  <br />

  - 사원의 커미션이 급여보다 적은 사원과 급여 범위 범위에 해당하는 급여 등급 조회

  - 사원번호, 사원명, 급여, 커미션, 등급

  ```sql
  SELECT EMPNO 사원번호
    , ENAME 사원명
    , SAL 급여
    , COMM 커미션
    , GRADE 등급
  FROM EMP E, SALGRADE SG
  WHERE E.COMM < E.SAL
  AND E.SAL >= SG.LOSAL
  AND E.SAL <= SG.HISAL;
  ```

  <br />

  - 각 사원이 속한 급여 등급을 구하고 급여 등급별로 최대 급여 조회

  - 칼럼명은 필요한 것만 작성

  ```sql
  SELECT GRADE 등급, MAX(SAL) 최대급여
  FRoM EMP E. SALGRADE SG
  WHERE E.SAL >= SG.LOSAL
  AND E.SAL <= SG.HISAL
  GROUP BY SG.GRADE;
  ```

<br />

- SELF JOIN(자체 조인) 예시 및 문제 풀이

  - 예시(부서별로 각 사원의 동료 목록을 구함)

  ```sql
  SELECT MY_EMP.EMPNO, MY_EMP.ENAME, MY_EMP.DEPTNO
    , OTHER_EMP.EMPNO AS 동료번호
    , OTHER_EMP.ENAME AS 동료이름
  FROM EMP MY_EMP, EMP OTHER_EMP
  WHERE MY_EMP.DEPTNO = OTHER_EMP.DEPTNO
  AND MY_EMP.EMPNO <> OTHER_EMP.EMPNO
  ORDER BY MY_EMP.DEPTNO, MY_EMP.EMPNO;
  ```

  <br />

  - 매니저가 KING인 사원들의 이름과 직급 출력

  - 나의사원번호 나의이름 킹의사원번호 매니저이름

  ```sql
  SELECT MY_EMP.EMPNO 나의사원번호
    , MY_EMP.ENAME 나의이름
    , OTHER_EMP.EMPNO "매니저 킹의 사원번호"
    , OTHER_EMP.ENAME 매니저이름
  FROM EMP MY_EMP, EMP OTHER_EMP
  WHERE MY_EMP.MGR = OTHER_EMP.EMPNO
  AND OTHER_EMP.ENAME = 'KING'
  ```

  <br />

  - 특정 사원의 매니저가 누구인지 조회

  - 00의 매니저는 00 입니다

  ```sql
  SELECT ME.ENAME || '의 매니저는 ' || OE.ENAME || ' 입니다'
  FROM EMP ME, EMP OE
  WHERE ME.MEG = OE.EMPNO;
  ```

  <br/>

  - JAMES와 동일한 근무지에서 근무하는 사원의 이름을 조회

  - 내이름 동료이름 근무지

  ```sql
  SELECT ME.ENAME 내이름
    , OE.ENAME 동료이름
    , D.LOC 근무지
  FROM EMP ME, EMP OE, DEPT D
  WHERE ME.DEPTNO = OE.DEPTNO
  AND ME.ENAME = 'JAMES'
  AND ME.DEPTNO = D.DEPTNO
  AND OE.DEPTNO = D.DEPTNO
  AND ME.ENAME <> OE.ENAME;
  ```

<br />

- OUTER JOIN(외부 조인)

  ```
  테이블이 조인될 때, 어느 한쪽의 테이블에 해당하는 데이터가 존재하는데 다른 쪽 테이블에는 데이터가 존재하지 않는 경우 그 데이터는 출력되지 않는 문제를 해결하기 위해 사용되는 조인기법
  ```

  - 한쪽에는 데이터가 있고 한쪽에는 데이터가 없는 경우, 데이터가 있는 쪽 테이블의 내용을 모두 출력하는 것

<br />

- OUTER JOIN(외부 조인) 문제 풀이

  - 사원들의 매니저 몰록 출력하되 상사 매니저가 없는 경우 C E O라고 출력

  - 직원명 직원번호 상사이름 상사번호

  ```sql
  SELECT E.ENAME 직원명
    , E.EMPNO 직원번호
    , NLV(ME.ENAME, 'C E O') 상사이름
    , ME.EMPNO 상사번호
  FROM EMP E, EMP ME
  WHERE E.MGR = ME.EMPNO(+);
  ```

<br />

- 표준 조인 기법(ANSI JOIN)

  - 다양한 DBMS에서 사용가능한 표준 조인 기법

  <br />

  1. CROSS JOIN

  ```sql
  SELECT *
  FROM EMP CROSS JOIN DEPT
  ORDER BY EMPNO;
  ```

  <br />

  2. INNER JOIN(내부 조인): WHERE 아닌 ON 사용

  ```sql
  SELECT *
  FROM EMP JOIN DEPT
  ON EMP>DEPTNO = DEPT.DEPTNO
  WHERE EMPNO >= 7500;

  SELECT *
  FROM EMP INNER JOIN DEPT
  ON EMP>DEPTNO = DEPT.DEPTNO
  WHERE EMPNO >= 7500;
  ```

  <br />

  3. OUTER JOIN(외부 조인): WHERE 아닌 ON 사용

  ```sql
  -- LEFT OUTER JOIN
  SELECT *
  FROM EMP EMPLOYEE LEFT OUTER JOIN DEPT DEPARTMENT
  ON EMPLOYEE.DEPTNO = DEPARTMENT.DEPTNO;

  SELECT *
  FROM EMP EMPLOYEE LEFT JOIN DEPT DEPARTMENT
  ON EMPLOYEE.DEPTNO = DEPARTMENT.DEPTNO;

  -- RIGHT OUTER JOIN
  SELECT *
  FROM EMP EMPLOYEE RIGHT OUTER JOIN DEPT DEPARTMENT
  ON EMPLOYEE.DEPTNO = DEPARTMENT.DEPTNO;

  SELECT *
  FROM EMP EMPLOYEE RIGHT JOIN DEPT DEPARTMENT
  ON EMPLOYEE.DEPTNO = DEPARTMENT.DEPTNO;

  -- FULL OUTER JOIN
  SELECT *
  FROM EMP EMPLOYEE FULL OUTER JOIN DEPT DEPARTMENT
  ON EMPLOYEE.DEPTNO = DEPARTMENT.DEPTNO
  ORDER BY EMPNO, EMPLOYEE.DEPTNO DESC;
  ```

<br />

- JOIN 문제 풀이(Oracle JOIN || ANSI JOIN)

  - 부서코드 30번 부서의 소속 사원의 이름 및 부서코드, 부서명 조회(ANSI JOIN)

  - 사원명 부서번호 부서명

  ```sql
  SELECT ENAME 사원명
    , EMP.DEPTNO 부서번호
    , DNAME 부서명
  FROM EMP INNER JOIN DEPT
  ON EMP.DEPTNO = DEPT.DEPTNO
  WHERE EMP.DEPTNO = 30;
  ```

  <br />

  - 매니저 사원번호가 7698인 사원들의 이름 및 부서명 조회

  - 사원명 상사번호 부서번호 부서명

  ```sql
  SELECT ENAME 사원명
    , MGE 상사번호
    , E.DEPTNO 부서번호
    , DNAME 부서명
  FROM EMP E JOIN DEPT D
  ON E.DEPTNO = D.DEPTNO
  WHERE MGR = 7698;
  ```

  <br />

  - 포지션이 골키퍼인 선수들에 대한 데이터만을 백넘버 순으로 조회

  - 선수명 백넘버 포지션 연고지

  ```sql
  SELECT PLAYER_NAME 선수명
    , BACK_NO 백넘버
    , POSITION 포지션
    , REGION_NAME 연고지
  FROM PLAYER INNER JOIN TEAM
  ON PLAYER.TEAM_ID = TEAM.TEAM_ID
  WHERE POSITION = 'GK';
  ```

  <br />

  - 각 팀의 이름과 경기장명을 출력하되 팀 ID가 빠른 순으로 정렬

  - 팀명 경기장명

  ```sql
  SELECT TEAM_NAME 팀명
    , STADIUM_NAME 경기장명
  FROM TEAM T FULL OUTER JOIN STADIUM S
  ON T.STADIUM_ID = S.STADIUM_ID
  ORDER BY TEAM_ID;
  ```

  <br />

  - 소속팀이 갖고 있는 전용 구장의 정보를 팀의 정보와 함께 출력하는 SQL문

  - 연고지명 팀명 스타디움ID 스타디움명 좌석수

  ```sql
  SELECT REGION_NAME 연고지명
    , TEAM_NAME 팀명
    , S.STADIUM_ID 스타디움ID
    , TO_CHAR(SEAT_COUNT, '999,999') 좌석수
  FROM TEAM T FULL OUTER JOIN STADIUM S
  ON T.STADIUM_ID = S.STADIUM_ID;
  ```

  <br />

  - 사원의 새로운 부서발령 장소를 작성

  - EMPNO DEPTNO DNAME NEW_DNAME

  - 10번 부서 유지, 20번 부서 R&D, 30번 부서 MARKETING

  ```sql
  SELECT EMPNO
    , E.DEPTNO
    , D.DNAME
    , CASE  WHEN D.DNAME = 'ACCOUNTING' THEN 'ACCOUNTING'
            WHEN D.DNAME = 'RESEARCH' THEN 'R&' || 'D'
            WHEN D.DNAME = 'SALES' THEN 'MARKETING'
      END AS NEW_DNAME
  FROM EMP E INNER JOIN DEPT D
  ON E.DEPTNO = D.DEPTNO
  ORDER BY E.DEPTNO;
  ```
