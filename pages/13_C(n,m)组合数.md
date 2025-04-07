## 14.C(n,m)组合数
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int main()
{
    ll t;
    cin >> t;
    while(t--)
    {
        ll n, m;
        cin >> n >> m;
        if(m == 0 || m == n)
        {
            cout << 1 << endl;
            continue;
        }
        m = min(m, n - m);
        ll a = 1, b = 1;
        ll mm = m;  // 使用临时变量 mm，避免修改原始变量 m

        for(ll i = 1; i <= mm; ++i)
        {
            a *= (n - i + 1);  // 计算分子 n * (n-1) * ... * (n-m+1)
            b *= i;            // 计算分母 m!
        }
        cout << a / b << endl;  // 输出结果
    }
    return 0;
}
//递归算法：
/*
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

// 递归函数计算组合数 C(n, m)
ll combination(ll n, ll m)
{
    // 递归终止条件
    if(m == 0 || m == n)
        return 1;
    if(m > n)
        return 0;

    // 递归公式
    return combination(n-1, m-1) + combination(n-1, m);
}

int main()
{
    ll t;
    cin >> t;  // 输入测试用例数
    while(t--)
    {
        ll n, m;
        cin >> n >> m;
        cout << combination(n, m) << endl;  // 调用递归函数并输出结果
    }
    return 0;
}
*/
```