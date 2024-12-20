#  NEUQ ACM预备队第三/四周周报(动态规划)

## ==**最优子结构	重叠子问题	无后效性**==

### 一、洛谷p1002过河卒

本题通过一个**二维**数组来记录各个子问题的解

### 通关代码:

```c++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    long long xb, yb;
    cin >> xb >> yb;
    vector<vector<long long>> map((xb + 3), vector<long long>((yb + 3)));
    map[2][1] = 1;

    long long hx, hy;
    cin >> hx >> hy;
    hx += 2;
    hy += 2;

    map[hx][hy] = -1;
    map[hx + 1][hy + 2] = -1;
    map[hx + 1][hy - 2] = -1;
    map[hx + 2][hy + 1] = -1;
    map[hx + 2][hy - 1] = -1;
    map[hx - 1][hy + 2] = -1;
    map[hx - 1][hy - 2] = -1;
    map[hx - 2][hy + 1] = -1;
    map[hx - 2][hy - 1] = -1;

    if (map[2][1] == -1 && map[1][2] == -1)
    {
        cout << 0 << endl;
    }

    else
    {
        for (size_t i = 2; i < xb + 3; i++)
        {
            for (size_t j = 2; j < yb + 3; j++)
            {
                if (map[i][j] == -1)
                {
                    continue;
                }
                else if (map[i - 1][j] == -1)
                {
                    map[i][j] = map[i][j - 1];
                }
                else if (map[i][j - 1] == -1)
                {
                    map[i][j] = map[i - 1][j];
                }
                else
                {
                    map[i][j] = map[i - 1][j] + map[i][j - 1];
                }
            }
        }
        cout << map[xb + 2][yb + 2] << endl;
    }
    
    return 0;
}
```

特别的,此处列出输入为 `8 6 0 4 ` 时的记录图

```
0  0  0  0  0  -1  0  -1  0
0  0  0  0  -1  0  0  0  -1
0  1  1  1  1  1  -1  0  0
0  0  1  2  -1  1  1  1  -1
0  0  1  3  3  -1  1  -1  -1
0  0  1  4  7  7  8  8  8
0  0  1  5  12  19  27  35  43
0  0  1  6  18  37  64  99  142
0  0  1  7  25  62  126  225  367
0  0  1  8  33  95  221  446  813
0  0  1  9  42  137  358  804  1617
```

***1.可以注意到,此处为了避免"马"的攻击范围溢出,将数表多开出两行两列***

***2.注意到首位的特殊性,此处将`map[1][3]`处赋值为1***

### 一、洛谷P1020 [NOIP1999 提高组] 导弹拦截

### 通关代码:

```c++
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <algorithm>

using namespace std;

// 函数声明
vector<int> get_data();
int get_lenis_length(const vector<int>& arr);
int get_min_systems(const vector<int>& arr);

int main() {
    vector<int> arr = get_data();

    // 计算最长不升子序列的长度
    int maxIntercept = get_lenis_length(arr);

    // 计算最少需要的拦截系统数量
    int minSystems = get_min_systems(arr);

    // 输出结果
    cout << maxIntercept << endl; // 最多能拦截的导弹数量
    cout << minSystems << endl;   // 最少需要的拦截系统数量

    return 0;
}

// 读取输入数据
vector<int> get_data() {
    vector<int> ans;
    string str;
    getline(cin, str);
    stringstream ss(str);
    int num;
    while (ss >> num) {
        ans.push_back(num);
    }
    return ans;
}

// 计算最长不升子序列的长度
int get_lenis_length(const vector<int>& arr) {
    vector<int> lenis; // 存储当前最长不升子序列的构造
    for (int val : arr) {
        auto it = upper_bound(lenis.begin(), lenis.end(), val, greater<int>()); // 二分查找
        if (it == lenis.end()) {
            lenis.push_back(val); // 如果找不到，则扩展序列
        }
        else {
            *it = val; // 替换更优位置
        }
    }
    return lenis.size();
}

// 计算最少需要的拦截系统数量
int get_min_systems(const vector<int>& arr) {
    vector<int> systems; // 存储每个拦截系统的顶端高度
    for (int val : arr) {
        auto it = lower_bound(systems.begin(), systems.end(), val); // 二分查找
        if (it == systems.end()) {
            systems.push_back(val); // 如果找不到，则新建一个拦截系统
        }
        else {
            *it = val; // 替换更优位置
        }
    }
    return systems.size();
}

```

**贪心与动态规划都能做**
