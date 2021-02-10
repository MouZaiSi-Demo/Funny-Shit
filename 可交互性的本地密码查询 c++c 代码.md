#### 可交互性的本地密码查询 c++/c 代码



```c++
#include<iostream>
#include<malloc.h>
using namespace std;

int main(void)
{
	char s[100];
	char str[100];
	system("netsh wlan show profiles");
	cout << "(输入区分大小写 然后输完记得按回车键哦)" << endl;
	cout << "上面是你电脑里已经保存的Wife 账号密码 现在请输出你想查的密码吧:";
	cin >> s;
	sprintf_s(str, "netsh wlan show profiles name = %s key = clear",s);
	system(str);
	cout << endl << "安全密匙 下面的 关键内容 就是密码啦";

	return 0;
}

```

遇到一个需求，在c++代码中调用system函数，在system函数里调用变量，

system()只接受常量 const char *

所以你必须在传进去之前把命令整合好

解决方法：使用sprintf函数预处理，然后再传到system去

```c++
char pcCMD[255];
sprintf(pcCMD, "sfdp -Tpng -Nfixedsize=true -Nwidth=%f -Nheight=%f 111.txt -o 111.png", a, b);
system(pcCMD);
```

------

##### sprintf用法(如果vs报不安全就加用sprintf_s)

```c++
#include <stdio.h>
int sprintf(char *str, const char *format, ...);
```

功能：

根据参数format字符串来转换并格式化数据，然后将结果输出到str指定的空间中，直到 出现字符串结束符 '\0' 为止。

参数：

str：字符串首地址

format：字符串格式，用法和printf**()**一样

返回值：

成功：实际格式化的字符个数

失败： **-** 1



##### 发布成exe的方法(以vs为例子)

新建一个控制台项目(非空项目)\

打入你的代码 

并在return 0;前加上

```c++
system("pause");
```

再Ctrl + F5 编译 最后去你的项目文件夹下的release文件夹找到与项目同名后缀为exe的文件