# 1일차

## DataBase 1일차

- 데이터베이스(DATABASE)

  - 넓은 의미에서의 데이터베이스: 일상적인 정보들을 모아놓은 것

  - 기업에서의 데이터베이스: 직원들을 관리하기 위해 직원들의 이름, 부서, 월급 등의 정보를 모아둔 것

  > 이러한 정보들을 관리하기 위해 엑셀과 같은 소프트웨어를 이용하여 저장 및 관리

  ```
  위의 방식에서 사소한 부주의로 중요한 데이터가 손상되거나 유실되면 안 되기에
  이러한 것들을 효율적으로 관리하고 손상을 피하며, 필요시 데이터를 복구하는 등의 강력한 소프트웨어가 필요했고
  그것이 DBMS(Database Management System)임
  ```

<br />

- RDBMS(Relational DBMS)

  - 수많은 데이터베이스 종류 중 기업에서 대부분 사용하고 있는 관계형 데이터베이스를 배움

  - 수학의 집합 논리에 입각한 것이므로, SQL도 데이터를 집합으로써 취급

<br />

- SQL(Structured Query Language)

  - SQL언어라고 부르며 관계형 데이터베이스에서 데이터 정의, 조작, 제어를 하기 위해 사용하는 언어

  - SQL 문장은 단순 스크립트가 아니라 이름에도 포함되어 있듯, 일반적인 개발 언어처럼 독립된 하나의 개발 언어

  - 일반적인 프로그래밍 언어와는 달리 SQL은 관계형 데이터베이스에 대한 전담 접속 용도로 사용되며 세미콜론(;)으로 분리되어 있는 SQL 문장 단위로 독립되어 있음

<br />

- SQL 문장들의 종류

  - 데이터 조작어(DML: Data Manipulation Language)

    - DB의 테이블에 들어있는 데이터에 변형을 가하는 종류의 명령어들

    1. SELECT: 데이터 조회, 검색 => Retrieve라고도 함

    2. INSERT: 테이블에 새로운 행을 집어넣음

    3. UPDATE: 데이터를 수정

    4. DELETE: 원하지 않는 데이터 삭제

  <br />

  - 데이터 정의어(DDL: Data Definition Language)

    - 테이블과 같은 데이터 구조를 정의하는 데 사용되는 명령어들(구조와 관련된 명령어들)

    1. CREATE: 테이블 구조 생성

    2. ALTER: 변경

    3. DROP: 삭제

    4. RENAME: 이름을 바꿈

  <br />

  - 데이터 제어어(DCL: Data Control Language)

    - 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어

    1. GRANT

    2. REVOKE

  <br />

  - 트랜잭션 제어어(TCL: Transaction Control Language)

    - 논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 작업 단위(트랜잭션) 별로 제어하는 명령어

    1. COMMIT

    2. ROLLBACK

<br />

- TABLE(테이블) 용어

  - 테이블: 행과 칼럼의 2차원 구조를 가진 데이터의 저장 장소, 데이터베이스의 가장 기본적인 개념

  - 칼럼(column) / 열: 2차원 구조를 가진 테이블에서 세로 방향으로 이루어진 특정 속성(더 이상 나눌 수 없는 특성)

  - 로우(row) / 행: 2차원 구조를 가진 테이블에서 가로 방향으로 이루어진 연결된 데이터

<br />

