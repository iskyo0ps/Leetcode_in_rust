# 2.1.2 Remove Duplicates from Sorted Array II
## 描述
Follow up for ”Remove Duplicates”: What if duplicates are allowed at most twice? 
For example, Given sorted array A = [1,1,1,2,2,3], 
Your function should return length = 5, and A is now [1,1,2,2,3]
- ## 分析
  代码1
  目的、原则、过程
  删除重复的数据，允许重复两次。
  输入是vector&，返回一个长度。
  
  
  目的：去重，
  原则：允许重复两次
  过程：那么我重复三次的时候就应该被删除掉，所以需要一个counter来告诉我是否删除掉。
  步骤 1 确定输入输出
  ```C++
  // Pseudocode
  // v0.1
  class Solution{
  pubilc: int removeDuplicates(vector<int>& nums){//Step 1 
  //函数名，返回一个int，
  //传入参数因为需要就地修改参数所以传入了 reference.
  	int result;  // 返回一个int 类型
  	return result;
  }
  };
  ```
  
  步骤 2 确定流程
  ```C++
  // Pseudocode
  // v0.2
  class Solution{
  pubilc: int removeDuplicates(vector<int>& nums){//Step 1 
  //函数名，返回一个int，
  //传入参数因为需要就地修改参数所以传入了 reference.
  	// Step 2 需要一个prev当先头兵 去按图索骥
  	// Step 3 需要一个Flag counter
  	// Step 4 比较
  	// Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	// Step 4.2 如果前后一致，Flag += 1；
  	// Step 4.2.1 当flag = 1时，用后面的值给前面的赋值
  	// Step 4.2.2 当flag > 1时，前面的值保持不变，丢弃后面的值,
  	// Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  	int result;  // 返回一个int 类型
  	return result;
  }
  };
  ```
  
  步骤3 添加亿点点细节
  ```C++
  // Pseudocode
  // v0.2
  class Solution{
  pubilc: int removeDuplicates(vector<int>& nums){//Step 1 
  //函数名，返回一个int，
  //传入参数因为需要就地修改参数所以传入了 reference.
  	// Step 2 需要一个prev当先头兵 去按图索骥
  	int prev = 0
  	// Step 3 需要一个Flag counter
  	int Flag_counter = 0;
  	// Step 4 比较需要遍历所有
  	for (int i = 1;i < nums.size();i++) // i is for current.
  	// 因为要拿prev 和for循环里的i比较，所有i从1开始
  	// Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	// Step 4.2 如果前后一致，Flag += 1；
  	// Step 4.2.1 当flag = 1时，用后面的值给前面的赋值
  	// Step 4.2.2 当flag > 1时，前面的值保持不变，丢弃后面的值,
  	// Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  	int result;  // 返回一个int 类型
  	return result;
  }
  };
  ```
  
  进到for循环里
  ```C++
  // Pseudocode
  // v0.3
  class Solution{
  pubilc: 
  	int removeDuplicates(vector<int>& nums){//Step 1 
  //函数名，返回一个int
        int result;  // 返回一个int 类型
  //传入参数因为需要就地修改参数所以传入了 reference.
  // Step 2 需要一个prev当先头兵 去按图索骥
  int prev = 0
  // Step 3 需要一个Flag counter
  int Flag_counter = 0;
  // Step 4 比较需要遍历所有
  for (int i = 1;i < nums.size();i++){ // i is for current.
  // 因为要拿prev 和for循环里的i比较，所有i从1开始
  
  	// Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	if ( nums[prev] != nums[i]){
  		nums[++prev] = nums[i];//为了把后面的放到前面来，所以要先加后用
  		Flag_count = 0;
  	}
  	// Step 4.2 如果前后一致，Flag += 1；
  	if （nums[prev] == nums[i]）{
  		Flag_counter += 1;
  		// Step 4.2.1 当flag = 1时，用后面的值给前面的赋值
  		if (Flag_counter <= 1){
  			nums[++prev] = nums[i];
  		}else{
  		// Step 4.2.2 当flag > 1时，前面的值保持不变，丢弃后面的值,
  			nums[++prev] = nums[i]; //值不变
  			// 那什么叫做丢弃呢？++ prev 已经和i相同了，指向的值又相同的，
  			// 所以 i 应该自觉点，把权力直接递交给下一个i
  			// 所以应该用
  			i++；
  		// Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  		}
  	}
  }
  result = prev + 1；
  return result;
  	}
  };
  ```
  走去leetcode
  
  pending....
  ```C++
  //**Custom Judge:**
  
  //The judge will test your solution with the following code:
  
  int[] nums = [...]; // Input array
  int[] expectedNums = [...]; // The expected answer with correct length
  
  int k = removeDuplicates(nums); // Calls your implementation
  
  assert k == expectedNums.length;
  for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
  }
  
  //If all assertions pass, then your solution will be **accepted**.
  ```

  这个代码跑起来一直没有办法输出争取的个数，总是在最后一位出问题。逻辑上总是处理不相等的，让flag = 0；遇到相等的，Flag自增。
  问题就在于，Flag的自增运算，应该在循环的最后结束，为什么不是最前面呢？
  
  这个和判断条件先后执行的关系有关。
  一旦优先对不等条件进行判断，那么在相等条件结束后，前后下一位不相等，那只能在另一次循环中结束了。所以每次都错位。
  
  是这样吗？
  换了一个input就不对了
  [1,2,3,4,5,5,6,6,7,7,7]

