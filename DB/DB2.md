# 2일차

## Database 2일차

- 오라클 전용 SQL 연산자(교재 45p)

  - between a and b

  - in(list)

  - LIKE '비교문자열': 비교 문자열과 형태가 일치하면 됨

    > LIKE 연산자는 검색하려는 값을 정확히 모를 경우에도 검색할 수 있도록 와일드카드와 함께 사용하여 원하는 내용을 검색하도록 함

    - 형식: column_name LIKE pattern

    - %: 문자가 없거나, 하나 이상의 문자에 어떤 값이 와도 상관없음

    - \_: 하나의 문자에 어떤 값이 와도 상관없음

  - is null

    ```sql
    SELECT *
    FROM EMP
    WHERE SAL BETWEEN 2000 AND 3000;

    SELECT * FROM EMP;

    SELECT *
    FROM EMP
    WHERE COMM IN(300, 500, 1400);
    ```

<br />

- 오라클 연산자 문제 풀이

  - 문제 1

    - 급여가 2000 미만이거나 3000 초과인 사원 검색

    - 사원명 직업 급여

    ```sql
    SELECT ename 사원명, job 직업, sal 급여
    FROM emp
    WHERE sal NOT BETWEEN 2000 AND 3000;
    ```

  <br />

  - 문제 2

    - 1982년에 입사한 사원을 출력하시오

    - 사원명 입사날짜 직업 급여

    ```sql
    SELECT ename 사원명, TO_CHAR(hiredate) 입사날짜, job 직업, sal 급여
    FROM emp
    WHERE TO_CHAR(hitedate) LIKE '82%';
    ```

  <br />

  - 문제 3

    - 7521이거나 7654이거나 7844 사원 번호인 사원 검색

    - 사원번호 사원명 직업 급여

    ```sql
    SELECT empno 사원번호, ename 사원명, job 직업, sal 급여
    FROM emp
    WHERE empno IN(7521, 7654, 7844);
    ```

  <br />

  - 문제 4

    - 매니저, 판매원이 아닌 사원들 검색

    - 사원명 직업 급여

    ```sql
    SELECT ename 사원명, job 직업, sal 급여
    FROM emp
    WHERE job NOt IN('MANAGER', 'SALESMAN');
    ```

  <br />

  - 문제 5

    - 사원 중에서 이름이 J로 시작하는 사원만 찾기

    - 사원명 직업 급여

    ```sql
    SELECT ename 사원명, job 직업, sal 급여
    FROM emp
    WHERE ename Lie 'J%';
    ```

  <br />

  - 문제 6

    - 사원 중에서 이름에 N이 포함되는 사람만 찾기

    - 사원명 직업 급여

    ```sql
    SELECT ename 사원명, job 직업, sal 급여, deptno 부서번호
    FROM emp
    WHERE ename LIKE '%N%';
    ```

  <br />

  - 문제 7

    - 이름이 "ER"로 끝나는 직원들을 검색

    - 사원명 직업 급여 부서번호

    ```sql
    SELECT ename 사원명, job 직업, sal 급여, deptno 부서번호
    FROM emp
    WHERE ename LIKE '%ER';
    ```

  <br />

  - 문제 8

    - 다섯 자리 이름 중 셋째 자리가 A인 사원만 검색

    - 사원번호 사원명 직업

    ```sql
    SELECT empno 사원번호, ename 사원명, job 직업
    FROM emp
    WHERE ename LIKE '__A__';
    ```

  <br />

  - 문제 9

    - MAN으로 끝나는 직군을 제외하고 검색

    - 사원번호 사원명 직업

    ```sql
    SELECT empno 사원번호, ename 사원명, job 직업
    FROM emp
    WHERE job NOT LIKE '%MAN';
    ```

    <br />

  - 문제 10

    - 다섯 자리 이름만 검색

    - 사원번호 사원명 직업

    ```sql
    SELECT empno 사원번호, ename 사원명, job 직업
    FROM emp
    WHERE job NOT LIKE '_____';
    ```

  <br />

  - 문제 11

    - 이름에 M과 I를 둘 다 포함하고 있는 직원들 검색

    - 사원번호 사원명 부서번호

    ```sql
    SELECT empno 사원번호, ename 사원명, deptno 부서번호
    FROM emp
    WHERE (ename LIKE '%M%') AND (ename LIKE '%I%');
    ```

  <br />

  - 문제 12

    - 사원들 중에서 이름 3번째 글자가 I인 사람만 찾기

    - 사원번호 사원명

    ```sql
    SELECT empno 사원번호, ename 사원명
    FROM emp
    WHERE ename LIKE '__I%'';
    ```

  <br />

  > "문제 13 ~ 15는 **AND, OR** 만 사용"

  <br />

  - 문제 13

    - 사원 테이블에서 job이 manager이면서 20번 부서에 속하거나, job이 clerk이면서 30번 부서에 속하는 사원의 정보 출력

    - ename, job, deptno

    ```sql
    SELECT ename, job, deptno
    FROM emp
    WHERE (job = 'MANAGER' AND deptno = 20)
      OR (job = 'CLERK' AND deptno = 30);
    ```

  <br />

  - 문제 14

    - 소속이 삼성 블루윙즈거나 전남 드래곤즈인 선수 중 포지션이 미드필더인 선수 구함

    - 선수명, 팀아이디, 포지션, 백넘버, 키

    ```sql
    SELECT player_name 선수명
      , team_id 팀아이디
      , position 포지션
      , back_no 백넘버
      , height 키
    FROM player
    WHERE (team_id = 'K02' OR team_id = 'K07')
      AND position = 'MF';
    ```

  <br />

  - 문제 15

    - 소속이 삼성 블루윙즈거나 전남 드래곤즈이고, 포지션이 미드필더야 하며, 키는 170cm 이상 180cm 이하인 선수들 조회

    - 선수번호, 선수명, 팀아이디, 포지션, 키

    ```sql
    SELECT player_id 선수명
      , player_name 선수명
      , team_id 팀아이디
      , position 포지션
      , height 키
    FROM player
    WHERE (team_id = 'K02' OR team_id = 'K07')
      AND (position = 'MF')
      AND (height >= 170 OR height <= 180);
    ```

  <br />

  - 문제 16

    - 국적(nation) 칼럼이 null이 아닌 선수와 국적 표시

    - 선수명 국적

    ```sql
    SELECT player_name 선수명
      , nation 국적
    FROM player
    WHERE nation IS NOT NULL;
    ```

