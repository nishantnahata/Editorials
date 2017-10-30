## Problem Name : Tree Queries-I

### Topic : Trees, LCA

#### Difficulty : Difficult

### EXPLANATION

For each node, store the maximum purity among all the nodes in its subtree. Now for a path between two nodes, u and v, the maximum purity will be the value at LCA(u,v), since the subtree of LCA contains subtrees of all the nodes in the path of u and v. You can find the maximum by storing the log value of the product. You can use sparse table to calculate the LCA.

#### Time Complexity : O($$NlogN$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 100010
#define mt make_tuple
#define mp make_pair
#define ll long long int
#define db double
#define ldb long double
#define inf 1000000000000000000ll
#define logn 20
#define mod 1000000007ll
 
using namespace std;

int st[nn][logn],l[nn],sign[nn];
ll a[nn],p[nn];
ldb lg[nn];
vector<int>adj[nn];

void dfs(int node,int par) //for calculating loveliness.
{
	for(int i=1;i<logn;i++)
		st[node][i]=st[st[node][i-1]][i-1]; //filling sparse table
	p[node]=a[node];
	if(a[node])
		lg[node]+=log2(a[node]);
	vector<int>::iterator it;
	for(it=adj[node].begin();it!=adj[node].end();it++)
	{
		if(*it==par)
			continue;
		l[*it]=l[node]+1; //level of node from the root
		st[*it][0]=node; // initializing sparse table
		dfs(*it,node);
		p[node]=(p[node]*p[*it])%mod; //loveliness of node modulo 1000000007.
		sign[node]*=sign[*it]; // sign of the product
		lg[node]+=lg[*it]; //logarithm value of product
	}
}

void dfs2(int node,int par) //for calculating purity of nodes.
{
	if(sign[node]<=0) //when sign is negative take purity as 0
	{
		p[node]=0;
		lg[node]=-1;
	}
	vector<int>::iterator it;
	ll tmp=p[node];
	ldb ml=lg[node];
	for(it=adj[node].begin();it!=adj[node].end();it++)
	{
		if(*it==par)
			continue;
		dfs2(*it,node);
		if(ml<lg[*it])
		{
			tmp=p[*it];
			ml=lg[*it];
		}
	}
	lg[node]=ml; //maximum possible value in its subtree.
	p[node]=tmp;
	return;
}

int lca(int u,int v) //calculating lca
{
	if(l[u]>l[v])
		swap(u,v);
	int a=v;
	for(int i=logn-1;i>=0;i--)
	{
		if(l[st[a][i]]>=l[u])
			a=st[a][i];
	}
	if(a==u)
	{
		return u;
	}
	for(int i=logn-1;i>=0;i--)
	{
		if(st[u][i]!=st[a][i])
		{
			u=st[u][i];
			a=st[a][i];
		}
	}
	return st[u][0];
}

int main()
{
    ios_base::sync_with_stdio(0);
	int n,q,u,v;
	cin>>n>>q;
	for(int node=1;node<=n;node++)
	{
		cin>>a[node];
		if(a[node]>0) //setting sign[node]
		{
			sign[node]=1;
		}
		else if(a[node]<0)
		{
			sign[node]=-1;
			a[node]*=-1;
		}
		else
			sign[node]=0;
	}
	for(int i=0;i<n-1;i++)
	{
		cin>>u>>v;
		adj[u].pb(v);
		adj[v].pb(u);
	}
	l[1]=1;
	dfs(1,1);
	dfs2(1,1);
  	//dfs and dfs2 are for precomputation purpose.
	int cnt=0;
	while(q--)
	{
		cin>>u>>v;
		if(u==v)
		{
			cout<<p[u]<<endl;
			continue;
		}
		int lc=lca(u,v);
		cout<<p[lc]<<endl; //final answer
	}
	return 0;
}
```