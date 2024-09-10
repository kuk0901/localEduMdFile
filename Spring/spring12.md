# Spring 12일차

## 이론

- **`파일 업로드`**

  - json은 file 처리 불가능, `form 태그만 file 전송 가능`

  - FormData: form을 쉽게 보내도록 도와주는 객체, FormData 객체는 HTML form 데이터를 나타냄

    - fetch 등의 네트워크 메서드가 FormData 객체를 body로 받는다는 건 FormData의 특징

    - 이때 브라우저가 보내는 HTTP 메시지는 인코딩 되고 Content-Type 속성은 multipart/form-data로 지정된 후 전송됨

    > input 태그의 multipart="multipart" 속성과 값 필요

  <br />

  - 필요한 데이터인데 예측이 불가능한 데이터를 가져옴

    - 트랜잭션: CRUD

    - mybatis

      - useGeneratedKeys: PK로 사용하겠다는 의미

      - selectKey tag: id를 생성 및 반환

      - insert.keyProperty: return 타입 => parameterType의 객체에 자동으로 set

      ```xml
      <!-- keyProperty의 return type을 parameterType의 VO 객체에 바로 set -->
      <insert id="freeBoardInsertOne" parameterType="com.edu.freeBoard.domain.FreeBoardVo" useGeneratedKeys="true" keyProperty="freeBoardId">
        <!-- useGeneratedKeys 이후 실행, insert 구문에서 keyProperty는 return 타입을 의미  -->
        <selectKey keyProperty="freeBoardId" resultType="int" order="BEFORE">
          SELECT FREE_BOARD_FILE_ID_SEQ.NEXTVAL
          FROM DUAL;
        </selectKey>

        INSERT INTO FREE_BOARD
        VALUE(FREE_BOARD_ID, MEMBER_NO, FREE_BOARD_TITLE, FREE_BOARD_CONTENT, CREATE_DATE, UPDATE_DATE)
        VALUES(#{freeBoardId}, #{memberNo}, #{freeBoardTitle}, #{freeBoardContent}, SYSDATE, SYSDATE)
      </insert>
      ```

  <br />

## **코드**

- FreeBoardController.class

```java
package com.edu.freeBoard.controller;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PatchMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartHttpServletRequest;
import org.springframework.web.servlet.ModelAndView;

import com.edu.freeBoard.domain.FreeBoardVo;
import com.edu.freeBoard.service.FreeBoardService;
import com.edu.util.Paging;

@RestController
@RequestMapping("/freeBoard")
public class FreeBoardController {

  private Logger log = LoggerFactory.getLogger(FreeBoardController.class);
  private final String logTitleMsg = "==FreeBoardController==";

  @Autowired
  private FreeBoardService freeBoardService;

  @RequestMapping(value = "/list", method = { RequestMethod.GET, RequestMethod.POST })
  public ModelAndView freeBoardList(@RequestParam(defaultValue = "1") int curPage) {
    log.info(logTitleMsg);
    log.info("@RequestMapping freeBoardList curPage: {}", curPage);

    int totalCount = freeBoardService.freeBoardSelectTotalCount();

    Paging pagingVo = new Paging(totalCount, curPage);
    int start = pagingVo.getPageBegin();
    int end = pagingVo.getPageEnd();

    List<FreeBoardVo> freeBoardList = freeBoardService.freeBoardSelectList(start, end);

    Map<String, Object> pagingMap = new HashMap<>();
    pagingMap.put("totalCount", totalCount);
    pagingMap.put("pagingVo", pagingVo);

    ModelAndView mav = new ModelAndView("freeBoard/FreeBoardListView");
    mav.addObject("freeBoardList", freeBoardList);
    mav.addObject("pagingMap", pagingMap);

    return mav;
  }

  @PostMapping("/")
  // @ModelAttribute: form tag를 받기 위한 annotation
  public ResponseEntity<Map<String, String>> freeBoardInsertCtr(@ModelAttribute FreeBoardVo freeBoardVo,
      MultipartHttpServletRequest mhr) {
    log.info(logTitleMsg);
    log.info("@PostMapping freeBoardInsertCtr freeBoardVo: {}", freeBoardVo);

    try {
      freeBoardService.freeBoardInsertOne(freeBoardVo, mhr);
    } catch (Exception e) {
      log.info("자유게시판 추가에서 문제 발생. 원래는 화면 처리");
      e.printStackTrace();
    }

    Map<String, String> jsonMap = new HashMap<String, String>();
    jsonMap.put("result", "success");

    return ResponseEntity.ok(jsonMap);
  }

  // 게시판 수정 화면으로 이동
  @GetMapping("/{freeBoardId}") // 비동기 -> json
  public ResponseEntity<Map<String, Object>> freeBoardUpdate(@PathVariable int freeBoardId) {
    log.info(logTitleMsg);
    log.info("@GetMapping freeBoardUpdate freeBoardId: {}", freeBoardId);

    Map<String, Object> resultMap = freeBoardService.freeBoardSelectOne(freeBoardId);

    return ResponseEntity.ok(resultMap);
  }

  // 게시판 수정 DB
  @PatchMapping("/{freeBoardId}") // 비동기 -> json
  public ResponseEntity<?> freeBoardUpdateCtr(@PathVariable int freeBoardId, @RequestBody FreeBoardVo freeBoardVo) {
    log.info(logTitleMsg);
    log.info("@PostMapping freeBoardUpdateCtr freeBoardId: {}, freeBoardVo: {}", freeBoardId, freeBoardVo);

    if (freeBoardVo.getMemberNo() == 0) {
      Map<String, String> errorResMap = new HashMap<>();
      errorResMap.put("errorMsg", "게시판 등록자가 아닙니다.");

      return ResponseEntity.status(HttpStatus.BAD_REQUEST).contentType(MediaType.APPLICATION_JSON).body(errorResMap);
    }

    System.out.println("????????????????check????????? " + freeBoardVo.getMemberNo());

    freeBoardService.freeBoardUpdateOne(freeBoardVo);

    // FIXME: 추후 수정해야 함
    // freeBoardVo = freeBoardService.freeBoardSelectOne(freeBoardId);

    return ResponseEntity.ok(freeBoardVo);
  }
}
```

