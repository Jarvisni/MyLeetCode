网格中中的动态规划算法，其状态转移方程可以归纳为如下：  
最短路径（带权值的网格）：dp[i][j] = min{dp[i - 1][j], dp[i][j - 1]} + grid[i][j]  
所有路径（包含障碍，1表示障碍，0表示通行）：if grid[i][j] ==0 :dp[i][j] = dp[i - 1][j] + dp[i][j - 1],否则,dp[i][j] = 0  
所有路径（不包含障碍）,状态方程同2中if，也可以直接返回组合数C(m+n)(m)，m，n为网格的行数和列数。  
本题：一条路径，状态方程和1一样，只不过每个点的权值相等。  
  
dp[i][j] == 1，直接忽略  
dp[i][j] == 0,分情况考虑:  
1.i == n - 1 && j == m - 1 , 忽略  
2.i == n - 1 : dp[i][j] = dp[i][j + 1] 只能往右走  
3.j == m - 1 : dp[i][j] = dp[i + 1][j] 只能往下走  
4.其他 : dp[i][j] = dp[i + 1][j] & dp[i][j + 1] 可以往下走也可以往右走  
DP遍历时 i, j 从大到小还是从小到大的问题，这个取决于状态转移方程，如果当前状态取决于 i + 1,那么i 就是从大到小，对j也是如此。反之亦然。  

```C++
class Solution {
public:
    vector<vector<int>> pathWithObstacles(vector<vector<int>>& obstacleGrid) {
        vector<vector<int>> path;
        dfs(obstacleGrid,0,0,path);
        return path;
    }

    bool dfs(vector<vector<int>>& grid,int x,int y,vector<vector<int>>& path)
    {
        int m=grid.size();
        int n=grid[0].size();
        if(grid[x][y]==1)  return false;
        if(x==m-1 && y==n-1)
        {
            path.push_back({x,y});
            return true;
        }
        path.push_back({x,y});
        if(x+1<m && dfs(grid,x+1,y,path))  return true;
        if(y+1<n && dfs(grid,x,y+1,path))  return true;

        path.pop_back();
        grid[x][y]=1;//visited标记此路往后不可以到达终点
        return false;
    }

};
```
  

链接：https://leetcode-cn.com/problems/robot-in-a-grid-lcci/solution/tong-yi-de-dpmo-ban-qiu-jie-ju-zhen-zhon-w3pg   
链接：https://leetcode-cn.com/problems/robot-in-a-grid-lcci/solution/c-dpyi-bian-guo-si-lu-zheng-li-by-spacex-cr1u  
