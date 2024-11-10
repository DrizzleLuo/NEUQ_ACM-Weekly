# NEUQ ACM预备队第一周周报(数学基础)

## 24 - 11 - 9

### 一、洛谷 p1143 进制转换

***1.getchar 的用法***

==使用 `getchar` 读取一行中的各个字符：==(比较常用)

``` c++
char ch;
vector<char> vec;
while ((ch = getchar()) != '\n')
{
    //操作
}
```
`getchar` 在读取字符时可能会遇到缓冲区中残留的==换行符==,需要先==清空缓冲区==.

``` c++
cin.ignore(numeric_limits<streamsize>::max(), '\n');//需要头文件<limits>
```
***2.ASCII码表的应用***

熟练记住特殊值的ASCII码值:	 **'0' = 48	'A' = 65	'a' = 97(不记得也可用字符代替)**

### 通关代码:

```c++
#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
#include <limits>
using namespace std;
int base_1, base_2;//两个数位
long long number = 0;//对应十进制数
vector<int> vec;
vector<char> after_mod;
int main()
{
    cin >> base_1;
    char ch;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');//清空缓存,对应头文件<limits>
    while ((ch = getchar()) != '\n')
    {
        if (ch >= '0' && ch <= '9')
        {
            vec.push_back(ch - '0');
        }
        if (ch >= 'A' && ch <= 'F')
        {
            vec.push_back(ch - 'A' + 10);
        }
    }

    for (size_t i = 0; i < vec.size(); i++)
    {
        number += vec[i] * pow(base_1, vec.size() - 1 - i);
    }
    cin >> base_2;
    long long num = number;
    while (num > 0)
    {
        if (num % base_2 <= 9)
        {
            after_mod.push_back(num % base_2 + '0');
        }
        else
        {
            after_mod.push_back(num % base_2 + 'A' - 10);
        }

        num /= base_2;
    }

    reverse(after_mod.begin(), after_mod.end());
    for (auto it = after_mod.begin(); it != after_mod.end(); it++)
    {
        cout << *it;
    }

    return 0;
}
```



### 二、 洛谷 p1469 找筷子

 ***异或运算符^的用法***

参加运算的两个数据,按二进制位进行“异或”运算.

运算规则：0^0=0; 0^1=1; 1^0=1;  1^1=0;

特别的：`a ^ a = 0`==**自反性**==(任何数与自身进行异或运算结果为0)

特别的：`a ^ 0 = a`==**恒等性**==(任何数与0进行异或运算结果为其自身)

**一些用法(虽然这题没用到)：**

- **判断奇偶性**：

​	一个数是否是奇数可以通过 `number ^ 1` 来判断.如果结果是比原数大1的数,则该数为奇数.

- **快速求和/差**：

​	异或运算可以用于快速计算没有进位的和,例如 `a + b` 没有进位时的结果与 `a ^ b` 相同.

### 通过代码:(直接置换,比较暴力)

```c++
#include <iostream>
using namespace std;
int main() {
    int n;
    scanf("%d", &n);

    int result = 0;
    int length;

    for (int i = 0; i < n; ++i) {
        scanf("%d", &length);
        result ^= length;//使用异或运算符达到两个对消的效果
    }

    printf("%d\n", result);
    return 0;
}
```

### 三、 洛谷p1100 高低位交换

### 通过代码:

```c++
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;
vector<int> vec;
int main()
{
	long long number;
	cin >> number;
	long long num = number;
	while (num >= 1)
	{
		vec.push_back(num % 2);
		num /= 2;
	}
	while (vec.size() < 32)
	{
		vec.push_back(0);
	}
	for (size_t i = 0; i < 16; i++)
	{
		swap(vec[i], vec[16 + i]);
	}
	long long result = 0;
	for (int i = 0; i < 32; i++)
	{
		if (vec[i] == 1)
		{
			result += pow(2, i);
		}
	}
	printf("%lld", result);
	return 0
}

```

**==但是!==** 我从某些地方看到了别人写的:

```c++
#include <iostream>
using namespace std;

int main() {
    unsigned int number;
    cin >> number;

    // 提取高16位和低16位
    unsigned int high = (number >> 16) & 0xFFFF;
    unsigned int low = number & 0xFFFF;

    // 交换高低16位
    unsigned int new_number = (low << 16) | high;

    // 输出新数值
    cout << new_number << endl;

    return 0;
}
```

通过使用按位运算符可以快速解决此题.
