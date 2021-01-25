# ft_services

## **Introduction**

- Ft_services will introduce you to Kubernetes. You will discover cluster management and
deployment with Kubernetes. You will virtualize a network and do "clustering".

## **General instructions**

- You must place all the necessary files for the configuration of your server in a folder called srcs.
- Your setup.sh file should be at the root of your repository. This script will setup
all your applications.
- This subject requires both old and new practices. We therefore advise you not to
be afraid to read a lot of documentation about Docker, Kubernetes, and all other
things useful for the project.

## **Mandatory Part**

- You must set up a web server with Nginx, in only one docker container. The container OS must be debian buster.

---

## 들어가며

> 이 글은 42seoul의 ft_server 과제를 해결하기 위한 자료로 작성되었습니다.

[https://kubernetes.io/ko/docs/tasks/tools/install-minikube/](https://kubernetes.io/ko/docs/tasks/tools/install-minikube/)

> 작동 환경 : masOS(Catalina) 10.15.5

---

## 용어정리

[https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-cluster](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-cluster) 표준 용어집

[https://zetawiki.com/wiki/쿠버네티스_용어](https://zetawiki.com/wiki/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4_%EC%9A%A9%EC%96%B4) 제타위키

## 필요한 services

- MetalLB(LoadBalancer)
- nginx(LoadBalancer)
- phpmyadmin(LoadBalancer)
- wordpress(LoadBalancer)
- MySQL(ClusterIP)
- FTPS(LoadBalancer)
- Grafana(LoadBalancer)
- InfluxDB(ClusterIP)

## 목차

1. minikube 설치하기

2. nginx 설치하기

3. SSH 설정하기

4. mysql

5. wordpress

6. phpmyadmin

7. ftps

8. influxdb

9. telegraf

10. grafana

11. MetalLB

---

1. minikube 설치
    - 가상화 지원 여부 확인

        맥OS에서 가상화 지원 여부를 확인하려면, 아래 명령어를 터미널에서 실행한다.

        ```
        sysctl -a | grep -E --color 'machdep.cpu.features|VMX'
        ```

        만약 출력 중에 (색상으로 강조된) `VMX`를 볼 수 있다면, VT-x 기능이 머신에서 활성화된 것이다.

    - minikube 설치하기

        ### **kubectl 설치**

        kubectl이 설치되었는지 확인한다. kubectl은 [kubectl 설치하고 설정하기](https://kubernetes.io/ko/docs/tasks/tools/install-kubectl/#macos%EC%97%90-kubectl-%EC%84%A4%EC%B9%98)의 요령을 따라서 설치할 수 있다.

        ### **하이퍼바이저(hypervisor) 설치**

        하이퍼바이저를 설치하지 않았다면, 다음 중 하나를 지금 설치한다.

        - [HyperKit](https://github.com/moby/hyperkit)
        - [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
        - [VMware Fusion](https://www.vmware.com/products/fusion)

        ### **Minikube 설치**

        가장 쉽게 맥OS에 Minikube를 설치하는 방법은 [Homebrew](https://brew.sh/)를 이용하는 것이다.

        `brew install minikube`

        실행 바이너리를 다운로드 받아서 맥OS에 설치할 수도 있다.

        `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 **\**
         && chmod +x minikube`

        Minikube 실행 파일을 사용자 실행 경로에 추가하는 가장 쉬운 방법은 다음과 같다.

        `sudo mv minikube /usr/local/bin`

    - 설치 확인

        하이퍼바이저와 Minikube의 성공적인 설치를 확인하려면, 다음 명령어를 실행해서 로컬 쿠버네티스 클러스터를 시작할 수 있다.

        > 참고: minikube start 시 --driver 를 설정하려면, 아래에 <driver_name> 로 소문자로 언급된 곳에 설치된 하이퍼바이저의 이름을 입력한다. --driver 값의 전체 목록은 VM driver 지정하기 문서에서 확인할 수 있다.

        > 주의: KVM을 사용할 때 Debian과 다른 시스템에서 libvirt의 기본 QEMU URI는 qemu:///session이고, Minikube의 기본 QEMU URI는 qemu:///system이다. 시스템이 이런 환경이라면, --kvm-qemu-uri qemu:///session을 minikube start에 전달해야 한다.

        `minikube start --driver=<driver_name>`

        필자는 virtualbox를 사용하므로, `minikube start --driver=vitrualbox`를 입력해준다.

        `minikube start` 가 완료되면, 아래 명령을 실행해서 클러스터의 상태를 확인한다.

        `minikube status`

        만약 클러스터가 실행 중이면, `minikube status` 의 출력은 다음과 유사해야 한다.

        ```
        host: Running
        kubelet: Running
        apiserver: Running
        kubeconfig: Configured
        ```

        Minikube가 선택한 하이퍼바이저와 작동하는지 확인한 후에는, Minikube를 계속 사용하거나 클러스터를 중지할 수 있다. 클러스터를 중지하려면 다음을 실행한다.

        `minikube stop`

2. nginx 설치하기

    [https://wiki.alpinelinux.org/wiki/Nginx](https://wiki.alpinelinux.org/wiki/Nginx)

    `nginx -g "daemon off;"` 명령어를 사용하면 nginx를 foreground에서 실행시킬 수 있다.

    나머지 nginx 설정은 이전에 ft_server에서 사용했던 부분을 이용하자.

    [https://wiki.alpinelinux.org/wiki/Nginx_as_reverse_proxy_with_acme_(letsencrypt)](https://wiki.alpinelinux.org/wiki/Nginx_as_reverse_proxy_with_acme_(letsencrypt)) reverse proxy

    reverse_proxy 할때 proxy_pass에 http://phpmyadmin-service:5000/; 슬래시 꼭 넣어주기

    proxy_redirect	/index.php /phpmyadmin/index.php; 해서 제대로 접속되는지 확인

3. SSH 설정하기

    [https://zetawiki.com/wiki/SSH_접속_가능한_알파인_도커_이미지_빌드](https://zetawiki.com/wiki/SSH_%EC%A0%91%EC%86%8D_%EA%B0%80%EB%8A%A5%ED%95%9C_%EC%95%8C%ED%8C%8C%EC%9D%B8_%EB%8F%84%EC%BB%A4_%EC%9D%B4%EB%AF%B8%EC%A7%80_%EB%B9%8C%EB%93%9C)

    공유키 충돌시 ssh-keygen -R [ IP or DomainName] 명령어로 초기화해준다. [참고](https://cpuu.postype.com/post/30065)

    [https://bcho.tistory.com/m/1264](https://bcho.tistory.com/m/1264) livenessProbe health check

    tcp를 이용하여 22번 port에 health check를 하면 된다

4. mysql

    [https://wiki.alpinelinux.org/wiki/MariaDB](https://wiki.alpinelinux.org/wiki/MariaDB)

    그냥 openrc 써서 sql파일만 넣어주고 서비스 종료 후 데몬 실행
    wordpress.sql 파일을 mysql에 import할 때 wordpress.sql 파일에서 USE wordpress; 로 DB를 사용하게 해주어야 한다.

    ```bash
    #!/bin/sh

    /etc/init.d/mariadb setup
    service mariadb start

    mysql < create_wordpress.sql
    mysql < wordpress.sql

    service mariadb stop

    /usr/bin/mysqld --datadir="/var/lib/mysql"
    ```

5. wordpress

    [https://wiki.alpinelinux.org/wiki/WordPress](https://wiki.alpinelinux.org/wiki/WordPress)

     admin 계정과 다수의 user 계정을 생성 후 sql 파일로 백업하여 설치화면이 아닌 blog 화면이 바로 뜨게 만들어야 한다.

    wordpress.sql를 export할 때 의 external IP와 mysql에서 import할 때의 external IP가 일치해야 하므로 export했던 sql파일에서 IP를 수정해서 import 해주자

6. phpmyadmin

    [https://wiki.alpinelinux.org/wiki/PhpMyAdmin](https://wiki.alpinelinux.org/wiki/PhpMyAdmin)

    php7-session, php7-mbstring 도 설치해주기

7. ftps

    [https://wiki.archlinux.org/index.php/Pure-FTPd](https://wiki.archlinux.org/index.php/Pure-FTPd)

    [https://www.pureftpd.org/project/pure-ftpd/doc/](https://www.pureftpd.org/project/pure-ftpd/doc/) pureftp

    pure-ftpd를 실행할 때 -p 옵션으로 port 범위를 지정해주고, -P 옵션으로 external IP 주소를 넣어준다.

    TLS를 적용한 경우에는 -Y 옵션에 따라 연결 시 TLS를 사용하지 않은 연결은 거부할 수 있다.

8. grafana

    [https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management#Add_a_Package](https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management#Add_a_Package)

    `apk add --repository [http://dl-3.alpinelinux.org/alpine/edge/testing/](http://dl-3.alpinelinux.org/alpine/edge/testing/)` 명령어를 입력하여 grafana를 바로 설치할 수 있다.

    [https://stackoverflow.com/questions/41009392/grafana-migrate-to-a-new-server-upgrade-to-v4](https://stackoverflow.com/questions/41009392/grafana-migrate-to-a-new-server-upgrade-to-v4)

    설치 후 service별로 dashboard를 만들어서 json으로 export하여 grafana의 dashboard 폴더에 넣어주거나, data/grafana.db를 통째로 복사하는 방법이 있다.

    telegraf에서 Docker plugin을 사용한 경우에는 [io.kubernetes.container.name](http://io.kubernetes.container.name) 항목을 선택해야 올바른 데이터를 읽어올 수 있다.

9. influxDB

    [https://dev-t-blog.tistory.com/34](https://dev-t-blog.tistory.com/34) 계정 설정

    [https://medium.com/naver-cloud-platform/grafana-influxdb를-활용한-모니터링-서비스-구축하기-62de4b07a505](https://medium.com/naver-cloud-platform/grafana-influxdb%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EC%84%9C%EB%B9%84%EC%8A%A4-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0-62de4b07a505)

10. telegraf

    [https://blog.voidmainvoid.net/91](https://blog.voidmainvoid.net/91)

    [https://support.cloudz.co.kr/support/solutions/articles/42000044793-telegraf-설정-가이드](https://support.cloudz.co.kr/support/solutions/articles/42000044793-telegraf-%EC%84%A4%EC%A0%95-%EA%B0%80%EC%9D%B4%EB%93%9C)
    
    telegraf.conf 파일에 influxdb-service:8086 

    docker plugin을 사용하면 각 service별로 telegraf를 설치하지 않고 telegraf pods 하나로 data를 관리할 수 있다. [https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/docker)

11. MetalLB

    [https://metallb.universe.tf/installation/](https://metallb.universe.tf/installation/)

    [https://boying-blog.tistory.com/16](https://boying-blog.tistory.com/16) MetalLB를 이용하여 LoadBalaner로 배포하기

    [https://metallb.universe.tf/usage/#ip-address-sharing](https://metallb.universe.tf/usage/#ip-address-sharing) single ip 설정하는 방법

- 유용한 링크

    [https://kubernetes.io/ko/docs/home/](https://kubernetes.io/ko/docs/home/) 쿠버네티스 공식 문서

    [https://kubernetes.io/ko/docs/concepts/workloads/pods/](https://kubernetes.io/ko/docs/concepts/workloads/pods/) 파드

    [https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/](https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/) 디플로이먼트

    [https://bcho.tistory.com/1255?category=731548](https://bcho.tistory.com/1255?category=731548) 쿠버네티스 소개

    [https://subicura.com/2019/05/19/kubernetes-basic-1.html#쿠버네티스란](https://subicura.com/2019/05/19/kubernetes-basic-1.html#%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4%EB%9E%80)

    [https://yohai.tistory.com/78?category=912493](https://yohai.tistory.com/78?category=912493)

    [https://www.cyberciti.biz/faq/how-to-enable-and-start-services-on-alpine-linux/](https://www.cyberciti.biz/faq/how-to-enable-and-start-services-on-alpine-linux/)

    [https://wiki.alpinelinux.org/wiki/Main_Page](https://wiki.alpinelinux.org/wiki/Main_Page)

    [https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/) k8s API example

    [https://harm-smits.github.io/42docs/projects/ft_services](https://harm-smits.github.io/42docs/projects/ft_services)

    [https://www.joinc.co.kr/w/Site/System_management/Proxy](https://www.joinc.co.kr/w/Site/System_management/Proxy) proxy에 대하여

    [https://dev.to/wes5510/nginx-https-proxy-3422](https://dev.to/wes5510/nginx-https-proxy-3422) nginx에서 https proxy 처리하기

    [https://gonna-be.tistory.com/20#Configuration File 구조 분석하기](https://gonna-be.tistory.com/20#Configuration%20File%20%EA%B5%AC%EC%A1%B0%20%EB%B6%84%EC%84%9D%ED%95%98%EA%B8%B0) nginx conf 구성

    [https://cleanupthedesk.tistory.com/16](https://cleanupthedesk.tistory.com/16) k8s+mysql+wp 구성하기

    [https://www.commandlinux.com/man-page/man1/busybox.1.html](https://www.commandlinux.com/man-page/man1/busybox.1.html) busybox Usage

    [https://www.datadoghq.com/blog/how-to-collect-and-graph-kubernetes-metrics/](https://www.datadoghq.com/blog/how-to-collect-and-graph-kubernetes-metrics/)
