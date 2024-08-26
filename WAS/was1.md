## WAS(Web Application Server)

<br />

- 아파치 톰캣(프로그램)

  - 툴이 필요없음

  - 개발자 툴이 필요하다면 그건 개발을 위해서임

<br />

- 아카이브(백업용)

- WAR file(서버에 올릴 수 있음)

  - export source file: 소스 파일이 모두 들어가 있는 파일(사용 X)

<br />

- server.xml

  ```xml
  <!-- 기본적으로 설정(포트, 인코드이) -->
  <Connector port="8090" URIEncoding="UTF-8" />
  ```
