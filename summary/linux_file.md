##Chapter 1 - 01-2

* File Descriptor (File Handle)
	**0** Standard Input
	**1** Standard Output
	**2** Standard Error
----
* File Open
```C
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *path, int flag);
```
	**path**  
