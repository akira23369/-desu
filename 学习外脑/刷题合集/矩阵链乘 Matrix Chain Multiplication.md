[矩阵链乘 Matrix Chain Multiplication - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/UVA442)

![[Pasted image 20231221222445.png]]
#算法/栈 
```cpp
#include <iostream>
#include <stack>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 30;

typedef std::pair<int, int> PII;
PII a[N];

int main()
{
    int n;
    std::cin >> n;
    for (int i = 1; i <= n; i++)
    {
        char ch;
        int x, y;
        std::cin >> ch;
        std::cin >> a[ch - 'A'].first >> a[ch - 'A'].second;
    }
    std::string str;
    stack<PII> s;
    while (std::cin >> str)
    {
        bool flag = false;
        int sum = 0;
        for (int i = 0; i < str.length(); i++)
        {
            if (isalpha(str[i]))
            {
                s.push(a[str[i] - 'A']);
            }
            if (str[i] == ')') 
            {
                auto ch2 = s.top(); s.pop();
                auto ch1 = s.top(); s.pop();
                int x = ch1.first, y = ch1.second, z = ch2.second;
                if (y != ch2.first)
                {
                    flag = true;
                    break;
                }
                sum += x * y * z;
                s.push({x, z});
            }
        }
        if (flag) std::cout << "error\n";
        else std::cout << sum << "\n";
    }
}

```