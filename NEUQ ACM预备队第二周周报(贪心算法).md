# NEUQ ACM预备队第二周周报(贪心算法)

## 24-11-15

### 一、洛谷 p1031 均分纸牌

****

**贪心策略：** 逐堆调整，始终将多余或不足的物品尽量传递到下一堆。由于是逐步调整的，当前堆的物品总能达到目标值，最终所有堆的物品都一致。

### 通关代码

```c++
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

int main() {
    int piles, sum = 0, moves = 0;
    cin >> piles;

    vector<int> vec(piles);

    for (int i = 0; i < piles; ++i) {
        cin >> vec[i];
        sum += vec[i];
    }

    int average = sum / piles;

    for (int i = 0; i < piles; i++) {
        if (vec[i] > average && i != piles - 1)
        {
            vec[i + 1] += vec[i] - average;
            moves++;
        }
        else if (vec[i] < average && i != piles - 1)
        {
            vec[i + 1] -= average - vec[i];
            moves++;
        }
        else if (vec[i] == average)
        {
            continue;
        }
    }

    cout << moves << endl;
    return 0;
}

```

## 二、洛谷P2695 骑士的工作

### 通关代码

**贪心思想**：优先用最小的金币满足最小需求的龙头，尽量为后续的龙头保留更大的金币。

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int ans = 0;
int main()
{
	int n, m;
	cin >> n >> m;
	vector<int> heads;
	vector<int> coins;
	for (size_t i = 0; i < n; i++)
	{
		int buffer;
		cin >> buffer;
		heads.push_back(buffer);
	}
	for (size_t i = 0; i < m; i++)
	{
		int buffer;
		cin >> buffer;
		coins.push_back(buffer);
	}
	if (n > m)
	{
		cout << "you died!";
	}
	else
	{
		sort(heads.begin(), heads.end());
		sort(coins.begin(), coins.end());
		int p = 0;
		for (size_t i = 0; i < coins.size(); i++)
		{
			if (heads.size() == 0)
			{
				break;
			}
				if (coins[i] >= heads[p])
				{
					ans += coins[i];
					heads.erase(heads.begin());
					continue;
				}
				else
				{
					continue;
				}

		}
		if (heads.size() == 0)
		{
			cout << ans << endl;
		}
		else
		{
			cout << "you died!" << endl;
		}
	}
}
```



## 三、洛谷P5639 【CSGRound2】守序者的尊严

**贪心策略**：每当遇到一个与前一个元素不同的元素时，就认为它是一个新的段。每一步做出局部最优的选择，最终得到全局最优解。

### 通关代码

```c++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
	int n;
	cin >> n;
	vector<int> vec;
	int ans = 1;
	for (size_t i = 0; i < n; i++)
	{
		int buffer;
		cin >> buffer;
		vec.push_back(buffer);
	}
	for (size_t i = 1; i < n; i++)
	{
		if (vec[i] == vec[i - 1])
		{
			continue;
		}
		else
		{
			ans++;
		}
	}
	cout << ans << endl;
}
```

### 三、洛谷P1106 删数问题

### 通关代码

```c++
#include <iostream>
#include <vector>
using namespace std;
int main(void)
{
	vector<char> vec;
	vector<int> zero;
	int Count = 0;
	char ch;
	while ((ch = getchar()) != '\n')
	{
		vec.push_back(ch);
		if (ch == '0')
		{
			zero.push_back(Count);//zero中为0开头数组位数
		}
		Count++;
	}

	//1.检测是否有0能消除。有的话一下消除的位数会增多，血赚
	//2.消除前导零之后前k个选择最优的
	int k;
	cin >> k;

	if (k >= vec.size())
	{
		cout << 0 << endl;
		return 0;
	}

	int k_c = k;
	int flag = -1;
	if (zero.size() != 0)
	{
		if ((zero[0] + 1) <= k)
		{
			for (size_t i = 0; i < vec.size(); i++)
			{
				if (vec[i] == '0')
				{
					flag++;
				}
				else
				{
					k_c--;
				}
				if (k_c == 0)
				{
					break;
				}
			}
		}
	}
	if (flag != -1)
	{
		k -= (zero[flag] - flag);
		vec.erase(vec.begin(), vec.begin() + zero[flag] + 1);
	}

	//for (size_t i = 0; i < vec.size(); i++)
	//{
	//	cout << vec[i];
	//}cout << "qwq" << k << endl;



	int awa = 0;//已经取得的位数
	while (k > 0)
	{
		char min = ':';
		int q = 0;
		for (size_t i = awa; i <= k + awa; i++)
		{
			/*cout << vec[i] << "***" << endl;*/
			if (vec[i] < min)
			{
				q = i;//q是最小值所在位置(数组)
				min = vec[i];
			}
		}
		//找到后k个数中最小的当下一位
		/*cout << q << ")))" << vec[q] << endl;
		cout << "awa" << awa << endl;*/
		if (q != awa)
		{
			k -= (q - awa);
			vec.erase(vec.begin() + awa, vec.begin() + q);
		}

		awa++;
	}
	//int awa = 0;
	//while (k > 0)
	//{
	//	char min = '9';
	//	int q = 0;
	//	for (size_t i = awa; i <= k; i++)
	//	{
	//		if (vec[i] <= min)
	//		{
	//			q = i;
	//			min = vec[i];
	//		}
	//	}
	//	if (q != awa)
	//	{
	//		k -= q;
	//		vec.erase(vec.begin() + awa, vec.begin() + awa + q);
	//	}
	//	if (vec[awa] == '0')
	//	{
	//		int size = awa;
	//		for (size_t j = 1; j < vec.size(); j++)
	//		{
	//			if (vec[j] == '0')
	//			{
	//				size++;
	//			}
	//			else
	//			{
	//				break;
	//			}
	//		}
	//		vec.erase(vec.begin() + awa, vec.begin() + size + awa);
	//	}
	//	awa++;
	//}
	if (vec.size() == 0)
	{
		cout << 0 << endl;
		return 0;
	}
	for (size_t i = 0; i < vec.size(); i++)
	{
		if (vec[0] == 0)
		{
			continue;
		}
		if (vec[i] == 0 && vec[i - 1] == 0 && i != 0)
		{
			continue;
		}
		cout << vec[i];
	}cout << endl;
	//除去前导0并输出
	return 0;
}
```

写的比较麻烦，但是个人觉得收获很大。
