## Problem Name : Winner

### Topic : Dynamic Programming

#### Difficulty : Medium

### EXPLANATION

Let $$dp_{i,j}$$ denotes the number of distinct sequences that ends at position, $$j$$, after $$i$$ operations. Then recurrence will be :-<br>

$$dp_{i,j}=\sum_{k}dp_{i-1,k}​$$, where $$k!=j​$$. 

#### Time Complexity : O($$K$$).

```c++
#include <bits/stdc++.h>
#define nn 1000100
#define mod 1000000007ll
 
using namespace std;

unsigned int dp[4][10000001];

int main()
{
    ios_base::sync_with_stdio(0);
    int n;
	cin>>n;
	dp[0][1]=0;
	dp[1][1]=1;
	dp[2][1]=1;
	dp[3][1]=1;
	for (int i=2;i<=n;i++)
	{
		dp[0][i]=(dp[1][i-1]+dp[2][i-1]+dp[3][i-1])%mod;
		dp[1][i]=(dp[0][i-1]+dp[2][i-1]+dp[3][i-1])%mod;
		dp[2][i]=(dp[0][i-1]+dp[1][i-1]+dp[3][i-1])%mod;
		dp[3][i]=(dp[0][i-1]+dp[1][i-1]+dp[2][i-1])%mod;
	}
	cout<<dp[0][n]<<endl;
    return 0;
}
```