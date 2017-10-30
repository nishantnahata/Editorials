## Problem Name : Over Achiever

###Topic : Greedy

#### Difficulty : Easy

###EXPLANATION

Let an array, *A*, be an array storing the marks of over achiever. *A* can be initialized with -1. 

$$A_{i}= $$ $$max$$(all the marks with subject labelled i)


####Time Complexity : O(N)

~~~c++
#include <bits/stdc++.h>
#define nn 1000100
#define ll long long int

using namespace std;

int a[nn];

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n,k;
    cin>>n>>k;
    memset(a, -1, sizeof a); // initializing array
    for(int i=0;i<k;i++)
    {
    	int b,c;
    	cin>>b>>c;
    	a[b]=max(a[b],c); //calculating maximum marks for each subject
    }
    for(int i=1;i<=n;i++)
    	cout<<a[i]<<' ';
    cout<<endl;
    return 0;
}
~~~