<br />

- FreeBoardDaoImp.class

```java
package com.edu.freeBoard.dao;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.edu.freeBoard.domain.FreeBoardVo;

@Repository
public class FreeBoardDaoImp implements FreeBoardDao {

  @Autowired
  private SqlSession sqlSession;

  String namespace = "com.edu.freeBoard.";

  @Override
  public List<FreeBoardVo> freeBoardSelectList(int start, int end) {

    Map<String, Object> map = new HashMap<>();
    map.put("start", start);
    map.put("end", end);

    return sqlSession.selectList(namespace + "freeBoardSelectList", map);
  }

  @Override
  public int freeBoardSelectTotalCount() {
    return sqlSession.selectOne(namespace + "freeBoardSelectTotalCount");
  }

  @Override
  public FreeBoardVo freeBoardSelectOne(int freeBoardId) {
    return sqlSession.selectOne(namespace + "freeBoardSelectOne", freeBoardId);
  }

  @Override
  public void freeBoardInsertOne(FreeBoardVo freeBoardVo) {
    sqlSession.insert(namespace + "freeBoardInsertOne", freeBoardVo);
  }

  @Override
  public void freeBoardUpdateOne(FreeBoardVo freeBoardVo) {
    sqlSession.update(namespace + "freeBoardUpdateOne", freeBoardVo);
  }

  @Override
  public void freeBoardFileInsertOne(Map<String, Object> map) {
    sqlSession.insert(namespace + "freeBoardFileInsertOne", map);
  }

  @Override
  public List<Map<String, Object>> freeBoardFileSelectList(int no) {
    return sqlSession.selectList(namespace + "freeBoardFileSelectList", no);
  }

}
```

<br />

- FreeBoardServiceImp.class

