# netwhat

1. IP 주소(IP Address)란 무엇인가?
    - **IP 주소(IP Address)**는 네트워크 계층의 기능을 수행하는 IP 프로토콜이 호스트를 구분하려고 사용하는 주소 체계다. 임의의 호스트를 인터넷에 연결하려면 반드시 IP 주소를 할당받아야 한다. IP 주소는 32비트의 이진 숫자로 구성되는데, 보통 8비트씩 네 부분으로 나누어 십진수로 표현한다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03a0fc6f-74c2-4e7e-93d8-61d307140ead/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F03a0fc6f-74c2-4e7e-93d8-61d307140ead%2FUntitled.png?table=block&id=582e8133-1356-4ff8-bdb9-1ad2cdb11a75&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    - IP 주소는 네트워크 부분과 호스트 부분으로 나뉜다. 네트워크 주소는 특정 네트워크의 주소이고 라우팅시 사용하며, 호스트 주소는 네트워크에 속한 호스트의 주소이다. 같은 네트워크에 있다면 라우터를 거치지 않고도 통신이 가능한데, 이러한 영역을 broadcast 영역이라고 한다.
    - IP 주소는 유일성을 보장하기 위해 국제 표준화 기구에서 전체 주소를 관리하고 할당해 중복 주소의 사용을 원천적으로 차단한다. IP 프로토콜이 처음 개발될 당시에는 현재처럼 폭넓게 활용되리라 예측하지 못했다. 따라서 IP 주소로 표현할 수 있는 최대 주소 공간의 크기를 32비트로 제한함으로써 확장성에 많은 문제점이 야기되고 있다. 이를 해결하려고 새로운 프로토콜 **IPv6(Internet Protocol Version 6)**에서는 주소 표현 공간을 128비트로 확장했다. 그리고 현재의 IP 프로토콜은 IPv6과 구분하기 위해 IPv4로 표현한다.
    - **IPv4**는 인터넷 프로토콜의 4번째 판이며, 전 세계적으로 사용된 첫 번째 인터넷 프로토콜이다. IPv4의 주소체계는 총 12자리이며 네 부분으로 나뉜다. 각 부분은 0~255까지 3자리의 수로 표현된다. IPv4 주소는 32비트로 구성되어 있으며, 현재 인터넷 사용자의 증가로 인해 주소공간의 고갈에 대한 우려가 높아지고 있다. 이에 따라 대안으로 128비트 주소체계를 갖는 IPv6가 등장하였다.
    - **IPv6(Internet Protocol version 6)**는 인터넷 프로토콜 스택 중 네트워크 계층의 프로토콜로서 버전 6 인터넷 프로토콜로 제정된 차세대 인터넷 프로토콜을 말한다. IPv6와 기존 IPv4 사이의 가장 큰 차이점은 바로 IP 주소의 길이가 128비트로 늘어났다는 점이다.

2. 넷마스크(Netmask)란 무엇인가?
    - 넷마스크란 IP 주소의 네트워크 부분을 가리거나 걸러서 호스트 컴퓨터의 주소 부분만이 남도록 하기 위해 0과 1이 조합되어 있는 문자열이다.
    - IP 주소와 넷마스크를 AND연산하면 네트워크 주소를 얻을 수 있다.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/915ec7b1-0416-417b-8f34-2b4ed5bf6889/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F915ec7b1-0416-417b-8f34-2b4ed5bf6889%2FUntitled.png?table=block&id=e5b4c24a-7cf7-4551-9f38-2db8b735cbfe&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

3. subnet이란 무엇인가?
    - 서브넷은 라우터를 통하지 않고 내부적으로 통신을 할 수 있는 영역이 하나의 서브넷이다. 서브넷 간에는 라우터를 이용하여 통신을 해야 하며, 이때 IP 주소의 어떤 영역시 서브넷을 가리키는지 나타내기 위하여 서브넷 마스크를 사용한다.
    - ex) 12.34.56.78 이라는 IP 주소가 있을 때, 서브넷 마스크를 255.255.255.0 으로 했다면, 앞의 12.34.56까지는 서브넷을 나타내고, 뒤의 78은 호스트를 나타낸다.
    - 이전에는 넷마스크와 서브넷 마스크를 구분해서 사용하였지만, CIDR 이후(현재)에는 서브넷 마스크만 사용하고 있다고 한다.
4. subnet의 broadcast address란 무엇인가?
    - 서브넷의 호스트 자리에 1을 채운 것으로 255.255.255.255를 사용하거나 위의 예시에서는 12.34.56이 서브넷이므로 12.34.56.255가 broadcast address가 된다.
    - broadcast address에 패킷을 전달할 경우, 해당 서브넷의 모든 호스트에 패킷을 전달하게 된다. 따라서 255.255.255.255에 패킷을 보낼 경우, 인터넷의 모든 IP 주소에 패킷을 보내게 되는 것이다.
