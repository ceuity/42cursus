# ft_server

## **Introduction**

- This topic is intended to introduce you to system administration.
- It will make you aware of the importance of using scripts to automate your tasks.
- For that, you will discover the "docker" technology and use it to install a complete web server.
- This server will run multiples services: Wordpress, phpmyadmin, and a SQL database.

## **General instructions**

- You must place all the necessary files for the configuration of your server in a folder called srcs.
- Your Dockerfile file should be at the root of your repository. It will build your container. You can’t use docker-compose.
- All the necessary files for your WordPress website should be in the folder srcs.

## **Mandatory Part**

- You must set up a web server with Nginx, in only one docker container. The container OS must be debian buster.
    - Your web server must be able to run several services at the same time. The services will be a WordPress website, phpmyadmin and MySQL. You will need to make sure your SQL database works with the WordPress and phpmyadmin.
    - Your server should be able to use the SSL protocol.
    - You will have to make sure that, depending on the url, your server redirects to the correct website.
    - You will also need to make sure your server is running with an autoindex that must be able to be disabled.

---

## 들어가며

> 이 글은 42seoul의 ft_server 과제를 해결하기 위한 자료로 작성되었습니다.

참고문서(kkang) [https://github.com/Kkan9ma/42cursus/tree/master/02_ft_server_docs](https://github.com/Kkan9ma/42cursus/tree/master/02_ft_server_docs)

> 작동 환경 : masOS(Catalina) 10.15.5 / Windows10 WSL2 Ubuntu

---

## 목차

1. Docker 설치

2. Container  생성

3. nginx 설치

4. SSL Protocol 사용

5. php-fpm 설치

6. MySQL과 phpmyadmin 설치

7. Wordpress 설치

8. Autoindex 적용

---

1. Docker 설치
    - Docker Desktop 버전을 설치한다.
        - [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
        - [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
2. Container  생성
    - Docker Container는 Docker image를 바탕으로 생성된다.
    - Docker image를 받은 후 `docker run` 명령어로 container를 생성할 수 있다.
    - Docker image를 받기 위해서는 `docker pull` 명령어를 사용하여 image를 다운로드 할 수 있다.

    **2.1 debian image pull 하기**

    - Mandatory Part에서 container의 OS는 `debian buster` 여야 한다고 명시되어 있다. 따라서 `debian buster` image를 pull 하여 container를 생성한다.
    - debian buster는 debian OS의 하나의 버전으로 debian 10에 해당하는 버전 이름이다.
        - [https://en.wikipedia.org/wiki/Debian_version_history](https://en.wikipedia.org/wiki/Debian_version_history)
    - Docker에선 `pull` 명령어를 사용하여 image를 다운로드 할 수 있다.
    - 처음 `docker images` 명령어를 입력하면 아무것도 없는 것을 확인할 수 있다.

        ![ft_server/images/Untitled.png](ft_server/images/Untitled.png)

    - `docker pull debian:buster` 명령어를 입력하여 image를 다운로드 한다.

        ![ft_server/images/Untitled%201.png](ft_server/images/Untitled%201.png)

    **2.2 image 확인 및 containner 생성하기**

    - image 확인
        - `docker images` 명령어로 잘 다운로드 되었는지 확인한다.

            ![ft_server/images/Untitled%202.png](ft_server/images/Untitled%202.png)

    - container 생성
        - `docker run -it -p 80:80 -p 443:443 —name test debian:buster` 명령어로 container를 생성한다.

            ![ft_server/images/Untitled%203.png](ft_server/images/Untitled%203.png)

            ```docker
            - docker run 명령어 형태
            - `$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]` (도커 홈페이지)

            [OPTIONS]
            -i : Keep STDIN open even if not attached (interactive의 약자로 docker 내부에서 표준입력으로 command를 입력받을 수 있도록 한다.)
            -t : Allocate a pseudo-TTY (가상의 터미널을 사용)
            --name : 컨테이너의 이름을 정하는 역할. 본 예시에선 test로 명명한다.
            -p : Publish a container's port(s) to the host (컨테이너와 호스트의 포트를 연결해준다)
            ```

        - http : 80번 port 사용
        - https : 443번 port 사용
    - container 확인
        - 다른 터미널 탭을 띄워 `docker ps` 명령어를 통해 현재 container 상태를 확인할 수 있다.

            ![ft_server/images/Untitled%204.png](ft_server/images/Untitled%204.png)

        - container 에 접속한 상태에서 exit 명령어를 통하여 container 를 종료할 수 있다. 종료했을 경우에 `docker ps -a` 명령어를 통해 종료된 것을 확인할 수 있다.

            ![ft_server/images/Untitled%205.png](ft_server/images/Untitled%205.png)

        - container를 종료했을 때, container에 다시 접속하고 싶다면 아래 명령어로 가능하다.

            ```docker
            (1)
             - docker start 'CONTAINER ID'
             - docker attach 'CONTAINER ID'

            (2)
             - docker start -i 'CONTAINER ID'
            ```

3. nginx 설치
    - APT(Advanced Packaging Tool)을 이용하여 container 내부에서 과제를 해결하는데 필요한 프로그램들을 설치해준다.
    - `apt-get update -y`, `apt-get upgrade -y` 명령어를 통해 APT를 최신버전으로 업데이트 해준다.

        ![ft_server/images/Untitled%206.png](ft_server/images/Untitled%206.png)

    **3.1 nginx 설치하기**

    - `apt-get install nginx -y` 명령어로 nginx 를 설치해준다.

    **3.2 nginx 시작, 상태 조회하기**

    - nginx는 항상 background에서 작동하고 있어야 하는 프로그램이다.
    - `etc/init.d` 폴더에 daemon 형태로 프로그램을 저장해둔다.
    - daemon 형태로 저장된 프로그램을 조작하는 명령어는 `service` 이다.

        ```docker
        service nginx start // nginx 시작
        service nginx status // nginx 상태 조회
        ```

    - service  명령어를 이용하여 nginx 를 시작한 후, 브라우저에서 [`localhost`](http://localhost) 또는 [`localhost:80`](http://localhost:80) 으로 접속하여 상태를 확인한다.

        ![ft_server/images/Untitled%207.png](ft_server/images/Untitled%207.png)

    - 해당 페이지가 제대로 뜨지 않을 경우, `https` 로 접속하지 않았는지 확인
    - 만약 해당 방법으로도 조회가 되지 않을 경우, `apt-get install curl` 명령어를 이용하여 `curl` 설치하여 `curl localhost` 명령어를 사용하여 소스가 조회되는지 확인한다.

        ![ft_server/images/Untitled%208.png](ft_server/images/Untitled%208.png)

4. SSL Protocol 사용

    **4.1 SSL Protocol 이란?**

    - 전송 계층 보안(Transport Layer Security, TLS, 과거 명칭: 보안 소켓 레이어/Secure Sockets Layer, SSL)은 컴퓨터 네트워크에 통신 보안을 제공하기 위해 설계된 암호 규약이다.
    - 자세한 내용은 [링크](https://www.opentutorials.org/course/228/4894) 참조

    **4.2 SSL 인증서 만들기**

    - 인증서는 공개키 기반 구조이므로, 개인키부터 만들어 진행해야 한다.
    - 인증서를 만드는 과정은 `개인키 생성, CSR 만들기, 인증서 만들기` 순서로 진행한다.
        - CSR은 Certificate Signing Request(인증서 서명 요청)이란 뜻으로, 인증서 발급을 위한 필요한 정보를 담고 있는 인증서 신청 형식 데이터이다. ([출처](https://m.blog.naver.com/PostView.nhn?blogId=swh0506&logNo=30186498871&proxyReferer=https:%2F%2Fwww.google.com%2F))
        - CSR에 포함되는 내용으로는 개인키 생성 단계에서 만들어진 개인키(Private Key)와 공개키(Public Key)의 키쌍 중에서 공개키가 포함되며, 인증서가 적용되는 도메인에 대한 정보 등이 포함된다.
    - CSR(인증서 서명 요청 : Certificate Signing Request)을 만들기위해 openssl을 설치한다.

        `apt-get install openssl -y`

    - openssl 명령어 구조

        `openssl req -out <CSR 파일> -keyout <개인키 파일> rsa:<키 비트 수>`

        ```docker
        openssl req -newkey rsa:4096 -nodes -x509 -keyout localhost.dev.key -out localhost.dev.crt -days 365 -subj "/C=KR/ST=Seoul/L=Seoul/O=42Seoul/OU=Hyulee/CN=localhost"
        ```

    - openssl 옵션
        - req : 주로 PKCS#10(Public key Cryptography Standard 공개 키 암호 표준) 인증서 요청을 만들고 처리
        - newkey : 새로운 인증서 요청 + 새로운 private key 생성
            - cf) -new: openssl req -new -key <개인키> -out <CSR 파일>
        - RSA : 원하는 암호화 비트수 (일반적으로 2048 또는 4096)
        - nodes : no DES(대칭키 암호 알고리즘 Data Encryption Standard), 암호 사용하여 개인키를 보호하지 않음
        - X.509: 공개키 인증서와 인증 알고리즘을 사용하기 위한 PKI 표준
        - subj: 암호 입력시 들어갈 정보
            - subj를 옵션으로 기재하지 않으면, 커맨드 실행 시 위 정보를 입력하는 창이 나타난다.
        - keyout: `.key` 파일명을 지정
        - out: `.crt` 파일명을 지정

    ![ft_server/images/Untitled%209.png](ft_server/images/Untitled%209.png)

    **4.3 Nginx에 SSL Protocol을 추가하기**

    - 우선 `mv` 명령어로 만든 키를 지정된 위치로 옮긴 후, 보안을 위해 권한 설정을 해준다.

        ```docker
        mv localhost.dev.crt etc/ssl/certs/
        mv localhost.dev.key etc/ssl/private/
        chmod 600 etc/ssl/certs/localhost.dev.crt etc/ssl/private/localhost.dev.key
        ```

    - 현재 http로는 접속 가능하나 https로의 접속은 불가능한 상태이다. 따라서 https로 접속을 가능하게 하기 위해 nginx 설정을 바꾸어준다.
        - nginx 설정 관련 폴더
            - sites-available
                - 설정을 저장하는 곳이다. 이곳에 저장한 설정은 실제로 nginx에 반영되지는 않는다.
                - 따라서 여기에 만든 설정을 sites-enabled에 복사 또는 심볼릭링크를 걸어서 반영한다.
            - sites-enabled
                - sites-available에 저장한 설정을 적용하기 위한 폴더.

    - `/etc/nginx/sites-available/default` 파일을 수정하여 SSL Protocol 설정 뿐만 아니라 http로 접속했을 때, https로 redirection 해주는 작업까지 필요로 한다.

        ```docker
        server {
        	listen 80 default_server;
        	listen [::]:80 default_server;
        	server_name localhost;
        	return 301 https://$server_name$request_uri;

        	root /var/www/html;

        	# Add index.php to the list if you are using PHP
        	index index.html index.htm index.php;

        	location / {
        		# First attempt to serve request as file, then
        		# as directory, then fall back to displaying a 404.
        		try_files $uri $uri/ =404;
        	}

        	# pass PHP scripts to FastCGI server
        	#
        	#location ~ \.php$ {
        	#	include snippets/fastcgi-php.conf;
        	#
        	#	# With php-fpm (or other unix sockets):
        	#	fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        	#	# With php-cgi (or other tcp sockets):
        	#	fastcgi_pass 127.0.0.1:9000;
        	#}

        	# deny access to .htaccess files, if Apache's document root
        	# concurs with nginx's one
        	#
        	#location ~ /\.ht {
        	#	deny all;
        	#}
        }

        server {
        	# SSL configuration
        	
        	listen 443;
        	listen [::]:443;
        	
        	ssl on;
        	ssl_certificate /etc/ssl/certs/localhost.dev.crt;
        	ssl_certificate_key /etc/ssl/private/localhost.dev.key;

        	root /var/www/html;

        	# Add index.php to the list if you are using PHP
        	index index.html index.htm index.php;

        	server_name _;

        	location / {
        		# First attempt to serve request as file, then
        		# as directory, then fall back to displaying a 404.
        		try_files $uri $uri/ =404;
        	}

        	# pass PHP scripts to FastCGI server
        	#
        	# location ~ \.php$ {
        	#	#	include snippets/fastcgi-php.conf;
        	#	# With php-fpm (or other unix sockets):
        	#	fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        	#	# With php-cgi (or other tcp sockets):
        	#	fastcgi_pass 127.0.0.1:9000;
        	}

        	# deny access to .htaccess files, if Apache's document root
        	# concurs with nginx's one
        	#
        	#location ~ /\.ht {
        	#	deny all;
        	#}
        }
        ```

        - [Nginx에서 자동 Redirection 설정하기](https://www.tuwlab.com/ece/26993)
    - 설정 후 `service nginx reload` 명령어를 통해 설정이 적용된 nginx를 다시 불러온다.
    - http로 접속해도 https로 접속되는 것을 확인할 수 있다.
    - 접속했을 때 경고가 뜨는 원인은 공인된 인증서가 아니기 때문이다.
5. php-fpm 설치

    서버와 다른 프로그램간의 상호작용을 위해 먼저 php-fpm을 설치한다.

    **5.1 PHP 란?**

    - php는 프로그래밍 언어의 일종으로, 원래는 동적 웹 페이지를 만들기 위해 설계되었으며 이를 구현하기 위해 PHP로 작성된 코드를 HTML 소스 문서 안에 넣으면 PHP 처리 기능이 있는 웹 서버에서 해당 코드를 인식하여 작성자가 원하는 웹 페이지를 생성한다. 근래에는 PHP 코드와 HTML을 별도 파일로 분리하여 작성하는 경우가 일반적이며, PHP 또한 웹서버가 아닌 php-fpm(PHP FastCGI Process Manager)을 통해 실행하는 경우가 늘어나고 있다.

    **5.1.1 php-fpm 이란?**

    - php-fpm( PHP FastCGI Process Manager)
    - php-fpm은 FastCGI다.
    - 요청할 때마다 새로운 프로세스 생성하는 CGI는 상대적으로 느리다.
    - 그래서 요청할 때마다 새로운 프로세스를 생성하는 것이 아니라 이미 생성한 프로세스를 재활용하는 방법을 사용하는 것이 고안되었다.
    - 즉, CGI보다 좀 더 빠른 버전이라고 할 수 있다.
    - [출처](https://seunguklee.github.io/2017/11/03/php-fpm/)
    - php-fpm는 php를 FastCGI 방식으로 동작하도록 하는 솔루션인데, 주로 Nginx와 함께 사용된다.

    **5.2 CGI 란?**

    - CGI(Common Gate Interface)란 서버와 외부 스크립트 또는 프로그램과 상호작용할 때 이루어지는 입출력을 정의한 표준이다.
    - 이 표준에 맞추어 만들어진 것이 CGI 스크립트 또는 CGI 프로그램으로, CGI 프로그램은 어떤 프로그래밍 언어로도 만들 수 있다.
    - 두 개 이상의 컴퓨터간의 자료들을 주고받는 프로그램 또는 주고받는 것 자체를 의미한다고 할 수 있다.
    - 웹페이지는 HTML언어에 의해서 기본적으로 만들어진다. 하지만 HTML만으로 모든 정보를 다 처리할 수는 없다. 왜냐하면 HTML언어는 서버로부터 HTML문서를 보여주는 역할만 할 뿐, 사용자의 동작을 바로 반영하여 업로드할 수는 없기 때문이다.
    - 따라서 홈페이지를 서버-클라이언트 모두 양방향으로 구성할 필요성이 있는 것이다.
    - 이를 해결한 여러 방법 중 하나가 외부 프로그램을 수행하여 그 결과를 HTML형태로 보여주는 방식인 CGI다.
    - 넓은 의미로는 CGI를 수행하는 프로그램을 CGI라고 하기도 한다.
    - 그 대표적인 예가 방명록, 게시판, 메모장 등이다.

    **5.3 php-fpm 설치하기**

    - `apt-get install php-fpm` 명령어로 php-fpm을 설치한다.
    - 설치가 완료되었으면, nginx의 default 파일을 수정하여 php-fpm과 연동한다.

    **5.4 nginx 에 적용하기**

    - `apt-get install php-fpm` 명령어로 php-fpm을 설치한다.
    - 설치가 완료되었으면, nginx의 default 파일을 수정하여 php-fpm과 연동한다.

        ![ft_server/images/Untitled%2010.png](ft_server/images/Untitled%2010.png)

    - fastcgi_pass 편집 시, 설치된 php-fpm의 버전과 맞게 기재해주어야 한다. 이번 케이스에선 설치된 php-fpm의 버전은 7.3이므로, php7.3-fpm.sock으로 설정하였다.
    - 또한 현재 상황은 redirection된 경우이기 때문에 https 설정인 listen 443이 있는 블록에 설정해주어야 한다.

    **5.5 적용되었는지 확인하기**

    - `service php7.3-fpm start` 명령어로 php-fpm 을 실행시킨다.
    - phpinfo()는 PHP 정보와 설정을 표로 정리해서 보여주는 함수이다.
    - `echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php` 명령어를 통해 `/var/www/html`에 `phpinfo.php` 라는 파일을 만들어 내용을 작성한다.
    - `service nginx reload` 명령어로 nginx를 재시작 한다.
    - [https://localhost/phpinfo.php](https://localhost/phpinfo.php) 로 접속하여 잘 적용되었는지 확인한다.

        ![ft_server/images/Untitled%2011.png](ft_server/images/Untitled%2011.png)

6. MySQL과 phpmyadmin 설치
    - SQL Database가 Wordpress와 phpmyadmin과 연동될 수 있도록 MySQL과 phpmyadmin을 설치한다.

    **6.1 MySQL 이란?**

    - MySQL은 데이터베이스 관리 시스템이다.
    - 데이터베이스 관리 시스템(영어: database management system, DBMS)은 다수의 사용자들이 데이터베이스 내의 데이터를 접근할 수 있도록 해주는 소프트웨어 도구의 집합이다.
    - DBMS은 사용자 또는 다른 프로그램의 요구를 처리하고 적절히 응답하여 데이터를 사용할 수 있도록 해준다.

    **6.2 phpmyadmin 이란?**

    - phpMyAdmin은 MySQL을 월드 와이드 웹 상에서 관리할 목적으로 PHP로 작성한 오픈 소스 도구이다.
    - 데이터베이스, 테이블, 필드, 열의 작성, 수정, 삭제, 또 SQL 상태 실행, 사용자 및 사용 권한 관리 등의 다양한 작업을 수행할 수 있다. 특히 웹 호스팅 서비스를 위한 가장 대중적인 MySQL 관리 도구 가운데 하나가 되었다.

    **6.3 설치하기**

    **6.3.1 MariaDB(MySQL) 설치**

    - Debian OS에서는 MySQL과 거의 동일한 프로그램인 MariaDB를 default로 사용하고 있다. 따라서 MariaDB를 설치한다.
    - `apt-get install mariadb-server php-mysql` 명령어로 MariaDB를 설치할 수 있다.

    **6.3.2 phpmyadmin 설치**

    - phpmyadmin은 APT로 설치할 수 없다. 따라서 wget이라는 프로그램을 이용하여 URL로 부터 파일을 다운받아서 설치한다.

    ```docker
    apt-get install wget // 명령어로 wget을 설치한다.
    wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
    tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz // 명령어로 압축을 푼다.
    ```

    - tar 명렁어의 주요 옵션은 아래와 같다.

    ```
          - 기본 형식: tar [OPTION...] [FILE]...
          - 옵션
              -f     : 대상 tar 아카이브 지정. (기본 옵션)
              -c     : tar 아카이브 생성. 기존 아카이브 덮어 쓰기. (파일 묶을 때 사용)
              -x     : tar 아카이브에서 파일 추출. (파일 풀 때 사용)
              -v     : 처리되는 과정(파일 정보)을 자세하게 나열.
              -z     : gzip 압축 적용 옵션.
              -j     : bzip2 압축 적용 옵션.
              -t     : tar 아카이브에 포함된 내용 확인.
              -C     : 대상 디렉토리 경로 지정.
              -A     : 지정된 파일을 tar 아카이브에 추가.
              -d     : tar 아카이브와 파일 시스템 간 차이점 검색.
              -r     : tar 아카이브의 마지막에 파일들 추가.
              -u     : tar 아카이브의 마지막에 파일들 추가.
              -k     : tar 아카이브 추출 시, 기존 파일 유지.
              -U     : tar 아카이브 추출 전, 기존 파일 삭제.
              -w     : 모든 진행 과정에 대해 확인 요청. (interactive)
              -e     : 첫 번째 에러 발생 시 중지.
    ```

    - phpmyadmin을 설치할 때 파일의 이름은 전통적으로 `phpmyadmin`으로 지어졌다.
    - [출처](https://swiftcoding.org/installing-phpmyadmin#sql-tool)
    - `mv phpMyAdmin-5.0.2-all-languages /var/www/html/phpmyadmin` 명령어로 phpmyadmin의 폴더명 변경과 폴더 이동을 해준다.
    - `rm phpMyAdmin-5.0.2-all-languages.tar.gz` 명령어로 불필요한 압축파일은 삭제해준다.

    **6.4 phpmyadmin 설정**

    - phpmyadmin 내에 config.sample.inc.php 의 샘플 파일을 복사하여 config.inc.php 파일을 만든다.

    **6.4.1 복사하기**

    - `cp /var/www/html/phpmyadmin/config.sample.inc.php /var/www/html/phpmyadmin/config.inc.php`

    **6.4.2 편집하기**

    - `vim /var/www/html/phpmyadmin/config.inc.php`
    - 파일 내부의 내용 중, blowfish_secret(암호화 문자열) 부분에 대한 설정이 필요하다.
        - config.inc.php 파일 내 `$cfg['blowfish_secret'] = ''; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */` 부분을 편집할 것이며, `''`에 비밀번호를 넣으면 된다.
        - 암호화 문자열은 hash 방식으로 만들어진 password를 넣어야 하며, [링크](http://www.passwordtool.hu/blowfish-password-hash-generator) 등을 통해 만들 수 있다.

            ![ft_server/images/Untitled%2012.png](ft_server/images/Untitled%2012.png)

    **6.4.3 설정 적용하기**

    - 지금까지 한 설정을 적용하기 위해 필요한 service들을 reload 해준다.

    ```docker
    service nginx reload
    service php7.3-fpm restart
    ```

    **6.5 데이터 테이블 만들기**

    - 이제 Datatable을 만들기 위해서 `service mysql start` 명령어로 MySQL을 작동시킨다.

    **6.5.1 MySQL에 테이블 불러오기**

    - MySQL로 데이터를 관리하기 위해 `mysql < var/www/html/phpmyadmin/sql/create_tables.sql -u root --skip-password` 명령어로 테이블을 불러온다.
        - `<` 로 외부 `.sql` 파일에서 데이터를 불러올 수가 있다.
            - table을 새로 만들어주는 sql문이 이미 들어있기에 이를 사용한다.
            - [출처](https://jybaek.tistory.com/316)
        - `-u` 옵션으로 user는 root로 설정한다.
        - `--skip-password`

    **6.5.2 MySQL 서버 관리하기**

    - mysqladmin -u root -p password 명령어로 MySQL 계정의 패스워드를 설정해준다.
        - `mysqladmin` - MySQL 서버를 관리하기 위한 클라이언트이다.
        - 주로 관리 연산을 수행하는 클라이언트로, 서버의 구성 및 현재의 상태를 체크하거나 데이터 베이스를 생성 및 제거하기 위해 사용한다.
        - `u` 옵션: 서버 접속 시 사용하는 mysql 사용자 이름
        - `p` 옵션: 서버 접속 시 사용하는 패스워드.
            - 설치 직후엔 root 사용자에 password가 없어 위처럼 사용 가능하다.
    - 기존 비밀번호는 없으니 enter로 넘어가고, new password와 confirm new password 창엔 원하는 비밀번호를 입력한다.
    - 입력하지 않는 경우엔 error 발생 가능.
    - (참고)단, 비밀번호 설정을 하지 않는 방법도 있다.
        - `var/www/html/phpmyadmin/config.inc.php` 내용 중, `AllowNoPassword`를 default인 `false`에서 `true`로 변경해주면 비밀번호 없이 접속이 가능하다.

    **6.5.3 MySQL 접속하기**

    - command line에 `mysql`을 입력하여 MySQL에 접속한다. 단, 패스워드가 있을 경우, `mysql -p${PASSWORD}`로 접속할 수 있다.

    ```docker
    show databases; // database 조회
    CREATE DATABASE IF NOT EXISTS wordpress; // 워드프레스를 위한 DB 만들기
    Grant all privileges on *.* to ‘user’@‘%’ identified by ‘설정한비밀번호’ with grant option;
    flush privileges;
    show databases; // 변경되었는지 조회
    exit
    ```

    ![ft_server/images/Untitled%2013.png](ft_server/images/Untitled%2013.png)

    - MySQL 문법
        - `GRANT 권한 ON 데이터베이스.테이블 TO '아이디'@'호스트' IDENTIFIED BY '비밀번호'`
        - all: grant option을 제외한 모든 권한
        - privileges: 권한
        - ``를 사용하면 모든 데이터베이스, 테이블을 제어 대상으로 함 `(*.*,class.*)`
            - [출처](https://wayhome25.github.io/mysql/2017/03/23/mysql-11-user-setting/)
        - `아이디@호스트` 중에서 호스트는 접속자가 사용하는 머신의 IP를 의미한다. IP를 특정하지 않으려면 `‘%’`를 사용
        - `dev@123.100.100.100` : IP 123.100.100.100인 머신에서 접속한 ID dev
        - `dev@%` : IP 관계없이 ID가 dev인 사용자
            - ip 관계없이 id가 user이고 비밀번호가 ~~인 사용자에게 권한을 부여한다.
        - `flush privileges;`
            - 보통은 INSERT, DELETE, UPDATE를 통해 사용자를 추가, 삭제, 권한 변경 등을 수행하였을 때 이 변경 사항을 반영하기 위하여 사용한다.
            - 이 때 `FLUSH PRIVILEGES`는 grant 테이블을 reload함으로서 변경 사항을 즉시 반영하도록 한다.
            - [출처](https://sarc.io/index.php/mariadb/355-mysql-flush-privileges)

    **6.5.4 phpmyadmin 작동 확인**

    - `service nginx reload` 명령어로 변경된 사항을 다시 적용한다.
    - [localhost/phpmyadmin](http://localhost/phpmyadmin) 에 접속하여 확인한다.
    - 아이디는 root, 비밀번호는 mysql에서 설정한 비밀번호이다.

    ![ft_server/images/Untitled%2014.png](ft_server/images/Untitled%2014.png)

7. Wordpress 설치

    **7.1 Wordpress 설치하기**

    - Wordpress도 마찬가지로 wget을 이용하여  설치를 진행한다.

    ```docker
    wget https://wordpress.org/latest.tar.gz
    tar -xvf latest.tar.gz
    mv wordpress/ var/www/html/
    chown -R www-data:www-data /var/www/html/wordpress
    ```

    - `chown`: chown은 파일을 소유하는 유저와 그룹을 변경하기 위해서 사용한다. ([출처](https://www.joinc.co.kr/w/man/1/chown))
        - 브라우저에서 `wordpress` 를 사용하기 위해 폴더에 접근권한을 바꾸어준다.
    - [chown -R 관련 링크](https://twpower.github.io/64-use-chown-to-subfiles-and-subfolders)

    **7.2 Wordpress 설정하기**

    - Wordpress도 마찬가지로 샘플 설정 파일이 주어지므로 수정하여 사용한다.
    - `cp var/www/html/wordpress/wp-config-sample.php var/www/html/wordpress/wp-config.php`
    - `wp-config.php` 파일을 본인의 password에 맞게 수정한다.

        ![ft_server/images/Untitled%2015.png](ft_server/images/Untitled%2015.png)

    **7.3 Wordpress 작동 확인**

    - `service nginx reload` 명령어로 변경사항을 적용시켜준다.
    - [localhost/wordpress](http://localhost/wordpress) 에 접속하면 다음과 같은 초기 설정 화면을 볼 수 있다.

        ![ft_server/images/Untitled%2016.png](ft_server/images/Untitled%2016.png)

    - 페이지를 만든 후 phpmyadmin에서 wordpress 데이터가 제대로 생성되었는지 확인한다.
8. Autoindex 적용하기
    - 현재 localhost로 접속하면 다음과 같은 화면을 볼 수 있다.

        ![ft_server/images/Untitled%2017.png](ft_server/images/Untitled%2017.png)

    - autoindex란, 인덱스 페이지를 디렉토리 목록으로 나타내는 방법을 말한다.
    - `/etc/nginx/sites-available/default` 파일을 편집하여 autoindex 기능을 활성화시킬 수 있다.
        - [http://nginx.org/en/docs/http/ngx_http_autoindex_module.html](http://nginx.org/en/docs/http/ngx_http_autoindex_module.html)
    - `/etc/nginx/sites-available/default` 파일을 다음과 같이 수정해준다.

        ![ft_server/images/Untitled%2018.png](ft_server/images/Untitled%2018.png)

    - `service nginx reload` 명령어로 변경사항을 적용시켜준 후 [localhost](http://localhost) 에 접속하면 autoindex가 적용된 화면을 볼 수 있다.

        ![ft_server/images/Untitled%2019.png](ft_server/images/Untitled%2019.png)

- 유용한 링크

    [https://www.opentutorials.org/course/228/4894](https://www.opentutorials.org/course/228/4894) TLS, SSL

    [http://pyrasis.com/Docker/Docker-HOWTO](http://pyrasis.com/Docker/Docker-HOWTO) Docker

[입력 명령어 모음](https://www.notion.so/0771e6a1185a4f6587dc170182807d83)

[Dockerfile](https://www.notion.so/Dockerfile-aa46a128a69549d59de148a033b73234)