问题并不是处在判断条件的执行先后。
而是没有处理最关键的第三个数据，其实需要三个人比来比去
prev，current，next

```C++

  // Pseudocode
  // v0.3
  class Solution{
  public: 
  	int removeDuplicates(vector<int>& nums){//Step 1 
  //函数名，返回一个int
        int result;  // 返回一个int 类型
  //传入参数因为需要就地修改参数所以传入了 reference.
  // Step 2 需要一个prev当先头兵 去按图索骥
  int prev = 0
  int current = 1；
  int next = 2；
  // Step 3 需要一个Flag counter
  int Flag_counter = 0;
  
  // Step 4 比较需要遍历所有
  // for (int current = 1;current < nums.size();current++){ // i is for current.
  for ( int i = 1; i < nums.size(); i++){
  // 因为要拿prev 和for循环里的i比较，所有i从1开始
  	// Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	if ( nums[prev] != nums[i]){
  		nums[++prev] = nums[i];//为了把后面的放到前面来，所以要先加后用
  		Flag_count = 0;
  	}
  	// Step 4.2 如果前后一致，Flag += 1；
  	if （nums[prev] == nums[i]）{
  		// Step 4.2.1 这里则需要考虑next是否相等，如果相等则不对next的数据进行同步
  		if (nums[i] == nums[++i]){  // ++i equal to next，but still keep i is be wanna one。
  		 ;// i ++;已经在nums[++i]中使用了
  		}else{
  		 nums[prev] = nums[i];
  		 nums[i] = nums[++i];
  		 i--;
  		}
  		// Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  		}
  	}
  }
  result = prev + 1；
  return result;
  	}
  };
```

放弃思考了....
```
=================================================================
==32==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x603000000060 at pc 0x000000345f8a bp 0x7fffadb971f0 sp 0x7fffadb971e8
READ of size 4 at 0x603000000060 thread T0
    #2 0x7f779c43f0b2  (/lib/x86_64-linux-gnu/libc.so.6+0x270b2)
0x603000000060 is located 0 bytes to the right of 32-byte region [0x603000000040,0x603000000060)
allocated by thread T0 here:
    #6 0x7f779c43f0b2  (/lib/x86_64-linux-gnu/libc.so.6+0x270b2)
Shadow bytes around the buggy address:
  0x0c067fff7fb0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff7fc0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff7fd0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff7fe0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c067fff8000: fa fa fd fd fd fa fa fa 00 00 00 00[fa]fa fa fa
  0x0c067fff8010: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8020: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8030: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8040: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
  Shadow gap:              cc
==32==ABORTING
```

 别看下面的代码这么工整，没用
