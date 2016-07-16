##Chapter 04, TCP 기반 서버, 클라이언트 구현

* **TCP 서버**에서의 기본적인 함수 호출 순서
socket() 소켓생성 -> bind() 소켓 주소 할당 -> listen() 연결요청 대기 상태 -> accept() 연결 허용 -> read()/write() 데이터 송수신 -> close() 연결 종료  

* 연결 요청 대기 상태로의 진입  
listen 함수가 호출되어야, 클라이언트는 연결 요청을 위해서 connect 함수를 호출할 수 있다.  
```c
#include <sys/socket.h>

int listen(int sock, int backlog);  //성공 시 0, 실패 시 -1 반환.
```
>sock : 연결 요청 대기 상태에 두고자 하는 소켓의 파일 디스크립터 전달. 이 함수의 인자로 전달된 디스크립터의 소켓이 서버 소켓이 된다.  
backlog : 연결 요청 대기 큐(Queue)의 크기 정보 전달.  

* 클라이언트의 연결 요청 수락
서버 소켓이 아닌, 소켓을 하나 더 만들어야 한다. 다음 함수의 호출 결과로 소켓이 만들어지고, 이 소켓은 연결 요청을 한 클라이언트 소켓과 자동으로 연결된다.  
```c
#include <sys/socket.h>

int accept(int sock, struct sockaddr * addr, socklen_t * addrlen);  //성공 시 생성된 소켓의 파일 디스크립터, 실패 시 -1 반환.
```
>sock : 서버 소켓의 파일 디스크립터 전달.  
addr : 연결요청한 _**클라이언트의 주소 정보를 담을 변수**_의 주소 값 전달.  
addrlen : 두 번째 매개변수 addr에 전달된 주소의 변수 크기를 바이트 단위로 전달, 단 크기 정보를 어떤 변수에 저장한 다음에 그 변수의 주소 값을 전달한다. 함수 호출이 완료되면 크기 정보로 채워져 있던 변수에는 클라이언트의 주소 정보 길이가 바이트 단위로 계산되어 채워진다.  


***


* TCP 클라이언트의 기본적인 함수 호출 순서  
socket() -> connect() 연결요청 -> read()/write() -> close()  

```c
#include <sys/socket.h>

int connect(int sock, struct sockaddr * servaddr, socklen_t addrlen);  //성공 시 0, 실패 시 -1 반환.
```
>sock : 클라이언트 소켓의 파일 디스크립터 전달.  
servaddr : 연결요청 할 **서버의 주소정보**를 담은 변수의 주소 값 전달.  
addrlen : 두 번째 매개변수 servaddr에 전달된 **주소의 변수 크기**를 바이트 단위로 전달.  

클라이언트 소켓의 주소정보는 connect 함수가 호출될 때, 운영체제(커널)에서 IP는 컴퓨터(호스트)에 할당된 IP로, PORT는 임의로 선택해서 할당이 된다.  
