## Problem Name : Bishop's Moves

###Topic : Greedy

#### Difficulty : Easy

###EXPLANATION

We know that chess board have two types of cells(black and white) connected diagonally. Also bishops can only move diagonally. So if Bishop is on a white cell then he can only reach a white cell and vice versa. Otherwise he cannot reach that cell. Now, if the cell Bishop needs to reach is same as the cell in which Bishop is standing, then the answer is 0. If the cell is on the same diagonal as Bishop, then Bishop can reach there in one move. Further, it can be observed that Bishop can move to the diagonal of the cell in one step. So the maximum answer can be 2.<br>

$$minSum = min(sum - A_i*i) $$, where $$ i\in A$$


####Time Complexity : O(1)

~~~c++
#include <bits/stdc++.h>
 
using namespace std;

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    ll r1,c1,r2,c2,n;
    cin>>n>>r1>>c1>>r2>>c2;
    if((r1+c1)%2 != (r2+c2)%2) //(a,b) and Bishop are on different types of cell.
    {
        cout<<-1<<endl;
        return 0;
    }
    if(r1==r2 && c1==c2) //Bishop is standing on (a,b)
    {
        cout<<0<<endl;
        return 0;
    }
    if((r1+c1) == (r2+c2) || (r1-c1) == (r2-c2)) //Bishop is on the same diagonal
    {
        cout<<1<<endl;
        return 0;
    }
    cout<<2<<endl; // answer is 2 otherwise
    return 0;
}
~~~