<br />

- ORDER절

  - 정렬을 위한 ORDER절

  - 표현식

    - ASC(어센딩): 조회한 데이터를 오름차순으로 정렬(기본값)

    - DESC(디센딩): 조회한 데이터를 내림차순으로 정렬

  ```sql
  SELECT [all/distinct] 보고 싶은 칼럼명1, 칼럼명2, ,,,
  FROM 해당 칼럼들이 있는 테이블명
  WHERE 조건식
  ORDER BY 정렬하고 싶은 기준 칼럼명 [ASC OR DESC];
  ```

  <br />

  - 정렬 방식

    | <b>  | 오름차순       | 내림차순              |
    | ---- | -------------- | --------------------- |
    | 문자 | 사전 순서      | 사전 반대 순서로 정렬 |
    | 날짜 | 빠른 날짜 순서 | 늦은 날짜 순서로 정렬 |
    | NULL | 가장 마지막    | 가장 먼저             |

  <br />

  - ORDER절 예시

  ```sql
  SELECT *
  FROM EMP
  ORDER BY SAL ASC;

  SELECT *
  FROM EMP
  ORDER BY HIREDATE;

  SELECT *
  FROM EMP
  WHERE JOB = 'MANAGER'
  ORDER BY SAL DESC;
  ```

<br />

