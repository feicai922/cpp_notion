# 欢迎来到我的算法笔记项目
## 1.快速二分查找
```
#include<bits/stdc++.h>
using namespace std;

int main()
{
    int n,q;
    cin>>n>>q;
    int a[n];
    for(int i=1;i<=n;i++) cin>>a[i];
    while(q--)
    {
        int op,l,r,x;
        cin>>op>>l>>r>>x;
        if(op == 1)
        {
            int t = lower_bound(a+l,a+r,x)-a;
            if(a[t] != x) cout<< -1<<'\n';
            else cout<<t<<'\n';
        }
        else if(op == 2)
        {
            int t = upper_bound(a+l,a+r,x)-a-1;
            if( a[t]!=x || t>r|| t<l) cout<<-1<<'\n';
            else cout<<t<<'\n';
        }
        else if(op == 3)
        {
            int t= lower_bound(a+l,a+r,x)-a;

            if(a[t] <x) cout<<-1<<'\n';
            else cout<<t<<'\n';

        }
        else
        {
            int t= upper_bound(a+l,a+r,x)-a;
            if(a[t] <= x) cout<<-1<<'\n';
            else cout<<t<<'\n';
        }
    }
    return 0;
}
```
## 2.lower_bound：第一个>=x的位置，那upper_bound呢?
```
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
# 3.双指针：A-B=C
```
#include <bits/stdc++.h>
#define ll long long

using namespace std;

const int N = 2e5 + 10;
int n , c;
int a[N];

int main ()
{
    cin >> n >> c;
    for(int i = 1 ; i <= n ; i ++) cin >> a[i];
    sort(a + 1 , a + 1 + n);
    int l = 1, r1 = 1 , r2 = 1;
    ll ans = 0;
    // r1 用于找到第一个满足 a[r1]−a[l] > c 的位置。
    // r2 用于找到第一个满足 a[r2]−a[l] ≥ c 的位置。
    // 如果 a[r2]−a[l]=c 且 a[r1−1]−a[l]=c，则认为在 [r2, r1 - 1] 区间内的所有数都满足条件。
    for(l = 1 ; l <= n ; l ++)
    {
        while(r1 <= n && a[r1] - a[l] <= c) r1 ++;
        while(r2 <= n && a[r2] - a[l] < c ) r2 ++;
        if(a[r2] - a[l] == c && a[r1 - 1] - a[l] == c && r1 - 1 >= 1)
            ans += r1 - r2;
    }
    cout << ans;
    return 0;
}
```
## 4.学校分数线与不满意度（二分）
https://www.luogu.com.cn/problem/P1678
```
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<int> sch(n), stu(m);

    for (int i = 0; i < n; i++) cin >> sch[i];

    for (int i = 0; i < m; i++) cin >> stu[i];

    sort(sch.begin(), sch.end());

    long long ans = 0;

    for (int i = 0; i < m; i++)
    {
        auto it = lower_bound(sch.begin(), sch.end(), stu[i]);

        // 如果学生分数小于所有学校分数线
        if (it == sch.begin()) ans += abs(sch[0] - stu[i]);
        //高于所有学校的分数线
        else if (it == sch.end()) ans += abs(stu[i] - sch[n - 1]);
        else
        {
            int lower = *(it - 1); // 前一个学校分数线
            int upper = *it;       // 当前学校分数线
            ans += min(abs(stu[i] - lower), abs(upper - stu[i]));
        }
    }

    // 输出最小的不满意度之和
    cout << ans << endl;

    return 0;
}
```
## 5.砍木头
https://www.luogu.com.cn/problem/P2440
```
#include<bits/stdc++.h>
using namespace std;
long long n,k;
long long a[1000005];
bool f(long long x)
{
    long long ans = 0;
    for(int i=0;i<n;i++)
    {
        ans += a[i]/x;
    }
    return ans >= k;
}
int main()
{
    cin>>n>>k;
    for(int i=0;i<n;i++) scanf("%d",&a[i]);
    long long l = 0,r = 100000001, mid;、
    // while(l + 1 < r) 的条件确保了在循环结束时，l 和 r 相邻（即 r = l + 1）
    while(l+1<r)
    {
        mid = (l+r)/2;
        if(f(mid)) l = mid;
        else r = mid;
    }
    cout<<l<<endl;
    return 0;
}
```
## 6.双指针：最长不重复子串
https://www.lanqiao.cn/problems/3265/learning/
```
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6 + 10;
int a[N],s[N];
int main()
{
    string str;
    cin>>str;
    int len = 0;
    for(int i=0;i<str.size();i++)
        a[i] = str[i] - 'a';
    // 以i作为最长不重复子序列的最右边一个数，j作为最左边的一个数
    for(int i=0,j=0;i<str.size();i++)
    {
        s[a[i]]++;
        while(s[a[i]]>1) // s[a[i]]>1说明存在重复字符
        {
            s[a[j]]--;// 左端右移
            j++;
        }
        // 当前子串的长度为 i-j+1
        len = max(len,i-j+1);
    }
    cout<<len<<endl;
    return 0;
}
```
## 7.双指针加前缀和——合并数列
https://www.lanqiao.cn/problems/17106/learning/
```
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5+10;
int a[N],b[N];
long long sa[N],sb[N];
int n,m;
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        cin >> a[i];
        sa[i]=sa[i-1]+a[i];
    }
    for(int i=1;i<=m;i++)
    {
        cin>>b[i];
        sb[i]=sb[i-1]+b[i];
    }
    int j=1,res=0;
    for(int i=1;i<=max(n,m);)
    {
        if(sa[i]==sb[j])
        {
            i++;
            j++;
        }
        if(sa[i]>sb[j])
        {
            res++;
            j++;
        }
        if(sb[j]>sa[i])
        {
            res++;
            i++;
        }
    }
    cout<<res;
    return 0;
}
```
## 8.最大堆与最小堆
```
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int m;
    cin>>m;
    priority_queue<int,vector<int>,greater<int>>pq;//最小堆
    priority_queue<int,vector<int>,less<int> >q;//最大堆

    while(m--)
    {
        string op;
        int x;
        cin>>op;
        if(op=="push")
        {
            cin>>x;
            pq.push(x);
        }
        else if(op=="remove")
        {
            if(pq.empty()) cout<<"empty"<<endl;
            else pq.pop();
        }
        else if(op=="min")
        {
            if(pq.empty()) cout<<"empty"<<endl;
            else cout<<pq.top()<<endl;
        }
        else
        {
            cin>>x;
            while(x--)
            {
                cout << pq.top() <<" ";
                pq.pop();
            }
            cout<<endl;
        }
    }
    return 0;
}
```
## 9.并查集
```
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int pre[N], h[N]; // h数组用于记录每个连通块的大小

