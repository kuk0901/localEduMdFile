# Spring 5일차

## **`이론`**

- Mapper.xml

  - trim을 사용하는 경우

  ```
  <if test="email != ''">EMAIL = #{email},</if>
  <if test="memberName != ''">MEMBER_NAME = #{memberName},</if>
  어떤 경우든 ,가 발생하는 sql 구문의 경우 작성해야 함

  아래의 update와 같이 MOD_DATE = SYSDATE가 마지막에 있는 경우 trim 존재 시 오류 발생

  <update id="memberUpdateOne" parameterType="memberVo">
    UPDATE MEMBER
    <set>
      <if test="email != ''">EMAIL = #{email},</if>
      <if test="memberName != ''">MEMBER_NAME = #{memberName},</if>
    </set>
  </update>
  ```

<br />

- Fetch API

  - fetch()

  - 비동기 통신으로 요청(request)을 발행하고, 해당 응답(response)을 취득하는 함수

  - 비동기 통신이라는 방법으로, 서버상에 있는 원하는 데이터를 취득할 수 있다는 의미

  - `동기 통신`: 요청에 대한 응답을 받을 때까지 다음 처리를 하지 못하고 기다려야 하는 것

  - `비동기 통신`: 동기 통신과 다르게 요청을 발행하고 응답을 받을 때까지 다른 처리 가능

  ```
  * 비동기 통신 정보
  브라우저와 서버의 통신으로 데이터 취득을 하려면 요청과 응답을 주고받는 것이 발생하는데,
  통상적으로 이를 "동기 통신"이라 하는 것으로 행해짐

  ex) web 페이지의 표시나 이동/수정을 하면 화면이 순간 새하얗게 되고 나서 표시가 되지만,
      해당 상태에서는 사이트 조작 등이 불가능


  * "비동기 통신"의 대표적인 예: 검색의 자동 완성 기능, 서제스트 기능

  => 검색란에 검색하려는 키워드를 입력하면 관련 목록이 폼 아래에 자동으로 표시
  => 1자를 입력할 때마다 검색 후보를 서버에 요청 후 응답을 페이지에 반영
  => 응답을 하는 와중에도 문자를 입력하거나 다른 처리를 할 수 있음


  * 비동기 통신은 동기 통신처럼 대기시간이 없으며 계속해서 다른 처리를 할 수 있다는 점에서,
     사용자 편의성 개선으로 이어짐
  ```

  > 정리: 서버로 원하는 데이터 요청을 전송하면 response를 기다리면서 다른 처리도 할 수 있음

<br />

- ERD(Entity Relationship Diagram)

  - 개체들의 관계를 다이어그램 형식으로 표현한 것

  - 데이터베이스의 구조를 한눈에 파악할 수 있게 시각화한 것

  - https://www.erdcloud.com
