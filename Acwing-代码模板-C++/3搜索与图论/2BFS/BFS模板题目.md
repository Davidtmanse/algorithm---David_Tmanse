# 844.走迷宫
### 1.使用`queue`库函数
```
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;  // 用 pair 表示一个坐标 (x, y)

const int N = 110;  // 迷宫最大尺寸

int n, m;           // 迷宫的行数和列数
int g[N][N];        // 存迷宫：0 表示空地，1 表示障碍
int d[N][N];        // 存距离：d[x][y] 表示起点到 (x,y) 的最短距离

int bfs()
{
    queue<PII> q;   // BFS 队列，存待扩展的点

    // 初始化距离数组为 -1，表示所有点都未访问
    memset(d, -1, sizeof d);

    // 起点 (0,0) 的距离是 0
    d[0][0] = 0;

    // 起点入队
    q.push({0, 0});

    // 方向数组：上、右、下、左（顺时针）
    int dx[4] = {-1, 0, 1, 0};
    int dy[4] = {0, 1, 0, -1};

    // BFS 主循环
    while (q.size())  // 只要队列不空
    {
        // 取出队头元素（当前要扩展的点）
        auto t = q.front();
        q.pop();

        // 枚举四个方向
        for (int i = 0; i < 4; i ++ )
        {
            // 计算新坐标
            int x = t.first + dx[i];
            int y = t.second + dy[i];

            // 判断新坐标是否合法：
            // 1. 不越界
            // 2. 不是障碍
            // 3. 没有被访问过
            if (x >= 0 && x < n && y >= 0 && y < m &&
                g[x][y] == 0 && d[x][y] == -1)
            {
                // 更新距离：当前点距离 = 上一个点距离 + 1
                d[x][y] = d[t.first][t.second] + 1;

                // 新点入队
                q.push({x, y});
            }
        }
    }

    // 返回右下角的最短距离
    return d[n - 1][m - 1];
}

int main()
{
    // 读入迷宫大小
    cin >> n >> m;

    // 读入迷宫
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            cin >> g[i][j];

    // 输出最短路径长度
    cout << bfs() << endl;

    return 0;
}
```
***

# 845.八数码
```
#include <iostream>
#include <algorithm>
#include <unordered_map>
#include <queue>

using namespace std;

int bfs(string state)
{
    queue<string> q;              // 队列：存棋盘状态
    unordered_map<string, int> d; // 状态 -> 步数

    q.push(state);   // 初始状态入队
    d[state] = 0;    // 初始步数为 0

    int dx[4] = {-1, 0, 1, 0}; // 上右下左
    int dy[4] = {0, 1, 0, -1};

    string end = "12345678x"; // 目标状态

    while (q.size())
    {
        auto t = q.front(); // 取出队头状态
        q.pop();

        if (t == end) return d[t]; // 到达目标

        int distance = d[t];       // 当前步数
        int k = t.find('x');       // x 的位置
        int x = k / 3, y = k % 3;  // 转成二维坐标

        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < 3 && b >= 0 && b < 3) // 不越界
            {
                swap(t[a * 3 + b], t[k]); // 移动 x

                if (!d.count(t)) // 如果这个状态没访问过
                {
                    d[t] = distance + 1;  // 更新步数
                    q.push(t);             // 新状态入队
                }

                swap(t[a * 3 + b], t[k]); // 恢复状态（关键）
            }
        }
    }

    return -1; // 无解
}

int main()
{
    char s[2];
    string state;

    for (int i = 0; i < 9; i ++ )
    {
        cin >> s;
        state += *s; // 拼成字符串
    }

    cout << bfs(state) << endl;
    return 0;
}
```