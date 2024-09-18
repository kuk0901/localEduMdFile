# Spring 10일차

## **코드**

- 태그 기반: 태그.each()

  ```js
  $(function() {
    const studentArr = [
      { name: "Franklin", age: "20", ment: "hello" },
      { name: "Nellie", age: "21", ment: "hi" },
      { name: "Louise", age: "30", ment: "yeah" },
      { name: "Mason", age: "33", ment: "omg" },
    ];

    $("div").each((i, obj) => {
      let htmlStr = "";

      htmlStr += "<h1" + (i + 1) + ">";
      htmlStr += "이름:&nbsp;" + obj.name + "&nbsp;나이:&nbsp;" + obj.age + "&nbsp;하고싶은&nbsp;말:&nbsp;" + obj.ment;
      htmlStr += "</h" + (i + 1) + ">";

      divEl[i].innerHTML = htmlStr;
    });&nbsp;
  })
  ```

<br />

- 데이터 기반: $.each()

  ```js
  $(function () {
    const studentArr = [
      { name: "Franklin", age: "20", ment: "hello" },
      { name: "Nellie", age: "21", ment: "hi" },
      { name: "Louise", age: "30", ment: "yeah" },
      { name: "Mason", age: "33", ment: "omg" }
    ];

    let htmlStrArr = new Array();

    $.each(studentArr, (i, obj) => {
      htmlStrArr[i] = "<h1" + (i + 1) + ">";
      htmlStrArr[i] +=
        "이름:&nbsp;" +
        obj.name +
        "&nbsp;나이:&nbsp;" +
        obj.age +
        "&nbsp;하고싶은&nbsp;말:&nbsp;" +
        obj.ment;
      htmlStrArr[i] += "</h" + (i + 1) + ">";
    });

    $("div").each((i, obj) => {
      $(obj).html(htmlStrArr[i]);
    });
  });
  ```

<br />

- filter()

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
      </style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          $("h1").filter(".my").css({
            backgroundColor: "black",

            color: "white"
          });

          $("h1")
            .filter((i) => {
              return i % 2 == 1;
            })
            .css({
              fontStyle: "oblique",

              fontWeight: "lighter"
            });
        });
      </script>
    </head>

    <body>
      <h1 class="my">item - 0</h1>
      <h1>item - 1</h1>
      <h1 class="my">item - 2</h1>
      <h1>item - 3</h1>
      <h1 class="my">item - 4</h1>
    </body>
  </html>
  ```

  <br />

  - 메서드 체이닝 사용

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
          $("h3")
            .each((i, h3El) => {
              $(h3El).text("item - " + i);
            })
            .filter((i) => i % 3 == 0)
            .css({
              border: "1px solid black",

              fontSize: "34px",

              textDecoration: "underline"
            });
        });
      </script>
    </head>

    <body>
      <h3 class="my">item - 0</h3>
      <h3>item - 1</h3>
      <h3 class="my">item - 2</h3>
      <h3>item - 3</h3>
      <h3 class="my">item - 4</h3>
      <h3 class="my">item - 0</h3>
      <h3>item - 1</h3>
      <h3 class="my">item - 2</h3>
      <h3>item - 3</h3>
      <h3 class="my">item - 4</h3>
    </body>
  </html>
  ```

<br />

