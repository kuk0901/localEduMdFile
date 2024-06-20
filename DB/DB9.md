# 9일차

## Database 9일차

- 식별자(Identifiers) 개념

  ```sql
  엔티티는 인스턴스들의 집합이라고 함
  여러 개의 집합체를 담고 있는 하나의 통에서 각각을 구분할 수 있는 논리적인 이름이 있어야 함
  이 구분자를 "식별자"라고 함

  - 식별자란?
  하나의 엔티티에 구성돼 있는 여러 개의 속성 중 엔티티를 대표할 수 있는 속성을 의미하며
  "하나의 엔티티는 반드시 하나의 유일한 식별자가 존재"해야 함

  * 보통 식별자와 키(Key)를 동일하게 생각하는 경우가 있으나 듦
  - 식별자: 업무적으로 구분이 되는 정보로 생각할 수 있으므로 논리 데이터 모델링 단계에서 사용
  - 키: 데이터베이스 테이블에 접근을 위한 매개체로서 물리 데이터 모델링 단계에서 사용
  ```

<br />

- 식별자의 특징

  > 주 식별자인지 비 식별자인지 등에 따라 특성이 다소 차이 남

  - 주 식별자인 경우

    - 엔티티 내에 모든 인스턴스들이 유일하게 구분되어야 함

    - 주 식별자를 구성하는 속성의 수는 유일성을 만족하는 최소의 수가 되어야 함

    - 지정된 주 식별자의 값은 자주 변하지 않는 것이어야 함

    - 주 식별자가 지정 되면 반드시 값이 들어와야 함

    ```
    > 유일성: 주 식별자에 의해 엔티티 내에 모든 인스턴스들이 유일하게 구분함

    > 최소성: 주 식별자를 구성하는 속성의 수는 유일성을 만족하는 최소의 수가 되어야 함

    > 불변성: 주 식별자가 한 번 특정 엔티티에 지정되면 그 식별자의 값은 변하지 않아야 함

    > 존재성: 주 식별자가 지정 되면 반드시 데이터값이 존재(Null X)
    ```

  > 비 식별자인 경우 주 식별자랑 특징이 일치하지 않으며, 참조무결성 제약조건에 따른 특징을 가짐

<br />

- 주 식별자 도출 기준

  - 해당 업무에서 자주 이용되는 속성을 주 식별자로 지정

  - 명칭, 내역 등과 같이 이름으로 기술되는 것들은 가능하면 주 식별자로 지정하지 않음

  - 복합으로 주 식별자로 구성할 경우 너무 많은 속성이 포함되지 않도록 함

<br />

- 비 식별자 관계(Non-Identifiying Relationship)

  - 부모 엔티티로부터 속성을 받았지만, 자식 엔티티의 주 식별자로 사용하지 않고 일반적인 속성으로만 사용하는 경우 비 식별자 관계라고 하며 외부 속성을 생성함

  ```
  - 자식 엔티티에서 받은 속성이 반드시 필수가 아니어도 무방하기 때문에 부모 없는 자식이 생성될 수 있음

  => 엔티티별로 데이터의 생명주기를 다르게 관리할 경우임

  => ex) 부모 엔티티에 인스턴스가 자식의 엔티티와 관계를 가지고 있지만 자식만 남겨두고 먼저 소멸 될 수 있는 경우

  => 여러 개의 엔티티가 하나의 엔티티로 통합되어 표현되었는데 각각의 엔티티가 별도의 관계를 가질 때 이에 해당

  => 자식 엔티티에 주 식별자로 사용하여도 되지만 자식 엔티티에서 별도의 주 식별자를 생성하는 것이 더 유리하다고 판단될 때,
  비 식별자 관계에 의한 외부 식별자로 표현
  ```

<br />

- CREATE

  - CREATE TABLE: 테이블을 생성하는 문장

  - 표현식

    ```
    CREATE TABLE 테이블이름(
      칼럼명1 DATATYPE [DEFAULT 형식],
      칼럼명2 DATATYPE [DEFAULT 형식],
      칼럼명3 DATATYPE [DEFAULT 형식],
      등등
    )
    ```

<br />

- 테이블 생성 시 주의사항

  - 테이블 명은 객체를 의미할 수 있는 적절한 이름 사용, 가능한 단수형 권고

  - 테이블 명은 다른 테이블 명과 중복 X

  - 한 테이블 내에서는 칼럼 명이 중복되게 지정 불가

  - 데이블 이름을 지정하고 각 칼럼들은 괄호()로 묶어 지정

  - 각 칼럼들은 콤마(,)로 구분되고 테이블 생성문의 끝은 항상 세미콜론(;)

  - 칼럼에 대해서는 다른 테이블까지 고려하여 데이터베이스 내에서는 일관성 있게 사용하는 것이 좋음(데이터 표준화 관점)

  - 칼럼 뒤에 데이터 유형은 꼭 지정돼야 함

  - 테이블 명과 칼럼 명은 반드시 문자로 시작, 벤더별로 길이에 대한 한계 존재

  - 벤더에서 사전에 정의한 예약어 사용 불가

  - 문자 데이터 유형은 반드시 가질 수 있는 최대 길이를 표시해야 함

  - 칼럼에 대한 제약조건이 있으면 CONSTRAINT를 이용해 추가 가능

  - 명명 규칙

    ```
    1. 반드시 문자로 시작

    2. A-Z까지의 대소문자와 0-9까지의 숫자, 특수 기호는(_, $, # 만 가능)

    3. 오라클에서 사용되는 예약어나 다른 객체 명과 중복 불가

    4. 공백 불허용

    5. 테이블 생성 시 대/소문자 구분은 하지 않음(기본적으로 테이블, 칼럼 명은 대문자로 생성됨)
    ```

