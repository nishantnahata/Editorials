## Problem Name : Flip The Coins

###Topic : Greedy

#### Difficulty : Easy

###EXPLANATION

Keep an array, UP, which stores, for each coin, whether to flip this coin or not. Initialize this array with 0. Zero at position i, means that $$i^{th}$$ coin is not to be flipped and one at position i, means $$i^{th}$$ is to be flipped. For each update, <br>

$$UP_i = UP_i xor 1$$ and $$UP_{i+A_{i}+1} = UP_{i+A_{i}+1} xor 1$$<br>

This means that update is to be added to all the positions greater than i, and update is to be removed for all the positions greater than $$i+A_{i}$$. XOR denotes the flipping of bits.<br>

Now the array, UP, can be finalized by moving these updates forward, i.e.,<br>

$$UP_i = UP_i xor UP_{i-1}$$<br>

Now, if number of zeroes in the array are more than number of ones(i.e. the coin is not flipped and it remains a head), then Alice wins. Otherwise, Bob wins.


####Time Complexity : O(NlogN)

~~~c++
#include <bits/stdc++.h>
#define nn 200100
 
using namespace std;

int upd[nn];
 
int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n,h=0,t=1;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int r;
        cin>>r;
        upd[i]^=1;
        upd[i+r+1]^=1;
    }
    for(int i=1;i<n;i++)
    {
        upd[i]^=upd[i-1];
        if(upd[i])
            t++;
        else
            h++;
    }
    if(h>t)
        cout<<"Alice"<<endl;
    else
        cout<<"Bob"<<endl;
    return 0;
} 
~~~