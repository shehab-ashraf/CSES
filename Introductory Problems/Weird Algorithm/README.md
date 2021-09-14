
## Weird Algorithm


#### Statement

( The original statement can be found [here](https://cses.fi/problemset/task/1068) )
you are given a positive integer n. If n is even, the algorithm divides it by two, and if n is odd,
the algorithm multiplies it by three and adds one. The algorithm repeats this, until n is one. 

Example
~~~
input : n = 3
output : 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1
~~~

Your task is to simulate the execution of the algorithm for a given value of n.


#### Solution

This problem is a simple simulation problem, which does not require much thinking. Here is a possible way to solve the problem :

~~~C++
#include <iostream>
using namespace std;
int main() {
    long long n;
    cin >> n;
    while (true) {
        cout << n << " ";
        if (n == 1) break;
        if (n % 2 == 0)
            n /= 2;
        else
            n = n * 3 + 1;
    }
    cout << endl;
}
~~~
space complexity : O(1).\
time complexity
>It is harder to find a time complexity since we don't even know if it always converges to 1 for any given N. 
 All we know (and you can test this) is that up to 10^6 the largest possible sequence you can get has size 524.