```C++
// Pseudocode
// v0.4
  class Solution{
    public: 
  	    int removeDuplicates(vector<int>& nums)
        {   //Step 1 
            //函数名，返回一个int
            int result;  // 返回一个int 类型
            //传入参数因为需要就地修改参数所以传入了 reference.
            // Step 2 需要一个prev当先头兵 去按图索骥
            int prev = 0;
            // Step 3 需要一个Flag counter
            // int Flag_counter = 0;
            // Step 4 比较需要遍历所有
            // for (int current = 1;current < nums.size();current++){ // i is for current.
            for ( int i = 1; i < nums.size(); i++)
            {
                // 因为要拿prev 和for循环里的i比较，所有i从1开始
  	            // Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	            if ( nums[prev] != nums[i])
                {
  		            nums[++prev] = nums[i];//为了把后面的放到前面来，所以要先加后用
  		            //Flag_count = 0;
  	            }
  	            // Step 4.2 如果前后一致，Flag += 1；
  	            if (nums[prev] == nums[i])
                {
  		            // Step 4.2.1 这里则需要考虑next是否相等，如果相等则不对next的数据进行同步
  		            if (nums[i] == nums[++i])
                    {  // ++i equal to next，but still keep i is be wanna one。
  		                ;// i ++;已经在nums[++i]中使用了
  		            }
                    else
                    {
                        
  		                nums[++prev] = nums[i];
  		                nums[i] = nums[++i];
  		                i--;
  		            }
  		            // Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  		        }
  	        }       
            result = prev + 1;
            return result;
  	    }
};
```
错得离谱，首先考虑 ++i；和i++在进入代码段前后的变化，
先剥离这个问题再看问题出在了哪里？
```C++
// Pseudocode
// v0.5
#include <iostream>     // std::cout
#include <algorithm>    // std::unique, std::distance
#include <vector>       // std::vector

int main() {
	std::vector<int> nums = { 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1 };           // 10 20 20 20 30 30 20 20 10
	int prev = 0;
	for (int i = 1; i < nums.size(); i++) {
		/*if (nums[prev] != nums[i]) {
			nums[++prev] = nums[i];
			
		}*/
		std::cout << "进入循环的i本体       ：" << i << std::endl;
		if (nums[prev] == nums[i++]) {
			std::cout << "if代码段历经i++后 ：" << i << std::endl;
		}
		std::cout << "i++之后跳出if的i本体  ：" << i << std::endl;
	}
	// std::cout << "i++之后跳出for代码段的i本体  ：" << i << std::endl;
	return 0;
```

我们看一下打印了什么
```C++
进入循环的i本体       ：1
if代码段历经i++后 ：2
i++之后跳出if的i本体  ：2
进入循环的i本体       ：3
if代码段历经i++后 ：4
i++之后跳出if的i本体  ：4
进入循环的i本体       ：5
if代码段历经i++后 ：6
i++之后跳出if的i本体  ：6
进入循环的i本体       ：7
if代码段历经i++后 ：8
i++之后跳出if的i本体  ：8
进入循环的i本体       ：9
if代码段历经i++后 ：10
i++之后跳出if的i本体  ：10
进入循环的i本体       ：11
if代码段历经i++后 ：12
i++之后跳出if的i本体  ：12
进入循环的i本体       ：13
if代码段历经i++后 ：14
i++之后跳出if的i本体  ：14
进入循环的i本体       ：15
if代码段历经i++后 ：16
i++之后跳出if的i本体  ：16
```
也就是说i++或者++i运算，确实是一个局部变量，每次都会根据自增运算变化。

