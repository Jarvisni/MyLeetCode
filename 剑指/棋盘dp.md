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
  
如果改成，在棋盘上寻找目标数字，棋盘从左向右递增，从上到下递增，返回是否有这个目标数字：  
此时关键在于初始点不再应该还从左上开始，而应该从左下或者右上开始，需要满足一个方向上能够有增大另一个方向上有减小  
```
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0 || matrix[0].size()==0 )  return false;
        int m=matrix.size()-1;
        int n=0;
        //从左下角开始，需要一个方向上能够有增大另一个方向上有减小
        while(m>=0 && n<matrix[0].size())
        {
            if(matrix[m][n]==target)  return true;
            if(matrix[m][n]<target)  n++;
            else if(matrix[m][n]>target)  m--;
        }
        return false;
    }
};
```