<br />

- 테이블 생성

  ```sql
  CREATE TABLE EMP01(
    EMPNO NUMBER(4),
    ENAME VARCHAR2(20),
    SAL NUMBER(7, 2)
  );

  SELECT *
  FROM EMP01;

  DESC EMP01;
  ```

<br />

- 테이블 생성 문제

  - DEPT 테이블을 분석해 DEPT01 테이블을 만듦

  ```sql
  DESC DEPT;

  CREATE TABLE DEPT01(
    DEPTNO NUMBER(2),
    DNAME VARCHAR2(14),
    LOC VARCHAR2(13)
  );

  DESC DEPT;

  DESC DEPT01;
  ```

<br />

- 서브 쿼리로 테이블 생성

  - AS 키워드를 사용해 서브 쿼리로 테이블 생성

  ```sql
  CREATE TABLE EMP02
  AS
  SELECT *
  FROM EMP;
  ```

  <br />

- 서브 쿼리로 테이블 생성 예시

  ```sql
  SELECT *
  FROM SALGRADE
  WHERE GRADE = 1;

  CREATE TABLE 대충
  AS
  SELECT *
  FROM SALGRADE
  WHERE GRADE = 1;

  SELECT *
  FROM 대충;

  DESC 대충;
  ```

<br />

- 서브 쿼리를 사용해 테이블 구조만 복사해 테이블 생성

  ```sql
  SELECT *
  FROM SALGRADE
  WHERE 0 = 1;

  CREATE TABLE 대충1
  AS
  SELECT *
  FROM SALGRADE
  WHERE 0 = 1;

  SELECT *
  FROM 대충1;

  DESC 대충1;
  ```

<br />

- 서브 쿼리를 사용한 테이블 생성 문제

  - TEAM 테이블을 이용해 TEAM2 테이블을 생성하되 내용을 모두 복제

  ```sql
  CREATE TABLE TEAM2
  AS
  SELECT *
  FROM TEAM;

  DESC TEAM2;

  SELECT *
  FROM TEAM2;
  ```

  <br />

  - EMP 테이블을 이용해 EMP03 테이블 생성하되 칼럼 명은 EMP03_SEQ, EMP03_NAME, EMP03_sal

  - 데이터는 급여가 1500 이하인 사원만 복제

  ```sql
  SELECT EMPNO, ENAME, SAL
  FROM EMP
  WHERE SAL <= 15000;

  CREATE TABLE EMP03(
    EMP03_SQL,
    EMP03_name,
    EMP03_sal
  )
  AS
  SELECT EMPNO, ENAME, SAL
  FROM EMP
  WHERE SAL <= 15000;

  CREATE TABLE EMP03
  AS
  SELECT EMPNO EMP03_SQL
    , ENAME EMP03_name
    , SAL EMP03_sal
  FROM EMP
  WHERE SAL <= 15000;
  ```

<br />

- DROP TABLE

  - 테이블을 삭제하는 문장

  - 불필요한 테이블을 삭제하는 명령어

  - 표현식

    ```
    DROP TABLE 테이블명 [CASCADE CONSTRAINT];

    -- 테이블의 모든 데이터 및 구조 삭제
    ```

    > CASCADE CONSTRAINT 옵션은 제약조건에 관련된 것들도 삭제한다는 것을 의미

<br />

- ALTER TABLE

  - 테이블 구조를 변경하는 명령어

  - 칼럼을 추가/삭제하거나 제약조건을 추가/삭제하는 작업 진행

  <br />

  1. ADD Column: 칼럼 추가

  ```
  ALTER TABLE 테이블명
  ADD 추가할칼럼명 데이터유형;
  ```

  > 추가된 칼럼은 무조건 마지막 위치에 생성, 칼럼의 위치는 지정 불가

<br />

- ALTER TABLE 문제 풀이

  - DEPT02 테이블을 DEPT에서 동일하게 만들고 난 후, 부서장(DMGR) 칼럼 추가

  - VARCHAR2 10BYTE 작성 가능

  ```sql
  CREATE TABLE DEPT02
  AS
  SELECT *
  FROM DEPT;

  ALTER TABLE DEPT02
  ADD DMGR VARCHAER2(10);

  DESC DEPT02;

  ALTER TABLE DEPT02
  ADD DEPT02_EMP_LENGTH NUMBER(2);

  DESC DEPT02;

  ALTER TABLE DEPT02
  ADD DMGR VARCHAER2(10)
  ADD DMGR VARCHAER2(10);

  DESC DEPT02;
  ```

