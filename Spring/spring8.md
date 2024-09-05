# Spring 8일차

## 이론

- jQuery

  - jQuery가 과거에 인기 많았던 이유

  ```
  1. 코드의 양이 줄어듦 => 가독성 상승

  2. 브라우저와 상관없이 같은 표준 제공 => 크로스 브라우징 방지

  3. 간단한 API로 document 내용 자체도 바꿀 수 있음
    => ex) HTML 전체 구조 새로 생성, 확장

  4. 페이지와 사용자 간 상호작용 처리, 세련된 방식
    => 웹 개발자를 괴롭히는 브라우저 사이의 불일치 문제 해결

  5. 시각적 효과 제공

  6. 페이지를 새로고침하지 않고 서버에서 정보를 가져옴
    => ex) Ajax(비동기 통신)
    => 클라이언트와 서버 사이에서 통신할 수 있는 훨씬 더 많은 기술 사용 가능

  7. 일반적인 자바스크립트 작업 단순화

  8. CSS 지식 최대한 활용 가능

  9. 언제나 집합으로 작업

  10. 여러 동작을 한 줄에 작성
    => 임시 변수 사용을 최소화하거나 불필요한 반복을 피함

  11. 모든 사람에게 무료로 제공
  ```

<br />

- 문제 풀이

  1. #root에 태그 삽입 및 내용

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <script src="../jquery-3.7.1.js"></script>

      <script>
        $(document).ready(function () {
          $("#root").html("<p>Dobby</p><div>1</div>");
        });
      </script>
    </head>

    <body>
      <div id="root" class="" style="border: 1px solid"></div>
    </body>
  </html>
  ```

  <br />

  2. jQuery 경로 문제 해결

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          alert("jQuery 경로 문제 해결!");
        });
      </script>
    </head>

    <body></body>
  </html>
  ```

  <br />

  3. h1 태그의 텍스트 색상 변경

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>
      <style></style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        window.onload = function () {
          const h1Els = document.querySelectorAll("h1");

          h1Els.forEach((el) => {
            el.style.color = "orange";
          });
        };
      </script>
    </head>

    <body>
      <div id="root" class="" style="border: 1px solid"></div>
      <h1>Hello jQuery</h1>
      <p>제이쿼리 장단점</p>
      <h1>Low War</h1>
      <p>나는 매우 졸리다</p>
      <div>안녕히 주무세요</div>
    </body>
  </html>
  ```

  <br />

  4. 버튼 클릭 시 li 태그의 텍스트를 #root에 삽입

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          // .on() 이벤트 등록, .click() click 이벤트 등록

          $("#oneBtn").click(function () {
            $("#root").html($("#one").attr("title"));
          });

          $("#twoBtn").click(function () {
            $("#root").html($("#two").attr("title"));
          });

          $("#threeBtn").click(function () {
            $("#root").html($("#three").attr("title"));
          });

          $("#fourBtn").click(function () {
            $("#root").html($("#four").attr("title"));
          });

          $("#fiveBtn").click(function () {
            $("#root").html($("#five").attr("title"));
          });
        });
      </script>
    </head>

    <body>
      <div style="border: 1px solid;">
        <ul>
          <li id="one" title="사과">Apple</li>
          <li id="two" title="바나나">Banana</li>
          <li id="three" title="체리">Cherry</li>
          <li id="four" title="개">Dog</li>
          <li id="five" title="코끼리">Elephant</li>
        </ul>
      </div>

      <div id="root" style="border: 1px solid;"></div>

      <button id="oneBtn" onclick="showTitle(this);">One!</button>
      <button id="twoBtn" onclick="showTitle(this);">Two!</button>
      <button id="threeBtn" onclick="showTitle(this);">Three!</button>
      <button id="fourBtn" onclick="showTitle(this);">Four!</button>
      <button id="fiveBtn" onclick="showTitle(this);">Five!</button>
    </body>
  </html>
  ```

  <br />

  5. a 태그의 텍스트를 인덱스와 함께 콘솔에 출력, li 태그의 배경색을 각각 다른 색으로 변경

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          // 		$("a").attr("href", function(index, obj) {
          // 			// 자바스크립트 => 제이쿼리화
          // 			// $(obj).attr("href")
          // 			console.log(obj);
          // 			console.log(index + "\t:\t" + obj);
          // 			console.log("======================================");
          // 		});

          // a태그의 html 내용을 콘솔에 인덱스와 함께 출력
          $("a").each(function (index) {
            console.log(index + "\t:\t" + $(this).text());
          });

          const bgColor = ["red", "green", "yellow", "blue", "pink"];

          // 		$("li").attr("id", function(i) {
          // 			$(this).css("background-color", bgColor[i]);
          // 		});

          $("li").each(function (i) {
            $(this).css("background-color", bgColor[i]);
          });
        });
      </script>
    </head>

    <body>
      <a href="http://1등.net" target="_blank" title="범용AI">chatGPT</a>
      <a href="http://2등.net" target="_parent" title="비즈니스AI">copilot</a>
      <a href="http://3등.net" target="_top" title="IIAI">perplexity AI</a>

      <div id="root" style="border: 1px solid">
        <ul>
          <li id="one">Apple</li>
          <li id="two">Banana</li>
          <li id="three">Cherry</li>
          <li id="four">Dog</li>
          <li id="five">Elephant</li>
        </ul>
      </div>
    </body>
  </html>
  ```

  <br />

  6. h1 태그의 클래스 명을 span 태그의 클래스 명에 추가한 다음 #root 안에 삽입

     - span 태그의 텍스트는 1부터 순차적으로 상승

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>

  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <style>
        .high-light {
          background-color: yellowgreen;
        }

        .high-light-0 {
          background-color: lightyellow;
        }

        .high-light-1 {
          background-color: lightgreen;
        }

        .high-light-2 {
          background-color: lightgray;
        }

        .high-light-3 {
          background-color: lightblue;
        }

        .high-light-4 {
          background-color: lightpink;
        }

        .high-light-5 {
          background-color: #ff474c;
        }

        .high-light-6 {
          background-color: #e4fff6;
        }
      </style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          let acc = "";

          // class add
          $("h1").each(function (i, el) {
            $(el).addClass("high-light-" + i);
          });

          // span 태그 생성
          $("h1").each(function (i, el) {
            acc +=
              "<span class='" +
              $(el).attr("class") +
              "'>" +
              (i + 1) +
              "</span>";
          });

          $("#root").html(acc);
        });
      </script>
    </head>

    <body>
      <h1>item - 0</h1>
      <h1>item - 1</h1>
      <h1>item - 2</h1>
      <h1>item - 3</h1>
      <h1>item - 4</h1>
      <h1>item - 5</h1>
      <h1>item - 6</h1>

      <div id="root"></div>
    </body>
  </html>
  ```
