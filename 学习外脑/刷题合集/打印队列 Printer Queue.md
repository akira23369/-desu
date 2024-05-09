[打印队列 Printer Queue - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/UVA12100)

#算法/队列

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <stack>
#include <cstring>
#include <algorithm>
using namespace std;
int T;
int n;

int main()
{
    std::cin >> T;
    while (T--)
    {
        int x;
        std::cin >> n >> x;
        std::vector<int> a, b;
        std::queue<int> q;
        for (int i = 0; i < n; i++) 
        {
            int x;
            std::cin >> x;
            a.push_back(x);
			b.push_back(x); // 重要度排序数组
            q.push(i);
        }
        std::sort(b.begin(), b.end(), greater<int>());
        int k = 0;
        while (q.size())
        {
            int t = q.front();
            if (a[t] < b[k])
            {
                q.pop();
                q.push(t);
            }
            else 
            {
                if (t == x) 
                {
                    std::cout << k + 1 << "\n";
                    break;
                }
                else 
                {
                    k++;
                    q.pop();
                }
            }
        }
    }
}
```