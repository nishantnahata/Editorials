## Problem Name : Mike and Infinite Grid

### Topic : Game Theory, DP

#### Difficulty : Difficult

### EXPLANATION

We know that a player can only move such that, $$n<=x<=p$$ and $$m<=y<=q$$. So we can say that size of the grid is atmost $$1000*1000$$(see the constraints). Let $$win_{i,j}$$ represent whether $$i,j$$ is a winning position or not. If there is a winning position in row $$i$$ or column $$j$$ or diagonal passing from $$i,j$$, then the position $$i,j$$ is a losing position, otherwise its winning position.<br>

We can precompute the winning positons, on a 1000*1000 grid.

#### Time Complexity : O($$N^2$$).

```c++
#include<iostream>
 
using namespace std;

int f1[1100][1100],f2[1100][1100],f3[1100][1100],win[1100][1100];
 
int main()
{
    f1[0][0]=1;
    f2[0][0]=1;
    f3[0][0]=1;
    for(int i=1;i<1100;i++)
    {
        for(int j=1;j<1100;j++)
        {
            if(f1[i][j-1])
            {
                f1[i][j]=1;
            }
            if(f2[i-1][j])
            {
                f2[i][j]=1;
            }
            if(f3[i-1][j-1])
            {
                f3[i][j]=1;
            }
            if(!f1[i][j-1] && !f2[i-1][j] && !f3[i-1][j-1])
            {
                f1[i][j]=1;
                f2[i][j]=1;
                f3[i][j]=1;
                win[i][j]=1;
            }
        }
    }
    int t;
    cin>>t;
    int n,m,p,q;
    while(t--)
    {
        cin>>n>>m>>p>>q;
        if(win[n-p][m-q])
        {
            cout<<"NO"<<endl;
        }
        else 
        {
            cout<<"YES"<<endl;
        }
    }
    return 0;
}
```