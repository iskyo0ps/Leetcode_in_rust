# 2.1.3 Search in Rotated Sorted Array 
## Description
Suppose a sorted array is rotated at some pivot unknown to you beforehand. (i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2). 
You are given a target value to search. 
If found in the array return its index, 
otherwise return -1. 
You may assume no duplicate exists in the array.

## 分析
既然是no duplicate exists in the array，那就无需要考虑返回多个index的情况了。
有趣的是，如果你想不明白这个数据集该怎么处理，怎么切入不同的if分支，分支上的条件应该是怎样的，那就证明，你一开始对数据集的理解就出错了。所以你至少应你应该绘制一个rorated的数据长什么样子，数形结合百般好。


```C++
class Solution{
public:    
	int search(const vector<int>& nums, int target){
		// 最暴力的办法就是从头找到为，但这显然很笨，不折腾电脑，就要折腾我们自己。
		// 一分为二从从中间开始找
		// 如果中间恰好等于target，直接返回mid即可。
		// 开始造mid
		int mid;
		int first = 0,last = nums.size();
		// mid = (first + last)/2;//这里有一个大争论，就是我first + last 超过了 int的max，你再一分为二，为时已晚。
		while(first!= last){
            mid = first + (last-first)/2;
            if(nums[mid] ==target){
                return mid;
            }
			if(nums[first] < nums[mid]){//左半边长，右半边短，右半边的last 一定是小于左半边first的,关注点在mid与两端比较
				if(nums[first] < target && target < nums[mid])
					last = mid;
				else//放弃在左半边寻找
					first = mid + 1;//为什么不是mid呢，应为一个条件只判断一次，mid在最上面给了判断。			
			}
			if(nums[first] > nums[mid]){
				if(nums[mid] < target && target < nums[last])
					first = mid;
				else//放弃在右半边寻找
					last = mid - 1;			
			}
		}
		return -1;
	}
};

```

其实在比较nums[first]和nums[last]的时候应该注意，看是否需要考虑它等于的情况
```C++
class Solution{
public:    
	int search(const vector<int>& nums, int target){
		// 最暴力的办法就是从头找到为，但这显然很笨，不折腾电脑，就要折腾我们自己。
		// 一分为二从从中间开始找
		// 如果中间恰好等于target，直接返回mid即可。
		// 开始造mid
		int mid;
		int first = 0,last = nums.size();
		// mid = (first + last)/2;//这里有一个大争论，就是我first + last 超过了 int的max，你再一分为二，为时已晚。
		while(first!= last){
            mid = first + (last-first)/2;
            if(nums[mid] ==target){
                return mid;
            }
			if(nums[first] <= nums[mid]){//左半边长，右半边短，右半边的last 一定是小于左半边first的,关注点在mid与两端比较,这里使用<=或者<都可以，因为else会兜底。
				if(nums[first] <= target && target < nums[mid])
					last = mid;
				else//放弃在左半边寻找
					first = mid + 1;//为什么不是mid呢，应为一个条件只判断一次，mid在最上面给了判断。			
			}else{
				if(nums[mid] < target && target <= nums[last-1])//index = size - 1 
					first = mid +1;
				else//放弃在右半边寻找
					last = mid;			
			}
		}
		return -1;
	}
};
```

快使用rust试一遍
```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> i32 {
        if nums.len() == 1 && nums[0] == target {
            return 0;
        }
        if nums.len() == 1 && nums[0] != target {
            return -1;
        }
		let mut mid : usize= 0;
		let mut first : usize= 0;
        let mut last : usize = nums.len();
		while(first!= last){
            mid = first + (last-first)/2;
            if(nums[mid] ==target){
                return mid as i32;
            }
			if(nums[first] <= nums[mid]){//左半边长，右半边短，右半边的last 一定是小于左半边first的,关注点在mid与两端比较
				if(nums[first] <= target && target < nums[mid]){
                    last = mid;
                }
				else{//放弃在左半边寻找
					first = mid + 1;//为什么不是mid呢，应为一个条件只判断一次，mid在最上面给了判断。
                }
			}else{
				if(nums[mid] < target && target <= nums[last-1]){//index = size - 1 
					first = mid +1;
                }
				else{//放弃在右半边寻找
					last = mid;
                }
			}
		}
		return -1;
	}
}
```

	18/04/2022更新撒啊啊啊啊啊啊啊啊啊啊啊aaqqqqq+az`AQ1 

别让复杂停止了你的脚步，此前用rust写了一个C语言风格的二分查找，知道发现rust真正的魅力在于零成本抽象，这意味所有的抽象都是高效的，既保证了代码的可读性，又保证了代码的执行效率，而那些让人头疼的细节都是编译器做的，你只需要专心研究问题是什么就可以了。

抽象才是真的美.

Don't let complexity stop you. I wrote a C-style binary search in rust before, knowing that the real charm of rust lies in zero-cost abstraction, which means that all abstractions are efficient, which not only ensures the readability of the code It ensures the execution efficiency of the code, and those troublesome details are all done by the compiler. You only need to concentrate on studying what the problem is.
```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> i32 {
        match nums.iter().position(|&x| x == target){
            Some(index) => index as i32,
            None => -1 as i32 ,
        }
    }
}
```

