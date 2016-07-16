##Chapter 03, 03-4 인터넷 주소의 초기화와 할당

* 소켓 생성 과정에서 흔히 등장하는 인터넷 주소 정보의 초기화 방법
```c
struct sockaddr_in addr;
char *serv_ip="211.217.168.13";
char *serv_port="9190";
memset(&addr, 0, sizeof(addr));
addr.sin_family=AF_INET;
addr.sin_addr.s_addr=inet_addr(serv_ip);  //문자열 기반의 IP 주소 초기화
addr.sin_port=htons(atoi(serv_port));     //문자열 기반의 PORT 번호 초기화
```
memset 함수는 동일한 값으로 바이트 단위 초기화를 할 때 호출하는 함수이다.  
atoi 함수는 문자열로 표현되어 있는 값을 정수로 변환해서 반환한다.  

* 문자열 정보를 __네트워크 바이트 순서__의 정수로 변환하기  
이 함수는 변환 과정에서 네트워크 바이트 순서로의 변환도 동시에 진행한다.  
유효하지 못한 IP주소에 대한 오류검출 능력도 가지고 있다.  
```c
#include <arpa/inet.h>

in_addr_t inet_addr(const char *string);  //성공 시 Big Endian으로 변환된 32비트 정수 값, 실패 시 INADDR_NONE 반환 
```
*inet_aton 함수는 구조체 변수 in_addr를 이용하는 형태라는 점에서 위 함수와 차이를 보인다.*
```c
#include <arpa/inet.h>

int inet_aton(const char * string, struct in_addr * addr); //성공 시 1(true), 실패 시 0(false) 반환
```
>string : 변환할 IP주소 정보를 담고 있는 문자열의 주소 값 전달.  
addr : 변환된 정보를 저장할 in_addr 구조체 변수의 주소 값 전달.  

* *inet_aton 함수의 반대기능을 제공하는 함수*  
이 함수는 네트워크 바이트순서로 정렬된 정수형 IP주소 정보를 우리가 눈으로 쉽게 인식할 수 있는 문자열의 형태로 변환해준다.  
```c
#include <arpa/inet.h>

char * inet_ntoa(struct in_addr adr);  //성공 시 변환된 문자열의 주소 값, 실패 시 -1 반환.
```
주의할 점은, 반환형이 char형 포인터라는 사실이다.  
"함수 내부적으로 메모리 공간을 할당해서 변환된 문자열 정보를 저장한다. 따라서 이 함수 호출 후에는 가급적 반환된 문자열 정보를 다른 메모리 공간에 복사해 두는 것이 좋다."  

***


* 클라이언트의 주소 정보 초기화  
서버 프로그램은 bind 함수, 클라이언트 프로그램은 connect 함수.

**INADDR-ANY**  
소켓의 IP주소를 위의 상수로 초기화할 경우 소켓이 동작하는 컴퓨터의 IP주소가 자동으로 할당되기 때문에 IP주소를 직접 입력하는 수고를 덜 수 있다. 뿐만 아니라, 컴퓨터 내에 두 개 이상의 IP를 할당 받아서 사용하는 경우, 할당 받은 IP중 어떤 주소를 통해서 데이터가 들어오더라도 PORT번호만 일치하면 수신할 수 있게 된다.  

IP주소는 컴퓨터에 장착되어 있는 NIC(랜카드)의 개수만큼 부여가 가능하다.  

__127.0.0.1__을 가리켜 루프백 주소(loopback address)라 하며 이는 컴퓨터 자신의 IP주소를 의미한다.  

***

* 소켓에 인터넷 **주소 할당**하기  
```c
#include <sys/socket.h>

int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen);  //성공 시 0, 실패 시 -1 반환.
```
>sockfd : 주소 정보를(IP와 PORT를) 할당할 소켓의 **파일 디스크립터**.  
myaddr : 할당하고자 하는 주소정보를 지니는 구조체 변수의 **주소 값**.
addrlen : 두 번째 인자로 전달된 구조체 변수의 길이정보.  

