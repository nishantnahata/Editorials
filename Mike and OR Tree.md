## Problem Name : Mike and OR Tree

### Topic : Trees, sqrt decomposition

#### Difficulty : Difficult

### EXPLANATION

The problem can be solved in different ways. The most easy idea is sqrt-optimization. Split all queries into *sqrt*(*n*) blocks. Each block we will process separately. Before processing each block, we should calculate minimum distances from every node to the closest node with value 1 using bfs. To answer the query we should update this value by shortest distances to nodes with value 1 in current block.

The solution becomes simple. Every *sqrt*(*n*) queries we make simple bfs and for every node *v* WE calculate value *d*[*v*] — the shortest distance to some node with value 1 from node *v*. Then to answer the query of type 2 you should calculate *min*(*d*[*v*], *dist*(*v*, *u*)), where *u* — every node with value 1, which is updated in current block of length *sqrt*(*n*). *dist(v,u)* can be calculated by using sparse table for LCA, <br>

$$dist(v,u) = level_v + level_u - 2*level_{lca(v,u)}$$<br>

Links: <a href="https://www.topcoder.com/community/data-science/data-science-tutorials/range-minimum-query-and-lowest-common-ancestor/">Sparse Table and LCA</a>, <a href="https://www.quora.com/How-does-the-technique-of-sqrt-N-decomposition-work-and-in-what-kind-of-problems-is-it-useful">SQRT Decomposition</a>.<br>

This question can also be done by <a href="https://threads-iiith.quora.com/Centroid-Decomposition-of-a-Tree">Centroid Decomposition</a>.

#### Time Complexity : O(N * sqrt(N) * log(N))

```c++
#include<bits/stdc++.h>
#define logn 20
#define nn 100100
#define pb push_back
#define sqrtm 350
#define mp make_pair
using namespace std;

vector<int>adj[nn];
int lca[nn][logn],dist[nn],l[nn],up[sqrtm];
bool v[nn];

void dfs(int node)
{
    for(int i=1;i<logn;i++)
        lca[node][i]=lca[lca[node][i-1]][i-1];
    v[node]=true;
    for(int i=0;i<adj[node].size();i++)
    {
        if(!v[adj[node][i]])
        {
            lca[adj[node][i]][0]=node;
            l[adj[node][i]]=l[node]+1;
            dist[adj[node][i]]=l[node]+1;
            dfs(adj[node][i]);
        }
    }
}

int dis(int u,int v)
{
    if(l[u]<l[v])
        swap(u,v);
    int a=u,b=v,lc;
    int tmp=pow(2,logn-1);
    for(int i=logn-1;i>=0;i--)
        if(l[lca[a][i]]>=l[v])
            a=lca[a][i];
    if(a==v)
        return l[u]+l[v]-2*l[a];
    for(int i=logn-1;i>=0;i--)
    {
        if(lca[a][i]!=lca[b][i])
        {
            a=lca[a][i];
            b=lca[b][i];
        }
    }
    lc=lca[a][0];
    return l[u]+l[v]-2*l[lc];
}

void bfs(int u)
{
    queue<int>q;
    dist[u]=0;
    q.push(u);
    int f;
    while(!q.empty())
    {
        f=q.front();
        q.pop();
        for(int i=0;i<adj[f].size();i++)
        {
            if(dist[adj[f][i]]>dist[f]+1)
            {
                q.push(adj[f][i]);
                dist[adj[f][i]]=dist[f]+1;
            }
        }
    }

}

int main()
{
    ios_base::sync_with_stdio(0);
    int n,m,t1,t2;
    cin>>n>>m;
    for(int i=0;i<n-1;i++)
    {
        cin>>t1>>t2;
        adj[t1].pb(t2);
        adj[t2].pb(t1);
    }
    dfs(1);
    int i=0,j,q,u,us=0,ans;
    while(i<m)
    {
        for(j=i;j<sqrtm+i && j<m;j++)
        {
            cin>>q>>u;
            if(q==1)
            {
                up[us++]=u;
            }
            else
            {
                ans=dist[u];
                for(int k=0;k<us;k++)
                {
                    ans=min(ans,dis(u,up[k]));
                }
                cout<<ans<<endl;
            }
        }
        while(us)
        {
            bfs(up[us-1]);
            us--;
        }
        i=j;
    }
    return 0;
}
```