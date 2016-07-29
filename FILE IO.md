## File Input / Output

다음은 스트림을 형성할 때 호출하는 함수이다. 이 함수의 호출을 통해서 프로그램 상에서 파일과의 스트림을 형성할 수 있다.  
```c
#include <stdio.h>

FILE * fopen(const char * filename, const char * mode); // 성공 시 해당 파일의 FILE 구조체 변수의 주소 값, 실패 시 NULL 포인터 반환.
```
이 함수는 해당 파일과의 스트림을 형성하고 **스트림 정보를 FILE 구조체 변수에 담아서** 그 변수의 주소 값을 반환한다.  
  
사실 **FILE 구조체**가 어떻게 정의되어 있는지를 알 필요는 없다. FILE 구조체 변수의 멤버에 **직접 접근할 일**이 없기 때문이다. 위 함수가 반환하는 **FILE 구조체의 포인터**는 *__파일을 가리키기 위한 용도__*로 사용된다. 즉 이 포인터를 이용해서 파일에 데이터를 저장하거나 파일에 저장된 데이터를 읽게 된다.  

```c
#include <stdio.h>

int main(void)
{
  FILE * fp=fopen("data.txt", "wt");  // "wt"를 전달함으로써 "텍스트(t)" 데이터 "출력(w)"용 스트림이 형성된다.
  if(fp==NULL){
    puts("파일오픈 실패!");
    return -1;
  }
  
  fputc('A', fp);
  fputc('B', fp);
  fputc('C', fp);
  fclose(fp);
  return 0;
}
```
출력 스트림을 형성하는 경우에는 해당 **파일이 생성**된다. 이렇게 해서 스트림이 형성되면, 이제부터 *__fp__*는 파일 *__data.txt.__*를 지칭하는 포인터가 된다.  

***

**fclose 함수**는 fopen 함수의 반대 기능을 제공한다. fclose 함수는 파일을 닫는 함수이다.  
```c
#include <stdio.h>

int fclose(FILE * stream);  // 성공 시 0, 실패 시 EOF를 반환.
```
* 개방되었던 파일을 닫아줘야 하는 이유.  
운영체제가 할당한 자원의 반환. (시스템의 자원, 주로 메모리)  
버퍼링 되었던 데이터의 출력.  

fclose 함수의 호출을 통해서 파일을 닫아주면 출력 버퍼에 저장되어 있던 데이터가 파일로 이동하면서 출력 버퍼는 비워지게 된다.  

***

**스트림을 종료하지 않고 버퍼만 비우고 싶을 때**에는 다음 **_fflush_** 함수를 호출하면 된다.  
```c
#include <stdio.h>

int fflush(FILE * stream);  // 함수 호출 성공 시 0, 실패 시 EOF 반환.
```
* 출력버퍼를 비운다는 것은 출력버퍼에 저장된 데이터를 목적지로 전송한다는 의미.  
* 입력버퍼를 비운다는 것은 입력버퍼에 저장된 데이터를 소멸시킨다는 의미.  
* **fflush 함수는 출력버퍼를 비우는 함수이다.**  
* fflush 함수는 입력버퍼를 대상으로 호출할 수 없다.  

파일 스트림의 입력버퍼를 비우는 함수는 사실 필요없다.  
***

```c
#include <stdio.h>

int main(void)
{
  int ch, i;
  FILE * fp=fopen("data.txt", "rt");  // "rt"를 전달함으로써 "텍스트(t)" 데이터 "입력(r)"용 스트림이 형성된다.
  if(fp==NULL){
    puts("파일 오픈 실패!");
    return -1;
  }
  
  for(i=0; i<3; i++)
  {
    ch=fgetc(fp);  // FILE 구조체의 포인터 fp가 지칭하는 파일에 저장된 문자 하나가 반환되어 변수 ch에 저장되고 있다.
    printf("%c \n", ch);
  }
  fclose(fp);
  return 0;
}
```

***

## File의 개방 모드 (MODE)  

