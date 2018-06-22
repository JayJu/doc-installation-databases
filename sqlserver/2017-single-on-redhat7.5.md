# SQLServer 2017 설치 및 환경구성

* 설치환경
    * RHEL 7.5
    * 참고자료: [SQL Server를 설치 하는 빠른 시작: Red Hat에 SQL Server를 설치하고 데이터베이스 만들기](https://docs.microsoft.com/ko-kr/sql/linux/quickstart-install-connect-red-hat?view=sql-server-linux-2017)
---

* 주의
  * **SQLServer 서비스 시작을 휘한 최소 메모리는 2G 이므로 OS메모리가 적어도 3G는 되어야 함.**
  
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

#### 3. SQL Server 설치
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
  
#### 4. SQL Server command line tools 설치
  * repository 구성 파일 다운로드
  ```
  # su - infra
  $ sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
  ```
  
  * 이전 버전의 mssql-tools 가 설치된 경우 삭제
  ```
  $ sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
  ```

  * unixODBC 개발자 패키지와 mssql-tools 설치
  ```
  $ sudo yum install -y mssql-tools unixODBC-devel
  ```
  
  * path 환경변수 추가
  ```
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
  echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
  source ~/.bashrc
  ```
  
  * as 로 로그인
  ```
  $ sqlcmd -S jhjdb1 -Usa -Pp@ssw0rd
  1> select @@servername
  2> go
  3> jhjdb1
  1> exit
  ```  
---

## 환경설정방법
1. 파라미터 설정
  * mssql-conf set 파라미터명 파라미터값
  ```
  $ sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
  ```
  * 서비스 재시작
  ```
  $ sudo systemctl restart mssql-server
  ```
2. 파라미터 제거
  * mssql-conf unset 파라미터명
  ```
  $ sudo /opt/mssql/bin/mssql-conf unset memory.memorylimitmb
  ```
  * 서비스 재시작
  ```
  $ sudo systemctl restart mssql-server
  ```
3. 
