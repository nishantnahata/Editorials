## Problem Name : Photograph

### Topic : Number Theory

#### Difficulty : Easy

### EXPLANATION

Minimum time needed to capture the photograph would be the LCM of all $$A_i's$$. LCM can be found using sieve.

#### Time Complexity : O($$NlogN$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 1000100
#define mp make_pair
#define ll long long int
#define mod 1000000007ll
 
using namespace std;

int p[nn], dp[nn];

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    #ifndef ONLINE_JUDGE
    freopen("input.txt","r",stdin);
    freopen("out.txt","w",stdout);
    #endif
    int n,a;
    cin>>n;
    for(int i=2;i<nn;i++)
    {
        if(p[i])
            continue;
        p[i]=i;
        for(ll j=i*1ll*i;j<nn;j+=i)
        {
            p[j]=i; //storing a prime factor of j.
        }
    }
    for(int i=0;i<n;i++)
    {
        cin>>a;
        map<int,int>m; //storing factors and their occurences
        while(a!=1)
        {
            m[p[a]]++;
            a/=p[a];
        }
        map<int,int>::iterator u;
        for(u=m.begin();u!=m.end();u++)
        {
            dp[u->ff]=max(dp[u->ff], u->se);
        }
    }
    ll ans=1; //stores lcm
    for(ll i=2;i<nn;i++)
    {
        if(dp[i]==0)
            continue;
        ans = (ans*pow(i,dp[i],mod))%mod;
    }
    cout<<ans<<endl;
    return 0;
}
```