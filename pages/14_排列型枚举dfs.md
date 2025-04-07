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