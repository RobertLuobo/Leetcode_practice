#### [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

简单

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        if not nums:
            return None;
        if len(nums)  == 1:
            return nums[0]
        
        left = self.majorityElement(nums[:len(nums)//2])
        right = self.majorityElement(nums[len(nums)//2:])

        if left == right:
            return left

        if(nums.count(left) > nums.count(right)):
            return left;
        else:
            return right;
```



```c++
class Solution {
public:

    int Count(vector<int>& nums, int l, int r, int num){
        int count = 0;
        for(int i=l; i<=r; i++){
            if(nums[i] == num) count++;
        }
        return count;
    }

    int fenzhididui(vector<int>& nums, int l, int r){
        if(l == r)  return nums[l];

        int mid = l + (r-l)/2;
        int left = fenzhididui(nums, l, mid);
        int right = fenzhididui(nums, mid+1, r);

        if(left == right) return left;

        int leftcount = Count(nums, l, r, left);
        int rightcount = Count(nums, l, r, right);

        return (leftcount > rightcount) ? left: right;

    }

    int majorityElement(vector<int>& nums) {
        int len = nums.size();
        if(len == 0) return 0;
        else if(len == 1) return nums[0];

        return fenzhididui(nums, 0, len-1);
    }
};
```

#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

难度简单

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例:**

```
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**进阶:**

如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return None;
        if(len(nums) == 1):
            return nums[0]
        length = len(nums)

        left = self.maxSubArray(nums[:length//2])
        right = self.maxSubArray(nums[length//2:])

        max_l = nums[length//2 - 1]
        tmp = 0
        for i in range(length//2-1, -1, -1):
            tmp += nums[i]
            max_l = max(tmp, max_l)

        max_r = nums[length//2 ]
        tmp = 0
        for i in range(length//2, length):
            tmp += nums[i]
            max_r = max(tmp, max_r)
        
        return max(max(left, right), max_l+max_r)
```

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int len = nums.size();
        if(len == 0) return 0;
        else if(len == 1)  return nums[0];

        cout << "len:" << len <<" INT_MIN:" << INT_MIN <<endl;
        int f[len];
        
        memset(f, INT_MIN, sizeof(f));
        f[0] = nums[0];

        // for(auto i:f) cout << i << endl;

        int res = INT_MIN ;

        for(int i = 1; i < len; i++){
            f[i] = max(nums[i], f[i-1]+nums[i]);
        }

        for(int item:f){
            res = max(res, item);
        }
        return res;
    }
};
```



#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

中等

实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数。

**示例 1:**

```
输入: 2.00000, 10
输出: 1024.00000
```

**示例 2:**

```
输入: 2.10000, 3
输出: 9.26100
```

**示例 3:**

```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```

**说明:**

- -100.0 < *x* < 100.0
- *n* 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            x = 1/x
            n = -n
        
        if n == 0:
            return 1

        if(n%2 == 1):
            p = x * self.myPow(x, n-1);
            return p;
        return self.myPow(x * x, n/2)
```