- 데이터 유형

  - CHARACTER(길이)

    - 고정 길이 문자열 정보(char)

    - 길이는 기본 길이 1바이트, 최대 길이 2,000 바이트

    - 길이만큼 최대 길이를 갖고 고정 길이를 가지고 있으므로 할당된 변수 값의 길이가 길이보다 작을 경우 그 차이 길이만큼 공간으로 채워짐

  <br />

  - varchar(s)

    - Character varying의 양자로 가변 길이 문자열 정보(varchar2로 표현)

    - s는 최소 길이 1바이트, 최대 길이 4,000 바이트

    - s만큼 최대 길이를 갖지만 가변 길이로 조정이 되기 때문에 할당된 변수 값의 바이트만 적용

  <br />

  - numeric

    - 정수, 실수 등 숫자 정보(number로 표현)

    - 처음에 전체 자릿수를 지정하고, 그다음 소수 부분의 자릿수를 지정

    ```
    ex) 정수 부분이 6자리이고 소수점 부분이 2자리인 경우 number(8, 2)와 같이 됨

      - number(6) -> 123456

      - number(8, 2) -> 123456.78
    ```

  <br />

  - datetime

    - 날짜와 시간 정보(date)로 표현

    - 1초 단위 관리

  <br />

  - NULL

    - 아직 정의되지 않은 값으로 0 또는 공백과 다름(공백은 하나의 문자)

    - 테이블을 생성할 때 NOT NULL 또는 PRIMARY KEY로 정의되지 않은 모든 데이터 유형은 NULL 값 포함 가능

    - NULL 값을 포함하는 연산의 경우 결과 값도 NULL

    - 모르는 데이터 숫자를 더하거나 빼도 결과는 마찬가지로 모르는 데이터인 것과 같음

    - NULL 값의 대상이 숫자 유형 데이터인 경우 주로 0(ZERO)

    - NULL 값의 대상이 문자 유형 데이터인 경우 블랭크보다는 'X' 같이 해당 시스템에서 의미 없는 문자로 바꾸는 것도 고려할 필요 있음

<br />

- 계정에 대한 기본 설명

  | 계정명     | 설명                                                                                                                                                                              |
  | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | **SYS**    | 오라클 Super 사용자 계정, 데이터베이스에서 발생하는 모든 문제들을 처리할 수 있는 권한을 가짐                                                                                      |
  | **SYSTEM** | 오라클 데이터베이스를 유지보수 및 관리할 때 사용하는 사용자 계정 <br /> sys 사용자와는 다르게 데이터베이스를 생성할 수 있는 권한이 없으며, 불완전 복구를 할 수 없다는 차이점 있음 |
  | SCOTT      | 처음 오라클을 사용하는 사용자의 실습을 위해 만들어 놓은 연습용 계정                                                                                                               |
  | HR         | 오라클에 접근할 수 있도록 샘플로 만들어 놓은 사용자 계정                                                                                                                          |

  > sys, system은 dba 권한을 가진 사용자 계정

<br />

- 로그인 실패 중 계정 잠김 또는 비밀번호 변경해야 하는 경우

  - dba가 일반 유저의 비밀번호를 변경해 줌

    - ALTER USER 계졍명 identified by 비밀번호 account unlock;

  <br />

  - dba급 계정 비밀번호를 모르는 경우

    - sqlplus system/아무글자입력(띄어쓰기X) as sysdba

<br />

- SELECT

  - 사용자가 입력한 데이터 언제든 조회 가능

  - 조회하기를 원하는 칼럼명을 select 다음에 콤마 구분자(,)로 구분하여 나열하고 from 다음에 해당 칼럼이 존재하는 테이블명 입력해 실행

  <br />

  - 표현식

    ```sql
    select [all/distinct] 보고 싶은 칼럼명1, 칼럼명2, ...
    from 해당 칼럼들이 있는 테이블명;
    ```

    - 기본값: all

    - distinct: 중복된 데이터가 있는 경우 1건으로 처리해 출력

  <br />

  - 데이터 딕셔너리 tab

    - 계정에 존재하는 테이블을 조회하는 방법

    - 테이블의 정보를 알려주는 데이터 딕셔너리: tab

    - 데이터 딕셔너리: 데이터베이스와 관련된 정보 제공

  <br />

  - 테이블 구조를 살펴보기 위한 desc[ribe]

    - desc 테이블명

<br />

- DB 코드 구조 파악

  - 런타임

  ```sql
  SELECT ENAME, SAL, SAL * 2
  FROM EMP;

  -- 1. FROM절 실행: TABLE 생성

  -- 2. SELECT절 실행
  ```

<br />

