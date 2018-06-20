# SQLServer 2017 설치 및 환경구성

* 설치환경
    * RHEL 7.4
    * 참고자료: [SQL Server를 설치 하는 빠른 시작: Red Hat에 SQL Server를 설치하고 데이터베이스 만들기](https://docs.microsoft.com/ko-kr/sql/linux/quickstart-install-connect-red-hat?view=sql-server-linux-2017)
---

#### 1. 사용자 추가 및 권한 설정
  * 사용자 추가
  ```
  # useradd infra -m -d /home/infra
  # passwd infra
  ```
  * sudoer 등록
  ```
  # visudo
  ## Allow root to run any commands anywhere
  root    ALL=(ALL)       ALL
  infra     ALL=(ALL)       ALL
  ```
