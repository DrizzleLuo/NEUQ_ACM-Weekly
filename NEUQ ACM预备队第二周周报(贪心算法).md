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

