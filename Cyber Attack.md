## Problem Name : Cyber Attack

### Topic : Greedy, Graphs

#### Difficulty : Medium

### EXPLANATION

This question can be solved greedily by finding Maximum Spanning Tree, MST(which can be found using minimum spanning tree algorithms). For the graph to remain connected, leave the edges of the MST. Now sort the remaining edges in the increasing order of their cost, remove the edges until the sum of costs of removed edges doesn't exceed the allowed total cost, *s*.

#### Time Complexity : O($$ElogE$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 50100
#define mp make_pair
#define ll long long int
#define inf 1000000000000000000ll
#define logn 20
#define mod 1000000007ll
 
using namespace std;

typedef pair<int,int> pii;
typedef pair<ll,int> pli;
typedef pair<int,ll> pil;
typedef pair<ll,ll> pll;

vector<pil>adj[nn];
vector<pair<pli,pii> >eg;
int p[nn];

int parent(int x)
{
    while(p[x]!=x)
    {
        p[x]=p[p[x]];
        x=p[x];
    }
    return x;
}

void union1(int x,int y)
{
    y=parent(y);
    x=parent(x);
    p[y]=x;
    return;
}

int main()
{
    ios_base::sync_with_stdio(0);
    int n,m;
    ll s;
    cin>>n>>m>>s;
    for(int i=0;i<m;i++)
    {
        int u,v;
        ll w;
        cin>>u>>v>>w;
        adj[u].pb(mp(v,w));
        adj[v].pb(mp(u,w));
        eg.pb(mp(mp(w,i),mp(u,v)));
    }
  	/*Using KRUSKAL's algorithm to find MST*/ 
    sort(eg.begin(),eg.end());
    reverse(eg.begin(),eg.end());
    for(int i=1;i<=n;i++)
        p[i]=i;
    vector<pli>e;
    for(int i=0;i<m;i++)
    {
        if(parent(eg[i].se.ff)==parent(eg[i].se.se))
        {
            e.pb(eg[i].ff); // edges which are not part of MST
            continue;
        }
        union1(eg[i].se.ff,eg[i].se.se);
    }
    sort(e.begin(),e.end()); // sorting the remaining edges.
    vector<ll>ans;
    vector<pli>::iterator u;
    ll cw=0;
    for(u=e.begin();u!=e.end();u++)
    {
        if(s-u->ff<0) // sum of cost of removed edges exceeds the total allowed cost.
            break;
        ans.pb(u->se);
        s-=u->ff;
        cw+=u->ff;
    }
    cout<<ans.size()<<' '<<cw<<endl;
    return 0;
}
```