- ORDER절 문제 풀이

  - 부서 번호가 빠른 사원부터 출력하되 같은 부서 내의 사원을 출력할 경우, 최근 입사한 사원부터 출력

  - 사원 번호, 입사일, 사원명, 급여

  ```sql
  SELECT EMPNO "사원 번호", HIREDATE 입사일
    , ENAME 사원명, SAL 급여
  FROM EMP
  ORDER BY DETPNO, HIREDATE DESC;
  ```

  <br />

  - 부서 테이블에서 부서명, 지역, 부서 번호 순으로 내림차순 정렬

  - DNAME, AREA, DEPTNO

  ```sql
  SELECT DNAME, LOC AREA, DEPTNO
  FROM DEPT
  ORDER BY DNAME DESC, AREA DESC, DEPTNO DESC;
  ```

<br />

- DUAL TABLE(듀얼 테이블)

  - 오라클에서만 사용 가능한 유용한 테이블(일종의 가상 테이블)

  - oracle은 SELECT 절과 FROM 절 두 개의 절을 SELECT 문장의 필수 절로 지정, 사용자 테이블이 필요 없는 SQL 문장의 경우에도 필수적으로 DUAL이라는 테이블을 FROM 절에 지정함

  <br />

  - DUAL TABLE(듀얼 테이블) 특성

  - 사용자가 SYS가 소유하며 모든 사용자가 액세스할 수 있는 테이블

  - SELECT ~ FROM ~ 의 형식을 갖추기 위한 일종의 DUMMY 테이블

  - DUMMY라는 문자열 유형의 칼럼에 'X'라는 값이 들어있는 행을 1건 포함하고 있음

  ```sql
  -- 잘못된 사용법
  SELECT 100 * 10
  FROM EMP;

  -- 옳은 사용법
  SELECT 100 * 10
  FROM DUAL;
  ```

<br />

- 함수(function)

  - 함수명(칼럼이나 표현식, args1, arg2, ,,,)

  - 단일행 함수 특징

    1. SELECT, WHERE, ORDER BY 절에 사용 가능

    2. 각 행(ROW)들에 대해 개별적으로 작용하여 데이터값들을 조작, 각각의 행에 대한 조작 결과 리턴

    3. 여러 인자(Arguments)를 입력해도 단 하나의 결과만 리턴

    4. 함수의 인자로 상수, 변수, 표현식 사용 가능하고 하나의 인수를 가질 수지만 여러 개의 인수를 가질 수도 있음

    5. 특별한 경우가 아니면 함수의 인자로 함수를 사용하는 **함수의 중첩** 가능

  <br />

  - 문자 함수

    - LENGTH(): 길이 반환

  ```sql
    SELECT LENGTH('Oracle'), LENGTH('오라클')
    FROM DUAL;
  ```

  <br />

  - LENGTHb(): 입력된 문자열의 길이의 바이트 값 반환

  ```sql
  SELECT LENGTH('Oracle'), LENGTH('오라클') -- 6, 3
    , LENGTHb('Oracle'), LENGTHb('오라클') -- 6, 9
  FROM DUAL;
  ```

  <br />

  - SUBSTR(): 지정한 위치에서 지정한 수만큼 글자를 잘라 반환

    - 인덱스 1부터 시작

  ```sql
  SELECT SUBSTR('Welcom to Oracle', 4, 3) -- com
  FROM DUAL;

  SELECT SUBSTR('Welcom to Oracle', 1, 3) -- Wel
  FROM DUAL;

  SELECT SUBSTR('Welcom to Oracle', 0, 3) -- Wel
  FROM DUAL;
  ```

<br />