```java
package com.edu.freeBoard.service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartHttpServletRequest;

import com.edu.freeBoard.dao.FreeBoardDao;
import com.edu.freeBoard.domain.FreeBoardVo;
import com.edu.util.FileUtils;

@Service
public class FreeBoardServiceImp implements FreeBoardService {

  private static final Logger log = LoggerFactory.getLogger(FreeBoardServiceImp.class);

  @Autowired
  public FreeBoardDao freeBoardDao;

  @Autowired
  private FileUtils fileUtils;

  @Override
  public List<FreeBoardVo> freeBoardSelectList(int start, int end) {
    return freeBoardDao.freeBoardSelectList(start, end);
  }

  @Override
  public int freeBoardSelectTotalCount() {
    return freeBoardDao.freeBoardSelectTotalCount();
  }

  @Override
  public Map<String, Object> freeBoardSelectOne(int freeBoardId) {
    Map<String, Object> resultMap = new HashMap<>();

    FreeBoardVo freeBoardVo = freeBoardDao.freeBoardSelectOne(freeBoardId);
    resultMap.put("freeBoardVo", freeBoardVo);

    List<Map<String, Object>> freeBoardFileList = freeBoardDao.freeBoardFileSelectList(freeBoardId);
    resultMap.put("freeBoardFileList", freeBoardFileList);

    return resultMap;
  }

  @Override
  public void freeBoardInsertOne(FreeBoardVo freeBoardVo, MultipartHttpServletRequest mhr) throws Exception {
    freeBoardDao.freeBoardInsertOne(freeBoardVo);

    System.out.println("?????????: " + freeBoardVo.getFreeBoardId());
    List<Map<String, Object>> fileList = fileUtils.parseInsertFileInfo(freeBoardVo.getFreeBoardId(), mhr);

    for (int i = 0; i < fileList.size(); i++) {
      freeBoardDao.freeBoardFileInsertOne(fileList.get(i));
    }
  }

  @Override
  public void freeBoardUpdateOne(FreeBoardVo freeBoardVo) {
    freeBoardDao.freeBoardUpdateOne(freeBoardVo);
  }

}
```

<br />

> util package 부분

- CommonUtils.class

```java
package com.edu.util;

import java.util.UUID;

public class CommonUtils {

  public static String getRandomString() {
    return UUID.randomUUID().toString().replaceAll("-", "");
  }

}
```

<br />

- FileUtils.class

```java
package com.edu.util;

import java.io.File;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.multipart.MultipartHttpServletRequest;

@Component("fileUtils")
public class FileUtils {

  private static final String FILE_PATH = "C:\\upload";

  public List<Map<String, Object>> parseInsertFileInfo(int parentId, MultipartHttpServletRequest mhr) throws Exception {
    Iterator<String> iterator = mhr.getFileNames(); // iterator로만 저장되어 있음 주의
    MultipartFile multipartFile = null;
    String originalFileName = null;
    String originalFileExtension = null;
    String storedFileName = null;

    List<Map<String, Object>> fileList = new ArrayList<Map<String, Object>>();
    Map<String, Object> fileInfoMap = null; // 하나의 column

    File file = new File(FILE_PATH); // 경로 세팅

    // 경로에 폴더가 없는 경우 생성
    if (file.exists() == false) {
      file.mkdir();
    }

    // iterator.hasNext(): 원시 파일
    while (iterator.hasNext()) {
      multipartFile = mhr.getFile(iterator.next()); // 파일

      // 비어있지 않은 경우 파일을 list에 담음
      if (multipartFile.isEmpty() == false) {
        originalFileName = multipartFile.getOriginalFilename(); // 실제 파일명(확장자 포함)

        // server에서 사용을 위한 중복 제거용
        originalFileExtension = originalFileName.substring(originalFileName.lastIndexOf(".")); // 확장자 분리
        storedFileName = CommonUtils.getRandomString() + originalFileExtension;

        file = new File(FILE_PATH, storedFileName);
        multipartFile.transferTo(file); // 실제 파일 생성

        // DB 저장
        fileInfoMap = new HashMap<>();
        fileInfoMap.put("parentId", parentId); // PK
        fileInfoMap.put("originalFileName", originalFileName);
        fileInfoMap.put("storedFileName", storedFileName);
        fileInfoMap.put("fileSize", multipartFile.getSize());

        fileList.add(fileInfoMap);
      }

    }

    return fileList;
  }
}
```

<br />

- WebMvcConfig.class

```java
package com.edu.util;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

  @Override
  public void addResourceHandlers(ResourceHandlerRegistry registry) {
    // 경로 치환 => 화면에서 사용되는 경로가 자동으로 치환
    registry.addResourceHandler("/img/**").addResourceLocations("file:///C:upload/");

  }

}
```

<br />

- FreeBoardMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.edu.freeBoard">

	<resultMap type="freeBoardVo" id="freeBoardResultMap">
		<id column="FREE_BOARD_ID" property="freeBoardId" />
		<result column="MEMBER_NO" property="memberNo" />
		<result column="MEMBER_NAME" property="memberName" />
		<result column="FREE_BOARD_TITLE" property="freeBoardTitle" />
		<result column="FREE_BOARD_CONTENT" property="freeBoardContent" />
		<result column="CREATE_DATE" property="createDate" javaType="java.util.Date" />
		<result column="UPDATE_DATE" property="updateDate" javaType="java.util.Date" />
	</resultMap>

	<select id="freeBoardSelectList" parameterType="map" resultMap="freeBoardResultMap">
		<![CDATA[
			SELECT RNUM, FREE_BOARD.FREE_BOARD_ID, FREE_BOARD.MEMBER_NO
				, FREE_BOARD.MEMBER_NAME, FREE_BOARD.FREE_BOARD_TITLE
        , FREE_BOARD.FREE_BOARD_CONTENT
				, FREE_BOARD.CREATE_DATE, FREE_BOARD.UPDATE_DATE
			FROM ( SELECT ROWNUM RNUM, FB.FREE_BOARD_ID, FB.MEMBER_NO, FB.MEMBER_NAME,
						FB.FREE_BOARD_TITLE, FB.FREE_BOARD_CONTENT, FB.CREATE_DATE,
						FB.UPDATE_DATE
						FROM ( SELECT OFB.FREE_BOARD_ID, M.MEMBER_NO, M.MEMBER_NAME,
									OFB.FREE_BOARD_TITLE, OFB.FREE_BOARD_CONTENT, OFB.CREATE_DATE,
									OFB.UPDATE_DATE
									FROM MEMBER M, FREE_BOARD OFB
									WHERE M.MEMBER_NO = OFB.MEMBER_NO
									ORDER BY OFB.FREE_BOARD_ID DESC) FB
			) FREE_BOARD
			WHERE RNUM >= #{start} AND RNUM <= #{end}
	 ]]>
	</select>

	<select id="freeBoardSelectTotalCount" resultType="java.lang.Integer">
	  SELECT COUNT(*)
		FROM FREE_BOARD
	</select>

  <select id="freeBoardSelectOne" parameterType="int" resultMap="freeBoardResultMap">
      SELECT FB.FREE_BOARD_ID, FB.MEMBER_NO, M.EMAIL
          , M.MEMBER_NAME, FB.FREE_BOARD_TITLE, FB.FREE_BOARD_CONTENT
          , FB.CREATE_DATE
      FROM FREE_BOARD FB INNER JOIN MEMBER M
      ON FB.MEMBER_NO = M.MEMBER_NO
      WHERE FB.FREE_BOARD_ID = #{freeBoardId}
   </select>

   <select id="freeBoardFileSelectList" parameterType="int"
      resultType="map">
      SELECT FBF.FREE_BOARD_FILE_ID as "freeBoardFileId" <!-- 대소문자 구문을 위해 "" 사용 -->
			    , FBF.FREE_BOARD_ID "freeBoardId", FBF.ORIGINAL_FILE_NAME as "originalFileName"
			    , FBF.STORED_FILE_NAME as "storedFileName"
			    , ROUND(FBF.FREE_BOARD_FILE_SIZE/1024, 1) as "freeBoardFileSize"
			FROM FREE_BOARD_FILE FBF
			WHERE FBF.FREE_BOARD_ID = #{freeBoardId}
   </select>

  <!-- keyProperty의 return type을 parameterType의 VO 객체에 바로 set -->
  <insert id="freeBoardInsertOne" parameterType="com.edu.freeBoard.domain.FreeBoardVo" useGeneratedKeys="true" keyProperty="freeBoardId">
    <!-- useGeneratedKeys 이후 실행, insert 구문에서 keyProperty는 return 타입을 의미  -->
    <selectKey keyProperty="freeBoardId" resultType="int" order="BEFORE">
      SELECT FREE_BOARD_ID_SEQ.NEXTVAL
      FROM DUAL
    </selectKey>

    INSERT INTO FREE_BOARD
    VALUE(FREE_BOARD_ID, MEMBER_NO, FREE_BOARD_TITLE, FREE_BOARD_CONTENT, CREATE_DATE, UPDATE_DATE)
		VALUES(#{freeBoardId}, #{memberNo}, #{freeBoardTitle}, #{freeBoardContent}, SYSDATE, SYSDATE)
  </insert>

  <insert id="freeBoardFileInsertOne" parameterType="map">
    INSERT INTO FREE_BOARD_FILE
			(FREE_BOARD_FILE_ID, FREE_BOARD_ID
			, ORIGINAL_FILE_NAME, STORED_FILE_NAME, FREE_BOARD_FILE_SIZE
			, CREATE_DATE, UPDATE_DATE)
		VALUES(FREE_BOARD_FILE_ID_SEQ.NEXTVAL, #{parentId}, #{originalFileName}, #{storedFileName}, #{fileSize}, SYSDATE, SYSDATE)
  </insert>

  <update id="freeBoardUpdateOne" parameterType="freeBoardVo">
    UPDATE FREE_BOARD
		SET FREE_BOARD_TITLE = #{freeBoardTitle}
		  , FREE_BOARD_CONTENT = #{freeBoardContent}
		  , UPDATE_DATE = SYSDATE
		WHERE FREE_BOARD_ID = #{freeBoardId}
		AND MEMBER_NO = #{memberNo}
  </update>

</mapper>
```

<br />

- FreeBoardListView.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/core"
prefix="c"%>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>자유게시판 목록</title>
    <style type="text/css">
      table,
      tr,
      th,
      td {
        border: 1px solid black;
        border-collapse: collapse;
      }

      tr > th {
        background-color: gray;
      }

      .aTagStyle {
        cursor: pointer;
      }

      .aTagStyle:hover {
        color: lightgreen;

        background-color: gray;
      }

      #container {
        border: 1px solid;
      }

      #container > tr {
        width: 980px;
      }

      .tableSubject {
        width: 245px;

        background-color: gray;
      }

      .tableValue {
        width: 245px;
      }
    </style>

    <script
      src="https://code.jquery.com/jquery-3.7.0.js"
      integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
      crossorigin="anonymous"
    ></script>

    <script type="text/javascript">
      function storeFileMakeUlFnc(freeBoardFileList) {
        const storeFileListUl = $("#storeFileList");

        storeFileListUl.html("");

        let listItem = null;

        if (freeBoardFileList.length == 0) {
          listItem = $("<li>저장된 파일이 없습니다.</li>");
          storeFileListUl.html(listItem);

          return;
        }

        let liHtmlStr = "";

        for (let i = 0; i < freeBoardFileList.length; i++) {
          listItem = document.createElement("li");
          // mapper에서 작성한 alias를 칼럼으로 사용 => originalFileName => 화면에서 사용
          // src="/img/" 경로는 WebMvcConfig class를 통해 자동 치환

          liHtmlStr =
            freeBoardFileList[i].originalFileName +
            "&nbsp;&nbsp;" +
            freeBoardFileList[i].freeBoardFileSize +
            "(kb)" +
            "<img alt='image not found' src='/img/" +
            freeBoardFileList[i].storedFileName +
            "' style='width: 150px;' />" +
            "<span><input type='button' value='수정' />" +
            "<input type='button' value='삭제' /></span>";

          listItem.innerHTML = liHtmlStr;
          storeFileListUl.append(listItem);
        }
      } // 파일 ui 제작 함수 end

      // jQuery
      $(function () {
        // 게시판 추가 화면
        $("#aFreeBoardInsert").on("click", function (event) {
          const myObj = $(this);

          event.preventDefault(); // 이벤트의 기본 동작을 막음

          const containerTag = $("#container");

          let htmlStr = "";

          htmlStr += '<table style="width: 1000px;">';
          htmlStr += "<tr>";
          htmlStr += '<td class="tableSubject">주제</td>';
          htmlStr += '<td style="width: 735px;">';
          htmlStr +=
            '<input type="text" id="freeBoardTitle" name="freeBoardTitle" value="" size="100px" />';
          htmlStr += "</td>";
          htmlStr += "</tr>";
          htmlStr += "<tr>";
          htmlStr += '<td style="width: 980px;" colspan="2">';
          htmlStr +=
            '<textarea id="freeBoardContent" name="freeBoardContent" rows="10" cols="100" style="width: 990px;">';
          htmlStr += "</textarea>";
          htmlStr += "</td>";
          htmlStr += "</tr>";
          htmlStr += "</table>";

          // 이미지 업로드 UI
          htmlStr += '<div id="fileContainer" style="border: 1px solid;">';
          htmlStr += '<label for="inputFreeBoardFile">파일</label>';
          htmlStr +=
            '<input type="file" id="inputFreeBoardFile" name="freeBoardFileList" multiple="multiple" />';
          htmlStr += '<ul id="fileList" class="fileUploadList"></ul>';
          htmlStr += "</div>";

          htmlStr += "<div>";
          htmlStr += "<span>";
          htmlStr +=
            '<button onclick="pageMoveFreeBoardListFnc();">이전페이지</button>';
          htmlStr += '<button id="btnFreeBoardInsert">작성 완료</button>';
          htmlStr += "</span>";
          htmlStr += "</div>";

          containerTag.html(htmlStr);

          const inputFreeBoardFile =
            document.getElementById("inputFreeBoardFile"); // multiple => Array

          const fileListUl = document.getElementById("fileList");

          inputFreeBoardFile.addEventListener("change", function (e) {
            fileListUl.innerHTML = ""; // initialize

            for (let i = 0; i < inputFreeBoardFile.files.length; i++) {
              let listItem = document.createElement("li");

              listItem.innerHTML =
                inputFreeBoardFile.files[i].name +
                "&nbsp;&nbsp;" +
                inputFreeBoardFile.files[i].size +
                "(byte)";

              fileListUl.appendChild(listItem);
            }
          }); // 이벤트 등록 end
        });

        // 동적 이벤트 등록
        // 게시판 추가 버튼 작동
        $("#container").on("click", "#btnFreeBoardInsert", function (event) {
          event.preventDefault();

          const inputMemberNoTag = $("#inputMemberNo");
          const freeBoardTitleTag = $("#freeBoardTitle");
          const freeBoardContentTag = $("#freeBoardContent");
          const inputFreeBoardFileArr = $("#inputFreeBoardFile")[0].files;

          const formDataObj = new FormData();

          formDataObj.append("freeBoardId", 0);
          formDataObj.append("memberNo", inputMemberNoTag.val());
          formDataObj.append("freeBoardTitle", freeBoardTitleTag.val());
          formDataObj.append("freeBoardContent", freeBoardContentTag.val());

          // file data 처리, file이 존재하는 경우에만 담음
          if (inputFreeBoardFileArr.length > 0) {
            for (let i = 0; i < inputFreeBoardFileArr.length; i++) {
              // file은 name이 같으면 안 되기에 index 활용
              // input 태그 생성 및 name="value" 형태로 설정
              formDataObj.append(
                "inputFreeBoardFileArr" + i,
                inputFreeBoardFileArr[i]
              );
            }
          }

          $.ajax({
            url: "/freeBoard/",
            method: "POST",
            /*	contentType: "application/json", // json은 파일 처리 불가 */
            contentType: false,
            processData: false, // jQuery에서 브라우저가 자동으로 content-type을 설정하도록 함
            data: formDataObj,
            success: function (data) {
              alert("156 line: 파일 추가? " + data);
              location.href = "./list";
            },
            error: function (xhr, status) {
              alert(xhr.status);
              alert(status);
            }
          }); // ajax end
        });
      }); // onload

      // 자유게시판 수정 form 화면 이동 혹은 생성
      function restRequestFreeBoardUpdateFnc(tableTdTag) {
        const tableTdTafObj = $(tableTdTag);
        const parentTr = tableTdTafObj.parent();
        const freeBoardIdStr = parentTr.children().eq(0).text();

        // alert("freeBoardIdStr: " + freeBoardIdStr);

        $.ajax({
          url: "/freeBoard/" + freeBoardIdStr,
          method: "GET",
          dataType: "json",
          success: function (data) {
            // alert("도착: " + data);

            const freeBoardVo = data.freeBoardVo;
            const freeBoardFileList = data.freeBoardFileList;
            let createDate = new Date(freeBoardVo.createDate).toLocaleString(
              "ko-KR",
              {
                year: "numeric",
                month: "2-digit",
                day: "2-digit",
                hour: "2-digit",
                minute: "2-digit",
                second: "2-digit"
              }
            );

            const containerTag = $("#container");

            let htmlStr = "";

            htmlStr += '<table style="width: 1000px;">';

            htmlStr += "<tr>";
            htmlStr += '<td class="tableSubject">주제</td>';
            htmlStr += '<td style="width: 735px;" colspan="3">';
            htmlStr +=
              '<input type="text" id="freeBoardTitle" name="freeBoardTitle" value="' +
              freeBoardVo.freeBoardTitle +
              '" size="100px" />';
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "<tr>";
            htmlStr += '<td class="tableSubject">작성자</td>';
            htmlStr +=
              '<td class="tableValue">' + freeBoardVo.memberName + "</td>";
            htmlStr += '<td class="tableSubject">게시판 번호</td>';
            htmlStr += '<td class="tableValue">';
            htmlStr +=
              '<input id="freeBoardId" name="freeBoardId" value="' +
              freeBoardVo.freeBoardId +
              '" readonly="readonly" />';
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "<tr>";
            htmlStr += '<td class="tableSubject">이메일</td>';
            htmlStr += '<td class="tableValue">' + freeBoardVo.email + "</td>";
            htmlStr += '<td class="tableSubject">작성일자</td>';
            htmlStr += '<td class="tableValue">';
            htmlStr += createDate;
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "<tr>";
            htmlStr += '<td style="width: 980px;" colspan="4">';
            htmlStr +=
              '<textarea id="freeBoardContent" name="freeBoardContent"';
            htmlStr += ' rows="10" cols="100" style="width: 990px;">';
            htmlStr += "</textarea>";
            htmlStr += "</td>";
            htmlStr += "</tr>";

            htmlStr += "</table>";

            // 이미지 업로드 UI
            htmlStr += '<div id="fileContainer" style="border: 1px solid;">';
            htmlStr += '<label for="inputFreeBoardFile">파일</label>';
            htmlStr +=
              '<input type="file" id="inputFreeBoardFile" name="freeBoardFileList" multiple="multiple" />';
            htmlStr += '<ul id="storeFileList" class="fileUploadList"></ul>';
            htmlStr += "</div>";

            // 권한의 여부
            const inputSessionMemberNoTag = $("#inputMemberNo");

            if (freeBoardVo.memberNo == inputSessionMemberNoTag.val()) {
              htmlStr += "<div>";
              htmlStr += "<span>";
              htmlStr +=
                '<button onclick="pageMoveFreeBoardListFnc();">이전페이지</button>';
              htmlStr +=
                '<button onclick="reseRequestFreeBoardUpdateCtrFnc();">수정 완료</button>';
              htmlStr += "</span>";
              htmlStr += "</div>";
            } else {
              htmlStr += "<div>";
              htmlStr += "<span>";
              htmlStr +=
                '<button onclick="pageMoveFreeBoardListFnc();">이전페이지</button>';
              htmlStr += "</span>";
              htmlStr += "</div>";
            }

            containerTag.html(htmlStr);

            const freeBoardContentTag = $("#freeBoardContent");
            freeBoardContentTag.text(freeBoardVo.freeBoardContent);

            storeFileMakeUlFnc(freeBoardFileList);

            const inputFreeBoardFile =
              document.getElementById("inputFreeBoardFile");
            const fileListUl = document.getElementById("fileList");

            inputFreeBoardFile.addEventListener("change", function (e) {
              fileListUl.innerHTML = ""; // initialize

              for (let i = 0; i < inputFreeBoardFile.files.length; i++) {
                let listItem = document.createElement("li");

                listItem.innerHTML =
                  inputFreeBoardFile.files[i].name +
                  "&nbsp;&nbsp;" +
                  inputFreeBoardFile.files[i].size +
                  "(byte)";

                fileListUl.appendChild(listItem);
              }
            }); // 이벤트 등록 end
          },

          error: function (xhr, status) {
            alert(xhr.status);
            alert(status);
          }
        }); // ajax end
      }

      // 자유게시판 DB 정보 수정
      function reseRequestFreeBoardUpdateCtrFnc(tableTdTag) {
        const inputMemberNoTag = $("#inputMemberNo");
        const freeBoardIdTag = $("#freeBoardId");
        const freeBoardTitleTag = $("#freeBoardTitle");
        const freeBoardContentTag = $("#freeBoardContent");

        const jsonDataObj = {
          freeBoardId: freeBoardIdTag.val(),
          memberNo: inputMemberNoTag.val(),
          freeBoardTitle: freeBoardTitleTag.val(),
          freeBoardContent: freeBoardContentTag.val(),
          createDate: null,
          updateDate: null
        };

        $.ajax({
          url: "/freeBoard/" + jsonDataObj.freeBoardId,
          method: "PATCH",
          contentType: "application/json",
          data: JSON.stringify(jsonDataObj),
          dataType: "json",
          success: function (data) {
            alert("게시판 수정 도착: " + data);

            let createDate = new Date(data.createDate).toLocaleString("ko-KR", {
              year: "numeric",
              month: "2-digit",
              day: "2-digit",
              hour: "2-digit",
              minute: "2-digit",
              second: "2-digit"
            });

            const containerTag = $("#container");

            let htmlStr = `
              <table style="width: 1000px;">
                <tr>
                  <td class="tableSubject">주제</td>
                  <td style="width: 735px;" colspan="3">
                    <input type="text" id="freeBoardTitle" name="freeBoardTitle" value="\${data.freeBoardTitle}" size="100px" />
                  </td>
                </tr>

                <tr>
                  <td class="tableSubject">작성자</td>
                  <td class="tableValue">\${data.memberName}</td>
                  <td class="tableSubject">게시판 번호</td>
                  <td class="tableValue">
                    <input id="freeBoardId" name="freeBoardId" value="\${data.freeBoardId}" readonly="readonly" />
                  </td>
                </tr>

                <tr>
                  <td class="tableSubject">이메일</td>
                  <td class="tableValue">\${data.email}</td>
                  <td class="tableSubject">작성일자</td>
                  <td class="tableValue">\${createDate}</td>
                </tr>

                <tr>
                  <td style="width: 980px;" colspan="4">
                  <textarea id="freeBoardContent" name="freeBoardContent" rows="10" cols="100" style="width: 990px;"></textarea>
                  </td>
                </tr>

              </table>

              <div>
                <span>
                  <button onclick="pageMoveFreeBoardListFnc();">이전페이지</button>
                  <button onclick="reseRequestFreeBoardUpdateCtrFnc();">수정 완료</button>
                </span>
              </div>
            `;

            containerTag.html(htmlStr);

            const freeBoardContentTag = $("#freeBoardContent");
            freeBoardContentTag.text(data.freeBoardContent);
          },

          error: function (xhr, status) {
            console.log(xhr.status);
            console.log(status);

            const errorMassage = xhr.responseJSON
              ? xhr.responseJSON.errorMsg
              : "알 수 없는 오류가 발생했습니다.";

            alert("오류: " + errorMassage);
          }
        }); // ajax end
      }

      function pageMoveFreeBoardDetailFnc(tableTdTag) {
        let parentTr = tableTdTag.parentNode;
        let freeBoardIdStr = parentTr.children[0].textContent;

        let userSelectFreeBoardIdObj = document.getElementById(
          "userSelectFreeBoardId"
        );
        userSelectFreeBoardIdObj.value = freeBoardIdStr;

        let freeBoardListFormObj = document.getElementById("freeBoardListForm");
        freeBoardListFormObj.submit();
      }
    </script>
  </head>

  <body>
    <jsp:include page="/WEB-INF/views/Header.jsp" />

    <div id="container">
      <h1>자유게시판 목록</h1>

      <p>
        <a id="aFreeBoardInsert" href="#">자유게시판 글쓰기</a>
      </p>

      <table>
        <tr>
          <th>번호</th>
          <th>주제</th>
          <th>작성자</th>
          <th>생성날짜</th>
          <th>수정날짜</th>
          <th>비고[삭제]</th>
        </tr>

        <c:forEach var="freeBoardVo" items="${freeBoardList}">
          <tr>
            <td>${freeBoardVo.freeBoardId}</td>
            <td
              class="aTagStyle"
              onclick="restRequestFreeBoardUpdateFnc(this);"
            >
              ${freeBoardVo.freeBoardTitle}
            </td>
            <td>${freeBoardVo.memberName}</td>
            <td>${freeBoardVo.createDate}</td>
            <td>${freeBoardVo.updateDate}</td>

            <td style="text-align: center">[삭제]</td>
          </tr>
        </c:forEach>
      </table>

      <jsp:include page="/WEB-INF/views/common/Paging.jsp">
        <jsp:param value="${pagingMap}" name="pagingMap" />
      </jsp:include>

      <form id="pagingForm" action="./list" method="post">
        <input
          type="hidden"
          id="curPage"
          name="curPage"
          value="${pagingMap.pagingVo.curPage}"
        />
      </form>
    </div>

    <jsp:include page="/WEB-INF/views/Tail.jsp" />

    <form id="freeBoardListForm" action="./list" method="post">
      <input
        id="userSelectFreeBoardId"
        type="hidden"
        name="freeBoardId"
        value=""
      />
    </form>
  </body>
</html>
```
