# Apache 배포하기

## 준비사항
- 서버로 사용될 pc 필요 (centOS 7)
- 개발이 완료된 웹 (현재는 React로 개발)

## 배포 순서
1. React로 개발된 Root 폴더에서 `npm rum build` 명령어를 이용하여 파일을 build 시킨다.
     - `npm run build` 명령어를 입력하면 Root폴더에 build라는 폴더가 생긴다.
     - build 폴더는 나중에 서버pc에 옮겨야 하기 때문에 잘 확인하자
     - 클라이언트 단에서 도메인을 입력하면 서버pc에서는 결국 이렇게 build된 파일을 전달하기 때문에 용량을 
       최대한 줄여줄 필요가 있다.

2. 터미널에서 원격으로 서버pc 접속 (예: `ssh younho@00.00.00.000`)
     - 서버pc에서 user를 생성해 주거나 user가 따로 없다면 Root폴더로 바로 이동
     - `[younho@localhost ~]$` 이런식으로 터미널이 바뀌면 원격으로 접속이 된 것이다.

3. centOS7에 Apache 설치
     - `sudo yum install httpd` 명령어로 설치
     - y를 눌러주면 설치 시작
     - 설치 후 `sudo systemctl start httpd` 명령어로 서버를 시작한다.
     - `sudo systemctl status httpd` 명령어로 서버가 돌아가는지(active / running)확인
     - 서버를 재실행 하고 싶다면 `sudo systemctl restart httpd`

4. 포트 열어주기
     - `sudo yum install firewalld` 명령어로 방화벽 설치
     - Apache가 사용하는 80포트로 접속하려면 해당 포트를 열어 주어야 한다. (CentOS 7.0 이상)
        - sudo firewall-cmd --zone=public --permanent --add-port=80/tcp //80포트 개방
        - sudo firewall-cmd --reload // 방화벽 리로드
        - sudo firewall-cmd --zone=public --list-all //열린 포트 확인
  
5. FileZilla 다운로드 및 연결

 <img src="./../Image/filezilla%20다운.png" width="600px" height="400px" alt="filezilla down"></img></br>

  - local에 있는 build 폴더를 서버pc로 옮겨준다.

      <img src="./../Image/filezilla%20서버pc%20연결.png" width="600px" height="400px" alt="filezilla 서버pc 연결"></img></br> 

      1. 좌측 상단의 아이콘을 클릭하면 사이트 관리자가 나오고 프로토콜, 호스트(서버pc 도메인), 사용자, 비번을 입력하고 연결을 클릭한다.
      2. 연결을 하게 되면 아래와 같이 긍정적인 메세지가 나오게 된다. 그럼 연결이 된 것이다.
   
      <img src="./../Image/filezilla%20연결%20성공.png" width="200px" height="100px" alt="filezilla 연결 성공"></img>

      1. 아래 이미지를 보면 좌측은 local의 디렉토리이고 우측은 서버pc의 디렉토리이다. 드래그로 파일을 
         옯기면 된다.

      <img src="./../Image/filezilla%20파일%20전송.png" width="750px" height="600px" alt="filezilla 파일 전송"></img></br>

  - 서버pc에 원격으로 접속하여 확인해보면 드래그한 위치에 build 폴더가 있을 것이다.

6. 서버pc의 Root 폴더 찾기
     - Root 폴더는 보통 `/var/www/html`에 위치한다. 
     - 확인 방법은 `cd /var/www/html` 명령어를 통하여 Root폴더로 이동 후 index.html 파일을 만들고 
       `sudo vi index.html`명령어로 파일에 들어간 후 간단한 글을 쓰고 저장(`: -> wq`)한다. 그리고 
       서버pc의 도메인을 들어가 보면 내가 작성한 글이 보인다. 그럼 성공!

7. build 폴더를 서버pc의 Root폴더로 이동
      - 폴더 이동 명령어는 `sudo cp -r 이동폴더이름 /var/www/html` 이다.
  
8. Apache 설정 변경
     -  `sudo vi /etc/httpd/conf/httpd.conf` 명령어를 통하여 설정파일로 이동

         <img src="./../Image/apache%20conf%20파일.png" width="650px" height="550px" alt="apache 설정 파일"></img></br>

     - 설정 창을 내리다 보면 Directory 관련 부분이 나온다. 아래 이미지처럼 수정을 하면 되는데
       기본적으로 `/var/www/html` 로 설정 되어 있다. build 폴더는 `/var/www/html/build`에 
       위치해 있기 때문에 이렇게 설정을 해주는 것이다.
     -  두 번째 Directory 설정에서 `Require all granted`를 추가하여 요청을 허용해주어야 한다.

         <img src="./../Image/apache%20conf%20파일%20설정%20수정.png" width="650px" height="550px" alt="apache 설정 수정"></img></br>

9. 접근 권한 에러 해결 방법

<img src="./../Image/apache%20접근권한%20에러.png" width="250px" height="150px" alt="apache 접근권한 에러"></img></br>
   
   1. User / Group 확인 
      - `sudo vi /etc/httpd/conf/httpd.conf` 에서 확인
      -  <img src="./../Image/apache%20user:group%20확인.png" width="600px" height="400px" alt="apache user/group 확인"></img></br>
      - 기본값은 apache로 되어 있을 것이다. 
      - conf 설정의 User / Group 권한과 build 폴더의 권한을 맞춰 주어야 한다. 
      - 나는 일단 테스트로 서버에 올려보는 것이기 때문에 User / Group의 권한을 younho로 맞출 것이다.
      - 보안을 위해서는 권한을 apache로 맞춰서 진행하는 것이 좋다고 한다.

   2. 8번의 Apache 설정 변경에서 경로 변경을 한 번더 확인 해보자. 현재 폴더의 위치를 정확하게 입력해줘야 하기 때문에 혹시 build 폴더의 위치가 변경 되었다면 
      현재 위치의 올바른 경로를 입력해준다.
   
   3. build 폴더 권한 확인 및 수정
  
      - <img src="./../Image/apache%20build폴더%20권한%20확인.png" width="150px" height="150px" alt="apache buil폴더 권한 확인"></img></br>
  
      - 위에서 설명한 바와 같이 conf설정과 맞춰 주어야 하는데 변경할 경우 아래의 명령어로 변경한다.
      - `sudo chown -R younho ./younho` -> younho 디렉토리의 하위에 있는 파일 전체의 권한을 younho로 바꾼다는 의미
      
   4. 리눅스 권한 비활성화
      - 이 방법은 실제 서비스에서는 권장하지 않는 방법으로 웹이 서버에 잘 올라갔는지 확인하는 용도로만 사용하자
      - 리눅스는 기본적으로 `enforce, permissive, disable` 이렇게 3가지 모드가 있는데 default값으로 `enforce`값이 적용되어 있다.
        이 값을 `permissive`값으로 변경해주면 정책에 어긋나는 동작은 감사 로그를 남기고 허용해준다. 이렇게 일시적으로 허용하게 해주면 
        접근이 가능하게 되어 서버에 올라갔는지 확인이 가능해진다
