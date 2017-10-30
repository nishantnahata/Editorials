## Problem Name : Mike and X

### Topic : Observation

#### Difficulty : Difficult

### EXPLANATION

Let an array, $$p$$, such that, <br>

$$p_i=1$$, if $$i$$ can be derived from numbers in array, $$A$$ and $$p_i=0$$, otherwise.<br>

The solution below, is optimized, and its complexity turns out to be $$N\sqrt N$$ due to optimizations.

#### Time Complexity : O($$N\sqrt N$$).

```c++
#include<bits/stdc++.h>
#define nn 200200
using namespace std;
 
int p[nn],a[nn];
 
int main()
{
    ios_base::sync_with_stdio(0);
    int n,k,q,x,t;
    cin>>n>>k>>q;
    p[0]=1;
    for(int i=0;i<k;i++)
    {
        cin>>a[i];
    }
    sort(a,a+k);
    for(int i=0;i<k;i++)
    {
        t=a[i];
        if(p[t])
            continue;
        for(int j=t;j<nn;j++)
            if(p[j-t])
                p[j]=1;
    }
    while(q--)
    {
        cin>>x;
        assert(x<=200000);
        if(p[x])
            cout<<"Yes\n";
        else
        {
            cout<<"No\n";
        }
    }
    return 0;
}
```