## Problem Name : XOR Sum-I

### Topic : Bitwise Operations

#### Difficulty : Medium

### EXPLANATION

Let $$k$$ be the xor of all the elements of the array. Now for each position, $$i$$, we will find $$p$$, which is equal to the xor of all the elements except the element $$i$$, i.e.<br>

$$p=k​$$ $$xor​$$ $$A_i​$$.

Now for each 0 bit in p, we will set this bit in the new number, $$c$$, if it is less than or equal to $$2*A_i$$. Now we can take answer as the maximum of $$p$$ $$xor$$ $$c$$ over all $$i's$$.

#### Time Complexity : O($$NlogN$$).

```c++
#include <iostream>
#define nn 100100
#define ll long long int
 
using namespace std;

ll a[nn];

int main()
{
    ios_base::sync_with_stdio(0);
    int n;
    cin>>n;
    ll sum=0;
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
        sum^=a[i];
    }
    ll ans=sum;
    for(int i=0;i<n;i++)
    {
        ll j=40, t=sum^a[i];
        ll op=0;
        while(op<2*a[i] && j>0)
        {
            j--;
            if((1ll<<j)&t)
                continue;
            ll tp=op|(1ll<<j);
            if(tp<=2*a[i])
                op=tp;
        }
        if((t^op)>=ans)
        {
            ans=(t^op);
        }
    }
    cout<<ans<<endl;
    return 0;
}
```