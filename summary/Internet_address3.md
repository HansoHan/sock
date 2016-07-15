##Chapter 03, 03-1 ~ 03-2  
IP 주소 체계는 IPv4(*__4바이트__* 주소체계)와 IPv6(*__6바이트__* 주소체계)로 나뉜다.  
* IPv4  
IPv4 기준의 4바이트 IP주소는 *__네트워크 주소__*와 *__호스트 주소__*로 나뉘며, 주소의 형태에 따라서 A, B, C, D, E 클래스로 분류가 된다. 참고로 클래스 E는 일반적이지 않은, 예약되어있는 주소 체계이다.  
  
  
![IPv4](http://wiki.mikrotik.com/images/e/ee/Image3001.gif)  

>클래스 A의 첫 번째 바이트 범위 : 0 이상 127 이하 -> 클래스 A의 첫 번째 비트는 항상 0으로 시작  
클래스 B의 첫 번째 바이트 범위 : 128 이상 191 이하 -> 클래스 B의 첫 두 비트는 항상 10으로 시작  
클래스 C의 첫 번째 바이트 범위 : 192 이상 223 이하 -> 클래스 C의 첫 세 비트는 항상 110으로 시작  

***
PORT번호는 하나의 운영체제 내에서 소켓을 구분하는 목적으로 사용되기 때문에, 하나의 운영체제 내에서 동일한 PORT번호를 둘 이상의 소켓에 할당할 수 없다.  
* 소켓의 구분에 활용되는 PORT번호  
컴퓨터에는 *__NIC(Network Interface Card)__*이라 불리는 데이터 송수신장치가 하나씩 달려있다. IP는 데이터를 NIC을 통해 컴퓨터 내부로 전송하는데 사용된다. 그러나 컴퓨터 내부로 전송된 데이터를 소켓에 적절히 분배하는 작업은 __운영체제__가 담당한다. 이 때 운영체제는 PORT번호를 활용(참조)한다. NIC을 통해서 수신된 데이터 안에 PORT번호가 새겨져 있다.  
  
PORT번호는 16비트로 표현된다. (0이상 65535이하)  
0부터 1023번까지는 'well-known PORT'라고 해서, 특정 프로그램에 할당하기로 예약되어있다.  
TCP와 UDP 소켓은 PORT번호를 공유하지 않기 때문에 중복되어도 상관없다.

***

* IPv4 기반의 주소표현을 위한 __구조체__  
"목적지 주소의 표현 방법", 이 구조체는 bind 함수에 주소정보를 전달하는 용도로 사용된다.  
```c
struct sockaddr_in
{
  sa_family_t     sin_family; //주소 체계(Address Family)
  unit16_t        sin_port;   //16비트 TCP/UDP PORT번호
  struct in_addr  sin_addr;   //32비트 IP주소
  char            sin_zero[8] //사용되지 않음
};
```
```c
struct in_addr
{
  in_addr_t       s_addr;     //32비트 IPv4 인터넷 주소
};
```
__POSIX__(Portable Operating System Interface)란 유닉스 계열의 운영체제에 적용하기 위한 표준을 의미한다. 즉 POSIX에서는 다음과 같이 추가로 자료형을 정의하고 있다.  

>헤더파일 sys/type.h에 선언된 자료형  
```c
int8_t    // signed 8-bit int  
uint8_t   // unsigned 8-bit int (unsigned char)  
int16_t   // signed 16-bit int  
uint16_t  // unsigned 16-bit int (unsigned short)  
int32_t   // signed 32-bit int  
uint32_t  // unsigned 32-bit int (unsigned long)  
```

>헤더파일 sys/socket.h에 선언된 자료형  
```c
sa_family_t   // 주소체계 (address family)  
socklen_t     // 길이정보 (length of struct)  
```

>헤더파일 netinet/in.h에 선언된 자료형  
```c
in_addr_t     // IP 주소 정보, uint32_t로 정의되어 있음  
in_port_t     // PORT 번호 정보, uint16_t로 정의되어 있음  
```
