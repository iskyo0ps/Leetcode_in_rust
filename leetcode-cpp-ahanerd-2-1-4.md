# 2.1.4 Search in Rotated Sorted Array II 
## 描述
Follow up for ”Search in Rotated Sorted Array”: What if duplicates are allowed? Would this affect the run-time complexity? How and why? 
Write a function to determine if a given target is in the array.

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
			}elseif{
				if(nums[mid] < target && target <= nums[last-1])//index = size - 1 
					first = mid +1;
				else//放弃在右半边寻找
					last = mid;			
			}else{
			  //skip duplicates one
			  first ++;
			}
		}
		return -1;
	}
};
```

针对之前的代码，需要对边界进行变更，那你直接改一下返回值不行吗？
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
                return true;
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
		return false;
	}
};
```
Wrong Answer

[Details](https://leetcode.com/submissions/detail/680870060/) 

Input

[1,0,1,1,1]  
0  

Output

false  

Expected

true

之前的版本只考虑了单调递增的数列，这是要出问题的，所以你是不是应该重新考虑下你的input，然后对判断条件有一个清醒的认识啊。

数据变化确实会引起条件的变更。
之前小于等于的边界条件，一旦数据集合重复，我直接返回mid，这样的mid = first + (last - first)/2的设计会让你失去一半的数据，而且你不知道目标数据在左半边还是右半边。

所以我们重新看一下设计
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
                return true;
            }
			if(nums[first] < nums[mid]){//左半边长，右半边短，右半边的last 一定是小于左半边first的,关注点在mid与两端比较,这里使用<=或者<都可以，因为else会兜底。
				if(nums[first] <= target && target < nums[mid])
					last = mid;
				else//放弃在左半边寻找
					first = mid + 1;//为什么不是mid呢，应为一个条件只判断一次，mid在最上面给了判断。			
			}else if (nums[first] > nums[mid]){
				if(nums[mid] < target && target <= nums[last-1])//index = size - 1 
					first = mid +1;
				else//放弃在右半边寻找
					last = mid;			
			}else{
				//skip duplicates one
				first ++；//这一是让最前面往后以后还是让最后面往前移动，本质上没有任何区别
				// last--； 
			}
		}
		return false;
	}
};
```

换成last-- 使用了一下，发现这是问题了。
那他为什么不行呢？
因为你起手的判断条件使用mid去和first比较，而针对这个条件你在收尾的时候，去操控last，这就有点out of control了。
所以最后要使用first进行判断。
我发现使用rust写代码，从一方面可以检验对代码的理解，另一方面可以加强对rust语法的掌握。

```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> bool {
        let mut first = 0;
        let mut last = nums.len();
        while(first != last){
            let mut mid = first + (last - first)/2;
            if (nums[mid] == target)
            {
                return true;
            }
            // We devied the vector into two parts,
            // then consider the rotated pivot is on the vector side
            if(nums[first] < nums[mid])
            {//left side is more longger than right
                if(nums[first] <= target && target < nums[mid])
                {
                    last = mid;
                }
                else
                {
                    first = mid + 1;
                }
            }
            else if (nums[first] > nums[mid])
            {//right side is more longger than left
                if(nums[mid] < target && target <= nums[last - 1])
                {
                    first = mid + 1;//drop mid before
                }
                else
                {
                    last = mid;// drop mid behind
                }
            }
            else
            {
                first = first + 1;
            }    
        }
        false
    }
}
```
Runtime: 2 ms, faster than 34.13% of Rust online submissions for Search in Rotated Sorted Array II.

Memory Usage: 2.1 MB, less than 45.24% of Rust online submissions for Search in Rotated Sorted Array II.

Next challenges:

[Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)[Find Minimum in Rotated Sorted Arr](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
慢到这个地步也是没谁了，你可以看看别人的答案，直接用迭代器做，都比你这个快
```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> bool {
     nums.iter().any(|&i| i == target)
    }
}
```

其实也没快，问题是一样的，从最坏的情况考虑从最后一定是一个挨着一个地找。
之前的人之所以能够rust 提前 defeat 100%的人，是因为他们来得早。

抄作业
Rust
函数式编程风格通常包含将函数作为参数值或其他函数的返回值、将函数赋值给变量以供之后执行等等。
-   **闭包**（_Closures_），一个可以储存在变量里的类似函数的结构
-   **迭代器**（_Iterators_），一种处理元素序列的方式
-   如何使用这些功能来改进第十二章的 I/O 项目。
-   这两个功能的性能。（**剧透警告：** 他们的速度超乎你的想象！）

有一个问题是这样的，在你看完这一章节关于闭包内容的介绍，会在内容的最后比较迭代器和for循环的速度，进而解释了什么叫做零成本抽象，就是rust的编译器生成的汇编和手写的高效汇编几乎是一样的，所以尽情的使用迭代器和其他的高级抽象。
迭代器.

直接展开多次，就避免了使用for的消耗。
好看又好用。

所有的for循环都用iterator来做吧。
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

We use a match to return the bool variables for answers.  
Using a iterator to call the method of next, it just works when we cost it.  
The way we cost .iter() is using .position(),  
Using closures to catch the variables in environment, which is means nums.  
If it exits, return true. else we return false, by using Some() method.

```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> bool {
        match nums.iter().position(|&x| x == target){
            Some(index) => true,
            None => false,
        }
    }
}
```