## Problem Name : Arranging Boxes

###Topic : Greedy

#### Difficulty : Medium

###EXPLANATION

 <br>

Let $$A$$ be an array, in which $$A_i$$ denotes the box number of the box at $$i^{th}$$ position. Now to move $$A_i$$ to its actual position, $$A_{A_i}$$ needs to be moved to its original position. So $$A_i$$ depends on $$A_{A_i}$$, $$A_{A_i}$$ depends on $$A_{A_{A_i}}$$ and so on. We know that every value box, which is not on its position, depends on some other (and only one)box which is at its position. Also, we know that for any box, there can be atmost one box depending on it. We can think of it as a graph, where nodes are 1, 2, 3, ..., n and there is an edge $$i->A_i$$ , if $$i$$ $${!=}$$ $$A_i$$. In this graph, each node has either indegree = outdegree = 0, or indegree = outdegree = 1. This means, each node with indegree = 1, is a part of a **cycle**. Therefore there will be, *K*, cycles in the graph and each cycle is independent of each other. We can individually arrange boxes in a cycle.<br>

Now for one cycle, of length *c*, we need to find an optimal way to move boxes. We can move any one box to the empty slot. Now there are *c-1* boxes in *c* slots. The empty slot is the desired slot of one of the *c-1* boxes, so we can move this box there. After moving this box to its desired position, the new empty slot is also desired slot of one of the *c-2* boxes. So on we can move all of the *c-1* boxes to their desired positions, and the the final empty slot will be the desired slot for the first box we moved out. In this way, for each cycle, of length *c*, boxes can be arranged in *c+1* moves.<br>

So the final answer to the question will be,<br>

$$finalAnswer = \sum_{cycles} length+1$$

~~~c++
#include <bits/stdc++.h>
#define nn 1000100
 
using namespace std;

int a[nn],v[nn];

int main()
{
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n;
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    int ans=0;
    for(int i=1;i<=n;i++)
    {
        if(a[i]==i || v[i]) /*already visited or already in position*/
            continue;
        int cnt=1 /*length of cycle*/,j=a[i];
        v[i]=1; // mark visited
        while(!v[j])
        {
            cnt++;
            v[j]=1;
            j=a[j];
        }
        ans+=(cnt+1);
    }
    cout<<ans<<endl;
    return 0;
}
~~~