5. Netmask로 IP Address를 나타내기 위한 방식에는 어떤 것이 있는가?
    - 서브넷과 넷마스크를 AND 연산하면 네트워크 주소를 얻을 수 있고, 그 뒤에 호스트 주소를 가져오면 IP 주소를 나타낼 수 있다.
6. Public IP와 Private IP는 어떻게 다른가?
    - 공인 IP (Public IP)는 인터넷 사용자의 로컬 네트워크를 식별하기 위해 ISP(인터넷 서비스 공급자)가 제공하는 IP 주소이다. 공용 IP 주소라고도 불리우며 외부에 공개되어 있는 IP 주소이다.
    - 공인 IP 주소가 외부에 공개되어 있기에 인터넷에 연결된 다른 PC로부터의 접근이 가능하다. 따라서 공인 IP 주소를 사용하는 경우에는 방화벽 등의 보안 프로그램을 설치할 필요가 있다.
    - 사설 IP (Private IP)는 일반 가정이나 회사 내 등에 할당된 네크워크의 IP 주소이며, 로컬 IP 또는 가상 IP 라고도 한다. IPv4의 주소부족으로 인해 서브넷팅된 IP 이기 때문에 라우터에 의해 로컬 네트워크 상의 PC 나 장치에 할당된다.
    - 사설 IP 주소대역은 다음 3가지 주소대역으로 고정된다.
        - Class A : 10.0.0.0 ~ 10.255.255.255
        - Class B : 172.16.0.0 ~ 172.31.255.255
        - Class C : 192.168.0.0 ~ 192.168.255.255

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3cf9d3fa-231b-4a78-a6e8-253f980ecf66/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3cf9d3fa-231b-4a78-a6e8-253f980ecf66%2FUntitled.png?table=block&id=f35120bd-3960-4f19-8b19-8af491500b2a&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    - 사설 IP 주소만으로는 인터넷에 직접 연결할 수 없다. 라우터를 통해 1개의 공인 IP 만 할당하고, 라우터에 연결된 개인 PC 는 사설 IP 를 할당 받아 인터넷에 접속할 수 있게 된다.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b10b784d-52de-450e-b1de-d5bc3a7d3591/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb10b784d-52de-450e-b1de-d5bc3a7d3591%2FUntitled.png?table=block&id=99de2dbf-3746-4dca-9109-3759aeeb6deb&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

7. IP Address의 Class에는 어떤 것이 있는가?
    - IP 주소는 Class라는 개념을 통하여 네트워크 영역과 호스트 영역을 구분하고 있다. 다시 말해, Class는 하나의 IP 주소에서 네트워크 영역과 호스트 영역을 나누는 방법이자 약속이다.
    - Class는 네트워크의 규모에 따라 결정되며, Class의 차이는 A~C(D, E도 있으나 멀티캐스트 또는 연구용으로 사용) 사이에 몇 개의 호스트 주소를 할당할 수 있느냐의 차이이다. 그러나 고정구간으로 낭비가 많아서 CIDR 체계로 변경되었다.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/39ede287-1142-4c33-abb6-08873074ddac/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F39ede287-1142-4c33-abb6-08873074ddac%2FUntitled.png?table=block&id=bd0de7cd-0fac-4733-af2c-806ac5850fe1&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/188aaf7a-4cfc-4acc-8a5b-1f7d209c6516/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F188aaf7a-4cfc-4acc-8a5b-1f7d209c6516%2FUntitled.png?table=block&id=15b685dd-5a12-48ec-8ca2-4d293264e35f&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