1. 읽기 위한 스트림인가, 쓰기 위한 스트림인가.  
데이터 READ 스트림 : 읽기만 가능. -> r, (r+ 읽기/쓰기 가능)    
데이터 WRITE 스트림 : 쓰기만 가능. -> w, (w+ 읽기/쓰기 가능)     
데이터 APPEND 스트림 : 쓰되 덧붙여 쓰기만 가능. -> a, (a+ 읽기/덧붙여 쓰기 가능)    
데이터 READ/WRITE 스트림 : 읽기, 쓰기 모두 가능.  

2. 텍스트 데이터를 위한 스트림인가, 바이너리 데이터를 위한 스트림인가.  
텍스트 파일 : 사람이 인식할 수 있는 문자를 담고 있는 파일.  
바이너리 파일 : 그 이외에 컴퓨터가 인식할 수 있는 데이터를 담고 있는 파일.  

**개행의 표현 방식**  
개행의 표현은 운영체제마다 차이가 있기 때문에 개행 문자가 포함되는 텍스트 데이터의 저장에는 주의가 필요하다. 개행의 표시 방법이 C언어와 다른 운영체제에서는 개행 정보를 파일에 어떻게 저장해야 할까. 이를 위해서는 **변환**이 필요하다. **파일을 텍스트 모드로 개방하면 된다**.  

파일을 텍스트 모드로 개방하면 위에서 말한 변환이 자동으로 이뤄진다. 때문에 우리가 직접 개행 문자의 변환을 신경 쓸 필요가 없다.  

텍스트 모드 : **rt, wt, at, r+t, w+t, a+t**  
바이너리 모드 : **rb, wb, ab, r+b, w+b, a+b**  

##File IO 함수의 기본  
```c
#include <stdio.h>

int main(void)
{
    FILE * fp=fopen("simple.txt", "wt");
    char str[30];
    int ch;
    if(fp==NULL){
        puts("파일 오픈 실패");
        return -1;
    }
    
    fputc('A', fp);
    fputc('B', fp);
    fputs("My name is Dan \n", fp);
    fputs("Your name is Hank \n", fp);
    fclose(fp);
    
    fp=fopen("simple.txt", "rt");
    if(fp==NULL){
        puts("파일 오픈 실패");
        return -1;
    }
    
    ch=fgetc(fp);
    printf("%c \n", ch);
    ch=fgetc(fp);
    printf("%c \n", ch);
    
    fgets(str, sizeof(str), fp);
    printf("%s", str);
    fgets(str, sizeof(str), fp);
    printf("%s", str);
    
    fclose(fp);
    return 0;
}
```

* feof 함수 기반의 파일 복사 프로그램  
때로는 파일의 마지막에 저장된 데이터까지 모두 읽어 들여야 하는 상황이 존재한다. 파일의 끝을 확인하는 방법이 필요한데, feof 함수가 이러한 목적으로 정의된 함수이다.  
```c
#include <stdio.h>
int feof(FILE * stream);  // 파일의 끝에 도달한 경우 0이 아닌 값 반환.
```
***

* **바이너리** 데이터의 입출력 : **fread**, **fwrite**  
```c
#include <stdio.h>
size_t fread(void * buffer, size_t size, size_t count, FILE * stream);

// 성공 시 전달인자 count, 실패 또는 파일의 끝 도달 시 count보다 작은 값 반환. 
```
< 예시 >  
```c
int buf[12];
fread((void*) buf, sizeof(int), 12, fp);
// sizeof(int) 크기의 데이터 12개를 fp로부터 읽어 들여서 배열 buf에 저장하라.
// 그리고 이 함수는 "실제로 읽어 들인" 데이터의 "갯수"를 반환한다.
```


```c
#include <stdio.h>
size_t fwrite(const void * buffer, size_t size, size_t count, FILE * stream);

// 성공 시 전달인자 count, 실패 시 count보다 작은 값 반환.
```
< 예시 >
```c
int buf[7]={1, 2, 3, 4, 5, 6, 7};
fwrite((void*)buf, sizeof(int), 7, fp);
//sizeof(int) 크기의 데이터 7개를 buf로부터 읽어서 fp에 저장하라.
```
