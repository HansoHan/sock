##Chapter 3, 03-3

4바이트 정수 1을 저장하는 방식이 CPU마다 다를 수 있다.  
00000000 00000000 00000000 00000001 또는 00000001 00000000 00000000 00000000  

*__저장__* 순서가 다르다는 것은 전송되어온 데이터의 *__해석__* 순서가 다르다는 것을 뜻한다.  

* 바이트 순서와 네트워크 바이트 순서  
CPU가 데이터를 메모리에 저장하는 방식은 다음과 같이 두 가지로 나뉜다. (0x12345678, 여기서 최하위 바이트는 0x78)    
 - **Big Endian** : 상위 바이트의 값을 작은 번지수에 저장하는 방식. (0x12 0x34 0x56 0x78)  
 - **Little Endian** : 상위 바이트의 값을 큰 번지수에 저장하는 방식. (0x78 0x56 0x34 0x12) -> 인텔 그리고 AMD 계열 CPU      

__CPU의 데이터 저장 방식 = Host Byte Order__  
_"호스트 바이트 순서가 다른 두 CPU가 데이터를 주고받을 때 발생할 수 있는 문제점"_  
네트워크를 통해서 데이터를 전송할 때에는 통일된 기준으로 데이터를 전송하기로 약속하였으며, 이 약속을 가리켜 Network Byte Order라 한다.  
*__"Big Endian 방식으로 통일하자__*

* 바이트 순서의 변환 (Endian Conversions)  
```c
unsigned short htons(unsigned short);  // short형 데이터를 host 바이트 순서에서 network 바이트 순서로 변환하라. 
unsigned short ntohs(unsigned short);  // s가 붙는 것은 PORT번호 변환에 사용됨.
unsigned long htonl(unsigned long);
unsigned long ntohl(unsigned long);    // l이 붙는 것은 IP주소 변환에 사용됨.
```
이 변환의 과정은 자동으로 이뤄지기 때문에, sockaddr_in 구조체 변수에 데이터를 채울 때 이외에는 바이트 순서를 신경쓰지 않아도 된다.  