- 산술연산자 문제 풀이 1

  - 선수테이블에서 키 - 몸무게 조회

  ```sql
  SELECT PLAYER_NAME, HEIGHT - WEIGHT "키 - 몸무게"
  FROM PLAYER;
  ```

  <br />

  - 모든 사원의 연봉 구함

  ```sql
  SELECT ENAME "사원", SAL "월급", JOB "직업",
    COMM "커미션", SAL * 12 "연봉",
    (SAL * 12) + COMM "연봉 + 커미션"
  FROM EMP;
  ```

<br />

- Alias(별칭) 부여하기

  - 조회된 결과에 일종의 별명(Alias)을 부여해서 칼럼 레이블 변경 가능

  - 별명에서 지켜야 할 사항

    - 칼럼명 바로 뒤에 옴

    - 칼럼명과 alias 사이에 as, **AS** 키워드를 사용할 수 있음

    - 이중 인용부호는 alias가 공백, 특수문자를 포함할 경우와 대소문자 구분이 필요할 경우 사용됨

<br />

- 산술연산자 문제 풀이 2

  - 선수들의 이름, 키, 몸무게, BMI 비만지수

  ```sql
  SELECT PLAYER_NAME 이름
    , HEIGHT 키
    , WEIGHT 몸무게
    , WEIGHT / ((HEIGHT * 0.01) * (HEIGHT * 0.01)) "BMI 비만지수"
  FROM PLAYER;
  ```

<br />

- CONCATENATION 연산자(||)

  - 결합 연산자

  - 자바에서 배운 + 연산자의 특징과 동일

  - 무조건 문자열로 변환해서 이어줌

<br />

- CONCATENATION 연산자 문제 풀이

  - 선수 체격 정보

  - 박지성 선수, 176 cm, 79kg

  ```sql
  SELECT PLAYER_NAME || ' 선수, ' || HEIGHT || ' cm, ' || WEIGHT || 'kg' "선수 체격 정보"
  FROM PLAYER;
  ```

<br />

- WHERE(조건절)

  - 자바의 if문과 비슷

  - 표현식

  ```sql
  SELECT [all/distinct] 보고 싶은 칼럼명1, 칼럼명2, ,,,
  FROM 해당 칼럼들이 있는 테이블명
  WHERE 조건식;
  ```

  <br />

  - WHERE 절은 FROM 절 다음에 위치, 조건식은 아래 내용으로 구성

    - 칼럼명(보통 조건식의 좌측에 위치)

    - 비교 연산자

    - 문자, 숫자, 표현식(보통 조건식의 우측에 위치)

    - 비교 칼럼명(JOIN 사용 시)

    ```sql
    SELECT empno, ename, sal
    FROM emp
    WHERE sal >= 3000;
    ```

<br />

- WHERE 절 문제 풀이

  - 급여가 1500 이하인 사원의 사원번호, 사원명, 급여를 조회하는 sql문

  ```sql
  SELECT EMPNO 사원번호, ENAME 사원명, SAL 급여
  FROM EMP
  WHERE SAL <= 1500;
  ```

  <br />

  - 이름이 FORD인 사원의 사원 번호, 부서 번호, 직업 조회

  ```sql
  SELECT EMPNO "사원 번호", DEPTNO "부서 번호", JOB 직업
  FROM EMP
  WHERE ENAME = 'FORD';
  ```

  <br />

  - 포지션이 미드필더(MF)인 선수 조회

    - 선수명, 포지션, 백넘버, 키

  ```sql
  SELECT PLAYER_NAME 선수명
    , POSITION 포지션
    , BACK_NO 백넘버
    , HEIGHT 키
  FROM PLAYER
  WHERE POSITION = 'MF';
  ```

  <br />

  - 소속팀이 삼성블루윙즈라는 팀에 속한 선수들을 조회

    - 선수명, 팀번호, 포지션, 백넘버, 키

  ```sql
  -- 팀 이름과 팀 번호를 확인하기 위해 사용
  desc TEAM;
  SELECT * FROM TEAM;

  SELECT PLAYER_NAME 선수명
    , TEAM_ID 팀번호
    , POSITION 포지션
    , BACK_NO 백넘버
    , HEIGHT 키
  FROM PLAYER
  WHERE TEAM_ID = 'K02';
  ```
