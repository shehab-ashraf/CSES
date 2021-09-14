## Counting Rooms

#### Statement

(The original statement can be found [here](https://cses.fi/problemset/task/1192))
You are given a map of a building, and your task is to count the number of its rooms.
The size of the map is n×m squares, and each square is either floor (<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c0/Location_dot_black.svg/1024px-Location_dot_black.svg.png" width="15" height="15">)
or wall (***********************#***********************) You can walk left, right, up, and down through the floor squares.

Example
~~~
Input :
5 8
# # # # # # # #
# . . # . . . #
# # # # . # . #
# . . # . . . #
# # # # # # # #

Output : 3
~~~

#### Solution
The problem asks us to calculate the number of rooms on the map, in other words, to calculate the number of groups consisting of connected dots.
One way to solve this problem is to consider the given grid as a graph where the floor characters represent the nodes and the directions  left, right, up, and down represent the edges. 
In this way, problem can be solved with a graph search algorithm, such as depth-first search , breadth-first.

~~~C++
#include <bits/stdc++.h>
using namespace std;

vector<int> directionInx {0,0,1,-1};
vector<int> directionIny {1,-1,0,0};

bool isValid(int x , int y , int n , int m) {
    if( x >= 0 && x < n && y >= 0 && y < m) return true;
    return false;
}

void DFS(vector<vector<char>>& graph , vector<vector<int>>& visited , int x , int y , int n , int m) {
    visited[x][y] = 1;
    for(int idx = 0 ; idx < 4 ; idx++) {
        int Newx = x + directionInx[idx];
        int Newy = y + directionIny[idx];
        if(isValid(Newx,Newy,n,m) && graph[Newx][Newy] == '.' && visited[Newx][Newy] == 0) {
            DFS(graph,visited,Newx,Newy,n,m);
        }
    }
}

int main() {
    int n , m;
    vector<vector<char>> graph;
    vector<vector<int>> visited;
    int numberOfrooms = 0;
    cin >> n >>  m;
    for(int i = 0 ; i < n ; i++) {
        graph.push_back({});
        visited.push_back({});
        for(int j = 0 ; j < m ; j++) {
            char square;
            cin >> square;
            graph[i].push_back(square);
            visited[i].push_back(0);
        }
    }
    
    for(int i  = 0 ; i < n ; i++) {
        for(int j = 0 ; j < m ; j++) {
            if(visited[i][j] == 0 && graph[i][j] == '.') {
                DFS(graph,visited,i,j,n,m);
                numberOfrooms++;
            }
        }
    }
    cout << numberOfrooms << endl;
}
~~~

time complexity : O(NxM)\
space complexity : O(NxM) 
