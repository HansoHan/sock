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

int open(const char *path, int flag);
```  
**path** : 파일 이름을 나타내는 문자열의 주소 값 전달. 
**flag** : 파일의 오픈 모드 정보 전달.
  
>오픈 모드  O_CREAT, 필요하면 파일을 생성  O_TRUNC, 기본 데이터 전부 삭제  O_APPEND, 기존 데이터 보존하고, 뒤에 이어서 저장  O_RDONLY, 읽기 전용으로 파일 오픈  O_WRONLY, 쓰기 전용으로 파일 오픈  O_RDWR, 읽기, 쓰기 겸용으로 파일 오픈

끝
