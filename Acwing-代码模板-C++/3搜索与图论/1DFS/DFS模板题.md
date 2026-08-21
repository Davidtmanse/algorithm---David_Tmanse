# 842.排列数字
```
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 10;
int n , path[N];
bool st[N]; // 状态数组

void dfs(int u ) // 第几个数字，一共几个数字
{
    if(u == n)// 递归到最后一个数字
    {
        for (int i = 0; i < n; i ++ ) cout << path[i] << ' '; // 输出保存的结果
        puts(" ");
    }

    for (int i = 1; i <= n; i ++ )
        if (!st[i]) // 没有被用过的数
        {
            path[u] = i ;
            st[i] = true; // i被用过
            dfs(u + 1);// 走到下一层
            st[i] = false;// 恢复现场
        }
}

int main()
{

    cin >> n;
    dfs(0);
    return 0;
}
```
***
### 位运算写法
```
#include <iostream>
using namespace std;

const int N = 10;  // 最多排列 1~10（因为 10! 已经很大了）

int n;             // 用户输入：排列长度 n
int path[N];       // 当前正在构造的排列

/**
 * DFS 搜索排列
 * u: 当前已经填好的位置个数（也是下一个要填的位置下标）
 * state: 状态压缩，用二进制表示 1~n 哪些数字已经用过
 *        第 i 位 = 1 → 数字 i+1 已用
 *        第 i 位 = 0 → 数字 i+1 未用
 */
void dfs(int u, int state)
{
    // ✅ 递归终止条件：已经填完 n 个位置
    if (u == n)
    {
        // 输出当前排列
        for (int i = 0; i < n; i++)
            printf("%d ", path[i]);
        puts("");  // 换行
        return;
    }

    // ✅ 枚举所有数字，尝试填到位置 u
    for (int i = 0; i < n; i++)
    {
        // 判断数字 i+1 是否没用过
        if (!(state >> i & 1))
        {
            path[u] = i + 1;                 // 填数字
            dfs(u + 1, state + (1 << i));    // 递归 + 标记使用
        }
    }
}

int main()
{
    scanf("%d", &n);   // 输入 n
    dfs(0, 0);         // 从第 0 个位置开始，初始状态：所有数字都未用
    return 0;
}
```
***

# 843.N-皇后问题
### 1.按照每一层放一个皇后的顺序进行dfs
```
#include <iostream>

using namespace std;

const int N = 20;
int n;
char q[N][N];
bool col[N], qd[N], uqd[N];

void dfs(int u){
    if (u == n){
        for (int i = 0; i < n; i ++){
            for (int j = 0; j < n; j ++){
                cout << q[i][j];
            }
            puts("");
        }
        puts("");
        return;
    }
    
    for (int i = 0; i < n; i ++){
        if (!col[i] && !qd[u - i + n] && !uqd[u + i]){
            q[u][i] = 'Q';
            col[i] = qd[u - i + n] = uqd[u + i] = true;
            dfs(u + 1);
            q[u][i] = '.';
            col[i] = qd[u - i + n] = uqd[u + i] = false;
        }
    }
}

int main(){
    cin >> n;
    
    for (int i = 0; i < n; i ++)
    for (int j = 0; j < n; j ++)
        q[i][j] = '.';
        
    
    dfs(0);
    
    return 0;
}
```
***
### 2.按照每一个空的顺序进行dfs
```
#include <iostream>

using namespace std;

const int N = 20;
int n;
char q[N][N];
bool raw[N], col[N], qd[N], uqd[N];

void dfs(int x, int y, int s){
    if (s > n) return;
    if (y == n) y = 0, x ++;
    
    if (x == n){
        if (s == n){
            for (int i = 0; i < n; i ++) puts(q[i]);
            cout << endl;
        }
        return;
    }
    
    // 如果这个空位不放皇后
    q[x][y] = '.';
    dfs(x, y + 1, s);
    
    // 如果这个空位放皇后
    if (!raw[x] && !col[y] && !qd[x - y + n] && !uqd[x + y]){
        q[x][y] = 'Q';
        raw[x] = col[y] = qd[x - y + n] = uqd[x + y] = true;
        dfs(x, y + 1, s + 1);
        q[x][y] = '.';
        raw[x] = col[y] = qd[x - y + n] = uqd[x + y] = false;
    }
}

int main(){
    cin >> n;
        
    dfs(0, 0, 0);
    
    return 0;
}
```