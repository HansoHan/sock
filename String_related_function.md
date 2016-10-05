##헤더파일 **string.h**에 선언된 문자열 관련 함수들

문자열의 길이를 반환하는 함수 : **strlen**  
```c
#include <string.h>
size_t strlen(const char * s);  
// 전달되 문자열의 길이를 반환하되, "널 문자는 길이에 포함하지 않는다."
```

```c
typedef unsigned int size_t;
```

***

문자열을 복사하는 함수들 : **strcpy**, **strncpy**  
```c
#include <string.h>
char * strcpy(char * dest, const char * src);
char * strncpy(char * dest, const char * src, size_t n);  
// 복사된 문자열의 주소 값 반환.
```
< 예시 >
```c
#include <string.h>

int main(void)
{
  char src[30]="Simple string";
  char dest[30];
  
  strcpy(dest, src);
  
  strncpy(dest, src, sizeof(dest));
}
```

strncpy 함수 호출문의 의미 : src에 저장된 문자열을 dest에 복사하되, src의 길이가 매우 길다면, sizeof(dest)가 반환한 값에 해당하는 문자의 수 만큼만 복사를 진행해라. **그러나, 그 마지막 수에 해당하는 요소가 NULL문자가 맞는지 아닌지는 상관하지 않으니 주의해야 한다.**  
***

문자열을 덧붙이는 함수들 : **strcat**, **strncat**  
```c
#include <string.h>
char * strcat(char * dest, const char * src);
char * strncat(char * dest, const char * src, size_t n);  
// 덧붙여진 문자열의 주소 값 반환.
```
덧붙임이 시작되는 위치는 널 문자 다음이 나닌, 널 문자가 저장된 위치에서부터이다.  
strncpy 함수와 달리 strncat 함수는 문자열의 끝에 널 문자를 자동으로 삽입해준다.  

***

문자열을 비교하는 함수들 : **strcmp**, **strncmp**  
```c
#include <string.h>
int strcmp(const char * s1, const char * s2);
int strncmp(const char * s1, const char * s2, size_t n);  
// 두 문자열의 내용이 "같으면 0", 같지 않으면 0이 아닌 값 반환.
```

