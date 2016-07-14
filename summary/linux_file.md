##Chapter 1, 01-2

* File Descriptor (File Handle)  
	**0** Standard Input  
	**1** Standard Output  
	**2** Standard Error  

***

* 파일 열기
```C
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *path, int flag); // 성공 시 파일 디스크립터, 실패 시 -1 반환
```  
**path** : 파일 이름을 나타내는 문자열의 주소 값 전달.  
**flag** : 파일의 오픈 모드 정보 전달.
  
>오픈 모드  
O_CREAT, 필요하면 파일을 생성  
O_TRUNC, 기본 데이터 전부 삭제  
O_APPEND, 기존 데이터 보존하고, 뒤에 이어서 저장  
O_RDONLY, 읽기 전용으로 파일 오픈  
O_WRONLY, 쓰기 전용으로 파일 오픈  
O_RDWR, 읽기, 쓰기 겸용으로 파일 오픈

***
* 파일 닫기
```C
#include <unistd.h>

int close (int fd); // 성공 시 0, 실패 시 -1 반환
```
***
* 파일에 데이터 쓰기
```C
#include <unistd.h>

ssize_t write(int fd, const void *buf, size_t nbytes); // 성공 시 전달한 바이트 수, 실패 시 -1 반환
```
**fd** 데이터 전송대상을 나타내는 파일 디스트립터 전달.
**buf** 전송할 데이터가 저장된 버퍼의 주소 값 전달.
**nbytes** 전송할 데이터의 바이트 수 전달.
>_t로 끝나는 자료형 = 'primitive' 자료형  
이들은 일반적으로 sys/types.h 헤더 파일에 typedef 선언을 통해서 정의되어 있다.  
프로그래머에 의해 정의된 것과 구별해서, 시스템(운영체제)에서 정의하는 자료형의 이름에는 _t가 붙어있다.  

***
