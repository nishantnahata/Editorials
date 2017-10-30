## Problem Name : Prime Bitcount

###Topic : Prefix sums, primes

#### Difficulty : Easy

###EXPLANATION

This problem can be solved by maintaining an array of prefix sums. Let $$dp$$ be an array, such that, <br>

$$dp_i = dp_{i-1} + F_i$$,<br>

where $$F_i$$ = 1, if number of set bits in $$i$$ is a prime number, and $$F_i$$ = 0, otherwise.

Now to process a query, [L, R],<br> 

$$ans = dp_R - dp_{L-1}$$

####Time Complexity : O(N)

~~~c++
#include <bits/stdc++.h>
#define nn 1000100
 
using namespace std;

int dp[nn],f[nn];
int main()
{
    ios_base::sync_with_stdio(0);
    int t;
    f[1]=f[0]=0;
    for(int i=2;i<64;i++)
    {
      	f[i]=1;
        for(int j=2;j<i;j++)
        {
            if((i%j)==0)
              f[i]=0;
        }
    }
    for(int i=1;i<nn;i++)
    {
        dp[i]=dp[i-1]+f[__builtin_popcount(i)]; 
      	//https://www.quora.com/What-is-__builtin_popcount-in-c++
    }
    cin>>t;
    while(t--)
    {
        int n,m;
        cin>>n>>m;
        int ans=dp[m];
        if(n)
            ans-=dp[n-1];
        cout<<ans<<endl;
    }
    return 0;
}
~~~