- 문자함수 문제 풀이

  - emp 테이블에서 hiredate를 1980년12월17일처럼 출력되도록 substr 함수 활용

  - 사원명, 직업, 입사일, 한국날짜화 입사일

  ```sql
  SELECT ENAME 사원명, JOB 직업, HIREDATE 입사일
    , (19 || SUBSTR(HIREDATE, 1, 2) || '년'
      || SUBSTR(HIREDATE, 4, 2) || '월'
      || SUBSTR(HIREDATE, 7, 2) || '일') AS "한국날짜화 입사일"
  FROM EMP
  ORDER BY HIREDATE;
  ```

  <br />

  - where ename like '%E'; 라는 SQL문을 substr 함수로 동일 결과 구현

  - 모든 사원 칼럼명

  ```sql
  SELECT ENAME, SUBSTR(ENAME, -1)
  FROM EMP;
  ```

  <br />

  - 80년도에 입사한 직원을 알아내기 위한 between 연산자 작성

  - 사원명, 직업, 입사일

  ```sql
  SELECT ENAME 사원명, JOB 직업, HIREDATE 입사일
  FROM EMP
  WHERE HIREDATE BETWEEN '80/01/01' AND '80/12/31';
  ```

  <br />

  - 80년도에 입사한 직원을 알아내기 위한 between 연산자 작성 코드를 substr 함수를 이용해서 구현

  - 사원명, 직업, 입사일

  ```sql
  SELECT ENAME 사원명, JOB 직업, HIREDATE 입사일
  FROM EMP
  WHERE SUBSTR(HIREDATE, 1, 2) = '80';
  ```

  <br />

  - 경기장의 지역번호와 전화번호와 전화번호의 길이를 조회

  - STADIUM_ID TEL T_LEN

  ```sql
  SELECT STADIUM_ID, DDD || TEL TEL
    , LENGTH(DDD || TEL) T_LEN
  FROM STADIUM
  ORDER BY STADIUM_ID;
  ```

<br />

- 날짜형 함수

  - 데이터베이스는 날짜를 숫자로 저장하기 때문에 덧셈, 뺄셈 같은 산술 연산자로도 계산 가능

  <br />

  - 단일행 날짜형 데이터 연산

  | 연산           | 결과   | 설명                                                |
  | -------------- | ------ | --------------------------------------------------- |
  | 날짜 + 숫자    | 날짜   | 숫자만큼의 날수를 날짜에 더함                       |
  | 날짜 - 숫자    | 날짜   | 숫자만큼의 날수를 날짜에서 뺌                       |
  | 날짜1 - 날짜2  | 날짜수 | 다른 하나의 날짜에서 하나의 날자를 빼면 일수가 나옴 |
  | 날짜 + 숫자/24 | 날짜   | 시간을 날자에 더함                                  |

  <br />

  - 예시

  ```sql
  SELECT SYSDATE -- 현재 날짜와 시각을 출력함
  FROM DUAL;

  SELECT SYSDATE - 1, SYSDATE, SYSDATE + 1
  FROM DUAL;

  SELECT EXTRACT(YEAR FROM HUREDATE), EXTRACT(MONTH FROM HUREDATE)
    , EXTRACT(DAY FROM HUREDATE)
  FROM EMP;
  ```

<br />

- 변환형 함수

  - 특정 데이터 타입을 다양한 형식으로 출력하고 싶은 경우 사용되는 함수

  <br />

  - 데이터 유형 변환의 종류

    | 종류                    | 설명                                                           |
    | ----------------------- | -------------------------------------------------------------- |
    | 명시적 데이터 유형 변환 | 데이터 변환형 함수로 데이터 유형을 변환하도록 명시해 주는 경우 |
    | 암시적 데이터 유형 변환 | 데이터베이스가 자동으로 데이터 유형을 변환하도록 계산하는 경우 |

    ```
    암시적(묵시적) 변환의 경우 성능 저하가 발생할 수 있으며,
    자동으로 DBMS가 알아서 계산하지 않는 경우가 있어 에러를 발생할 수 있으므로
    명시적인 변환 방법을 사용하는 것이 바람직
    ```

  <br />

  - 형 변환 함수의 종류

    - TO_NUBER : 문자형을 숫자형으로 변환

    - TO_CHARACTER : 날짜형 혹은 숫자형을 문자형으로 변환

    - TO_DATE : 문자형을 날짜형으로 변환
