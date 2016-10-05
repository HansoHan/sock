##Stream, 한 방향으로 흐르는 데이터의 흐름  
운영체제는 **외부장치**와 **프로그램**과의 **데이터 송수신**의 도구가 되는 스트림을 제공하고 있다.  

파일과의 연결을 위한 스트림의 생성은 우리가 직접 요구해야 하지만, 콘솔과의 연결을 위한 스트림의 생성은 요구할 필요가 없다.  

**콘솔 입출력을 위한 '입력 스트림'과 '출력 스트림'은 프로그램이 실행되면 자동으로 생성되고, 프로그램이 종료되면 자동으로 소멸되는 스트림이다.**  

즉, 이 둘은 기본적으로 제공되는 Standard stream이다.  

stdin : 표준 입력 스트림, 키보드 대상으로 입력.  
stdout : 표준 출력 스트림, 모니터 대상으로 출력.  
stderr : 표준 에러 스트림, 모니터 대상으로 출력.  

이후에 '입출력 리다이렉션(redirection)'이라는 기술을 익히고 나면, 표준 에러 스트림의 출력 대상을 변경시킬 수 있어서 stdout과 stderr의 용도를 구분할 수 있게 될 것이다.  
  
##*문자* 단위 입출력 함수  

**문자 출력 함수** : putchar, fputc  
```c
#include <stdio.h>

int putchar(int c);
int fputc(int c, FILE * stream);  
// 함수 호출 성공 시 쓰여진 문자 정보가, 실패 시 EOF 반환.
```
**putchar** 함수는 인자로 전달된 문자 정보를 stdout으로 표현되는 표준 출력 스트림으로 전송하는 함수이다.  
단, **fputc** 함수는 문자를 전송할 스트림을 지정할 수 있다. 즉 fputc 함수를 이용하면 stdout 뿐만 아니라, 파일을 대상으로도 데이터를 전송할 수 있다.  

**문자 입력 함수** : getchar, fgetc  
```c
#include <stdio.h>

int getchar(void);
int fgetc(FILE * stream);  // 파일의 끝에 도달하거나 함수호출 실패 시 EOF 반환.
```
**getchar** 함수는 stdin으로 표현되는 표준 입력 스트림으로부터 하나의 문자를 입력 받아서 반환하는 함수이다.  
**fgetc** 함수는 문자를 입력 받을 스트림을 지정할 수 있다.  

**'엔터 키'도 아스키 코드 값이 10인 '\n'으로 표현되는 문자이다. 따라서 입출력의 대상이 된다.**  

**getchar 함수와 fgetc 함수의 반환형**이 char형이 아닌 **int**형인 이유 : EOF가 -1로 지정된 상수이기 때문. 그것을 왜 신경써야 하냐면, char을 unsigned char로 처리하는 컴파일러가 있기 때문에.    

**EOF** : End Of File, 파일의 끝을 표현하기 위해서 정의해 놓은 상수. fgetc함수와 getchar 함수는 함수 호출이 실패했거나, Windows에서 CRTL+Z키, Linux에서 CTRL+D 키가 입력되는 경우 EOF를 반환한다.  
```c
#include <stdio.h>

int main(void)
{
  int ch;
  
  while(1)
  {
    ch=getchar();
    if(ch==EOF)
      break;
    putchar(ch);
  }
  return 0;
}
```
-> 문장이 입력되면 문장을 구성하는 문자의 수만큼 getchar 함수가 호출되면서 모든 문자를 읽어 들일 것이다.  

##**문자열** 단위 입출력 함수  
scanf 함수는 공백이 포함된 형태의 문자열을 입력 받는데 제한이 있었다.  

**문자열 출력 함수** : puts, fputs  
```c
#include <stdio.h>

int puts(const char * s);
int fputs(const char * s, FILE * stream);  
// 성공 시 음수가 아닌 값을, 실패 시 EOF 반환.
```
두 함수 사이에 있는 중요한 차이 : **puts 함수가 호출되면 문자열 출력 후 자동으로 개행이 이뤄지지만, fputs 함수가 호출되면 문자열 출력 후 자동으로 개행이 이뤄지지 않는다.**  

**문자열 입력 함수** : gets, fgets  
```c
#include <stdio.h>

char * gets(char * s);
char * fgets(char * s, int n, FILE * stream);  
// 파일에 끝에 도달하거나, 함수호출 실패 시 NULL 포인터 반환.
```
< 예시 >  
```c
int main(void)
{
  char str[7];
  gets(str);
  fgets(str, sizeof(str), stdin);  
  // sizeof(str)의 길이만큼만 저장해라. 
  //문자열을 입력 받으면 문자열의 끝에 자동으로 널 문자가 추가된다.
}
```
```c
#include <stdio.h>

int main(void)
{
  char str[7];
  int i;
  
  for(i=0; i<3; i++)
  {
    fgets(str, sideof(str), stdin);
    printf("Read %d : %s \n", i+1, str);
  }
  return 0;
}
```
**fgets 함수는 \n을 만날 때까지 문자열을 읽어 들이는데, \n을 제외시키거나 버리지 않고 문자열의 일부로 받아들인다.**  

ClearLineFromReadBuffer 함수는 사실 입력버퍼를 통째로 비우는 함수가 아니라, \n이 읽혀질 때까지 입력버퍼에 저장된 문자들을 지우는 함수이다.  
```c
void ClearLineFromReadBuffer(void)
{
  while(getchar()!='\n')
}
```
