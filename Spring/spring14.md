# Spring 14일차

## **이론**

- `게시판 난이도`

  1. 게시판

  2. 페이징: 블럭의 위치 포함

  3. 검색

  4. 파일(자료실): 업로드, 다운 받기

<br />

- 게시판 필요 기능

  - 페이지네이션: 자유게시판에 추가된 기능

  - 검색: 회원 리스트 화면에 추가해야 하는 기능

    > form 태그를 사용해 검색 기능 생성

<br />

- mybatis mapper xml

  - 변수 사용법

  ```
  - #{}: ''이 default로 붙음, value를 사용하기 위해 작성

  - ${}: 칼럼, 테이블을 변수로 사용하기 위해 작성
  ```

<br />

## **코드**

> view -> controller -> service -> dao -> mapper 순서로 구현(기존 순서와 반대)

- MemberListView.jsp

  ```html
  <!-- 검색 -->
  <div>
    <form id="pagingForm" action="./list" method="get">
      <select name="searchOption">
        <c:choose>
          <c:when test="${searchMap.searchOption == 'all'}">
            <option value="all" selected="selected">이름 + 이메일</option>
            <option value="memberName">이름</option>
            <option value="email">이메일</option>
          </c:when>

          <c:when test="${searchMap.searchOption == 'memberName'}">
            <option value="all">이름 + 이메일</option>
            <option value="memberName" selected="selected">이름</option>
            <option value="email">이메일</option>
          </c:when>

          <c:when test="${searchMap.searchOption == 'email'}">
            <option value="all">이름 + 이메일</option>
            <option value="memberName">이름</option>
            <option value="email" selected="selected">이메일</option>
          </c:when>
        </c:choose>
      </select>
      <input
        name="keyword"
        value="${searchMap.keyword}"
        placeholder="회원명 or 이메일 검색"
      />
      <input type="submit" value="search" />
    </form>
  </div>
  ```

<br />

- MemberController.class

  ```java
  @GetMapping("/list")
  public String getMemberList(@RequestParam(defaultValue = "all") String searchOption
      , @RequestParam(defaultValue = "") String keyword, Model model) {
    log.info(logTitleMsg);
    log.info("@GetMapping getMemberList searchOption: {}, keyword: {}", searchOption, keyword);

    // service
    List<MemberVo> memberList = memberService.memberSelectList(searchOption, keyword);

    Map<String, Object> searchMap = new HashMap<>();

    // 사용자가 검색한 단어를 유지하기 위해 사용
    searchMap.put("searchOption", searchOption);
    searchMap.put("keyword", keyword);

    model.addAttribute("memberList", memberList);
    model.addAttribute("searchMap", searchMap);

    return "member/MemberListView";
  }
  ```

<br />

- MemberService.class

  - interface를 implements

  ```java
  @Override
  public List<MemberVo> memberSelectList(String searchOption, String keyword) {
    Map<String, Object> map = new HashMap<>();

    map.put("searchOption", searchOption);
    map.put("keyword", keyword);

    return memberDao.memberSelectList(map);
  }
  ```

<br />

- MemberDao.class

  - interface를 implements

  ```java
  @Override
  public List<MemberVo> memberSelectList(Map<String, Object> map) {
    // xml 파일의 쿼리문을 가져옴 => id로 접근
    return sqlSession.selectList(namespace + "memberSelectList", map);
  }
  ```

<br />

- MemberMapper.xml

  ```xml
  <!-- mybatis에서 제공하는 공통단을 위한 일종의 함수 -->
  <sql id="search">
    <choose>
      <when test="searchOption == 'all'">
        MEMBER_NAME LIKE '%' || #{keyword} || '%'
        OR EMAIL LIKE '%' || #{keyword} || '%'
      </when>
      <when test="searchOption == 'memberName'">
        MEMBER_NAME LIKE '%' || #{keyword} || '%'
      </when>
      <otherwise>
        EMAIL LIKE '%' || #{keyword} || '%'
      </otherwise>
    </choose>
  </sql>

  <select id="memberSelectList" parameterType="map" resultMap="memberResultMap">
    SELECT MEMBER_NO, EMAIL, MEMBER_NAME, CRE_DATE
    FROM MEMBER
    <where> <!-- include 안에 아무런 값이 없으면 WHERE절 삭제, 있으면 WHERE절화 시켜줌(mybatis 제공) -->
      <include refid="search"></include>
    </where>
    ORDER BY MEMBER_NO DESC
  </select>
  ```
