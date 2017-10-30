## Problem Name : Array Queries

### Topic : Segment Trees

#### Difficulty : Medium

### EXPLANATION

We can initially compute $$f(i,k)$$ for all $$1<=i<=N$$. We can build a sum segment tree initially. Now whenever an update comes, we know that each element is added to some $$f(i,k)$$ for $$i$$ in a range. So we can lazily update at all the positions, $$i$$(it will be a range or two ranges). See the code for more clarity.

#### Time Complexity : O($$NlogN$$).

```c++
#include <bits/stdc++.h>
#define pb push_back
#define nn 100100
#define ll long long int
#define mod 1000000007ll
 
using namespace std;

ll t[nn*4], lz[nn*4];
int a[nn];

void update(int node,int st,int en,int l,int r,ll val) //add val in ll the elements in range [l,r]
{
    if(lz[node])
    {
        t[node]+=(en-st+1)*lz[node];
        if(st!=en)
        {
            lz[node*2+1]+=lz[node];
            lz[node*2+2]+=lz[node];
        }
        lz[node]=0;
    }
    if(st>r || en<l)
        return;
    if(st>=l && en<=r)
    {
        t[node]+=(en-st+1)*val;
        if(st!=en)
        {
            lz[node*2+1]+=val;
            lz[node*2+2]+=val;
        }
        return;
    }
    int mid=st+en>>1;
    update(node*2+1,st,mid,l,r,val);
    update(node*2+2,mid+1,en,l,r,val);
    t[node]=t[node*2+1]+t[node*2+2];
}

ll query(int node,int st,int en,int l,int r) //find the sum of f[i,k] for i in range [l,r]
{
    if(lz[node])
    {
        t[node]+=(en-st+1)*lz[node];
        if(st!=en)
        {
            lz[node*2+1]+=lz[node];
            lz[node*2+2]+=lz[node];
        }
        lz[node]=0;
    }
    if(st>r || en<l)
        return 0;
    if(st>=l && en<=r)
        return t[node];
    int mid=st+en>>1;
    ll p1=query(node*2+1,st,mid,l,r);
    ll p2=query(node*2+2,mid+1,en,l,r);
    return p1+p2;
}

int main()
{
    ios_base::sync_with_stdio(0);
    int n,q,k;
    cin>>n>>k>>q;
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
        int l=i,r=i+k-1;
        if(r>=n)
        {
            update(0,0,n-1,l,n-1,a[i]);
            update(0,0,n-1,0,r-n,a[i]);
        }
        else
        {
            update(0,0,n-1,l,r,a[i]);
        }
    }
    while(q--)
    {
        int ch,x,y;
        cin>>ch>>x>>y;
        if(ch==1)
        {
            int l=x-1,r=x-2+k;
            if(r>=n) // cyclic update on ranges [l,n-1] and [0,r-n].
            {
                update(0,0,n-1,l,n-1,y-a[l]);
                update(0,0,n-1,0,r-n,y-a[l]);
            }
            else //simple update on range [l,r]
            {
                update(0,0,n-1,l,r,y-a[l]);
            }
            a[l]=y;
            continue;
        }
        ll ans=query(0,0,n-1,x-1,y-1); //simple query
        cout<<ans<<endl;
    }
    return 0;
}
```