8. TCP vs UDP
    - TCP는 Transmission Control Protocol의 약자이고, UDP는 User Datagram Protocol의 약자이다. 두 프로토콜은 모두 패킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 IP 프로토콜을 기반으로 구현되어 있지만, 서로 다른 특징을 가지고 있다.
    - 수신자와 수신이 제대로 되었는지 확인과정이 이루어지는 TCP에 반해, UDP는 일방적으로 데이터를 송신만 할 뿐, 수신 확인은 하지 않는다. 즉, 신뢰성이 요구되는 애플리케이션에서는 TCP를 사용하고 간단한 데이터를 빠른 속도로 전송하고자 하는 애플리케이션에서는 UDP를 사용한다.

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa9ed1c8-4501-4990-a2c9-8e3552e9abea/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ffa9ed1c8-4501-4990-a2c9-8e3552e9abea%2FUntitled.png?table=block&id=a9b41b82-8d06-4987-b1ed-167e03f9c16a&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    - 표로 비교하는 TCP vs UDP

        ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2b01d5eb-af6c-4ba7-a92c-5b360ce46f0e/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2b01d5eb-af6c-4ba7-a92c-5b360ce46f0e%2FUntitled.png?table=block&id=5050b177-9a64-4189-a2e5-f2e4b22a55ca&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ### TCP(Transmission Control Protocol)

    TCP는 네트워크 계층 중 전송 계층에서 사용하는 프로토콜로서, 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 신뢰성을 보장하는 연결형 서비스 이다. TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 옥텟(데이터, 메세지, 세그먼트라는 블록 단위)를 안정적으로, 순서대로, 에러없이 교환할 수 있게 한다.

    ### TCP의 특징

    - 연결형 서비스 : 연결형 서비스로 가상 회선 방식을 제공한다.
    - 흐름제어 (Flow control) : 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지
    - 혼잡제어 (congestion control) : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지
    - 신뢰성이 높은 전송 (Reliable transmission)
        - Dupack-based retransmission
            - 정상적인 상황에서는 ACK 값이 연속적으로 전송되어야 한다.
            - 그러나 ACK값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청한다.
        - Timeout-based retransmission
            - 일정시간동안 ACK 값이 수신을 못할 경우 재전송을 요청한다.
        - 전이중, 점대점 방식
            - **전이중 (Full-Duplex)**전송이 양방향으로 동시에 일어날 수 있다.
            - **점대점 (Point to Point)**각 연결이 정확히 2개의 종단점을 가지고 있다.

            => 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.

    ### TCP Header 정보

    ![https://nesoy.github.io/assets/posts/20181010/1.png](https://nesoy.github.io/assets/posts/20181010/1.png)

    응용 계층으로부터 데이터를 받은 TCP는 `헤더`를 추가한 후에 이를 IP로 보낸다. 헤더에는 아래 표와 같은 정보가 포함된다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca0ba54a-3be1-4b82-9891-9882afe8e55b/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fca0ba54a-3be1-4b82-9891-9882afe8e55b%2FUntitled.png?table=block&id=88c7cee2-a281-4454-9bff-523e6b3db223&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ### 제어 비트(Flag Bit) 정보

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a539542f-a4b2-4829-8db1-ffe4037e0f8c/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa539542f-a4b2-4829-8db1-ffe4037e0f8c%2FUntitled.png?table=block&id=d825ff81-1a0d-497a-91f7-8ba1b5e49ed2&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ### ACK 제어비트

    - ACK는 송신측에 대하여 **수신측에서 긍정 응답**으로 보내지는 전송 제어용 캐릭터
    - ACK 번호를 사용하여 패킷이 도착했는지 확인한다.

        -> 송신한 패킷이 제대로 도착하지 않았으면 **재송신**을 요구한다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3439cf87-1eb1-4721-b720-c064e8534304/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3439cf87-1eb1-4721-b720-c064e8534304%2FUntitled.png?table=block&id=52d28994-f1a7-4c8c-a120-7fee2e1c5083&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ### TCP의 연결 및 해제 과정

    ![https://nesoy.github.io/assets/posts/20181010/2.png](https://nesoy.github.io/assets/posts/20181010/2.png)

    ### TCP Connection (3-way handshake)

    1. 먼저 open()을 실행한 클라이언트가 `SYN`을 보내고 `SYN_SENT` 상태로 대기한다.
    2. 서버는 `SYN_RCVD` 상태로 바꾸고 `SYN`과 응답 `ACK`를 보낸다.
    3. `SYN`과 응답 `ACK`을 받은 클라이언트는 `ESTABLISHED` 상태로 변경하고 서버에게 응답 `ACK`를 보낸다.
    4. 응답 `ACK`를 받은 서버는 `ESTABLISHED` 상태로 변경한다.

    ### TCP Disconnection (4-way handshake)

    1. 먼저 close()를 실행한 클라이언트가 FIN을 보내고 `FIN_WAIT1` 상태로 대기한다.
    2. 서버는 `CLOSE_WAIT`으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
    3. ACK를 받은 클라이언트는 상태를 `FIN_WAIT2`로 변경한다.
    4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 `FIN`을 클라이언트에 보내 `LAST_ACK` 상태로 바꾼다.
    5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 `TIME_WAIT`으로 상태를 바꾼다. `TIME_WAIT`에서 일정 시간이 지나면 `CLOSED`된다. ACK를 받은 서버도 포트를 `CLOSED`로 닫는다.

    > 주의반드시 서버만 CLOSE_WAIT 상태를 갖는 것은 아니다.서버가 먼저 종료하겠다고 FIN을 보낼 수 있고, 이런 경우 서버가 FIN_WAIT1 상태가 됩니다.누가 먼저 close를 요청하느냐에 따라 상태가 달라질 수 있다.

    ### UDP Header 정보

    응용 계층으로부터 데이터 받은 UDP도 UDP 헤더를 추가한 후에 이를 IP로 보낸다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/898006d1-8b89-4852-9dac-f578a3a0a502/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F898006d1-8b89-4852-9dac-f578a3a0a502%2FUntitled.png?table=block&id=e8d87d09-5c51-4c91-bf0d-d0591168934b&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    TCP 헤더와 다르게 UDP 헤더에는 포함된 정보가 부실한 느낌마저 든다.UDP는 수신자가 데이터를 받는지 마는지 관심이 없기 때문이다. 즉, 신뢰성을 보장해주지 않지만 간단하고 속도가 빠른 것이 특징이다.

    # 정리

    ## 공통점

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b32ba487-ff69-48b9-939d-16caef0ecec4/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb32ba487-ff69-48b9-939d-16caef0ecec4%2FUntitled.png?table=block&id=bf908397-24ac-42f1-9cda-e56a877fcbac&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ## 차이점

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33393e1f-417b-4549-8e57-f5edf0abbc66/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F33393e1f-417b-4549-8e57-f5edf0abbc66%2FUntitled.png?table=block&id=8fb50ff1-3c9b-4b72-8870-a312a9f3f08a&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

9. Network layers는 무엇인가?
    - 네트워크 상에서 여러 대의 컴퓨터가 데이터를 주고 받으려면 이들을 서로 연동할 수 있도록 표준화된 인터페이스를 지원해야한다. OSI 7모델과 TPC/IP 모델 모두 `계층 구조`를 갖고 있기 때문에, 자세히 알아보기 전에 먼저 계층 구조가 어떤 것인지, 적용하면 어떤 점이 좋은지를 알 필요가 있다. 계층 구조(Layered)는 네트워크 뿐만 아니라 운영체제 등 다양한 분야에서 적용되는데, 계층 구조를 사용하는 목적은 `분할 정복(Divide and Conquer)` 때문이다. 어떠한 복잡한 문제를 해결하고자 할 때, 나누어 생각하면 쉽게 해결할 수 있다는 취지인 것이다.
    - 계층 구조의 또다른 특징은 위, 아래 층으로만 이동할 수 있다는 점이다. 건너뛰어 한번에 맨위 또는 아래로 갈 수 없다. 즉, 다음 단계로 넘어가려면 이전 계층이 `전제조건`이 되어야한다.
10. OSI model은 무엇인가?

    OSI 7 Model은 네트워크 통신 과정을 `7개의 계층`으로 구분한 산업 표준 참조 모델이다. 초창기의 네트워크는 각 컴퓨터마다 시스템이 달랐기 때문에 하드웨어와 소프트웨어의 논리적인 변경없이 통신할 수 있는 표준 모델이 나타나게 되었다.

    OSI 참조 모델은 위의 그림과 같이 7개의 층으로 이루어져 있다.

    [https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F25303F355755856B02](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F25303F355755856B02)

    > PDU 란?OSI 7계층에서는 PDU 개념을 중요시 하는데, PDU(Process Data Unit)란 각 계층에서 전송되는 단위이다. 1계층에서 PDU가 비트(Bit)라고 생각하기 쉽지만 PDU라고 하지 않고 여기서 비트는 단위라기 보다는 단지 전기 신호의 흐름일 뿐이다.PDU는 2계층-프레임(Frame), 3계층-패킷(Packet), 4계층-세그먼트(Segment) 만 생각하면 된다. 네트워크 통신과정을 깊게 이해하기 위해서는 왜 각각의 계층의 PDU가 다른지 알아야 하고, 역할에 대해 알고 있어야 한다.

    ### 1계층 : 물리계층 (Physical Layer)

    물리계층은 OSI 모델의 최하위 계층에 속하며, 상위 계층에서 전송된 데이터를 물리 매체(허브, 라우터, 케이블 등)를 통해 다른 시스템에 전기적 신호를 전송하는 역할을 한다.

    즉, 기계어를 전기적 신호로 바꿔서 와이어에 실어주는 것이다.

    - PDU : 비트(Bit)
    - 프로토콜 : Modem, Cable, Fiber, RS-232C
    - 장비 : 허브, 리피터

    ### 2계층 : 링크계층 (Link Layer)

    링크계층은 네트워크 기기들 사이의 데이터 전송을 하는 역할을 한다. 시스템 간의 오류 없는 데이터 전송을 위해 패킷을 `프레임`으로 구성하여 물리계층으로 전송한다. 3계층에서 정보를 받아 주소와 제어정보를 헤더와 테일에 추가한다.

    - PDU : 프레임(Frame)
    - 프로토콜 : 이더넷, MAC, PPP, ATM, LAN, Wifi
    - 장비 : 브릿지, 스위치

    ### 3계층 : 네트워크계층 (Network Layer)

    네트워크계층은 기기에서 데이터그램(Datagram)이 가는 경로를 설정해주는 역할을 한다. 라우팅 알고리즘을 사용하여 최적의 경로를 선택하고 송신측으로부터 수신측으로 전송한다. 이때, 전송되는 데이터는 `패킷` 단위로 분할하여 전송한 후 다시 합쳐진다. 2계층이 노드 대 노드 전달을 감독한다면, 3계층은 각 패킷이 목적지까지 성공적이고 효과적으로 전달되도록 한다.

    - PDU : 패킷(Packet)
    - 프로토콜 : IP, ICMP 등
    - 장비 : 라우터, L3 스위치

    ### 4계층 : 전송계층 (Transport Layer)

    발신지에서 목적지(End-to-End) 간 제어와 에러를 관리한다. 패킷의 전송이 유효한지 확인하고 전송에 실패된 패킷을 다시 보내는 것과 같은 신뢰성있는 통신을 보장하며, 헤드에는 `세그먼트`가 포함된다. 주소 설정, 오류 및 흐름 제어, 다중화를 수행한다.

    - PDU : 세그먼트(Segment)
    - 프로토콜 : TCP, UDP , ARP, RTP
    - 장비 : 게이트웨이, L4 스위치

    ### 5계층 : 세션계층 (Session Layer)

    통신 세션을 구성하는 계층으로, `포트(Port)`번호를 기반으로 연결한다. 통신장치 간의 상호작용을 설정하고 유지하며 동기화한다. 동시송수신(Duplex), 반이중(Half-Duplex), 전이중(Full-Duplex) 방식의 통신과 함께 체크 포인팅과 유후, 종료, 다시 시작 과정 등을 수행한다.

    - 프로토콜 : NetBIOS, SSH, TLS

    ### 6계층 : 표현계층 (Presentation Layer)

    표현계층은 송신측과 수신측 사이에서 `데이터의 형식`(png, jpg, jpeg...)을 정해준다. 받은 데이터를 코드 변환, 구문 검색, 암호화, 압축의 과정을 통해 올바른 표준방식으로 변환해준다.

    - 프로토콜 : JPG, MPEG, SMB, AFP

    ### 7계층 : 응용계층 (Application Layer)

    응용계층은 사용자와 바로 연결되어 있으며 응용 SW를 도와주는 계층이다. 사용자로부터 정보를 입력받아 하위 계층으로 전달하고 하위 계층에서 전송한 데이터를 사용자에게 전달한다.

    파일 전송, DB, 메일 전송 등 여러가지 응용 서비스를 네트워크에 연결해주는 역할을 한다.

    - 프로토콜 : DHCP, DNS, FTP, HTTP

    ## 5. TCP/IP 모델

    그렇지만 OSI 참조 모델은 말그대로 참조 모델일 뿐 실제 사용되는 인터넷 프로토콜은 을 7계층 구조를 완전히 따르지는 않는다. 인터넷 프로토콜 스택(Internet Protocol Stack)은 현재 대부분 TCP/IP를 따른다.

    TCP/IP는 인터넷 프로토콜 중 가장 중요한 역할을 하는 `TCP`와 `IP`의 합성어로 데이터의 흐름 관리, 정확성 확인, 패킷의 목적지 보장을 담당한다. **데이터의 정확성 확인은 TCP가, 패킷을 목적지까지 전송하는 일은 IP가 담당한다.**

    > TCP/IP의 4계층TCP/IP는 OSI 참조 모델과 달리 표현계층, 세션계층을 응용계층에 다 포함시키고 있지만, 사실상 TCP/IP Model의 Application 계층 하나에서 Application, Presentatiom, Session 계층의 구현을 다 하고 있다고 이해하는 게 올바르다.

    ![https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-1.png](https://madplay.github.io/img/post/2018-02-04-network-tcp-udp-tcpip-1.png)

    데이터는 아래 그림과 같이 단계 별로 헤더(Data **→** Segment **→** Datagram **→** Frame)를 붙여 전송하며 이를 `데이터 캡슐화`라고 한다.

    [https://t1.daumcdn.net/cfile/tistory/27786B485715047219](https://t1.daumcdn.net/cfile/tistory/27786B485715047219)

11. DHCP Server와 DHCP Protocol은 무엇인가?

    # DHCP 프로토콜

    > 동적 호스트 설정 프로토콜(Dynamic Host Configuration Protocol)로서 해당 호스트에게 IP주소, 서브넷 마스크, 기본게이트웨이 IP주소, DNS서버 IP주소 를 자동으로 일정 시간 할당해주는 인터넷 프로토콜

    DHCP를 통한 IP 주소 할당은 `임대(Lease)`라는 개념을 가지고 있는데 이는 DHCP 서버가 IP 주소를 영구적으로 단말에 할당하는 것이 아니고 **임대기간(IP Lease Time)을 명시하여 그 기간 동안만 단말이 IP 주소를 사용**하도록 하는 것이다. 단말은 임대기간 이후에도 계속 해당 IP 주소를 사용하고자 한다면 **IP 주소 임대기간 갱신(IP Address Renewal)**을 DHCP 서버에 요청해야 하고 또한 단말은 임대 받은 IP 주소가 더 이상 필요치 않게 되면 **IP 주소 반납 절차(IP Address Release)**를 수행하게 된다.

    즉,

    [유동 IP](https://velog.io/@hidaehyunlee/%EA%B3%B5%EC%9D%B8Public-%EC%82%AC%EC%84%A4Private-IP%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90#4-%EA%B3%A0%EC%A0%95-ip%EC%99%80-%EC%9C%A0%EB%8F%99-ip)를 할당한다 == DHCP서버로부터 IP를 DHCP서버에서 설정해놓은 사용시간만큼 임대해온다.

    ## 1.1. DHCP의 구성

    ### 1) DHCP 서버

    DHCP서버는 인터넷을 제공해주는 곳의 서버에서 실행되는 프로그램으로 일정한 범위의 IP주소를 다른 클라이언트에게 할당하여 자동으로 설정하게 해주는 역할을 한다. DHCP서버는 클라이언트에게 할당된 IP주소를 변경없이 유지해 줄 수 있다. DHCP가 설정해주는 주소 정보들은 아래와 같다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8d4447a-ecc8-4a88-b4a5-7aa84b5a0487/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa8d4447a-ecc8-4a88-b4a5-7aa84b5a0487%2FUntitled.png?table=block&id=bcf36399-eb43-4055-93a5-398e63a8d2e7&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    ### 2) DHCP 클라이언트

    클라이언트들은 시스템이 시작하면 DHCP서버에 자신의 시스템을 위한 IP주소를 요청하고, DHCP 서버로부터 IP주소를 부여받으면 TCP/IP 설정은 초기화되고 다른 호스트와 TCP/IP를 사용해서 통신을 할 수 있게 된다.

    즉, 서버에게 IP를 할당받으면 TCP/IP 통신을 할 수 있다.

    [https://t1.daumcdn.net/cfile/tistory/2519193C5715E03427](https://t1.daumcdn.net/cfile/tistory/2519193C5715E03427)

    ## 1.2. DHCP 임대 절차

    IP 주소 할당(임대) 절차에 사용되는 DHCP 메시지는 아래 그림과 같이 **4개의 메시지**로 구성되어 있다.

    [https://lh5.googleusercontent.com/kOITXoaKRPVHm9mukDqihbpiiCmv9e4DFFbp3ASRWOETsg69sqJfnv6AkRHpVgyn4YsQ7VpFm3d1rxkapCYeTqUbGWx6SaDojhFfv1umhRbDvO190Zr3tuLjuwDHJO4D2rsdOYZu](https://lh5.googleusercontent.com/kOITXoaKRPVHm9mukDqihbpiiCmv9e4DFFbp3ASRWOETsg69sqJfnv6AkRHpVgyn4YsQ7VpFm3d1rxkapCYeTqUbGWx6SaDojhFfv1umhRbDvO190Zr3tuLjuwDHJO4D2rsdOYZu)

    ### 1) **DHCP Discover**

    - **패킷 방향 :** 클라이언트 -> DHCP 서버
    - **브로드캐스트 패킷** : Destination MAC = FF:FF:FF:FF:FF:FF
    - **의미** : 클라이언트가 DHCP 서버를 찾기 위한 메시지. 그래서 LAN상에(동일 subent상에) 브로드캐스팅을 하여 "거기 혹시 DHCP 서버 있으면 내게 응답 좀 해 주세요"라고 단말이 메세지를 보낸다. 이 Discover 패킷에는 IP 주소가 필요한 호스트의 MAC 주소가 담겨져 있어서 DHCP 서버가 응답할 때 패킷을 수신할 수 있게 된다.
    - **주요 파라미터(패킷 내용) :**
        - Client MAC : 클라이언트의 MAC 주소

    ### 2) DHCP Offer

    - **패킷 방향 :** DHCP 서버 -> 클라이언트
    - **브로드캐스트 메시지 :** Destination MAC = FF:FF:FF:FF:FF:FF 혹은 유니캐스트.

        이는 클라이언트가 보낸 DHCP Discover 메시지 내의 Broadcast Flag의 값에 따라 달라지는데, 이 Flag=1이면 DHCP 서버는 DHCP Offer 메시지를 Broadcast로, Flag=0이면 Unicast로 보내게 된다.

    - **의미**: DHCP 서버가 "저 여기 있어요~"라고 응답하는 메시지. 단순히 DHCP 서버의 존재만을 알리지 않고, **클라이언트에 할당할 IP 주소 정보를 포함한 다양한 "네트워크 정보"를 함께 실어서 클라이언트에 전달한다.**
    - **주요 파라미터(패킷 내용) :**
        - Client MAC: 단말의 MAC 주소
        - Your IP: 단말에 할당(임대)할 IP 주소
        - Subnet Mask (Option 1)
        - Router (Option 3): 단말의 Default Gateway IP 주소
        - DNS (Option 6): DNS 서버 IP 주소
        - IP Lease Time (Option 51): 단말이 IP 주소(Your IP)를 사용(임대)할 수 있는 기간(시간)
        - DHCP Server Identifier (Option 54): 본 메시지(DHCP Offer)를 보낸 DHCP 서버의 주소. 2개 이상의 DHCP 서버가 DHCP Offer를 보낼 수 있으므로 각 DHCP 서버는 자신의 IP 주소를 본 필드에 넣어서 단말에 보냄.

    ### 3) **DHCP Request**

    - **패킷 방향:** 클라이언트 -> DHCP 서버
    - **브로드캐스트 메시지 :** Destination MAC = FF:FF:FF:FF:FF:FF
    - **의미**: 단말은 DHCP 서버(들)의 존재를 알았고, DHCP 서버가 단말에 제공할 네트워크 정보(IP 주소, subnet mask, default gateway등)를 알았다. 이제 단말은 DHCP Request 메시지를 통해 하나의 DHCP 서버를 선택하고 **해당 서버에게 "단말이 사용할 네트워크 정보"를 요청**한다.
    - **주요 파라미터(패킷 내용) :**
        - Client MAC: 단말의 MAC 주소
        - Requested IP Address (Option 50): 난 이 IP 주소를 사용하겠다. (DHCP Offer의 Your IP 주소가 여기에 들어감)
        - DHCP Server Identifier (Option 54): 2대 이상의 DHCP 서버가 DHCP Offer를 보낸 경우, 단말은 이 중에 마음에 드는 DHCP 서버 하나를 고르게 되고, 그 서버의 IP 주소가 여기에 들어감. 즉, DHCP Server Identifier에 명시된 **DHCP** **서버에게** **"DHCP Request"** **메시지**를 보내어 단말 IP 주소를 포함한 네트워크 정보를 얻는 것.

    ### 4) **DHCP Ack**

    - **패킷 방향:** DHCP 서버 -> 클라이언트
    - **브로드캐스트 메시지 :** Destination MAC = FF:FF:FF:FF:FF:FF 혹은 유니캐스트.

        이는 단말이 보낸 DHCP Request 메시지 내의 Broadcast Flag=1이면 DHCP 서버는 DHCP Ack 메시지를 Broadcast로, Flag=0이면 Unicast로 보내게 된다.

    - **의미**: DHCP 절차의 마지막 메시지로, DHCP 서버가 단말에게 "네트워크 정보"를 전달해 주는 메시지. 앞서 설명한 DHCP Offer의 '네트워크 정보"와 동일한 파라미터가 포함된다.
    - **주요 파라미터(패킷 내용) :** DHCP Request 패킷의 파라미터와 동일
12. DNS Server와 DNS Protocol은 무엇인가?

    # DNS 프로토콜

    > 도메인 네임과 IP 주소의 대응 관계를 데이터베이스로 구축해 사용하는 인터넷 프로토콜

    위에서 배웠듯 클라이언트에게 DNS (Domain Name Server)를 제공하는 것은 DHCP 서버의 책임이다. DNS는 **브라우징을 단순화하는 매우 특별한 목적을 수행하는 인터넷 상의 또 다른 컴퓨터**라고 볼 수 있다.

    네트워크의 각 컴퓨터에는 고유한 IP 주소가 있고, 이는 인터넷에서도 마찬가지이다. 인터넷에 연결된 모든 네트워크 또는 컴퓨터 서버에도 고유한 주소가 있다. 우리가 자주 방문하는 사이트의 각 IP 주소를 매번 기억하는 것은 사실 불가능한 일에 가깝다. 그래서 우리는 도메인 이름(www.으로 시작하는 주소)을 사용하는데, 사용자가 [https://velog.io/@hidaehyunlee](https://velog.io/@hidaehyunlee) 과 같이 입력한 주소를 125.209.222.141와 같은 IP 주소로 변환해주는 일을 바로 DNS가 수행하는 것이다. 이러한 DNS를 운영하는 서버를 네임서버(Name Server)라고 한다.

    ## DNS 절차

    1. 특정 사이트를 방문하기위해 사용자가 브라우저에 URL을 입력한다.
    2. 그러면 브라우저는 DNS에 접속하여 입력한 도메인 이름과 관련된 IP 주소를 요청한다.
    3. 획득한 IP 주소를 사용하여 브라우저는 그 컴퓨터와 통신하고 사용자로부터 요청된 특정 페이지를 요청할 수 있다.

    ![https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png](https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png)

13. 두 개의 IP Address 사이에 통신하기 위한 규칙은 무엇이 있는가?

    [https://ko.wikipedia.org/wiki/인터넷_프로토콜](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%EB%84%B7_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)

14. Routing은 어떻게 작동하는가?

    # IP 라우팅(routing) 동작 과정

    ## 1.1. 라우터의 개념

    라우터는 전용회선을 통해 LAN에 연결된 컴퓨터들이 동시에 인터넷을 사용할 수 있게 해주는 장비로 데이터를 목적지까지 전달하는 기능을 수행하며 2개 이상의 서로 다른 네트워크를 접속하고 이들 간 데이터를 주고 받게하는 중계 기능도 한다. 대부분의 라우터는 IP 라우팅 기능뿐 아니라 LAN용 프로토콜인 IPX, AppleTalk등의 브리징 기능도 함께한다.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3f46f76-b4cc-448d-b8bf-483337408428/Untitled.png](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb3f46f76-b4cc-448d-b8bf-483337408428%2FUntitled.png?table=block&id=9e51c56d-0956-470f-b453-dcfab84e0106&width=3840&userId=46f3d4d1-8bde-4163-b59f-a3460263d02e&cache=v2)

    즉, **라우터는 IP네트워크, 서브넷을 관리하면서 다른 네트워크를 거쳐 패킷을 전송하는 역할을 하는 장비**이고, **라우팅은 그 패킷을 보낼 경로를 선택하는 과정**이라고 볼 수 있다.

    ## 1.2. 라우터의 동작원리

    라우터는 패킷의 전송경로를 결정하기 위해 `랜테이블`, `네트워크테이블`, `라우팅테이블`을 사용한다. 라우터는 위의 3가지 테이블을 관리함으로써 다른 네트워크에 연결된 장치들을 비롯하여 **네트워크에 연결된 모든 장치들의 주소를 인식하고 이것을 바탕으로 패킷의 전송경로를 결정**한다.

    동일 네트워크 상에 있는 장치로 패킷을 보낼 때 라우터에서는 아래 순서를 매번 거친다.

    1. `랜테이블` 검사를 한다. 이곳에서는 패킷의 목적지가 같은 네트워크에 있는지 아니면 다른 네트워크에 있는지를 확인한다.
    2. `네트워크테이블`을 검사하여 패킷을 전달할 네트워크 주소를 찾아낸다.
    3. `라우팅테이블`을 검색하여 가장 적합한 경로를 찾아내서 패킷을 보낸다.
    - 랜테이블

        랜테이블은 라우터에 연결되어 있는 랜 세그먼트 내 장치의 주소를 관리하고 있으며 필터링작업에 사용된다.

    - 네트워크테이블

        네트워크상의 모든 라우터의 주소를 보관하며 패킷의 수신지 라우터를 식별하는데 사용된다.

    - 라우팅테이블

        각각의 라우터에 구축되어 있으며 각 경로에 대한 정보를 유지하고 있어서 다른 세그먼트로 전송 되는 **패킷의 가장 효율적인 경로를 결정**하는데 사용된다.

    ## 1.3. 라우터의 목적지 학습 방법

    ### 1) Connected (연결)

    자신과 **물리적으로 직접 연결**되어있는 장비의 IP 주소를 자동으로 알아온다. 이때 IP는 네트워크 주소로 라우팅 테이블에 저장된다.

    ### 2) Static (정적)

    관리자가 직접 라우팅 경로를 선택해서 보내는 설정.

    - 장점 : 관리자가 테이터가 전송될 경로를 직접 설정하므로 경로관리에 가장 효율적이다.
    - 단점 : 네트워크 변화에 대한 대처가 느리다.

    ### 3) Dynamic (동적)

    각 라우터들이 갖고 있는 정보를 서로에게 공유하여 라우팅 테이블에 저장한다. 주시적으로 최적경로를 계산하여 라우팅 테이블의 정보를 유지하는 방식이다.

    - 장점 : 네트워크 변화에 대한 대처가 빠르다.
    - 단점 : 주기적으로 경로를 계산해야하므로 리소스 소비량(CPU사용량)이 많아진다.

    ### 4) Redistribution (재분배)

    정보 교환이 이루어지지 않는 장비끼리 관리자가 강제로 교환하는 방식.

15. Routing의 기본 Gateway는 무엇인가?
    - Default Routing은 라우팅 테이블 안에 없는 원격 네트워크를 수신지로 패킷을 인접한 라우터로 전송하는 프로세스이다.
    - 경로를 찾아내지 못한 모든 네트워크들을 디폴트 라우트로 빠져 나가라고 정의해 놓은 것
    - 사용 용도는 인터넷을 사용하는 라우터와 Stub 네트워크 라우터이다.
    - ip route 0.0.0.0 0.0.0.0 [xxx.xxx.xxx.xxx](http://xxx.xxx.xxx.xxx) 앞의 0.0.0.0 들은 Default Network 이고, 뒤의 주소는 Default Network 로 보낼 패킷 주소이다.

16. IP의 관점에서 Port는 무엇이며, 두 기기간의 통신하는데 사용되는 것은 무엇인가?

    [https://ko.wikipedia.org/wiki/포트_(컴퓨터_네트워킹)](https://ko.wikipedia.org/wiki/%ED%8F%AC%ED%8A%B8_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%82%B9))

- 참고자료
    - [https://terms.naver.com/entry.nhn?docId=2271790&cid=51207&categoryId=51207&expCategoryId=51207](https://terms.naver.com/entry.nhn?docId=2271790&cid=51207&categoryId=51207&expCategoryId=51207)
    - [https://velog.io/@hidaehyunlee/series/나의-Network](https://velog.io/@hidaehyunlee/series/%EB%82%98%EC%9D%98-Network)
    - [https://juner417.github.io/blog/network-101-ip-subnet/](https://juner417.github.io/blog/network-101-ip-subnet/)
    - [https://blog.naver.com/mint0490/221617318133](https://blog.naver.com/mint0490/221617318133)
    - [https://blog.naver.com/so15284/220768336133](https://blog.naver.com/so15284/220768336133)