<br />

- 테이블에 칼럼을 여러 개 추가하는 법

  ```sql
  ALTER TABLE EMP01
  ADD (TEST1 VARCHAR2(4000), DEPTNO NUMBER(4));
  ```

  > 하나씩 추가하는 것이 컨트롤 더 쉬움

<br />

- DROP Column

  - 칼럼 삭제

  - 표현식

  ```
  ALTER TABLE 테이블명
  DROP COLUMN 삭제할칼럼명;
  ```

<br />

- DROP 예시

  ```sql
  ALTER TABLE EMP01
  DROP COLUMN TEST1;
  ```

<br />

- 기존의 칼럼 수정하기

  - 테이블에 존재하는 칼럼에 대해서 ALTER TABLE 명령을 이용해 칼럼의 데이터 유형, 디폴트 값, NOT NULL 제약조건에 대한 변경을 포함할 수 있음

  - 표현식

  ```
  ALTER TABLE 테이블명
  MODIFY (
    칼럼명1 데이터유형 [DEFAULT식] [NOT NULL],
    칼럼명2 데이터유형 [DEFAULT식] [NOT NULL],
    칼럼명3 데이터유형 [DEFAULT식] [NOT NULL],
  );
  ```

  <br />

- 칼럼 변경 시 주의사항

  1. 해당 칼럼의 크기를 늘릴 수 있지만 줄이지는 못함

  2. 해당 칼럼이 NULL 값만 가지고 있거나 테이블에 아무 행도 없으면 칼럼의 폭을 줄일 수 있음

  3. NULL 값만 가지고 있는 칼럼은 데이터 유형 변경 가능

  4. 해당 칼럼의 DEFAULT 값을 바꾸면 변경 작업 이후 발생하는 행 삽입에만 영향을 미침

  5. 해당 칼럼에 NULL 값이 없을 때만 NOT NULL 제약조건을 추가할 수 있음

<br />

- 데이터 딕셔너리

  - 개념

    - 자원을 효율적으로 관리하기 위한 다양한 정보를 저장하는 시스템 테이블

    - 사용자가 테이블을 생성하거나 사용자를 변경하는 등의 작업을 할 때 데이터베이스 서버에 의해 자동으로 갱신되는 테이블

    - 사용자는 데이터 딕셔너리의 내용을 직접 수정하거나 삭제할 수 없음

    ```
    > 데이터 딕셔너리 원 테이블을 직접 조회하는 것은 불가능

    => DBMS만 이해할 수 있기 때문

    > 사용자가 이해할 수 있는 데이터를 산출해 줄 수 있도록 데이터 딕셔너리에서 파생한 데이터 딕셔너리 뷰 제공

    > 데이터 딕셔너리 안에는 중요한 정보가 많이 있기 때문에 사용자는 이를 활용하기 위해
      데이터 딕셔너리 뷰를 자주 사용하게 됨
    ```

<br />

- 데이터 딕셔너리 뷰

  > 접두어에 따라 세 종류로 나뉨

  1. DBA_XXX: 데이터베이스 관리자만 접근이 가능한 객체 등의 정보 조회

  2. ALL_XXX: 자신의 계정이 소유하거나 권한을 부여받은 객체 등에 관한 정보 조회

  3. USER_XXX: 자신의 계정이 소유한 객체 등에 관한 정보 조회

<br />

- USER\_데이터 딕셔너리

  - 자신의 계정이 소유한 객체에 관한 정보 조회 가능

  - USER_SEQUENCES, USER_INDEXS, USER_VIEWS 등

<br />

- 데이터 딕셔너리 사용 예시

  ```sql
  SELECT *
  FROM USER_TABLES;

  SELECT *
  FROM USER_USERS;

  SELECT *
  FROM USER_CONSTRAINTS;
  ```

<br />

- DML

  - 테이블 내용 CRUD

  - 테이블 행을 추가하는 INSERT 문

    - EXPRESSION

    ```
    INSERT INTO 테이블명 (칼럼명 리스트)
    VALUES (칼럼명 리스트에 넣을 VALUE LIST);

    INSERT INTO 테이블명
    VALUES (전체 칼럼에 넣을 VALUE LIST);
    ```

<br />

- INSERT 문 문제 풀이

  - 서브 쿼리문을 이용해 sam01 테이블 생성

  - empno, ename, job, sal

  ```
  1000 APPLE police 10000
  1010 BANANA nurse 15000
  1020 orange DOCTOR 25000
  ```

  ```sql
  CREATE TABLE SAM01_TABLE
  AS SELECT EMPNO, ENAME, JOB, SAL
  FROM EMP
  WHERE 0 = 1;

  DESC SAM01_TEST;

  INSERT ALL
    INTO SAM01_TEST VALUES (1000, 'APPLE', 'police', 10000)
    INTO SAM01_TEST VALUES (1010, 'BANANA', 'nurse', 15000)
    INTO SAM01_TEST VALUES (1020, 'orange', 'DOCTOR', 25000)
  SELECT *
  FROM DUAL;

  SELECT *
  FROM SAM01_TEST;
  ```
