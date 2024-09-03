# Spring 6일차

## 이론

- 페이지네이션

```java
package com.edu.util;

import java.io.Serializable;

public class Paging implements Serializable {

  // 페이지당 게시물 수
  public static final int PAGE_SCALE = 10;
  // 화면당 블럭 수
  public static final int BLOCK_SCALE = 5;

  private int curPage = 0; // 현재 페이지

  private int totPage; // 전체 페이지 수
  private int totBlock; // 전체 페이지 블럭 수

  private int curBlock; // 현재 페이지 블럭

  private int prevBlock;
  private int nextBlock;

  private int pageBegin;
  private int pageEnd;

  private int blockBegin;
  private int blockEnd;

  public Paging(int count, int curPage) {
    this.curBlock = 1;

    setTotPage(count);

    if (curPage > totPage) {
      curPage = totPage;
    }

    this.curPage = curPage;

    setPageRange();
    setTotBlock();
    setBlockRange();
  }

  public void setBlockRange() {
    curBlock = (int) Math.ceil((curPage - 1) / BLOCK_SCALE) + 1;
    blockBegin = (curBlock - 1) * BLOCK_SCALE + 1;
    blockEnd = blockBegin + BLOCK_SCALE - 1;

    if (blockEnd > totPage) {
      blockEnd = totPage;
    }

    prevBlock = (curPage == 1) ? 1 : (curBlock - 1) * BLOCK_SCALE;

    nextBlock = curBlock > totBlock ? (curBlock * BLOCK_SCALE) : (curBlock * BLOCK_SCALE) + 1;

    if (prevBlock <= 0) {
      prevBlock = 1;
    }

    if (nextBlock >= totPage) {
      nextBlock = totPage;
    }
  }

  public void setTotBlock() {
    this.totBlock = (int) Math.ceil((double) totPage / (double) BLOCK_SCALE);
  }

  public void setPageRange() {
    pageBegin = (curPage - 1) * PAGE_SCALE + 1;
    pageEnd = pageBegin + PAGE_SCALE - 1;
  }

  public int getCurPage() {
    return curPage;
  }

  public void setCurPage(int curPage) {
    this.curPage = curPage;
  }

  public int getTotPage() {
    return totPage;
  }

  public void setTotPage(int totPage) {
    this.totPage = (int) Math.ceil(totPage * 1.0 / PAGE_SCALE);
  }

  public int getTotBlock() {
    return totBlock;
  }

  public void setTotBlock(int totBlock) {
    this.totBlock = totBlock;
  }

  public int getCurBlock() {
    return curBlock;
  }

  public void setCurBlock(int curBlock) {
    this.curBlock = curBlock;
  }

  public int getPrevBlock() {
    return prevBlock;
  }

  public void setPrevBlock(int prevBlock) {
    this.prevBlock = prevBlock;
  }

  public int getNextBlock() {
    return nextBlock;
  }

  public void setNextBlock(int nextBlock) {
    this.nextBlock = nextBlock;
  }

  public int getPageBegin() {
    return pageBegin;
  }

  public void setPageBegin(int pageBegin) {
    this.pageBegin = pageBegin;
  }

  public int getPageEnd() {
    return pageEnd;
  }

  public void setPageEnd(int pageEnd) {
    this.pageEnd = pageEnd;
  }

  public int getBlockBegin() {
    return blockBegin;
  }

  public void setBlockBegin(int blockBegin) {
    this.blockBegin = blockBegin;
  }

  public int getBlockEnd() {
    return blockEnd;
  }

  public void setBlockEnd(int blockEnd) {
    this.blockEnd = blockEnd;
  }

  @Override
  public String toString() {
    StringBuilder builder = new StringBuilder();

    builder.append("Paging [curPage=");
    builder.append(curPage);
    builder.append(", totPage=");
    builder.append(totPage);
    builder.append(", totBlock=");
    builder.append(totBlock);
    builder.append(", curBlock=");
    builder.append(curBlock);
    builder.append(", prevBlock=");
    builder.append(prevBlock);
    builder.append(", nextBlock=");
    builder.append(nextBlock);
    builder.append(", pageBegin=");
    builder.append(pageBegin);
    builder.append(", pageEnd=");
    builder.append(pageEnd);
    builder.append(", blockBegin=");
    builder.append(blockBegin);
    builder.append(", blockEnd=");
    builder.append(blockEnd);
    builder.append("]");

    return builder.toString();
  }

}
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/core"
prefix="c"%> <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>

<style type="text/css">
  nav > ul {
    list-style-type: none;
    padding: 0px;
    overflow: hidden;
    background-color: #333333;
    /*     width: 1000px; */ /* 넓이를 주면 고정  */
    display: table; /* table을 주면  요소의 내용에 맞게 자동으로 크기 */
    margin-left: auto;
    margin-right: auto;
  }

  nav > ul > li {
    float: left;
  }

  nav > ul > li > a {
    display: block;
    color: white;
    text-align: center;
    padding: 16px;
    text-decoration: none;
  }

  nav > ul > li > a:hover {
    color: #ffd9ec;
    background-color: #5d5d5d;
    font-weight: bold;
  }

  .active {
    color: #ffd0ec;
    background-color: #5d5d5d;
    font-weight: bold;
  }
</style>

<script
  src="https://code.jquery.com/jquery-3.7.1.js"
  integrity="sha256-eKhayi8LEQwp4NKxN+CfCh+3qOVUtJn3QNZ0TciWLP4="
  crossorigin="anonymous"
></script>

<script type="text/javascript">
  function goPage(pageNumber) {
    const curPage = $("#curPage");

    curPage.val(pageNumber);

    $("#pagingForm").submit();
  }

  window.onload = function () {
    const curPageDoc = $("#curPage"); // input

    const pageButtonId = document.getElementById(
      "pageButton" + curPageDoc.val()
    );

    $(pageButtonId).addClass("active");
  };
</script>

<nav>
  <ul>
    <!--    ㄷ 한자 -->

    <c:if test="${pagingMap.pagingVo.prevBlock ne 1}">
      <li>
        <a href="#" onclick="goPage(${pagingMap.pagingVo.prevBlock});">
          <span>≪</span>
        </a>
      </li>
    </c:if>

    <c:forEach
      var="num"
      begin="${pagingMap.pagingVo.blockBegin}"
      end="${pagingMap.pagingVo.blockEnd}"
    >
      <li id="pageButton${num}">
        <a href="#" onclick="goPage(${num});">
          <c:out value="${num}" />
        </a>
      </li>
    </c:forEach>

    <c:if test="${pagingMap.pagingVo.curBlock lt pagingMap.pagingVo.totBlock}">
      <li>
        <a href="#" onclick="goPage(${pagingMap.pagingVo.nextBlock});">
          <span>≫</span>
        </a>
      </li>
    </c:if>
  </ul>
</nav>
```
