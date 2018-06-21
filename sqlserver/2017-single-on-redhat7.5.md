# SQLServer 2017 설치 및 환경구성

* 설치환경
    * RHEL 7.5
    * 참고자료: [SQL Server를 설치 하는 빠른 시작: Red Hat에 SQL Server를 설치하고 데이터베이스 만들기](https://docs.microsoft.com/ko-kr/sql/linux/quickstart-install-connect-red-hat?view=sql-server-linux-2017)
---

#### 1. yum 업데이트
  * yum update
  ```
  # yum update
  ```
  * 관리자도구설치
  ```
  # yum groupinstall 'Development Tools'
  ```
  
#### 2. 사용자 추가 및 권한 설정
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

#### 3. SQLServer 설치
  * repository 구성 파일 다운로드
  ```
  # su - infra
  $ sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
  ```
  
  * SQLSever 설치
  ```
  $ sudo yum install -y mssql-server
  ```
  
  * SQL Setup 실행
  ```
  $ sudo /opt/mssql/bin/mssql-conf setup
  ```
  * 방화벽 오픈
  ```
  $ sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
  $ sudo firewall-cmd --reload
  ```
  
  * 서비스 실행/확인
  ```
  $ sudo systemctl status mssql-server
  $ sudo journalctl -u mssql-server --no-pager | more
  $ sudo cat /var/opt/mssql/mssql.conf 
  ```