- on() 사용 및 css 컨트롤

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>

  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <style>
        .h1BgColor {
          background-color: silver;
        }
      </style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          // 각각의 h1을 클릭하면 경계선, 배경색이 입혀짐
          $("h1").on("click", function (event) {
            $(this).css({
              border: "1px solid black",

              backgroundColor: "lightpink"
            });
          });

          // div 버튼 클릭 시 h1 전부의 경계선 스타일 제거
          $("#btnInitCss").on("click", function () {
            $("h1").css("border", "none");
          });
        });
      </script>
    </head>

    <body>
      <div id="root">
        <h1>item - 0</h1>
        <h1>item - 1</h1>
        <h1>item - 2</h1>
      </div>

      <div id="btnInitCss">
        난 버튼이야 나를 클릭하면 h1의 경계선을 초기 상태로 되돌려줘
      </div>
    </body>
  </html>
  ```

<br />

- 메서드 체이닝 사용

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
          $("h3")
            .each((i, h3El) => {
              $(h3El).text("item - " + i);
            })
            .filter((i) => i % 3 == 0)
            .css({
              border: "1px solid black",
              fontSize: "34px",
              textDecoration: "underline"
            });
        });
      </script>
    </head>

    <body>
      <h3 class="my">item - 0</h3>
      <h3>item - 1</h3>
      <h3 class="my">item - 2</h3>
      <h3>item - 3</h3>
      <h3 class="my">item - 4</h3>
      <h3 class="my">item - 0</h3>
      <h3>item - 1</h3>
      <h3 class="my">item - 2</h3>
      <h3>item - 3</h3>
      <h3 class="my">item - 4</h3>
    </body>
  </html>
  ```

<br />

- 콜백함수 사용

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>

  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>Insert title here</title>

      <style>
        .h1BgColor {
          background-color: silver;
        }
      </style>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          // 각각의 h1을 클릭하면 경계선, 배경색이 입혀짐
          $("h1").on("click", function (event) {
            $(this).css({
              border: "1px solid black",
              backgroundColor: "lightpink"
            });
          });

          // div 버튼 클릭 시 h1 전부의 경계선 스타일 제거
          $("#btnInitCss").on("click", function () {
            $("h1").css("border", "none");
          });
        });
      </script>
    </head>

    <body>
      <div id="root">
        <h1>item - 0</h1>
        <h1>item - 1</h1>
        <h1>item - 2</h1>
      </div>

      <div id="btnInitCss">
        난 버튼이야 나를 클릭하면 h1의 경계선을 초기 상태로 되돌려줘
      </div>
    </body>
  </html>
  ```

<br />

- json 사용 1

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
        $(document).ready(function () {
          const jsonObj = { keyword: "value" };

          // object -> json
          console.log(JSON.stringify(jsonObj));

          // json -> object
          console.log(JSON.parse(JSON.stringify(jsonObj)));

          const jsonObj2 = { keyword: "키밸류로 적는다", newKeyword: 3 };

          // object
          console.log("점표기법: " + jsonObj2.keyword);
          console.log("대괄호표기법: " + jsonObj2["keyword"]);
          console.log("점표기법: " + jsonObj2.newKeyword);
          console.log("대괄호표기법: " + jsonObj2["newKeyword"]);

          console.log("==============================================");

          const jsonArrObj = { keyword: ["김", "유", "경", "4"] };

          // array
          console.log("점표기법: " + jsonArrObj.keyword);
          console.log("대괄호표기법: " + jsonArrObj["keyword"]);

          // array[i]
          for (let i = 0; i < jsonArrObj.keyword.length; i++) {
            console.log(jsonArrObj.keyword[i]);
          }

          console.log("==============================================");

          const jsonObjOfObj = { keyword: { kind: "포유류", 한글: "우수성" } };

          for (let key in jsonObjOfObj.keyword) {
            console.log(jsonObjOfObj.keyword[key]);
          }

          console.log("==============================================");

          // 단순 출력을 위한 메서드 체이닝 사용
          Object.values(jsonObjOfObj.keyword).forEach((val) =>
            console.log(val)
          );
        });
      </script>
    </head>

    <body></body>
  </html>
  ```

<br />