所有你想用++i，当作next使用会有问题的。
```C++
// Pseudocode
// v0.6
  class Solution{
    public: 
  	    int removeDuplicates(vector<int>& nums)
        {   //Step 1 
            //函数名，返回一个int
            int result;  // 返回一个int 类型
            //传入参数因为需要就地修改参数所以传入了 reference.
            // Step 2 需要一个prev当先头兵 去按图索骥
            int prev = 0;
            int current = 1;
            int next = 2
            // Step 3 需要一个Flag counter
            // int Flag_counter = 0;
            // Step 4 比较需要遍历所有
            // for (int current = 1;current < nums.size();current++){ // i is for current.
            for ( int i = 1; i < nums.size(); i++)
            {
                // 因为要拿prev 和for循环里的i比较，所有i从1开始
  	            // Step 4.1 如果前后不一致，flag = 0; 用后面的值给前面的赋值
  	            if ( nums[prev] != nums[i])
                {
  		            nums[++prev] = nums[i];//为了把后面的放到前面来，所以要先加后用
  		            //Flag_count = 0;
  	            }
  	            // Step 4.2 如果前后一致，Flag += 1；
  	            if (nums[prev] == nums[current])
                {
  		            // Step 4.2.1 这里则需要考虑next是否相等，如果相等则不对next的数据进行同步
  		            if (nums[current] == nums[next])
                    {  // ++i equal to next，but still keep i is be wanna one。
  		                ;// i ++;已经在nums[++i]中使用了
  		            }
                    else
                    {
                        
  		                nums[++prev] = nums[current];
  		                nums[++curent] = nums[next++];
  		            }
  		            // Step 4.2.3 直到前后不一致为止。此时flag重新置零。
  		        }
  	        }       
            result = prev + 1;
            return result;
  	    }
};
```

卧槽，我放弃思考了，有这么费劲吗，都他妈写一天了，清明节写了一天。

我觉哥们你对 for循环 不够理解

for循环三大要义
1，起点：初始第一个变量是谁？怎么处理
2，终点：这个for循环怎么停下？
3，过程：迭代的步长是什么？迭代基是什么？迭代的基础的缩写，迭代基。

其实，这里是一个ahamoment，恰如我是ahanerd

for循环的本质，是一个广义的数学归纳法。只是被用在了计算机科学领域就有了for循环。

>The simplest and most common form of mathematical induction infers that a statement involving a natural number n (that is, an integer n ≥ 0 or 1) holds for all values of n. The proof consists of two steps:
>
>The induction step, inductive step, or step case: prove that for every n, if the statement holds for n, then it holds for n + 1. In other words, assume that the statement holds for some arbitrary natural number n, and prove that the statement holds for n + 1.
>
>The hypothesis in the inductive step, that the statement holds for a particular n, is called the induction hypothesis or inductive hypothesis. To prove the inductive step, one assumes the induction hypothesis for n and then uses this assumption to prove that the statement holds for n + 1.
>
>Authors who prefer to define natural numbers to begin at 0 use that value in the base case; those who define natural numbers to begin at 1 use that value.
>--wikipedia，https://en.wikipedia.org/wiki/Mathematical_induction

只是对于计算机领域的for循环需要停下来！n最好是一个有限值。

所以每一次写一个for循环，都是在用归纳法，即基本情况，归纳步骤。

让我们来回到问题上来

基本情况是什么？
归纳步骤是什么？

一组已经排序过的数组，最多允许重复两个
如果建立一个Flag来计数，你看到了上面的代码，我已经放弃了。针对prev，current，next处理循环变量太难了，居然有的时候需要回退。

到底谁是next，当我们描述问题的时候，进行关系递推的时候，我们说current，一旦嵌套循环的时候，current就变成了next，这样的话，它无论是current还是next，都只是对i变量在不同时期的一个注释。for循环就是一个时间的序列，当下就是未来。

凡是你讲不明白的都是因为你没想明白。

使用target描述我们想要的变量，target响应current变量情况。

对current情况的描述
当i = 0时，没有办法比较；
当i = 1时，和nums[0]比较，就算重复也是允许的；
当i = 2时，这时情况发生了变化，可以拿nums[2] 和nums[1]比较，其实完全可以不在是否相同，此时，应该和nums[0]去比较，判断是否相等。

一旦相等，nums[]本身是一个有序数组，哪意味着，三个连续变量相等了，target应该指向nums[1],返回长度2。
一旦不等，那么没事儿，咱们把目标转向下一个。最后返回taget +1 为长度即可。 

