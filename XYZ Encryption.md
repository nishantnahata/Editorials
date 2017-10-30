## Problem Name : XYZ Encryption

### Topic : Greedy, Graphs

#### Difficulty : Medium

### EXPLANATION

First observation is that, if two nodes, *u* and *v* are not connected, then one of them is "x" and another one is "z". Second observation is that if $$S_1$$ is a set of nodes with value "x", and set $$S_2$$ is a set of nodes with value "z", then interchanging them will not change the encrypted graph, i.e. assign value "z" to set $$S_1$$ and value "x" to set $$S_2$$. So first we will search for a pair of nodes, *u* and *v*, which are not connected, assign one of them to "x" and another to "z". Now for the rest of the nodes, if a node, *w* is connected to both *u* and *v*, assign value "y" to it. If a node, *w* is connected to node *u* only, then assign value "x" to it. If a node, *w* is connected to node *v* only, then assign value "z" to it. Now since you have assigned values to all of the nodes, for every pair of nodes, check whether it satisfies the rules of encryption. If it does not, then answer is NO.

#### Time Complexity : O($$N^2$$).

```c++
#include <bits/stdc++.h>
#define ff first
#define se second
#define pb push_back
#define nn 510




                /* VALUES */
#define x 1
#define y 2
#define z 3
 
using namespace std;

int adj[nn][nn];
int val[nn];

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n,m;
    cin>>n>>m;
    for(int i=0;i<m;i++)
    {
        int u,v;
        cin>>u>>v;
        adj[u][v]=adj[v][u]=1;
    }
    int u=-1,v=-1;
    for(int i=1;i<=n && u==-1;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            if(!adj[i][j])
            {
                u=i;
                v=j;
                val[u]=x;
                val[v]=z;
                break;
            }
        }
    }
    if(u==-1) // if all the nodes are pairwise connected, then you cn assign same value to all the nodes
    {
        cout<<"Yes"<<endl;
        return 0;
    }
    for(int i=1;i<=n;i++)//assigning values to nodes.
    {
        if(i==u || i==v)
            continue;
        if(adj[i][u] && adj[i][v]) //nodes is connected to both u and v.
            val[i]=y;
        else if(adj[i][u]) //connected to only u
            val[i]=x;
        else if(adj[i][v])  //connected to only v
            val[i]=z;
        else
        {
            cout<<"No"<<endl;
            return 0;
        }
    }
    for(int i=1;i<=n;i++) //verifying the encryption
    {
        for(int j=i+1;j<=n;j++)
        {
            if(abs(val[i]-val[j])<=1 && !adj[i][j]) //"x"-"x","x-y","y"-"y","y"-"z","z"-"z" should be connected, otherwise answer is No.
            {
                cout<<"No"<<endl;
                return 0;
            }
            if(abs(val[i]-val[j])>1 && adj[i][j]) //"x"-"z" should not be connected.
            {
                cout<<"No"<<endl;
                return 0;
            }
        }
    }
    cout<<"Yes"<<endl; 
    return 0;
}
```