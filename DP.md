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