确定每次跨越两个变量的方式，那么则有
```C++
// Pseudocode
// v0.7
if(nums[current] != nums[target - 2]){
	target = target；   //target维持原位置不变；
	current++；    // 更新current，指向next
}
if(nums[current] != nums[Target - 2]){
	nums[target] = nums[current]； //terget接收当前变量
	target++；      //target 右移，等待下一次笔更新
	current++；     //更新current，指向下一个
}
```

综上，
1° 考虑第一步，初始值设定。
target从上面对变量i的分析，可知i= 2时开始生效，故target=2。for变量中的自增变量应该从2开始。
```C++
// Pseudocode
// v0.8
int target = 2；
for(int current = 2; current < nums.size();  )
{
	if(nums[current] != nums[target - 2]){
		target = target；   //target维持原位置不变；
		current++；    // 更新current，指向next
	}
	if(nums[current] != nums[Target - 2]){
		nums[target] = nums[current]； //terget接收当前变量
		target++；      //target 右移，等待下一次更新
		current++；     //更新current，指向下一个
	}
}
```

2° 提取选相同的部分，current++ 可以在for()中做，于是有
```C++
// Pseudocode
// v0.9
int target = 2；
for(int current = 2; current < nums.size(); current++ )
{
	if(nums[current] != nums[target - 2]){
		target = target；   //target维持原位置不变；
							//这里target相当于没有做任何更新，那该if分支可以删除
	}
	if(nums[current] != nums[Target - 2]){
		nums[target] = nums[current]； //terget接收当前变量
		target++；      //target 右移，等待下一次更新,
	}
}
```
3° 使用变量i代替current，以后可能都是用i来理解current，看看是否有其他可以优化项？
```C++
// Pseudocode
// v1.0
int target = 2；
for(int i = 2; i < nums.size(); i++ )
{
	if(nums[i] != nums[Target - 2]){
		nums[target] = nums[i]； //terget接收当前变量
		target++；      //target++ 先用后加，所以可以移动到上式子中。
	}
}
```

4° 最终可以这么写
```C++
// Pseudocode
// v1.1
int target = 2；
for(int i = 2; i < nums.size(); i++ )
{
	if(nums[i] != nums[Target - 2]){
		nums[target++] = nums[i]； //terget接收当前变量
	}
}
```
5° 合并下
```C++
// Pseudocode
// v1.2
class Solution{
pubilc:
 int removeDuplicates(vector<int>& nums){//Step 1 
	//函数名，返回一个int
	int result;
	//传入参数因为需要就地修改参数所以传入了 reference.
	int target = 2；
	for(int i = 2; i < nums.size(); i++ )
	{
		if(nums[i] != nums[Target - 2])
		{
			nums[target] = nums[i]； //terget接收当前变量
			target++；      //target++ 先用后加，所以可以移动到上式子中。
		}
	}
	result = target + 1;	
	return result;
  }
  };
```

6° All in.
```C++
// Pseudocode
// v1.3
class Solution{
pubilc:
 int removeDuplicates(vector<int>& nums)
 {//Step 1 
	//函数名，返回一个int
	int length;
	//传入参数因为需要就地修改参数所以传入了 reference.
	int target = 2；
	for(int i = 2; i < nums.size(); i++ )
	{
		if(nums[i] != nums[Target - 2])
		{
			nums[target] = nums[i]； //terget接收当前变量
			target++；      //target++ 先用后加，所以可以移动到上式子中。
		}
	}
	length = target + 1;	
	return length; //让名字更有意义。
	}
};
```

面向编译器修改，哦，别忘了处理极端情况，类似于判空。
```C++
// Pseudocode
// v1.4
class Solution{
public:
 int removeDuplicates(vector<int>& nums)
 {//Step 1 
     if(nums.size() <= 2)
     {
         return nums.size();
     }
	//look at fn，it ask we return a int
	int length; // Define a result here, and let its name truly express its own meaning
	//Because the parameter needs to be modified in place, the reference type is passed in
	int target = 2;
	for(int i = 2; i < nums.size(); i++ )
	{
		if(nums[i] != nums[target - 2])
		{
			nums[target] = nums[i]; //terget takes the current variable
			target++;    //target++ means take value of target first then plus ifself by 1
		}
	}
	length = target; //due to the target++ in the loops, so we jsut return target is enough
	return length; 
	}
};
```

