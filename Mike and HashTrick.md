## Problem Name : Mike and HashTrick

### Topic : Greedy

#### Difficulty : Medium

### EXPLANATION

We know everytime a number repeats, it is given a new hash value, which is equal to the number of distinct numbers occurred till now. So we can just store the last occurrences of all the numbers and assign the values incrementally, in increasing order of the positions of their last occurrences.

#### Time Complexity : O($$NlogN$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 100100
#define ll long long int
 
using namespace std;

map<int, int>last; //stores the last position of every integer that occurred in the array.
int a[nn];

bool compare(int a,int b) //sorting by last position
{
    return last[a]<last[b];
}

int main()
{
    ios_base::sync_with_stdio(0);
    int n;
    cin>>n;
    vector<int>v;
    for(int i=0;i<n;i++)
    {
    	cin>>a[i];
        if(last.find(a[i])==last.end())
            v.pb(a[i]);
        last[a[i]]=i;
    }
    sort(v.begin(),v.end(),compare);
    for(int i=0;i<v.size();i++)
    {
        cout<<v[i]<<endl;
    }
    return 0;
}
```