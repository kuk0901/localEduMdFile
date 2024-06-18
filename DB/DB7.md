# 7일차

## Database 7일차

- 연관 서브 쿼리

  - 서브 쿼리 내에 메인쿼리 칼럼이 사용된 서브 쿼리

  - EXISTS 서브 쿼리는 항상 연관 서브 쿼리로 사용됨

<br />

- 연관 서브 쿼리 예시

  - 선수 자신이 속한 팀의 평균 키보다 작은 선수들의 정보

  ```sql
  SELECT T.TEAM_NAME
    , P.PLAYER_NAME
    , P.POSITION
    , P.BACK_NO
    , P.HEIGHT
  FROM PLAYER P, TEAM T
  WHERE P.TEAM_ID = T.TEAM_ID
  AND P.HEIGHT < (SELECT AVG(S.HEIGHT)
                  FROM PLAYER S
                  WHERE S.TEAM_ID = P.TEAM_ID
                  AND S.HEIGHT IS NOT NULL
                  GROUP BY S.TEAM_ID)
  ORDER BY P.PLAYER_NAME;
  ```

  <br />

  -

  ```sql
  SELECT STADIUM_ID ID, STADIUM_NAME 경기장명
  FROM STADIUM A
  WHERE EXISTS (
                  SELECT 1
                  FROM SCHEDULE X
                  WHERE X.STADIUM_ID = A.STADIUM_ID
                  AND X.SCHE_DATE BETWEEN '20120501' AND '20120502'
  );
  ```

<br />

- 스칼라 서브 쿼리

  - 한 행, 한 칼럼만을 반환하는 서브 쿼리

  - 대부분의 칼럼 명을 작성할 수 있는 곳이라면 대부분 사용 가능

  - SELECT 절에 서브 쿼리 사용하기

<br />

- FROM 절에서 서브 쿼리 사용(INLINE VIEW)

  - INLINE VIEW: FROM 절에서 사용되는 서브 쿼리

  > FROM 절에는 테이블 명이 오도록 되어있음

  - 서브쿼리의 결과가 마치 실행 시에 동적으로 생성된 테이블인 것처럼 사용 가능

  - SQL문이 실행될 때만 임시로 생성되는 동적인 뷰이기 때문에, 데이터베이스에 해당 정보 저장 X

  - 일반적인 뷰를 정적 뷰(Static View)라고 하고 인라인 뷰를 동적 뷰(Dynamic View)라고 함

  - 테이블 명이 올 수 있는 곳에서 사용 가능

  ```
  서브쿼리의 칼럼은 메인쿼리에서 사용할 수 없지만 인라인 뷰는 동적으로 생성된 테이블

  => 인라인 뷰를 사용하는 것인 조인 방식을 사용하는 것과 같음
  ```

  <br />

  - 예시

  ```sql
  SELECT *
  FROM (SELECT ENAME, SAl, TO_CHAR(SAL * 12, '999,999') YEAR_SAL
        FROM EMP E
        WHERE E.SAL > 2000) MY_ACCOUNT_SAL_INFO
  ORDER BY YEAR_SAL DESC;
  ```

<br />

- 인라인 뷰(Inline View) 문제풀이

  - 사원 중 5~6월 사이 입사한 사원 조회하되 입사한 달을 오름차순 해서 조회

  - 사원명 입사한날짜 입사한달

  ```sql
  SELECT *
  FROM (SELECT ENAME 사원명
          , TO_CHAR(HIREDATE, 'YYYY-MM-DD') 입사한날짜
          , LTRIM(TO_CHAR(HIREDATE, 'MM'), 0) || '월' AS 입사한달
        FROM EMP
        WHERE TO_NUMBER(TO_CHAR(HIREDATE, 'MM')) BETWEEN 5 AND 6
        ORDER BY 입사한달);

  SELECT *
  FROM (SELECT ENAME 사원명
          , TO_CHAR(HIREDATE, 'YYYY-MM-DD') 입사한날짜
          , TO_CHAR(HIREDATE, 'FMMM') || '월' AS 입사한달
        FROM EMP
        WHERE TO_NUMBER(TO_CHAR(HIREDATE, 'FMMM')) BETWEEN 5 AND 6
        ORDER BY 입사한달);
  ```

<br />