if you can cpp, now, we rust it！
```rust
//V0.1
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
    //the least length in nums will affect the result, so we need handle this
        if (nums.len() <= 2){
	        nums.len() as i32;
        }
        let mut target = 2; 
        for current in 2..nums.len() {
            if (nums[current] != nums[target - 2]){
                nums[target] = nums[current];
                target = target + 1;
            }  
        }
        target as i32 // due to index is form zero, length = index + 1;  
    }           
}
```

使用match语句，可以节省内存空间
```C++
//V0.2
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
    //the least length in nums will affect the result, so we need handle this
        match (nums.len()){
	        0 | 1 => nums.len() as i32,// arm braces need end with ,
            _ =>{   let mut target = 2; 
                    for current in 2..nums.len() {
                        if (nums[current] != nums[target - 2]){
                            nums[target] = nums[current];
                            target += 1;
                        }
                    }    
                    target as i32 
                }       
        }           
    }
}    
```

看看别人是怎么做的
### 1. Remove Duplicates
```
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        let mut i = 2;

        while i < nums.len() {
            if nums[i] == nums[i - 2] {
                nums.remove(i);
            } else {
                i += 1;
            }
        }

        nums.len() as i32
    }
}
```

### 2. Two Pointers
```
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        let mut i = 0;

        for j in 0..nums.len() {
            if i < 2 || nums[j] != nums[i - 2] {
                nums[i] = nums[j];
                i += 1;
            }
        }

        i as i32
    }
}
```

自己写的如下：
```rust
//V0.3
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        let mut i = 2;
        while i < nums.len(){
            if(nums[i] == nums[i-2]){
                nums.remove(i);
            }else{
                i += 1;
            }
        } 
        nums.len() as i32// nums.len() is  better than i,because i >= 2 
    }
}    
```
那既然rust可以remove ，Cpp的vector是不是也可以remove操作啊
https://www.cplusplus.com/reference/vector/vector/erase/
针对数组没有remove属性，但是有erase，擦除的时候要传入Iterator。
```C++
// erasing from vector
#include <iostream>
#include <vector>

int main ()
{
  std::vector<int> myvector;

  // set some values (from 1 to 10)
  for (int i=1; i<=10; i++) myvector.push_back(i);

  // erase the 6th element
  myvector.erase (myvector.begin()+5);

  // erase the first 3 elements:
  myvector.erase (myvector.begin(),myvector.begin()+3);

  std::cout << "myvector contains:";
  for (unsigned i=0; i<myvector.size(); ++i)
    std::cout << ' ' << myvector[i];
  std::cout << '\n';

  return 0;
}
```
而remove操作是归属于algorithm的
```C++
// remove algorithm example
#include <iostream>     // std::cout
#include <algorithm>    // std::remove

int main () {
  int myints[] = {10,20,30,30,20,10,10,20};      // 10 20 30 30 20 10 10 20

  // bounds of range:
  int* pbegin = myints;                          // ^
  int* pend = myints+sizeof(myints)/sizeof(int); // ^                       ^

  pend = std::remove (pbegin, pend, 20);         // 10 30 30 10 10 ?  ?  ?
                                                 // ^              ^
  std::cout << "range contains:";
  for (int* p=pbegin; p!=pend; ++p)
    std::cout << ' ' << *p;
  std::cout << '\n';

  return 0;
}
```
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 2;
        switch(nums.size()){
            case 0|1:
                return nums.size();
            default:{
                    while( i < nums.size()){
                        if(nums[i] == nums[i-2]){
                            nums.erase(nums.begin()+i);
                        }
                        i++;
                    }
                return nums.size();
            }
        }    
    }
};
```

Wrong Answer

Runtime: 0 ms

Your input

[1,1,1,2,2,3]  
[0,0,1,1,1,1,2,3,3]  

Output

[1,1,2,2,3]  
[0,0,1,1,1,2,3,3]  

Diff

Expected

[1,1,2,2,3]  
[0,0,1,1,2,3,3]

分析问题：因为erase之后会让你的current 向后位移，缺失了对这一个位置的判断。所以需要变更策略，相同的时候擦掉，不相同的时候向后移动。
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 2;
        switch(nums.size()){
            case 0|1:
                return nums.size();
            default:
                {
                    while( i < nums.size()){
                        if(nums[i] == nums[i-2]){
                            nums.erase(nums.begin()+i);
                        }else{
                            i++; 
                        }  
                    }
                    return nums.size();
                }
            }
    }
};
```
Runtime: 4 ms, faster than 83.23% of C++ online submissions for Remove Duplicates from Sorted Array II.

