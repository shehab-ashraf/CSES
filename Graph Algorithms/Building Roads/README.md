## Building Roads

#### Statement

( The original statement can be found [here](https://cses.fi/problemset/task/1666))
Byteland has n cities, and m roads between them. The goal is to construct new roads so that there is a route between any two cities.
Your task is to find out the minimum number of roads required, and also determine which roads should be built.

Example
~~~
Input :
4 2
1 2
3 4

Output :
1
2 3
~~~

#### solution
If we separate the cities by groups where each group is only composed of cities that are reachable from each other we will get the following representation.


<img src="https://user-images.githubusercontent.com/61033121/133524808-80ab845a-6369-4809-bb81-3fc5729a9ae6.png" width="350" height="200">
The image above contains 2 groups and only requires one connection to make every city reachable from each other.so, we can say we have an unconnected graph and we want to convert it to a fully connected graph.<br/>
<br/>

To solve this problem, we could use an algorithm like depth-first search to recursively traverse all the nodes in each group and mark them as visited.so, we should be able to count the number of groups. Knowing the number of groups is crucial since the idea behind the solution is to connect the groups.


The next step is to find which nodes we should connect. One way of finding out is to choose the nodes that weren't yet visited in the iteration process. As unvisited nodes weren't explored by the depth-first search algorithm, we can assume it belongs to a new road.

code 
~~~C++
#include <bits/stdc++.h>
using namespace std;


#define maxInput 202020

vector<vector<int>> cities (maxInput);
vector<bool> visited (maxInput, false);
vector<int> bridges;

void DFS(int city) {
    visited[city] = true;
    for(auto &connectedCity : cities[city]) {
        if(visited[connectedCity] == false) {
            DFS(connectedCity);
        }
    }
}

int main() {
    int n , m;
    cin >> n >> m;
    for(int i = 0 ; i < m ; i++) {
        int city1 , city2;
        cin >> city1 >> city2;
        cities[city1].push_back(city2);
        cities[city2].push_back(city1);
    }
    for (int i = 1; i <= n; i++) {
      if (visited[i] == false) {
        bridges.push_back(i);
        DFS(i);
      }
    }
    cout << bridges.size() -1 << endl;
    for(int i = 0 ; i < bridges.size()-1 ;i++) {
        cout << bridges[i] << " " <<bridges[i+1] << endl;
    }
}
~~~
