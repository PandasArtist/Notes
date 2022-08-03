winioctl.h(10011): [C4668] 没有将“_WIN32_WINNT_WIN10_RS2”定义为预处理器宏，用“0”替换“#if/#elif”

```c++
#include "Windows/AllowWindowsPlatformTypes.h"
#include "Windows/PreWindowsApi.h"
#include <winsock.h>
#include "Windows/PostWindowsApi.h"
#include "Windows/MinWindows.h"
```

