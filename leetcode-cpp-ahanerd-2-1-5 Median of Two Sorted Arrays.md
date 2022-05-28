# 2.1.5 Median of Two Sorted Arrays 
# 描述
There are two sorted arrays A and B of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log(m + n)).

Runtime: 0 ms, faster than 100.00% of Rust online submissions for Median of Two Sorted Arrays.  
Memory Usage: 2.7 MB, less than 5.16% of Rust online submissions for Median of Two Sorted Arrays.

```rust
impl Solution {
    pub fn find_median_sorted_arrays(nums1: Vec<i32>, nums2: Vec<i32>) -> f64 {
        let mut nums: Vec<i32> = Vec::new();
        nums.extend(nums1.iter());
        nums.extend(nums2.iter());
        nums.sort();
        let n = nums.len() as f32;
        let i = (&n / 2.0).floor() as usize;
        return if n == 0.0 {
            0.0
        } else if n % 2.0 == 1.0 {
            nums[i] as f64
        } else {
            ((nums[i - 1] as f64) + (nums[i] as f64)) / 2.0
        }
    }
}
```

Comments: 2

BestMost VotesNewest to OldestOldest to Newest

Preview

Post

[![whoan's avatar](https://assets.leetcode.com/users/whoan/avatar_1573560755.png)](https://leetcode.com/whoan)

[whoan](https://leetcode.com/whoan)43

June 27, 2021 6:31 PM

Read More

The overall run time complexity is not O(log (m+n)) here

1

Show 2 replies

Reply

Share

Report

[![changtimwu's avatar](https://assets.leetcode.com/users/changtimwu/avatar_1530548501.png)](https://leetcode.com/changtimwu)

[changtimwu](https://leetcode.com/changtimwu)0

July 12, 2021 7:12 AM

Read More

I wouldn't appreciate this solution if I was the interviewer. It relies too much on rust's std stuff that makes time complexity calculation difficult. The O() of vec::extend() is also arguable.

基本方法1
先合并，后查找。
**Time Complexity:** O(n)     T = m + n + k；
**Auxiliary Space :** O(m + n)

基本方法2

边合并，变查找
**Time Complexity:** O(k)   
**Auxiliary Space:** O(1)

