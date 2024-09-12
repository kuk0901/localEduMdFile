# Spring 13일차

## 이론

- 게시물 수정(title, content, img)

```
=======================================================
1. 테이블 두 개 조인해서 조회
2. 파일 테이블에서 삭제하려는 데이터를 삭제
3. 새로 수정된 데이터를 이용해 프리보드 업데이트
4. 파일 테이블에 프리보드 pk를 fk로 사용해 인서트

- A 픽(업데이트)
=======================================================
1. 테이블 두 개 조인해서 조회
2. 파일 테이블에서 삭제하려는 데이터 삭제
3. 게시글 아이디 데이터 반환
4. 해당 게시글 딜리트
5. 새로 게시글을 인서트 => 파일 테이블도 인서트

- B 픽(인서트)
```

<br />

## 코드

- freeboard controller

```java
// 게시판 수정 DB
@PatchMapping("/{freeBoardId}") // json => @RequestBody // form => @RequestParam 사용 => 자동 형 변환
public ResponseEntity<?> freeBoardUpdateCtr(@PathVariable int freeBoardId
    , @RequestParam Map<String, String> freeBoardMap
    , MultipartHttpServletRequest mhr
    // 일치하는 name을 받음, required = true는 값이 필수라는 의미(기본 값)
    , @RequestParam(value="delFreeBoardFileIdList", required = false) List<Integer> delFreeBoardFileIdList
) {
  log.info(logTitleMsg);
  log.info("@PostMapping freeBoardUpdateCtr freeBoardId: {}, freeBoardMap: {}, delFreeBoardFileIdList: {}"
      , freeBoardId, freeBoardMap, delFreeBoardFileIdList);

  FreeBoardVo freeBoardVo = new FreeBoardVo();
  freeBoardVo.setFreeBoardId(freeBoardId);
  freeBoardVo.setMemberNo(Integer.parseInt(freeBoardMap.get("memberNo")));
  freeBoardVo.setFreeBoardTitle(freeBoardMap.get("freeBoardTitle"));
  freeBoardVo.setFreeBoardContent(freeBoardMap.get("freeBoardContent"));

  try {
    freeBoardService.freeBoardUpdateOne(freeBoardVo, mhr, delFreeBoardFileIdList);
  } catch (Exception e) {
    e.printStackTrace();
    Map<String, String> errorResMap = new HashMap<>();
    errorResMap.put("errorMsg", "게시판 등록자가 아닙니다.");

    return ResponseEntity.status(HttpStatus.BAD_REQUEST).contentType(MediaType.APPLICATION_JSON).body(errorResMap);
  }

  // 화면에 사용될 수정된 데이터를 가져옴
  Map<String, Object> resultMap = freeBoardService.freeBoardSelectOne(freeBoardId);

  return ResponseEntity.ok(resultMap);
}
```

<br />

- freeboard service

```java
@Transactional // 트랜잭션으로 만듦 => 하나라도 실패하는 경우 RollBack
@Override
public void freeBoardUpdateOne(FreeBoardVo freeBoardVo, MultipartHttpServletRequest mhr,
    List<Integer> delFreeBoardFileIdList) throws Exception {

  freeBoardDao.freeBoardUpdateOne(freeBoardVo);

  int parentSeq = freeBoardVo.getFreeBoardId();

  if (delFreeBoardFileIdList != null) {
    List<Map<String, Object>> tempFileList = freeBoardDao.fileSelectStoredFileName(delFreeBoardFileIdList);

    // JPA 명명규칙, 구문과 관련된 부분을 앞에 작성 - by를 기준으로 뒷부분에 행하는 변수 작성
    freeBoardDao.deleteFileByFreeFilIds(delFreeBoardFileIdList);

    if (tempFileList != null) {
      fileUtils.parseDeleteFileInfo(tempFileList);
    }
  } // if문

  // 재사용 사례
  List<Map<String, Object>> fileInsertList = fileUtils.parseInsertFileInfo(parentSeq, mhr);

  if (fileInsertList.isEmpty() == false) {
    for (Map<String, Object> map : fileInsertList) {
      freeBoardDao.freeBoardFileInsertOne(map);
    }
  }
}
```

<br />

- freeboard mapper

```xml
<update id="freeBoardUpdateOne" parameterType="freeBoardVo">
  UPDATE FREE_BOARD
  SET
    FREE_BOARD_TITLE = #{freeBoardTitle}
    , FREE_BOARD_CONTENT = #{freeBoardContent}
    , UPDATE_DATE = SYSDATE
  WHERE FREE_BOARD_ID = #{freeBoardId}
  AND MEMBER_NO = #{memberNo}
</update>

<select id="fileSelectStoredFileName" parameterType="list" resultType="map">
  SELECT FBF.FREE_BOARD_FILE_ID, FBF.STORED_FILE_NAME
  FROM FREE_BOARD_FILE FBF
  WHERE FBF.FREE_BOARD_FILE_ID IN
  <!-- 내부적으로 실행 collection 값으로 타입을 일치시킴, separator는 반복할 때마다 해당 값을 입힘 (1, 2, 3) 형태  -->
  <foreach item="delFreeBoardFileId" collection="list" open="(" separator="," close=")">
    #{delFreeBoardFileId}
  </foreach>
</select>

<delete id="deleteFileByFreeFilIds" parameterType="list">
  DELETE FROM FREE_BOARD_FILE
  WHERE FREE_BOARD_FILE_ID IN
  <foreach item="delFreeBoardFileId" collection="list" open="(" separator="," close=")">
    #{delFreeBoardFileId}
  </foreach>
</delete>
```

<br />

- file utils

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

  // 실제 파일 삭제 => DB를 기반으로 data를 가져와 파일이 존재할 경우 실제 파일 삭제
  public void parseDeleteFileInfo(List<Map<String, Object>> tempFileList) throws Exception {
    for (Map<String, Object> tempFileMap : tempFileList) {
      String storedFileName = (String) tempFileMap.get("STORED_FILE_NAME");

      File file = new File(FILE_PATH + "/" + storedFileName);

      if (file.exists()) {
        file.delete();
      } else {
        System.err.println("파일이 존재하지 않습니다.");
      }
    }

  }
}
```

> FreeBoardListView 코드(ajax)는 깃허브에서 확인
