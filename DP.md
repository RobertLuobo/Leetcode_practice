最长公共子序列
最长回文子序列
「编辑距离」
「公共子序列」

#### lc  [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

中等

给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(*n2*) 。

**进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?

```c++
dp[i] 代表的是以nums[i]为结尾的时候，的最长上升子序列

class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len  = nums.size();
        if(len == 0) return 0;
        else if(len == 1) return 1;
        int res = 0;
        // int dp[len];
        vector<int> dp (len, 1);
        // memset(dp, 1, sizeof(dp));
        //for(int i: dp) cout << i << endl;
        // cout << sizeof(dp) << endl;
       
       dp[0] = 1;
       dp[1] = 1;

        for(int i = 0; i < len; i++){
            for(int j = 0;j < i; j++){
                //cout << dp[i] << " "<< dp[j] << endl;
                if(nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j]+1);
            }
        }

        for(int i: dp)
            res = max(res, i);
        
        return res;
        
    }
};
```

lc 53[最大子序和](https://leetcode-cn.com/problems/maximum-subarray/) 简单

题目与lc300差不多，dp[i]定义为 以nums[i]为结尾的时候的最大子序和，那么这里的状态转移方程

不是dp[i] = max(dp[i-1], dp[i-1] + nums[i]); 因为子序列可以舍弃前面的，应该是dp[i] = max(nums[i], dp[i-1] + nums[i]); 



lc674 [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/) 简单

```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int len = nums.size() ;
        if (len == 0) return 0;
        else if(len == 1) return 1;

        vector<int> dp (len, 1);
        int res = 1;

        dp[0] = 1;
        dp[1] = (nums[0] < nums[1])? 2 : 1;
        for(int i = 1; i < len; i++){
            if(nums[i] > nums[i-1])
                dp[i] = max(dp[i-1]+1, dp[i]);
        }

        for(int i : dp){
            // cout << i << " " << res << endl;
            res = max(res, i);
        }

        return res;
    }
};
```

#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s == "") return "";
        int len = s.size();
        if(len == 0) return NULL;
        else if(len == 1) return s;

        int max_len = 1;
        int start = 0;
        int cur_len = 0;
        vector<vector<bool>> dp (len,vector<bool>(len, false));
        // for(auto str: dp) cout << str << endl;
        // for(int i = 0;i < len;i++)
        //     for(int j = 0; j < len;j++)
        //         cout << dp[i][j] << endl;

        for(int j = 1; j < len; j++){
            for(int i = 0; i < j; i++){
                if(s[i] == s[j]){
                    if(j - i < 3) dp[i][j] = true;
                    else dp[i][j] = dp[i+1][j-1];
                }
                if(dp[i][j] == true){
                    cur_len = j - i + 1;
                    if(cur_len > max_len){
                        max_len = cur_len;
                        start = i;
                    }
                } 
            }
        }
        //cout << start << " " << max_len << endl;
        return s.substr(start,max_len);
    }
};
```

#### [516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        if(s == "") return 0;
        int len = s.size();
        if(len == 0) return 0;
        else if(len == 1) return 1;
        vector<vector<int>> dp (len, vector<int>(len, 0));

        //for(int i = 0; i < len; i++) dp[i][i] = 1;

        for(int i = (len-1); i >= 0; i--){
            dp[i][i] = 1;
            for(int j = i + 1; j < len; j++){
                if(s[i] == s[j])    dp[i][j] = dp[i+1][j-1] + 2;
                else    dp[i][j] = max( dp[i][j-1], dp[i+1][j] );
            }
        }
        
        return dp[0][len-1];
    }
};
```
#### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

难度困难1084

给你两个单词 *word1* 和 *word2*，请你计算出将 *word1* 转换成 *word2* 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

1. 插入一个字符
2. 删除一个字符
3. 替换一个字符

 

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```



```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.length();
        int m = word2.length();

        vector<vector<int>> dp (n+1, vector<int>(m+1, 0));
        // int dp[n+1][m+1];
        for(int i = 0;i < n+1 ; i++) dp[i][0] = i;
        for(int j = 0;j < m+1 ; j++) dp[0][j] = j;

        for(int i = 1; i< n+1; i++){
            for(int j = 1; j < m+1;j++){
                if(word1[i-1] == word2[j-1]) 
                    dp[i][j] = dp[i-1][j-1];
                else 
                    dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1]) ) + 1 ;
            }
        }
        
        return dp[n][m];
    }
};
```


#### [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

难度中等358

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都**围成一圈，**这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下，**能够偷窃到的最高金额。

**示例 1:**

```
输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2:**

```
输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

```C++
class Solution {
public:
    int helper(vector<int>& nums){
        int len = nums.size();

        if(len == 0) return 0;
        else if(len == 1) return nums[0];
        else if(len == 2) return max(nums[0], nums[1]);

        vector<int> dp (len, 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i = 2; i < len; i++){
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }
        int res = 0;
        for(int i:dp)  res = max(res, i);

        return res;
    }

    int rob(vector<int>& nums) {
        int len = nums.size();

        if(len == 0) return 0;
        else if(len == 1) return nums[0];
        else if(len == 2) return max(nums[0], nums[1]);

        
        vector<int> nums_0 (nums.begin(), nums.end()-1);
        vector<int> nums_end (nums.begin()+1, nums.end());

        for(auto i: nums_0) cout << i << endl;
        cout << "nums_end" << endl;
        for(auto i: nums_end) cout << i << endl;

        return max(helper(nums_0), helper(nums_end));
    }
};
```

