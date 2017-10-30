## Problem Name : Mike and Lift Game

### Topic : Dynamic Programming

#### Difficulty : Difficult

### EXPLANATION

There is a clear O($$N^3$$) dp solution. In this solution, we know that from floor, $$i$$, Mike can move to any floor, $$k$$, such that, $$|b-i|>|i-k|$$. So if Mike is at floor, $$i$$ after $$j$$ steps, then Mike can reach floor, $$k$$ after $$j+1$$ steps. Let $$DP_{n*n}$$, be a matrix, such that $$DP{i,j}$$ stores the number of ways to reach floor, $$i$$, after exactly $$j$$ steps. So,<br>

$$DP_{k,j+1}=\sum_{i}^{|i-b|>|k-i|}DP_{i,j}$$<br>

But since, N is 5000, O($$N^3​$$) won't work. Now we can optimize it to O($$N^2​$$). We can simply find that, the range of $$k​$$ in above relation, for each value of $$i​$$, is union of two continuous segments of floors.<br>

When $$i<b$$, $$k \in [2*i-b+1,i-1]U[i+1,b-1]$$ (simplifying from $$k \in [i-|b-i|+1,i-1]U[i+1,b-1]$$).<br>

When $$i>b$$, $$k \in [b+1,i-1]U[i+1,2*i-b-1]$$ (simplifying from $$k \in [b+1,i-1]U[i+1,i+|i-b|-1]$$).<br>

We can simply maintain an update array, by adding an update for range $$[L,R]$$ at position, $$L$$, and removing this update at position, $$R+1$$.<br>

Now the total number of sequences of steps will be equal to :-<br>

$$ans=\sum_{i=1}^{N}DP_{i,K}$$<br>

#### Time Complexity : O($$N^2$$).

```c++
#include <bits/stdc++.h>
#define nn 5100
#define ll long long int
#define mod 1000000007ll
#define set0(a) memset(a,0,sizeof a)
 
using namespace std;

int dp0[nn],dp1[nn]; //dp0 stores the number of ways to reach floors and dp1 is the update array

int main()
{
    ios_base::sync_with_stdio(0);
    int n,a,b,k;
    cin>>n>>a>>b>>k;
    dp0[a]=1; // intializing dp[a][0]=1
    for(int i=1;i<=k;i++) // denoting the number of the step.
    {
        set0(dp1); // initializing update array.
        for(int j=1;j<=n;j++) //traversing over all the floors.
        {
            if(j==b)
                continue;
            if(j<b)
            {
                int dis=b-j-1;
                dp1[max(j-dis,1)]=(dp1[max(j-dis,1)]+dp0[j])%mod; //adding update.
                dp1[b]=(dp1[b]-dp0[j]+mod)%mod; //removing update.
                continue;
            }
            int dis=j-b-1;
            dp1[b+1]=(dp1[b+1]+dp0[j])%mod; //adding update.
            dp1[min(n+1,j+dis+1)]=(dp1[min(n+1,j+dis+1)]-dp0[j]+mod)%mod; //removing update.
        }
        for(int i=1;i<=n;i++) //updating the final array after traversing through all the floors
        {
            dp1[i]=(dp1[i]+dp1[i-1])%mod;
            dp0[i]=(dp1[i]-dp0[i]+mod)%mod;
        }
    }
    ll ans=0;//final answer
    for(int i=1;i<=n;i++)
        ans=(ans+dp0[i])%mod;
    cout<<ans<<endl;
    return 0;
}
```