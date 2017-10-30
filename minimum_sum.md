## Problem Name : Over Achiever

###Topic : Greedy

#### Difficulty : Easy

###EXPLANATION

Let *A*, be a map storing the occurrences of each integer present in the multiset, S. Then the solution to the problem can be written as :-<br>

$$minSum = min(sum - A_i*i) $$, where $$ i\in A$$


####Time Complexity : O(NlogN)

~~~c++
#include <bits/stdc++.h>
#define ll long long int
 
using namespace std;

map<int, int>a;

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    ll ans=0, sum=0;
    for(int i=0;i<n;i++)
    {
        ll t;
        cin>>t;
        sum+=t;
        a[t]++;
        ans=max(ans,a[t]*t);
    }
    sum-=ans;
    cout<<sum<<endl;
    return 0;
}
~~~