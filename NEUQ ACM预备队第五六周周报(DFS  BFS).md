# NEUQ ACM预备队第三/四周周报(DFS / BFS) 

### 1.P2404 自然数的拆分问题

#### ==**DFS**==

```c++
#include <iostream>
#include <vector>
using namespace std;

int num;
vector<int> ans; // 用于存储当前组合

void dfs(int start, int sum) {
    if (sum == num) {
        // 输出满足条件的组合
        for (size_t i = 0; i < ans.size(); i++) {
            if (ans[i] == sum)
            {
                break;
            }
            if (i != 0) cout << "+";
            cout << ans[i];
        }
        cout << endl;
        return;
    }

    for (int i = start; i <= num; i++) {
        if (sum + i > num) break; // 剪枝
        ans.push_back(i);         // 选择当前数字
        dfs(i, sum + i);          // 递归调用
        ans.pop_back();           // 回溯
    }
}

int main() {
    cin >> num;
    dfs(1, 0);
    return 0;
}

```

### 2.P1036 [NOIP2002 普及组] 选数

#### ==**DFS**==

```c++
#include <iostream>
#include <cmath>
using namespace std;
void dfs(int start, int Count , int sum, int k, int n);
bool is_prime(int num);
int ans = 0;
int arr[20];
int main()
{
	int n, k;
	cin >> n >> k;

	for (size_t i = 0; i < n; i++)
	{
		cin >> arr[i];
	}//输入

	dfs(0, 0, 0, k, n);
	cout << ans << endl;
    return 0;
}
void dfs(int start, int Count , int sum, int k, int n)
{
	if (Count == k)
	{
		if (is_prime(sum))
		{
			ans++;
		}
		return;
	}//到底判断

	for (size_t i = start; i < n; i++)
	{
		dfs(i + 1, Count + 1, sum + arr[i], k, n);//递归
	}
}
bool is_prime(int num)
{
	if (num <= 1)
	{
		return false;
	}
	for (size_t i = 2; i <= sqrt(num); i++)
	{
		if (num % i == 0)
		{
			return false;
		}
	}
	return true;
}//质数的判断
```

