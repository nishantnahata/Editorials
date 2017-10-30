## Problem Name : Arranging Boxes-II

### Topic : LIS

#### Difficulty : Medium

### EXPLANATION

Find the **longest increasing subsequence** of the box numbers. The minimum number of moves required for sorting the boxes will be equal to $$N-lis$$, where $$lis$$ is the length of longest increasing subsequence.<br>

LIS can be found in *nlogn*. Here, I used segment tree to find LIS. You can learn another way of finding LIS in nlogn <a href="http://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/">here</a>.

#### Time Complexity : O($$NlogN$$).

```c++
#include <iostream>
#include <stdio.h>
#include <algorithm>
#include <map>
#define nn 200100
using namespace std;

int a[nn],lis[nn],t[nn<<2];

void build(int node,int st,int en)
{
	t[node]=0;
	if(st==en)
		return;
	int mid=st+en>>1;
	build(node*2+1,st,mid);
	build(node*2+2,mid+1,en);
}

void update(int node,int st,int en,int pos,int val)
{
	if(st==en)
	{
		t[node]=max(t[node],val);
		return;
	}
	int mid=st+en>>1;
	if(pos<=mid)
		update(node*2+1,st,mid,pos,val);
	else
		update(node*2+2,mid+1,en,pos,val);
	t[node]=max(t[node*2+1],t[node*2+2]);
}

int query(int node,int st,int en,int l,int r)
{
	if(st>r || en<l)
		return 0;
	if(st>=l && en<=r)
		return t[node];
	int mid=st+en>>1;
	return max(query(node*2+1,st,mid,l,r),query(node*2+2,mid+1,en,l,r));
}
 
int main()
{
	int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
    	lis[i]=1;
        cin>>a[i];
    }
    build(0,1,n);
    update(0,1,n,a[1],1);
    for(int i=2;i<=n;i++)
    {
    	lis[i]=query(0,1,n,1,a[i])+1;
    	update(0,1,n,a[i],lis[i]);
    }
    int ans=1;
    for(int i=1;i<=n;i++)
        ans=max(ans,lis[i]);
    ans=(n-ans);
    cout<<ans<<endl;
    return 0;
}
```