- json 사용 2

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>jsonTest1</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script type="text/javascript">
        $(document).ready(function () {
          $("#handleClick").click(() => {
            const menuObj = {};

            // object of key set
            const firstKey = $("input[name=drinkName]").attr("name");
            const firstKeyOfVal = [];

            $("input[name=drinkName]").each((i, obj) => {
              firstKeyOfVal.push($(obj).val());
            });

            // object of value set
            const secondKey = $("input[name=price]").attr("name");
            const secondKeyOfVal = [];

            $("input[name=price]").each((i, obj) => {
              secondKeyOfVal.push($(obj).val());
            });

            menuObj[firstKey] = firstKeyOfVal;
            menuObj[secondKey] = secondKeyOfVal;

            console.log(JSON.stringify(menuObj));
          });
        });
      </script>
    </head>

    <body>
      <input id="handleClick" type="button" value="json객체 생성" />

      <fieldset>
        <legend>메뉴판</legend>
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
      </fieldset>
    </body>
  </html>
  ```

<br />

- json 사용 3

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>jsonTest1</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script type="text/javascript">
        $(document).ready(function () {
          $("#handleClick").click(() => {
            const menuObj = {
              drinkName: $("input[name=drinkName]")
                .map((_, el) => $(el).val())
                .get(),

              price: $("input[name=price]")
                .map((_, el) => $(el).val())
                .get()
            };

            console.log(menuObj);
          });
        });
      </script>
    </head>

    <body>
      <input id="handleClick" type="button" value="json객체 생성" />

      <fieldset>
        <legend>메뉴판</legend>
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
      </fieldset>
    </body>
  </html>
  ```

<br />

- json 사용 4

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <title>jsonTest1</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script type="text/javascript">
        $(document).ready(function () {
          $("#handleClick").click(() => {
            const drinkNameArr = $("input[name=drinkName]");
            const priceArr = $("input[name=price]");
            const menuObj = {};

            for (let i = 0; i < drinkNameArr.length; i++) {
              menuObj[drinkNameArr[i].value] = priceArr[i].value;
            }

            console.log(menuObj);
          });
        });
      </script>
    </head>

    <body>
      <input id="handleClick" type="button" value="json객체 생성" />

      <fieldset>
        <legend>메뉴판</legend>
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
        <hr />
        음료명<input type="text" name="drinkName" /> 음료가격<input
          type="text"
          name="price"
        />
      </fieldset>
    </body>
  </html>
  ```

<br />

- json 파일, ajax 사용

  ```json
  {
    "employees": [
      {
        "firstName": "john",
        "lastName": "doe"
      },
      {
        "firstName": "anna",
        "lastName": "smith"
      },
      {
        "firstName": "대",
        "lastName": "상현"
      }
    ]
  }
  ```

  <br />

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>

  <!DOCTYPE html>
  <html lang="kor">
    <head>
      <meta charset="UTF-8" />
      <title>applyJsonAjaxBasic</title>

      <script
        src="https://code.jquery.com/jquery-3.7.0.js"
        integrity="sha256-JlqSTELeR4TLqP0OG9dxM7yDPqX1ox/HfgiSLBj8+kM="
        crossorigin="anonymous"
      ></script>

      <script>
        $(function () {
          $("#transfer").on("click", () => {
            const jsonKeyStr = $("#param").val();

            $.ajax({
              url: "./employeesDoc.json",
              type: "POST",
              dataType: "json",
              success: function (jsonObj) {
                let htmlStr = "";

                $.each(jsonObj[jsonKeyStr], (i, item) => {
                  htmlStr +=
                    i +
                    "&nbsp; : &nbsp;" +
                    item.firstName +
                    item.lastName +
                    "<br />";
                });

                $("#dic").html(htmlStr);
              }
            }); // ajax end
          });
        });
      </script>
    </head>

    <body>
      <button id="transfer">Click me</button>
      <input name="param" id="param" value="" />
      <div id="dic"></div>
    </body>
  </html>
  ```

<br />

- **`json`**

  - JSON.stringify(): object -> json

  - JSON.parse(): json -> object

  ```js
  let jsonStr = '{"employees":';
  jsonStr += "[";
  jsonStr += '{"firstName" : "john", "lastName" : "doe"},';
  jsonStr += '{"firstName" : "anna", "lastName" : "smith"},';
  jsonStr += '{"firstName" : "대", "lastName" : "상현"}';
  jsonStr += "]";
  jsonStr += "}";

  alert(jsonStr);
  alert(jsonStr.employees);

  const jsonObject = JSON.parse(jsonStr);

  alert(jsonObject);
  alert(jsonObject.employees);
  ```
