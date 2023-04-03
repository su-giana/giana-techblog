---
title: "Brute-force"
---
from [[algorithm]]

## Recursion and Exhaustive Search
### Algorithm to find all combinations that pick m out of n elements

```C++
void pick(int n, vector<int>& picked, int toPick)
{
    // picked : numbers of elements that have been chosen before
    // toPick : number of elements to pick 
    if(toPick==0)   {cout<<picked; return;}
    int smallest = picked.empty() ? 0 : picked.back() + 1;
    for(int next = smalledst ; next<n ; next++)
    {
        picked.push_back(next);
        pick(n, picked, toPick-1);
        picked.pop_back();
    }
}
```
### Boggle game with recursion
```C++
const int dx[8] = {-1, -1, -1, 1, 1, 1, 0, 0};
const int dy[8] = {-1, 0, 1, -1, 0, 1, 01, 1};
bool hasWord(int y, int x, const string& word)
{
    // if (x, y) is not in range, we do not have to compute them
    if(!inRange(y, x)) return false;
    // return false if the first character of the string is not equal
    if(board[y][x] != word[0])  return false;
    // return true, if the length of string is 1
    if(word.size() == 1)    return true;
    // compute recursion to eight cases near (y,x)
    for(int direction = 0 ; direction < 8 ; ++direction)
    {
        int nextY = y + dy[direction], nextX = x + dx[direction];
        if(hasWord(nextY, nextX, word.substr(1)))   return true;
    }
    return false;
}
```
### Time complexity analysis
Time complexity of exhaustive search is O(available candidates we need to check ^ n)

### Recipt of exhaustive search
1. Time to compute result is in proportion to the number of solution
2. Break the process of generating candidates for all possible answers into multiple choices, each of which becomes a piece of the answer generation process
3. Make part of answer with each of them, get rest of answers with recursion
4. In case of only one piece remained or any piece doesn't remain, choose this to base case because this case generated answer.

### Picnic problem
input : first line c(the number of testcase), next line n(the number of student) and m(the number of pairs of friend)
output : the number of cases to match two students who are friend each other
#### wrong code
``` C++
int n;
bool areFriends[10][10];
int countPairings(boot taken[10])
{
    bool finished = true;
    for(int i = 0 ; i<n ; i++)  if(!taken[i]) finished = false;
    if(finished)    return 1;
    int ret = 0;
    for(int i = 0 l i<n ; i++)
    {
        for(int j = 0 ; j<n ; j++)
        {
            if(!taken[i] && !taken[j] && areFriends[i][j])
            {
                taken[i] = taken[j] = true;
                ret += countPairing(taken);
                taken[i] = taken[j] = false;
            }
        }
    }
    return ret;
}
```
This solution is wrong because they count (1, 0) and (0, 1) seperately

#### Right code
``` C++
int n;
bool areFriends[10][10];
int countPairings(bool taken[10])
{
    int firstFree = -1;
    for(int i = 0 ; i<n ; i++)
    {
        if(!taken[i])
        {
            firstFree = i;
            break;
        }
    }
    if(firstFree == -1) return 1;
    for(int pairWith = firstFree + 1 ; pairWith<n ; pairWith++)
    {
        if(!taken[pairWith] && areFriends[firstFree][pairWith])
        {
            taken[firstFree] = taken[pairWith] = true;
            ret += countPairings(taken);
            taken[firstFree] = taken[pairWith] = false;
        }
    }
    return ret;
}
```
To solve that problem. This code find the first student who are not matched yet and match him first

### Game board problem
input : game board
output : number of cases to fill game board except black area
In this case, **we need to force order to put blocks**. If we not, program will count (put 3 - 1 -2) and (put 1 - 2 - 3) seperately.
``` C++
// relative position of four blocks
const int coverType[4][3][2] = 
{
    {{0, 0}, {1, 0}, {0, 1}},
    {{0, 0}, {0, 1}, {1, 1}},
    {{0, 0}, {1, 0}, {1, 1}},
    {{0, 0}, {1, 0}, {1, -1}}
};

bool set(vector<vector<int> >& board, int x, int y, int type, int delta)
{
    bool ok = true;
    for(int i = 0 ; i<3 ; i++)
    {
        const int ny = y + coverType[type][i][0];
        const int nx = x + coverType[type][i][1];
        if(ny<0 || ny>=board.size() || nx<0 || nx>=board.size())
            ok = false;
        else if((board[nx][ny] += delta) > 1)
            ok = false;
    }
    return ok;
}

int cover(vector<vector<int> >& board)
{
    int y=-1, x=-1;
    for(int i=0 ; i<board.size() ; i++)
    {
        for(int j=0 ; j<board.size() ; j++)
        {
            if(board[i][j] == 0)
            {
                x = i;
                y = j;
                break;
            }
        }
        if(y != -1)  break;
    }
    if(y==-1)   return 1;
    int ret = 0;
    for(int type = 0 ; type<4 ; type++)
    {
        if(set(board, x, y, type, 1))
            ret += cover(board);
        set(board, x, y, type, -1);
    }
    return ret;
}
```
## Optimization problem
### Traveling Salesman Problem(TSP)
``` C++
int n;
double dist[MAX][MAX];
double shortestPath(vector<int>& path, vector<bool>& visited, double currentLength)
{
    if(path.size() == n)    return currentLength + dist[path[0]][path.back()];

    double ret = INF;
    for(int next = 0 ; next<n ; next++)
    {
        if(visited[next])   continue;
        int here = path.back();
        path.push_back(next);
        visited[next] = true;
        double cand = shortestPath(path, visited, currentLength + dist[here][next]);
        ret = min(ret, cand);
        visited[next] = false;
        path.pop_back();
    }
    return ret;
}
```
### Rotating clock
```C++
const int INF=9999, SWITCHES=10, CLOCKS=16;
const char linked[SWITCHES][CLOCKS+1] = 
{
    "xxx.........."
};
// check if the clock is on 12
bool areAligned(const vector<int>& clocks);

void push(vector<int>& clocks, int swtch)
{
    for(int clock = 0 ; clock<CLOCKS ; clock++)
    {
        if(linked[swtch][clock] == 'x')
        {
            clocks[clock] += 3;
            if(clock[clock] == 15)  clocks[clock] =3;
        }
    }
}

int solve(vector<int>& clocks, int swtch)
{
    if(swtch == SWITCHES) return areAligned(clocks) ? 0 : INF;
    int ret = INF;
    for(int cnt = 0 ; cnt<4 ; cnt++)
    {
        ret = min(ret, cnt + solve(clocks, swtch+1));
        push(clocks, swtch);
    }
    return ret;
}
```