## Message Route

#### Statement

( The original statement can be found [here](https://cses.fi/problemset/task/1667/)) Syrjälä's network has n computers and m connections. Your task is to find out if Uolevi can send a message to Maija, and if it is possible, what is the minimum number of computers on such a route.

Example
~~~
Input:
5 5
1 2
1 3
1 4
2 3
5 4

Output:
3
1 4 5
~~~

#### solution

we have n computers and m connections and our task is finding shortest path from 1 to n.
for problem such this we can use breadth-first search and here we not need the lenght of the path only,
we need also what is the path ? so, here we will use The parent array that is filled over when we execute bfs(). this array is very useful for finding
interesting paths through a graph.<br/>
if we take our example above, we have 5 computers and 5 connections.

<img src="https://user-images.githubusercontent.com/61033121/133679473-be303f49-8c64-40ae-9ceb-fbdb7c97b113.png" width="270" height="200">
if we execute BFS() on this example!<br/>
1) we first pushing computer 1 in queue. (queue [1])<br/>
2) poping computer 1 and pushing all computer connected with it. (queue[2,3,4]) we also doing additional thing here and it is making parent of(2,3,4) is one in      parent array because we walk from 1 to 2 , from 1 to 3 and from 1 to 4 and from this parent array we will talk our path.<br/>
3) we poping 2 from queue and pushing all non visited computer 2 is connected with , and here we find 2 is connected with 3 only an computer 3 is visited so we      here poping computer 2 only. (queue [3,4])<br/>
4) we find computer 3 is same as computer 2. (queue [4])<br/>
5) we poping computer 4 from queue and pushing all non visited computer 4 is connected with, and here we find computer 4 connected with computer 5, and this is      our goal so we terminate from BFS() here. we only here make parent[5] = 4.
                  
| computer  |          1|         2|3         |4         |         5|
| --------- |---------- |--------- |--------- |--------- |--------- |
| parent    |0          |         1|         1|         1|        4 |

And, here based on parent array we backtrack from n to 1 and printing our path!


#### code 
~~~C++
#include <bits/stdc++.h>
using namespace std;

int BFS(vector<vector<int>>& graph , vector<int>& visited , int n , int m , vector<int>& parent) {
    queue<pair<int,int>> q;
    q.push({1,1});
    visited[1] = 1;
    while(!q.empty()) {
        int computer = q.front().first;
        int Distance = q.front().second;
        q.pop();
        for(int idx = 0  ; idx < graph[computer].size() ; idx++) {
            if(visited[graph[computer][idx]] == 0) {
                parent[graph[computer][idx]] = computer;
                if(graph[computer][idx] == n) { cout << Distance+1 << endl ; return 1;}
                q.push({graph[computer][idx],Distance+1});
                visited[graph[computer][idx]] = 1;
            }
        }
    }
    return 0;
}

int main() {
    int n , m;
    cin >> n >>  m;
    vector<vector<int>> graph(n+1);
    vector<int> visited (n+1,0);
    vector<int> parent (n+1,0);
    for(int j = 0 ; j < m ; j++) {
        int computer1 ; int computer2;
        cin >> computer1 >> computer2;
        graph[computer1].push_back(computer2);
        graph[computer2].push_back(computer1);
    }
    if(BFS(graph,visited,n,m,parent)) {
        int step = n;
        vector<int> path;
        while(step != 0) {
            path.push_back(step);
            step = parent[step];
        }
        reverse(path.begin(), path.end());
        for(auto & step : path) {
            cout << step << " " ;
        }
        cout << endl;
    } else {
        cout << "IMPOSSIBLE" << endl;
        }
    return 0;
}
~~~

time complexity : O(n+m)<br/>
space complexity : O(n)