- ROWNUM

  ```
  oracle의 rownum은 칼럼과 비슷한 성격의 Pesudo Column으로써

  SQL 처리 결과 집합의 각 행에 대해 임시로 부여되는 일련번호이며,

  테이블이나 집합에서 원하는 만큼의 행만 가져오고 싶을 때

  WHERE 절에서 행의 개수를 제한하는 목적으로 사용
  ```

  > 1로만 구성되어 있으면 맞을 때 증가하는 형태

  <br />

  - 예시

  ```sql
  -- 1건
  SELECT PLAYER_NAME
  FROM EMP
  WHERE ROWNUM = 1;

  -- 3건, 이미 조회한 내용에서 3개 출력 => 단순 출력의 개수를 의미함 인지
  SELECT PLAYER_NAME
  FROM EMP
  WHERE SAL > 2000
  AND ROWNUM <= 3;
  ```

<br />

- INLINE VIEW + ROWNUM 예시

  ```sql
  SELECT CUSTOM_EMP.EMPNO
    , CUSTOM_EMP.ENAME
    , CUSTOM_EMP.JOB
    , CUSTOM_EMP.SAL
  FROM (SELECT ROWNUM AS RANKING, EMPNO, ENAME, JOB, SAL
        FROM EMP
        ORDER BY SAL DESC) CUSTOM_EMP
  WHERE ROWNUM <= 3;
  ```

  ```sql
  SELECT ROWNUM AS RANKING, EMPNO, ENAME, JOB, SAL
  FROM EMP
  ORDER BY SAL DESC;

  -- 미완성
  SELECT ROWNUM AS RANKING, EMPNO, ENAME, JOB, SAL
  FROM (SELECT EMPNO, ENAME, JOB, SAL
        FROM EMP
        ORDER BY SAL DESC) CUSTOM_EMP;

  -- 완성
  SELECT *
  FROM (SELECT ROWNUM AS RANKING, EMPNO, ENAME, JOB, SAL
        FROM (SELECT EMPNO, ENAME, JOB, SAL
              FROM EMP
              ORDER BY SAL DESC) CUSTOM_EMP)
  WHERE RANKING = 3;

  SELECT *
  FROM (SELECT ROWNUM AS RANKING, EMPNO, ENAME, JOB, SAL
        FROM (SELECT EMPNO, ENAME, JOB, SAL
              FROM EMP
              ORDER BY SAL DESC) CUSTOM_EMP)
  WHERE RANKING = 3
  OR RANKING >= 10;
  ```

<br />

- INLINE VIEW + ROWNUM 문제 풀이

  - 모든 선수들 중에서 가장 키가 큰 5명의 선수 조회하되 키 값이 없는 선수들은 제외

  - 선수명 포지션 백넘버 키

  ```sql
  SELECT 선수명, 포지션, 백넘버, 키
  FROM (SELECT ROWNUM AS RANKING
          , PLAYER_NAME 선수명
          , POSITION 포지션
          , BACK_NO 백넘버
          , HEIGHT 키
        FROM PLAYER
        WHERE HEIGHT IS NOT NULL
        ORDER BY HEIGHT DESC)
  WHERE ROWNUM <= 5;
  ```

  <br />

  - K-리그 선수 중에서 포지션이 미드필더인 선수들의 소속팀 명 및 선수 정보를 조회하되 선수명을 오름차순으로 하며 11명의 선수 조회

  - RNUM 팀명 선수명 백넘버

  ```sql
  SELECT *
  FROM (SELECT ROWNUM AS RNUM
          , TEAM_NAME 팀명
          , PLAYER_NAME 선수명
          , BACK_NO 백넘버
        FROM (SELECT T.TEAM_NAME
                , P.PLAYER_NAME
                , P.BACK_NO
                FROM PLAYER P, TEAM T
                WHERE P.TEAM_ID = T.TEAM_ID
                AND P.POSITION = 'MF'
                ORDER BY PLAYER_NAME))
  WHERE ROWNUM <= 11;
  ```

  <br />

  - K-리그 선수 중에서 포지션이 미드필더인 선수들의 소속팀 명 및 선수 정보를 조회하되 선수명을 오름차순으로 하며 12번째에서 11명의 선수 조회

  - RNUM 팀명 선수명 백넘버

  ```sql
  SELECT *
  FROM (SELECT ROWNUM AS RNUM
          , TEAM_NAME 팀명
          , PLAYER_NAME 선수명
          , BACK_NO 백넘버
        FROM (SELECT T.TEAM_NAME
                , P.PLAYER_NAME
                , P.BACK_NO
                FROM PLAYER P, TEAM T
                WHERE P.TEAM_ID = T.TEAM_ID
                AND P.POSITION = 'MF'
                ORDER BY PLAYER_NAME))
  WHERE ROWNUM >= 12
  AND RNUM < (12 + 11);
  ```
