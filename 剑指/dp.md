1、最大子数组问题  
此题由于数字可能为负数，所以不可以使用滑动窗口。滑动窗口算法无非就是双指针形成的窗口扫描整个数组/子串，但关键是清楚地知道什么时候应该移动右侧指针来扩大窗口，什么时候移动左侧指针来减小窗口。对于这道题目当窗口扩大的时候可能遇到负数，窗口中的值也就可能增加也可能减少，这种情况下不知道什么时机去收缩窗口，也就无法求出「最大子数组和」。  
假如使用dp[i]表示nums[0..i]中的最大子数组和，无法由dp[i]得出dp[i+1]因为子数组一定是连续的，按照我们当前dp数组定义并不能保证nums[0..i]中的最大子数组与nums[i+1]是相邻的，也就没办法从dp[i]推导出dp[i+1]。  
对于这类子数组问题，我们就要重新定义dp数组的含义：以nums[i]为结尾的「最大子数组和」为dp[i]。这种定义之下，想得到整个nums数组的「最大子数组和」，不能直接返回dp[n-1]，而需要遍历整个dp数组：  
```C++
for (int i = 0; i < n; i++) 
{
    res = max(res, dp[i]);
}
```
假设我们已经算出了dp[i-1]，如何推导出dp[i]呢？可以做到，dp[i]有两种「选择」，要么与前面的相邻子数组连接，形成一个和更大的子数组；要么不与前面的子数组连接，自成一派，自己作为一个子数组。  
** dp[i] = max(nums[i], nums[i] + dp[i - 1]);**  
```C++
    // 边际1：数组长为0，返回0
    // 第一个元素前面没有子数组
    dp[0] = nums[0];
    // 状态转移方程
    for (int i = 1; i < n; i++) 
    {
        dp[i] = max(nums[i], nums[i] + dp[i - 1]);
    }
    // 得到 nums 的最大子数组
    int res = Integer.MIN_VALUE;//这里由于有负数以该设置为负无穷->在C++中  正无穷：如果是int，可以用INT_MAX表示正无穷，INT_MIN表示负无穷，需要包含limits.h头文件  0x7fffffff赋值给maxInt,0x80000000赋值给minInt
    for (int i = 0; i < n; i++) {
        res = max(res, dp[i]);
    }
```
  
动规代码：  
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        if(n==0)  return 0;
        vector<int> dp(n,0);
        dp[0]=nums[0];
        for(int i=1;i<n;i++)
        {
            dp[i]=max(nums[i],dp[i-1]+nums[i]);
        }
        int sum=0x80000000;
        for(int i=0;i<n;i++)
        {
            if(dp[i]>sum)
                sum=dp[i];
        }
        return sum;
    }
};
```
  
分治代码：  
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        return fz(nums,0,n-1);
    }

    int fz(vector<int>& nums,int l,int r)
    {
        if(l==r)  return nums[l];
        int mid=(l+r)/2;
        int lnum=fz(nums,l,mid);//最大连续数组在mid左边
        int rnum=fz(nums,mid+1,r);//最大连续数组在mid右边
        //最大连续数组在中间并且含有mid本身，此时从mid向左右延伸找到最大
        //寻找包含中间的向左右最大，则找向左最大，向右最大，相加
        int maxnuml=0x80000000;
        int templ=0;
        for(int i=mid;i>=l;i--)
        {
            templ+=nums[i];
            maxnuml=max(templ,maxnuml);
        }
        int maxnumr=0x80000000;
        int tempr=0;
        for(int i=mid+1;i<=r;i++)
        {
            tempr+=nums[i];
            maxnumr=max(tempr,maxnumr);
        }
        int maxmid=maxnuml+maxnumr;
        return max(max(lnum,rnum),maxmid);
    }
};
```

