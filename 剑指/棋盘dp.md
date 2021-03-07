在一个 m * n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？  
常规二维dp解法：
```
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if(grid.empty())  return 0;
        int m=grid.size();
        int n=grid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                dp[i][j]=grid[i][j]+max(i-1>=0?dp[i-1][j]:0,j-1>=0?dp[i][j-1]:0);
        return dp[m-1][n-1];

    }
};
```
滚动数组应用：
```
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        if(grid.empty())  return 0;
        int m=grid.size();
        int n=grid[0].size();
        vector<int> dp(n+1,0);
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[j] = max(dp[j], dp[j - 1]) + grid[i - 1][j - 1];
            } 
        }
        return dp[n];
    }
};
```
