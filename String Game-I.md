## Problem Name : String Game-I

### Topic : Game Theory

#### Difficulty : Difficult

### EXPLANATION

This game can be easily reduced to the **game of nim**. Each segment of consecutive same letters(for eg. "aaa"), can be considered as a pile of game of nim. For a single string, we can just take xor of the piles we defined earlier.<br>

Now, let the two arrays, $$pre$$ and $$suff$$, be the arrays, such that,<br>

$$pre_i = $$ xor value for the substring $$S[1...i]$$.<br>

$$suff_i=$$xor value for the substring $$S[n...i]$$.<br>

Now if the second kid breaks the string after $$i^{th}$$ position in a string, then the xor value of the game will be:-

$$pre_i$$^$$suff_{i+1}$$. Traverse through all $$i's$$, and if there exists any $$i$$, such that $$pre_i$$^$$suff_{i+1}$$ = 0, the answer is Yes, otherwise No.<br>

You can learn about game theory from this <a href="https://www.topcoder.com/community/data-science/data-science-tutorials/algorithm-games/">link</a>.

#### Time Complexity : O($$N$$).

```c++
#include <bits/stdc++.h>
#define nn 100100
#define yes "YES"
#define no "NO"
 
using namespace std;

int dp0[nn],dp1[nn];
//dp0 is pre array, dp1 is suff array

int main()
{
    ios_base::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--)
    {
        string s;
        cin>>s;
        int st=1,sr=1,n=s.length();
        dp0[0]=1,dp1[n-1]=1;
        for(int i=1;i<n;i++)
        {
            if(s[i]==s[i-1])
            {
                dp0[i]=dp0[i-1]^st^(st+1);
                st++;
            }
            else
            {
                dp0[i]=dp0[i-1]^1;
                st=1;
            }
            if(s[n-i]==s[n-i-1])
            {
                dp1[n-i-1]=dp1[n-i]^sr^(sr+1);
                sr++;
            }
            else
            {
                dp1[n-i-1]=dp1[n-i]^1;
                sr=1;
            }
        }
        if(!dp0[n-1]) //if second kid wins without breaking string.
        {
            cout<<yes<<endl;
            continue;
        }
        int flag=0;
        for(int i=0;i<n-1;i++)
        {
            if(dp0[i]^dp1[i+1])
                continue;
            flag=1;break;
        }
        if(flag)
            cout<<yes<<endl;
        else
            cout<<no<<endl;
    }
    return 0;
}
```