void init(int n)
{
    for (int i = 1; i <= n; i++)
    {
        pre[i] = i; // 初始化每个节点的父节点为自身
        h[i] = 1; // 每个节点初始时是一个独立的连通块，大小为1
    }
}

int find(int x)
{
    if (pre[x] == x)
        return x;
    return pre[x] = find(pre[x]); // 路径压缩
    // attention！一定是等号，而不是==
    // 上一句也可以是return find(pre[x])
}

bool same(int a, int b)
{
    return find(a) == find(b); // 判断两个节点是否在同一连通块
}

void join(int a, int b)
{
    a = find(a);
    b = find(b);
    if (a == b)
        return; // 如果已经在同一个连通块，直接返回
    if (h[a] > h[b]) // 合并时，将较小的连通块合并到较大的连通块
    {
        pre[b] = a;
        h[a] += h[b]; // 更新连通块大小
    }
    else
    {
        pre[a] = b;
        h[b] += h[a];
    }
}

int main()
{
    int n, m;
    cin >> n >> m;
    init(n);
    while (m--)
    {
        string op;
        int a, b;
        cin >> op;
        if (op == "C")
        {
            cin >> a >> b;
            join(a, b);
        }
        else if (op == "Q1")
        {
            cin >> a >> b;
            if (same(a, b))
                cout << "Yes" << endl;
            else
                cout << "No" << endl;
        }
        else
        {
            cin >> a;
            cout << h[find(a)] << endl; // 直接输出连通块的大小
        }
    }
    return 0;
}
```
## 10.单调栈
```
#include<bits/stdc++.h>
using namespace std;
int n,x;
vector<int> v,a,b,c,d;
void processa()
{
//左边第一个比他大的元素
    stack<int>st;
    for(int i=0;i<n;i++)
    {
        // 当前元素>=v[栈顶元素]时一直弹出
        while(!st.empty()&&v[i]>=v[st.top()])
        {
            st.pop();
        }
        // 如果栈不为空，说明v[栈顶元素]＞当前元素，存入a中
        if(!st.empty())
            a[i]=v[st.top()];
        // ⚠️st存的是索引，而不是值
        st.push(i);
    }
}
void processb()
{
//右边第一个比他大的元素
    stack<int>st;
    for(int i=n-1;i>=0;i--)
    {
        while(!st.empty()&&v[i]>=v[st.top()])
        {
            st.pop();
        }
        if(!st.empty())
            b[i]=v[st.top()];
        st.push(i);
    }
}
void processc()
{
//左边第一个比他小的元素
    stack<int> st;
    for (int i = 0; i < n; i++)
    {
        while (!st.empty() && v[i] <= v[st.top()])
        {
            st.pop();
        }
        if (!st.empty())
            c[i] = v[st.top()];
        st.push(i);
    }
}
void processd()
{
//右边第一个比他小的元素
    stack<int> st;
    for (int i = n - 1; i >= 0; i--)
    {
        while (!st.empty() && v[i] <= v[st.top()])
        {
            st.pop();
        }
        if (!st.empty())
            d[i] = v[st.top()];
        st.push(i);
    }
}
int main()
{
    cin>>n;
    v.resize(n);
    a.resize(n,-1);
    b.resize(n,-1);
    c.resize(n,-1);
    d.resize(n,-1);

    for(int i=0;i<n;i++)
        cin>>v[i];
    processa();
    processb();
    processc();
    processd();
    for(auto &c:a) cout<<c<<" ";
    cout<<endl;
    for(auto &c:b) cout<<c<<" ";
    cout<<endl;
    for(auto &c:c) cout<<c<<" ";
    cout<<endl;
    for(auto &c:d) cout<<c<<" ";
    cout<<endl;
}
```
## 11.分解质因数
```
#include<bits/stdc++.h>
using namespace std;
const int N = 3e5+10;
map<int,bool>mp;
void init() // 初始化素数表
{
    for(int i=1;i<=N;i++)
        mp[i]=true;
    mp[1]=false;
    for(int i=2;i*i<=N;i++)
    {
        if(mp[i])//i是素数，i的倍数不是素数
        {
            for(int j=i*i;j<=N;j+=i)
                mp[j]=false;
        }
    }
}
void process(int x)
{
    if(mp[x])
    {
        cout << x << "=" << x << endl;
        return;
    }
    else
        cout<<x<<"=";
    vector<int>a;
    for(int i=2;i*i<=x;i++)
    {

        while(x%i==0&&mp[i])
        {
            a.push_back(i);
            x/=i;
        }
    }
    // 确保在分解过程中，如果x被除到小于i * i时，剩余的x（如果大于1）被正确处理
    if(x>1) a.push_back(x);
    for(int i=0;i<a.size();i++)
    {
        cout << a[i];
        if (i < a.size() - 1) cout << "*";
    }
    cout<<endl;
}
int main()
{
    init();
    int a,b;
    cin>>a>>b;
    for(int i=a;i<=b;i++)
        process(i);
    return 0;
}
```
## 12.素数环：素数➕深度优先搜索
```
#include <bits/stdc++.h>
using namespace std;
int n;
int a[20],visited[20],cnt=0;
bool check(int x,int y)//判断两数之和是否为素数
{
    int t = x+y;
    for(int i=2;i<t;i++)
    {
        if(t%i==0) return false;
    }
    return true;
}
void dfs(int x)
{
    if(x==n+1&&check(a[1],a[n]))
    {
        cnt++;
        for(int i=1;i<=n;i++)
            cout<<a[i]<<" ";
        cout<<endl;
        return;
    }
    for(int i=2;i<=n;i++)
    {
        // 如果当前位置的 i 未使用，且和 a[x-1] 之和为素数
        if(visited[i]==0&&check(i,a[x-1]))
        {
            visited[i]=1;
            a[x]=i;// 令当前位置（x）为 i
            dfs(x+1);
            visited[i]=0;
        }
    }
}
int main()
{
    cin>>n;
    a[1]=1;
    dfs(2);
    if(cnt==0)
        cout<<"No Answer"<<endl;
    return 0;
}
```
## 13.快速幂，b^p % k
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll quick_power(ll base, ll power, ll k)
{
    // result = b^p % k
    // (a*b)%k = [(a%k)*(b%k)]%k
    ll result = 1;
    while(power)
    {
        if(power%2==1)// power是奇数时
        {
            power--;
            // result = result * b % k
            result = result * base % k;
        }
        // 此时 power 一定是偶数
        power /= 2;
        // base = base^2 % k
        // b^p, (b^2)^(power/2),效果一样，而且更快
        base = base*base%k;
    }
    return result;
}
int main()
{
    ll b,p,k;
    cin>>b>>p>>k;
    cout<<quick_power(b,p,k)<<endl;
    return 0;
}
```
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
## 15.排列型枚举dfs
```
#include<bits/stdc++.h>
using namespace std;
const int N = 8;
int n;
vector<int>temp;
vector<bool>used(N,false);
vector<vector<int>> ans;
void dfs(int x)
{
    if(x==n)
    {
        ans.push_back(temp);
        return;
    }
    for(int i=1;i<=n;i++)
    {
        if(!used[i])
        {
            temp.push_back(i);
            used[i]=true;
            dfs(x+1);
            temp.pop_back();
            used[i]=false;
        }
    }
}
int main()
{
    cin>>n;
    dfs(0);
    sort(ans.begin(),ans.end());
    for(int i=0;i<ans.size();i++)
    {
        for(int j=0;j<ans[i].size();j++)
            cout<<ans[i][j]<<" ";
        cout<<endl;
    }
    return 0;
}
```
## 16.指数型枚举
```
#include<bits/stdc++.h>
using namespace std;
int n;
vector<vector<int>> ans;
vector<int>temp;
void dfs(int x)
{
    if(x>n)
    {
        ans.push_back(temp);
        return;
    }
    // 不选 x
    dfs(x+1);
    // 选 x
    temp.push_back(x);
    dfs(x+1);
    // 回溯
    temp.pop_back();
}
int main()
{
    cout<<" ";
    cin>>n;
    // 从第 1 个元素开始枚举
    dfs(1);
    for(auto &subset:ans)
    {
        for (int num: subset)
            cout << num << " ";
        cout<<endl;
    }
    return 0;
}
```
## 17.组合型枚举
```
#include<bits/stdc++.h>
using namespace std;
int n,m;
const int N = 50;
vector<int>temp;
vector<int>used(N,false);
vector<vector<int>>ans;
void dfs(int start,int size)
{
    if(size==m)
    {
        ans.push_back(temp);
        return;
    }
    for(int i=start;i<=n;i++)
    {
        if(!used[i])
        {
            temp.push_back(i);
            used[i]=true;
            dfs(i,size+1);
            temp.pop_back();
            used[i]=false;
        }
    }
}
int main()
{
    cin>>n>>m;
    dfs(1,0);
    for(auto& sub:ans)
    {
        for(auto& num:sub)
            cout<<num<<" ";
        cout<<endl;
    }
    return 0;
}
```
## 18.贪心加动态规划
https://blog.csdn.net/Kyrieeeeeeeee/article/details/140701302
```
#include <bits/stdc++.h>
using namespace std;

typedef struct node
{
    int t, d, p; // 时间、截止时间、报酬
} work;
bool cmp(const work a,const work b)
{
    if(a.d!=b.d) return a.d<b.d;
    return a.t<b.t;
}
int main()
{
    int T, n;
    cin >> T;
    while (T--)
    {
        cin >> n;
        vector<work> a(n);
        int deadline = 0;
        for (int i = 0; i < n; i++)
        {
            cin >> a[i].t >> a[i].d >> a[i].p;
            deadline = max(deadline, a[i].d); // 更新最晚截止时间
        }

        // 按截止时间从小到大排序，如果截止时间相同，则按时间从小到大排序
        sort(a.begin(), a.end(),cmp);

        // dp[i] 表示在时间 i 时的最大报酬
        vector<int> dp(deadline + 1, 0);

        for (int i = 0; i < n; i++)
        {
            int t = a[i].t, d = a[i].d, p = a[i].p;
            // 从后往前更新 dp 数组，防止重复计算
            for (int j = deadline; j >= 0; j--)
            {
                if (j + t <= d)
                { // 如果当前工作可以在截止时间前完成
                    dp[j + t] = max(dp[j + t], dp[j] + p); // 更新最大报酬
                }
            }
        }

        // 输出最大报酬
        cout << *max_element(dp.begin(), dp.end()) << endl;
    }
    return 0;
}
```
## 19.最少操作数，bfs
```
// n+1,n-1,n*2,通过最少的操作次数变成k
#include<bits/stdc++.h>
using namespace std;
int n,k,op=0;
const int N = 1e5+10;
int vis[N];
map<int,int>mp;
queue<int>q;
void bfs(int s,int t)
{
    while(!q.empty())
    {
        int now = q.front();
        q.pop();
        if(now==t)
        {
            cout<<mp[now]<<endl;
            return;
        }
        for(int i=1;i<=3;i++)
        {
            int next;
            if(i==1) next = now + 1;
            if(i==2) next = now - 1;
            if(i==3) next = now * 2;
            if(next>k)
                mp[next] = mp[now] + next - k;
            // 当 next 超过 k 的时候，减去 (next-k) 次 1
            if(!vis[next]&&next>=1&&next<=1e5)
            {
                vis[next]=1;
                mp[next]=mp[now]+1;
                q.push(next);
            }
        }
    }
    return;
}
int main()
{
    cin>>n>>k;
    q.push(n);
    mp[n]=0;// 变到 n 的操作次数
    bfs(n,k);
    return 0;
}
```
## 20.01背包问题，内层逆序！
```
#include<bits/stdc++.h>
using namespace std;
int n,m;
typedef struct node
{
    int value,weight;
}bag;
int main()
{
    cin>>n>>m;
    bag a[n+1];
    for(int i=1;i<=n;i++)
        cin>>a[i].weight>>a[i].value;
    int dp[m+1];
    memset(dp, 0, sizeof(dp));
    for(int i=1;i<=n;i++)// i 是物品个数
    {
        // 容量一定是逆序遍历，才能确保每个物品只被选一次
        for(int j=m;j>=a[i].weight;j--)// j 是容量
        {
            dp[j]=max(dp[j-a[i].weight]+a[i].value,dp[j]);
        }
    }
    cout<<dp[m]<<endl;
    return 0;
}
```
## 21.完全背包问题，内层正序
```
// 和 01背包的区别就在于内层循环的正序的
#include<bits/stdc++.h>
using namespace std;
int n,m;
typedef struct node
{
    int value,weight;
}bag;
int main()
{
    cin>>n>>m;
    bag a[n+1];
    for(int i=1;i<=n;i++)
        cin>>a[i].weight>>a[i].value;
    int dp[m+1];
    memset(dp,0,sizeof dp);
    for(int i=1;i<=n;i++)
    {
        // 正序遍历，可以使得每个物品被重复选择
        for(int j=a[i].weight;j<=m;j++)
            dp[j] = max(dp[j],dp[j-a[i].weight]+a[i].value);
    }
    cout<<dp[m]<<endl;
    return 0;
}
```
## 22.多重背包1，内层逆序
```
https://www.lanqiao.cn/problems/19920/learning/
#include <bits/stdc++.h>
using namespace std;
int dp[1005],n,m,value,weight,s;
int main()
{
    cin>>n>>m;
    while(n--)
    {
        cin>>weight>>value>>s;
        while(s--)
        {
            // 这里又是逆序遍历了！
            for (int j = m; j >= weight; j--)
                dp[j] = max(dp[j], dp[j - weight] + value);
        }
    }
    cout<<dp[m];
}
```
## 23.贡献法
https://www.luogu.com.cn/problem/P8715
```
#include<bits/stdc++.h>
using namespace std;

int main()
{
    string s;
    cin >> s;
    int n = s.size();
    vector<vector<int>> pos(26); // 存储每个字符的所有出现位置
    for (int i = 0; i < n; ++i)
        pos[s[i] - 'a'].push_back(i);

    long long ans = 0;
    for(int i=0;i<n;i++)
    {
        char c = s[i];
        int c_idx = c-'a';
        auto &v = pos[c_idx];
        auto it = lower_bound(v.begin(),v.end(),i);// 当前字符的位置
        int left,right;

        if(it==v.begin()) left=-1; // 左边没有相同字符
        else left = *prev(it); // 左边最近的相同字符的位置

        if(next(it)==v.end()) right=n; // 右边没有相同字符，已经在末尾了
        else right=*next(it);// 右边最近的相同字符的位置
        ans+=(i-left)*(right-i); // 计算贡献
    }
    cout<<ans<<endl;
    return 0;
}
```