Memory Usage: 10.9 MB, less than 40.57% of C++ online submissions for Remove Duplicates from Sorted Array II.

Next challenges:

[Pancake Sorting](https://leetcode.com/problems/pancake-sorting/)[Cinema Seat Allocation](https://leetcode.com/problems/cinema-seat-allocation/)[Remove One Element to Make the Array Strictly Increasing](https://leetcode.com/problems/remove-one-element-to-make-the-array-strictly-increasing/)


```rust
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        let mut i = 2;

        while i < nums.len() {
            if nums[i] == nums[i - 2] {
                nums.remove(i);
            } else {
                i += 1;
            }
        }

        nums.len() as i32
    }
}
```
Success

[Details](https://leetcode.com/submissions/detail/677387427/) 

Runtime: 0 ms, faster than 100.00% of Rust online submissions for Remove Duplicates from Sorted Array II.

Memory Usage: 2.1 MB, less than 46.34% of Rust online submissions for Remove Duplicates from Sorted Array II.

Next challenges:

[Circular Array Loop](https://leetcode.com/problems/circular-array-loop/)[Long Pressed Name](https://leetcode.com/problems/long-pressed-name/)[Minimized Maximum of Products Distributed to Any Store](https://leetcode.com/problems/minimized-maximum-of-products-distributed-to-any-store/)
完工！午饭。


看一下rust remove的prototype
```rust
  /// Removes and returns the element at position `index` within the vector,
    /// shifting all elements after it to the left.
    ///
    /// Note: Because this shifts over the remaining elements, it has a
    /// worst-case performance of *O*(*n*). If you don't need the order of elements
    /// to be preserved, use [`swap_remove`] instead. If you'd like to remove
    /// elements from the beginning of the `Vec`, consider using
    /// [`VecDeque::pop_front`] instead.
    ///
    /// [`swap_remove`]: Vec::swap_remove
    /// [`VecDeque::pop_front`]: crate::collections::VecDeque::pop_front
    ///
    /// # Panics
    ///
    /// Panics if `index` is out of bounds.
    ///
    /// # Examples
    ///
    /// ```
    /// let mut v = vec![1, 2, 3];
    /// assert_eq!(v.remove(1), 2);
    /// assert_eq!(v, [1, 3]);
    /// ```
    #[stable(feature = "rust1", since = "1.0.0")]
    #[track_caller]
    pub fn remove(&mut self, index: usize) -> T {
        #[cold]
        #[inline(never)]
        #[track_caller]
        fn assert_failed(index: usize, len: usize) -> ! {
            panic!("removal index (is {}) should be < len (is {})", index, len);
        }

        let len = self.len();
        if index >= len {
            assert_failed(index, len);
        }
        unsafe {
            // infallible
            let ret;
            {
                // the place we are taking from.
                let ptr = self.as_mut_ptr().add(index);
                // copy it out, unsafely having a copy of the value on
                // the stack and in the vector at the same time.
                ret = ptr::read(ptr);

                // Shift everything down to fill in that spot.
                ptr::copy(ptr.offset(1), ptr, len - index - 1);
            }
            self.set_len(len - 1);
            ret
        }
    }
```

