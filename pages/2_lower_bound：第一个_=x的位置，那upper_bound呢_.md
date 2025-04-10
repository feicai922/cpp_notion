## 2.lower_bound：第一个>=x的位置，那upper_bound呢?
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() 
{
    int n, m;
    cin >> n >> m;
    vector<int> a(n), b(m);

    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    
    for (int i = 0; i < m; i++) scanf("%d", &b[i]);
    
    for (int i = 0; i < m; i++) 
    {
        // 使用lower_bound查找第一个 >= b[i]的元素的位置
        auto it = lower_bound(a.begin(), a.end(), b[i]);

        // 检查是否找到等于b[i]的元素
        if (it != a.end() && *it == b[i]) 
        {
            cout << it - a.begin() + 1 << " ";  // 输出位置（从1开始编号）
        } 
        else 
        {
            cout << -1 << " ";  // 没有找到，输出-1
        }
    }
    return 0;
}
```