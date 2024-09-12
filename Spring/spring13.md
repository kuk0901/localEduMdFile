# Spring 14일차

## 이론

- SLF4J와 Logback을 이용한 로그 남기기

  - Java 진영에는 많은 로깅툴이 존재함

  - commons-logging, log4j, logback 등

  > 현재는 SLF4J나 Log4j2 등의 버전들을 사용

<br />

- LogBack logger / Appender 추가

<br />

- Logback Log의 패턴

  - %thread: 실행 스레드명

  - %msg: 로깅 내용

  - %n: 개행 문자

  - %-5level: 로깅 레벨 출력

    - 고정폭 5자리, 로깅 레벨이 info일 경우 빈칸 하나 추가

  - %D{yyyy-MM-dd HH:mm:ss}: 로깅하고 있는 현재시간, 시분초

  - %logger: 패키지 포함 클래스 정보

  - %method: 로깅하고 있는 클래스의 메소드

  - %line: 로깅하고 있는 소스의 line

<br />

- Logbak Log Level

  | head  | color  | description                                             |
  | ----- | ------ | ------------------------------------------------------- |
  | TRACE | GREEN  | DEBUG 레벨보다 좀 더 상세한 메시지 출력                 |
  | DEBUG | GREEN  | 디버깅을 위해 출력하는 메시지 출력                      |
  | INFO  | GREEN  | 상태 변경과 같은 정보 전달 목적의 메시지 출력           |
  | WARN  | YELLOW | 향후에 에러의 원인이 될 수 있는 경고성 메시지 출력      |
  | ERROR | RED    | 요청을 처리하는 과정에 발생한 치명적인 에러 메시지 출력 |

  <br />

  - 특정 로그 레벨을 지정하면 상위 로그가 모두 출력됨

    - 개발 단계: DEBUG나 TRACE로 설정

    - 운영 단계: WARN 이상으로 변경

<br />

## 코드

> RESTful API 자유게시판 삭제 기능 관련 코드는 다른 레포지토리 확인
