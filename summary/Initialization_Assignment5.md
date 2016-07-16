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

char * inet_ntoa(struct in_addr adr);  //성공 시 
```


