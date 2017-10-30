## Problem Name : Good Factors

### Topic : DP, Bitmasking

#### Difficulty : Medium-hard

### EXPLANATION

Since the good factors can only be 1 or prime factors less than 40, there can be atmost 13 good factors. This means there can be atmost $$2^{13}$$ combinations of good factors possible. Now for all these combinations, we can precompute <a href="http://www.geeksforgeeks.org/dynamic-programming-set-7-coin-change/">coin change</a>. Let $$mask$$, denote one of the combinations of good factors, then-<br>

$$dp_{mask,i}=$$ minimum number of operations needed to make $$i$$ from the factors in $$mask$$.<br>

#### Time Complexity : O($$2^{13}*13*1000$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 1001
#define mp make_pair
#define ll long long int
#define mod 1000000007ll
 
using namespace std;

int pr[]={1,2,3,5,7,11,13,17,19,23,29,31,37};

int dp[1<<13][nn];
ll a[nn*100];
int b[nn*100],cnt;

void fill(int mask) // filling dp array for a particular mask
{
    vector<int> v;
    for(int i=0;i<13;i++)
    {
        if(mask&(1<<i))
            v.pb(pr[i]);
    }
    if(v.size()==13)
        cnt++;
    for(int i=1;i<nn;i++)
    {
        dp[mask][i]=2000;
        for(int j=0;j<v.size();j++)
        {
            if(v[j]>i)
                break;
            dp[mask][i] = min(dp[mask][i], 
                                dp[mask][i-v[j]] + 1);
        }
    }
}

int main()
{
    ios_base::sync_with_stdio(0);
    int n;
    for(int mask=0;mask<(1<<13);mask++) //all possible combinations
    {
        fill(mask);
    }
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>a[i];
    int ans=0, tmp=0;
    for(int i=0;i<n;i++)
    {
        int mask=0;
        for(int j=0;j<13;j++)
        {
            if(a[i]%pr[j]==0)
                mask|=(1<<j);
        }
        cin>>b[i];
        tmp+=b[i];
        ans+=dp[mask][b[i]]; //adding number of operations.
    }
    cout<<ans<<endl;
    return 0;
}
```