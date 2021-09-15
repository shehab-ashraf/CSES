## Labyrinth

#### Statement

( The original statement can be found [here](https://cses.fi/problemset/task/1193/) ) In this problem, we are given with nxm matrix.
Each character is ****.**** (floor) , ****#**** (wall), A (start), or B (end). There is exactly one A and one B in the input.
We need to find the minimum path from A to B. Print 'NO' if there is no path, else print 'YES,' length of the path and 'U,' 'D,' 'L,' 'R'' for directions from A to B.

Example
~~~
Input:
5 8
########
#.A#...#
#.##.#B#
#......#
########

Output:
YES
9
LDDRRRRRU
~~~

#### solution 
here we're looking for the shortest path. so, we can use BFS to solve this problem.
we will make a standard BFS as we know it but here we need also to find our path such as **LDDRRRRRU**
for this we will make other matrix to track our road from **A** to **B** and when we reach to endPoint 
we backtrack from it to startPoint and store our path in array and reverse it and finally print our path


~~~C++
#include <bits/stdc++.h>
using namespace std;

// 0 = up, 1 = right, 2 = down, 3 = left
string stepDirection = "URDL";
vector<int> directionInx {-1,0,1,0};
vector<int> directionIny {0,1,0,-1};

// previousStep stores the previous direction that we moved in to arrive that this cell
vector<vector<int>> previousStep (1000,vector<int> (1000,0));

pair<int,int> endPoint = {INT_MIN,INT_MIN };
pair<int,int> startPoint = {INT_MIN,INT_MIN };
int findingPath = 0;

bool isValid(int x , int y , int n , int m) {
    if( x >= 0 && x < n && y >= 0 && y < m) return true;
    return false;
}

void BFS(vector<vector<char>>& graph , vector<vector<int>>& visited , int x , int y , int n , int m) {
    queue<pair<int,int>> q;
    q.push({x,y});
    visited[x][y] = 1;
    while(!q.empty()) {
        auto position = q.front();
        q.pop();
        for (int idx = 0; idx < 4; idx++) {
            int newX = position.first + directionInx[idx];
            int newY = position.second + directionIny[idx];
            if(isValid(newX,newY,n,m) && graph[newX][newY] != '#' && visited[newX][newY] == 0) {
                previousStep[newX][newY] = idx;
                if(graph[newX][newY] == 'B') {
                    endPoint = {newX,newY};
                    findingPath = 1;
                    return;
                }
                q.push({newX,newY});
                visited[newX][newY] = 1;
            }
        }
    }
}

int main() {
    int n , m;
    vector<vector<char>> graph;
    vector<vector<int>> visited;
    cin >> n >>  m;
    for(int i = 0 ; i < n ; i++) {
        graph.push_back({});
        visited.push_back({});
        for(int j = 0 ; j < m ; j++) {
            char square;
            cin >> square;
            graph[i].push_back(square);
            if(square == 'A') startPoint = {i,j};
            visited[i].push_back(0);
        }
    }
    BFS(graph,visited,startPoint.first,startPoint.second,n,m);
    if(findingPath) {
        cout << "YES" << endl;
        vector<int> steps;
        // here we backtrack from endPoint to startPoint!
        while (endPoint != startPoint) {
            int p = previousStep[endPoint.first][endPoint.second];
            steps.push_back(p);
            endPoint = {endPoint.first - directionInx[p], endPoint.second - directionIny[p]};
        }
        reverse(steps.begin(), steps.end());
        cout << steps.size() << endl;
        for(char step : steps) {
            cout << stepDirection[step];
        }
        cout << endl;
    } else {
        cout << "NO" << endl;
        }
    return 0;
}
~~~

time complexity : O(NxM)\
sapce complexity : O(NxM)
