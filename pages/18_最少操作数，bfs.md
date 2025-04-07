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