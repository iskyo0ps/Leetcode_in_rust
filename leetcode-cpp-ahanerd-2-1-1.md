# EE非要写CS的代码
>Shall I tell you, my friend, how you will come to understand it?
>Go and write a book on it.
>                                             — Henry Home, Lord Kames (1696–1782), to Sir Gilbert Elliot

To my families
To bugnofree
To soulmachine
To  [Learn Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/index.html#learn-rust-with-entirely-too-many-linked-lists)
To 刘未鹏 mindhacks
To 汤质看本质 wechat 

## 两种思维
	 [[批判性思维]]：输入任何信息，就要刻意明确地输出区分论题、论据、结论、立场、观点、事实的信息。
	 [[系统性思维]]：输入任何复杂对象，你都要刻意明确地输出明确区分要素、关系、功能、反馈回路的信息。

## 原则
Think about it before take it.

我不想在全文说你应该如何如何？这样很没礼貌。
写作时，人称”我“是未来的读者，人称“你”是写作时的作者。

这样，看不懂的时候，我就可以骂你傻逼。
没错，我就是傻逼。

---
如果同意上述观点，那我们继续。

Q_0_0:我想知道为什么人称是反的？

>反者，道之动也。

颠倒是为了更好的思考。

问题中蕴含着答案。

可以追溯到三段论

>大前提 ==> 小前提 ==> 结论

问题才是大前提，没有问题就没有答案。

但我听过程序员的金句啊，

>Talk is cheap, show me the code.

Q_0_1: 那你要怎么解释？

>问题是什么？
>
><<征服C指针>>

只说答案，谁知道问题到底是什么，谁知道推导过程是怎样的。

哪怕是错误的思考也比正确的答案更有意义。

## 底层与表象的关系

Q_0_2：我们为什么要探究底层和表象是怎样的关系呢?

别着急，我们先看看这个问题本身。

>底层原理 ==>  ？ ==> 表层现象
>底层原理 <==  ？ <== 表层现象
>Q_0_2_1: 用什么来代替？比较合适？
>Q_0_2_2: 如果用”描述“和”解释“的这两个词来描述，你觉得应该是怎样的，你是如何确定的?

Tips：谁是具象，谁是抽象？

>底层原理 ==> 描述 ==> 表层现象
>底层原理 <== 解释 <== 表层现象

那换一组对象【代码VS交互、界面、逻辑】，这里的逻辑是指实现目的的步骤。

>Q_0_3：谁描述了谁，谁解释了谁
>代码 ==>  ？ ==> 交互
>代码 <==  ？ <== 交互

>问题Q_0_4：谁是（目的、底层），谁是（手段、工具、表现）？
>代码（    ）==> 描述 ==>交互、界面、逻辑（    ）
>代码（    ）<== 解释 <== 交互、界面、逻辑（    ）

>代码（手段、工具，表层）==> 描述 ==>交互、界面、逻辑（目的，底层）
>代码（手段、工具，表层）<== 解释 <== 交互、界面、逻辑（目的，底层）

所以，应该拿着交互逻辑去解释代码怎么写，而代码仅是对交互逻辑的描述。

## 拆解数据结构与算法

我想知道为什么会有线性表（Why？）线性表本身是什么样？（What？）怎么玩转它（How？）

以后所有的内容都会按照这个结构展开
- Why ：线性表的出现是为了解决数据在内存不连续情况下的存储。
- What：通过使用指针对存在对应地址上的内容机型访问，根据指针的访问方式不同，仅有向下访问的链表为单链表，既能向上访问又能向下访问的链表为双链表。
- How：针对链表的操作有，增删改查等等。无非就是0,1...n ==> n ...1, 0

具体题目顺序和解法，参考了soulmachine 的leetcode-cpp

2.1.1  Remove Duplicates from Sorted Array
  hl-page:: 8
  ls-type:: annotation
  
  Given a sorted array 
  hl-page:: 8
  ls-type:: annotation
- 给了一个已经排序的数组

remove the duplicates in place such that each element appear only once
  ls-type:: annotation
  hl-page:: 8
- 在原地，删除重复元素，要求尽可以出现一次。

Do not allocate extra space for another array, you must do this in place with constant memory.
  ls-type:: annotation
  hl-page:: 8
- 不可以异地操作。

代码1想要干什么【目的，原则，过程】
目的Purpose，原则Principle，过程Process
从目的出发，思考设计原则，最后才是实现过程才是需要编码的地方。
- 【目的】已排序的数组，就地删除重复元素
- 【原则】用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
	- 如果不相等，index向后移动，目标元素 = index 所在元素。
	- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值。
- 【过程】
	- 思考点 
		- 我想知道函数名怎么起？有什么规则？ 
			[[谷歌编程指南]][[函数命名]]
		- 我想知道它的输入/输出参数应该是怎样的？为什么是那样的?
		- 中间的具体实现怎么一步步搞？

```C++
// V0.1
class Solution{   // 先声明一个class
public :          // 函数可以被别人调用吗？
				  // 是的话，pubilc，否则，private
				  // 做绝了就是protect，谁也访问不到也改不了。
  int removeDuplicates（vector<int>& nums）{ // 返回一个int类型，使用Google code style.
 // 传入一个参数nums，这是题目给的
 // vector<int> 是说一个装了int类型数据的vector
 // 后面追加一个&，是说，我传入的reference，而不是别的什么。
    return length of 去完重复元素的数组；//规定了返回是谁，类型应该是int，响应最开头函数的类型
  }//对应int fn的开始
}//对应class的开始
```

第一次尝试伪代码，考虑输入输出是什么？

```C++
// V0.2
class Solution{C++
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;// 每次fn要什么，我们就返回一个什么
		return result;// 每次都返回result
	}
}
```

第二次导入3p，目的Purpose，原则Priceples，过程Process
Pseudocode
- 【目的】已排序的数组，就地删除重复元素
- 【原则】用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
	- 如果不相等，index向后移动，目标元素 = index 所在元素。
	- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值。
- 【过程】实现细节
	- 目标是什么？借助什么达到目的？Step1
	- 谁是我们应该关注的对象，谁是返回的对象？Step2
	- 考虑变更范围，在多大范围内搞事情？？Step3
	  
```C++
// V0.3
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		}
	//- 如果不相等，index向后移动，目标元素 = index 所在元素。
	//- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值	
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}
```

进一步考虑实现的细节
Step4~5
```C++
//V0.4 
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		}
	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
	if (nums[index] != nums[i]){
		index ++；
		nums[index] = nums[i]
	}
	//Step5：- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值
	if ( nums[index] == nums[i] ){
	   // nums[index] 不赋值，等待下一次
	   index ++；
 	}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}
```
哥们儿，我觉得Step4-5是不是应该在for循环里面对齐下啊？

是的，变更范围是全体都有。
```C++
// V0.5
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index] != nums[i]){
				index ++；
				nums[index] = nums[i]
			}
			//Step5：- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值
			if ( nums[index] == nums[i] ){
			   // nums[index] 不赋值，等待下一次
			   index ++；
		 	}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}
```

这就完了？可以优化下吗？
你所有的index++都在{}里,如果每次在if里同步不是更好吗？
```C++
// V0.6
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i]
			}
			//Step5：- 如果相等，index 向后移动，舍弃index所有元素，不向index中进行赋值
			if ( nums[index++] == nums[i] ){
			   // nums[index] 不赋值，等待下一次
			   //index ++；
		 	}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}
```

那你第二个if是不是啥也没干啊，删掉不行吗？
马上删。
```C++
// V0.6
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i]
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}
```

你run一下，我看看！
bug！！！！！
为什么看你Step4，先判断下一个是否不相等，然后回过头给上一个赋值？
操操操，等我回头看一下。
从哪里开始错的？
```c++
// V0.5
	  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index] != nums[i]){
				index ++；
				nums[index] = nums[i]；
			}
```

应该是
```C++
// V0.5 'a'
	  	//Step4：- 如果不相等，目标元素 = index 所在元素，index向后移动，。
			if (nums[index] != nums[i]){
				nums[index] = nums[i]；
				index ++；
			}
```

合并index到if分支后
```C++
// V0.5 'b'
	  	//Step4：- 如果不相等，目标元素 = index 所在元素，index向后移动，。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i]；
			}
```

尼玛，合并后，这不还是错得离谱吗？
没有，index++ 先取index，与i比较，比较完，再++;
是这么回事吗?你看V0.7里面的代码和V0.6有差别吗？
```C++
// V0.7
class Solution{
public:
	int removeDuplicates（vecto<int>& nums）{
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0；//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i]
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index；//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
}

```
操，被你给骗了，明明就是index++的问题，你前面没错，但你是懵对的。index先取自己，后进行++.
走上leetcode！
你是不是忘了github的密码了？
啊哈哈哈
```C++
// V0.8
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index;//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
};

```
跑了好几版，上面代码里有中文标点，且没有在Solution最后加；
但是wrong answer啊。

pending.....
```C++
// V0.8
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你		
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 0; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i];
			}
		}
```

哥们你先看一下你的边界，好像有点问题的，你从一开始就起不来，进步到循环里面去吧
index++ 先取index 后++，那你这i = 0；index=0； index与i一直同步啊。

所以是不是应该让index 和 i在一开始错开。至少差1个身位？
那初始化的额时候，让i = 1；
```C++
// V0.9 'a'
        int index = 0;// 最好把它初始化为零，以防止编译器随便塞一个数给你
		// Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i];
			}
		}
```
你为什么是 i =1；而不是动index，是不是因为有一个分析在这里？
index和i到底谁在前面的问题?

- 如果i =1；意味着index必须从0开始，才能开始比较，所有的逻辑都是那后面的i的数据和前面的index比较，最后返回index即可。
- 如果i = 0；意味着index必须从1开始，index始终比i大，最后没办法去重啊。

那问题来，是在if的分支里进行自增运算，还是在if的代码段里进行？
```C++
// V0.9 'b'
        int index = 0;// 最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index] != nums[i]){
				nums[index++] = nums[i];
			}
		}
```

你能不能把index++和++index都试一下啊。然后再分析一下。

不pending了.....
```C++
// V0.9 'a'
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index;//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
```

```
Wrong Answer   Runtime: 0 ms

Your input

[1,1,2]  

Output

[1,1]  

Expected

[1,2]
```
what the fuck?

静一静，然我们分析一下后移操作的逻辑。
```C++
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	//Step4：- 如果不相等，index向后移动，目标元素 = index 所在元素。
			if (nums[index++] != nums[i]){
				nums[index] = nums[i];
			}
		}
```
Step4 肯定有问题，返回的数量是正确的，但是不是预期的。

先不管他为什么错了？我们先想一下怎样才是对的？
```C++
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，
            // 不同则让下一次index所存的数据和当前数据相同，为下次比较做准备
            // nums[index] = nums[i]
			if (nums[index] != nums[i]){
				index ++;
                nums[index] = nums[i];
			}
		}
```
run the fucking code！

```
Wrong Answer    Runtime: 0 ms

Your input

[1,1,2]  

Output

[1]  

Expected

[1,2]
```
长度不对？还是你移位存储不对？

```
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index ;//对index才是我们要关注的对象。
	    return result; //每次都返回result
	}
```

index 从0开始，所有返回的时候，问长度应该+1才对
```C++
public:
	int removeDuplicates(vector<int>& nums){
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
				index ++;
                nums[index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		//result = index ;//对index才是我们要关注的对象。
        result = index + 1; //index 从0开始，所有返回的时候，问长度应该+1
	    return result; //每次都返回result
	}
```

过啦！
End of here?
```C++
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
				index ++;
                nums[index] = nums[i];
			}
```
index操作是不是可以动一动啊？

这样？
```C++
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[index++] = nums[i];
			}
```

哥们儿，你有点随意啊？

index++ 是已经变化了index传到下面，而你这直接移动，index没变啊，下一圈才是加了1 的。你的赋值操作相当于原地踏步了啊。

那用++index？先加，然后再用。
```C++
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[++index] = nums[i];
			}
```

最终长这样
```C++
// V1.0
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	int result;//每次fn要什么，我们就返回一个什么
		//原则principle
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[++index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		// result = index ;//对index才是我们要关注的对象。
        result = index +1; //index 从0开始，所以返回的时候，问长度应该+1
	    return result; //每次都返回result
	}
};
```

7s一下，干净利索点

```C++
// V1.1
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	int result;//每次fn要什么，我们就返回一个什么
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[++index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index + 1;//index 从0开始，所以返回的时候，问长度应该+1
	    return result; //每次都返回result
	}
};
```

还有再优化的可能吗？你设置的这个算法是不是只考虑了数组有元素的情况？
```C++
// V1.2
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	//Step 5 小于两个
	if(nums.size() < 2) return nums.size();
	int result;//每次fn要什么，我们就返回一个什么
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[++index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index + 1;//index 从0开始，所以返回的时候，问长度应该+1
	    return result; //每次都返回result
	}
};
```

再优化
```C++
// V1.3
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	//Step 5 用等式条件判断
	if(nums.size() == 0 | nums.size() == 1) return nums.size();
	int result;//每次fn要什么，我们就返回一个什么
		//用index标记目标元素，分别与数组中的值进行比较，原地比较，就地赋值
		//Step1：给我一个index
		int index = 0;//最好把它初始化为零，以防止编译器随便塞一个数给你
		//Step3：变更范围是哪一部分？全体还是一部分？
		for(int i = 1; i < nums.size(); i++){
		  	// Step4：index 在前面，i在后面，
            // 每次比较，相同则跳过，index++，不同则nums[index] = nums[i]
			if (nums[index] != nums[i]){
                nums[++index] = nums[i];
			}
		}
		//Step2：result = sizeof(nums)？//这样不是跟没比一样吗
		result = index + 1;//index 从0开始，所以返回的时候，问长度应该+1
	    return result; //每次都返回result
	}
};
```
用等式判断比不等式快了3ms。
Success

[Details](https://leetcode.com/submissions/detail/673664813/) 

Runtime: 8 ms, faster than 93.64% of C++ online submissions for Remove Duplicates from Sorted Array.

Memory Usage: 18.4 MB, less than 74.66% of C++ online submissions for Remove Duplicates from Sorted Array.

Next challenges:

如用rust写会写怎样？
rust？不是说好了cpp吗？
cpp可以这样的。
```C++
for(int i = 5; i > -5; i--){
   int summary = 15；
   summary /=i；// i=0 的时候编译器也不报错，它会在运行时跑得很开心	
}
```
你知道这个会在那个版本的编译器会这样吗？
啊，这？

那问题来了，改写CPP的代码到rust
需要解决如下几个问题？
```rust
// rust有class吗？
// rust怎么定义函数的输入/输出？
// rust提供循环吗？
```

来试一试，
```rust
// rust有class吗？
// rust怎么定义函数的输入/输出？
fn remove_decuplicates(vector<int>& nums){
return i32;
}
// rust提供循环吗？
```
你真会开玩笑，这就开始瞎几把写了？

rust提供了一个隐式的返回，所以你的代码一看就有股子骗人味道。
那应该是这样?
```rust
impl Solution {  //impl相当于提供一个入口呗
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
    // pub是公开的，fn是函数的关键字
    // 函数命名规则是snake_cases
    // nums : 后面是他的类型， mut 是可变的 &是借用borrow Vec是向量？里面装了i32  
    // —> 返回一个i32  
    }
}
```

那接着下来回到最开始的三个问题，
```rust
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
    // rust 怎么初始化变量
    // rust 怎么写循环
    // rust 需要return吗？
    }
}
```
你是不是要边写边google？
啊哈哈哈，差不多吧。
rust中文翻译 https://kaisery.github.io/trpl-zh-cn/

剩下的就抄作业呗
```rust
//V0.1
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
    //V0.1
	    // rust 怎么初始化变量
	    // Step1.0
	    let index : i32 = 0;
	    // rust 怎么写循环
	    for element in nums{
		    if (nums[index] != nums[element]){
			    nums[++index] = nums[element];
		    }  
	    }
    nums,    
	    // rust 需要return吗？
    }
}   
```

如果说Cpp写不出来算法，那rust这里是写不对语法。
rust的精髓就是面向complier coding.

complier error
```Rust
Line 10, Char 13: expected expression, found `+` (solution.rs)
   |
10 |                 nums[++index] = nums[element];
   |                      ^ expected expression
Line 13, Char 9: expected one of `!`, `.`, `::`, `;`, `?`, `{`, `}`, or an operator, found `,` (solution.rs)
   |
13 |     nums,
   |         ^ expected one of 8 possible tokens
Line 9, Char 11: the type `[i32]` cannot be indexed by `i32` (solution.rs)
   |
9 |             if (nums[index] != nums[element]){
   |                 ^^^^^^^^^^^ slice indices are of type `usize` or ranges of `usize`
   |
   = help: the trait `SliceIndex<[i32]>` is not implemented for `i32`
   = note: required because of the requirements on the impl of `std::ops::Index<i32>` for `Vec<i32>`
Line 9, Char 26: the type `[i32]` cannot be indexed by `&mut i32` (solution.rs)
   |
9 |             if (nums[index] != nums[element]){
   |                                ^^^^^^^^^^^^^ slice indices are of type `usize` or ranges of `usize`
   |
   = help: the trait `SliceIndex<[i32]>` is not implemented for `&mut i32`
   = note: required because of the requirements on the impl of `std::ops::Index<&mut i32>` for `Vec<i32>`
For more information about this error, try `rustc --explain E0277`.
error: could not compile `prog` due to 4 previous errors
mv: cannot stat '/leetcode/rust_compile/target/release/prog': No such file or directory
```
Line 10，不提供自增运算吗？那我 index = index + 1；
Line 13 看不懂
Line 9 切片需要usize，那我换成u8？
element的type也是不对的？

改动一下
```rust
//V0.2
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // rust 怎么初始化变量
	    // Step 1
	    let index = 0；//在前面使用 prev,我干脆不给你类型,rust不是能自己推断嘛
	    // rust 怎么写循环
	    for element in 1..nums.len() {
		    if (nums[index] != nums[element]){
			    //nums[++index] = nums[element];
			    index = index + 1;
			    nums[index] = nums[element];
		    }  
	    }
	    nums,
	    // rust 需要return吗？
    }
}
```

还是报一样的错，再改一下，
```rust
//V0.3
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // rust 怎么初始化变量，循环里面只能使用usize
	    // Step 1 
	    let index = 0;//在前面使用 prev,你说不是u8，u8也不能循环，那么让rust自己去推断吧
	    // rust 怎么写循环
	    for element in 1..nums.len() {
		    if (nums[index] != nums[element]){
			    //nums[++index] = nums[element];
			    index = index + 1;
			    nums[index] = nums[element];
		    }  
	    }
	    nums;  //不能是逗号，
	    // rust 需要return吗？
    }
} 
```

compiler error
```rust
Line 3, Char 54: mismatched types (solution.rs)
  |
3 |     pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
  |            -----------------                         ^^^ expected `i32`, found `()`
  |            |
  |            implicitly returns `()` as its body has no tail or `return` expression
For more information about this error, try `rustc --explain E0308`.
error: could not compile `prog` due to previous error
mv: cannot stat '/leetcode/rust_compile/target/release/prog': No such file or directory
```
Line 3 ，tuples () 这样的东西没有实现return 这个tail.

```rust
//V0.4
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // rust 怎么初始化变量，循环里面只能使用usize
	    // Step1.0
	    let index = 0;//在前面使用 prev,你说不是u8，u8也不能循环，那么让rust自己去推断吧
	    // rust 怎么写循环
	    for element in 1..nums.len() {
		    if (nums[index] != nums[element]){
			    //nums[++index] = nums[element];
			    index = index + 1;
			    nums[index] = nums[element];
		    }  
	    }
	    nums as i32;  //不能是逗号，我casting，强制转换行不行？
	    // rust 需要return吗？
    }
} 
```

compiler error
```rust
Line 3, Char 54: mismatched types (solution.rs)
   |
3  |     pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
   |            -----------------                         ^^^ expected `i32`, found `()`
   |            |
   |            implicitly returns `()` as its body has no tail or `return` expression
...
15 |         nums as i32 ;
   |                     - help: consider removing this semicolon
Line 15, Char 6: casting `&mut Vec<i32>` as `i32` is invalid (solution.rs)
   |
15 |         nums as i32 ;
   |         ^^^^^^^^^^^
   |
   = help: cast through a raw pointer first
Some errors have detailed explanations: E0308, E0606.
For more information about an error, try `rustc --explain E0308`.
error: could not compile `prog` due to 2 previous errors
mv: cannot stat '/leetcode/rust_compile/target/release/prog': No such file or directory
```
nums as i32，强制类型转换失败，类似编译器他也做不到。你这return的东西不对呀。
长度！

容我更正下
瞎几把试吧，每次改正一点点。
```rust
//V0.5
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // Step1.0  How to define a value in rust？
        // let index :u8 = 0;  //// complier tell me usize is not works.
	    let index = 0;// let complier to interfer
	    // Step3：how can we write a loop in rust？ 
	    for element in 1..nums.len() {
            // Step4 ：if they are same，we do nothing
            // the index is means previous element in vector
            // if previous is not equal to the current one，
            // let we move the index for current
            // then we give the current value for index
		    if (nums[index] != nums[element]){
			    //nums[++index] = nums[element];
			    index = index + 1;
			    nums[index] = nums[element];
		    }  
	    }
        // Step 2 return a value
	    // nums as i32 ;
        // return length of the new vector
	    (index + 1);// due to index is form zero ,so we need add i for length.  
    }
} 
```

我不打算copy complier error了，改就完了
```rust
//V0.6
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // Step1.0  How to define a value in rust？
        // let index :u8 = 0;  //// complier tell me usize is not works.
	    let index = 0;// let complier to interfer
					  // can not assign twice	    
	    // Step3：how can we write a loop in rust？ 
	    for element in 1..nums.len() {
            // Step4 ：if they are same，we do nothing
            // the index is means previous element in vector
            // if previous is not equal to the current one，
            // let we move the index for current
            // then we give the current value for index
		    if (nums[index] != nums[element]){
			    //nums[++index] = nums[element];
			    index = index + 1;
			    nums[index] = nums[element];
		    }  
	    }
        // Step 2 return a value
	    // nums as i32 ;  // complier tell me this is invaild.
        // return length of the new vector
	    (index + 1) as i32 // due to index is form zero ,so we need add i for length.
        
    }
} 

```

complier error
```rsut

Line 16, Char 8: cannot assign twice to immutable variable `index` (solution.rs)
   |
6  |         let index = 0;// let complier to interfer
   |             -----
   |             |
   |             first assignment to `index`
   |             help: consider making this binding mutable: `mut index`
...
16 |                 index = index + 1;
   |                 ^^^^^^^^^^^^^^^^^ cannot assign twice to immutable variable
For more information about this error, try `rustc --explain E0384`.
error: could not compile `prog` due to previous error
mv: cannot stat '/leetcode/rust_compile/target/release/prog': No such file or directory

```
mutable variable 与immutable的区别要注意啊！兄弟！

```rust
//V0.7
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // Step 5 empty?
         match nums.is_empty() {
            true => 0,
            false => {
                        // Step 1  How to define a value in rust？
                        // let index :u8 = 0;  // complier tell me usize is not works.
                        let mut index = 0;// let complier to interfer,but need a keyword mut, 
                        // Step 3：how can we write a loop in rust？ 
                        for element in 1..nums.len() {
                            // Step 4 ：if they are same，we do nothing
                            // the index is means previous element in vector
                            // if previous is not equal to the current one，
                            // let we move the index for current
                            // then we give the current value for index
                            if (nums[index] != nums[element]){
                                //nums[++index] = nums[element];
                                index = index + 1;   
                                // otherwise complier can not assign twice to immutale variable
                                nums[index] = nums[element];
                            }  
                        }
                        // Step 2 return a value
                        // nums as i32 ;  // complier tell me this is invaild.
                        // return length of the new vector
                        (index + 1) as i32 // due to index is form zero,length = index + 1; 
                    }
       }
    }
}
```
但这个只能打败59%。用match相当于switch case。

其实可以优化判断边界的条件。
```rust
//V0.8
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
	    // Step 5 empty?
        if nums.len() == 0 || nums.len() == 1 {
            return nums.len() as i32;
        }
        // Step 1  How to define a value in rust？
        // let index :u8 = 0;  // complier tell me usize is not works.
        let mut index = 0;// let complier to interfer,but need a keyword mut, 
        // Step 3：how can we write a loop in rust？ 
        for element in 1..nums.len() {
            // Step 4 ：if they are same，we do nothing
            // the index is means previous element in vector
            // if previous is not equal to the current one，
            // let we move the index for current
            // then we give the current value for index
            if (nums[index] != nums[element]){
                //nums[++index] = nums[element];
                index = index + 1;   
                // otherwise complier can not assign twice to immutale variable
                nums[index] = nums[element];
            }  
        }
        // Step 2 return a value
        // nums as i32 ;  // complier tell me this is invaild.
        // return length of the new vector
        (index + 1) as i32 // due to index is form zero, length = index + 1;  
    }           
}
```

Success

[Details](https://leetcode.com/submissions/detail/673667472/) 

Runtime: 0 ms, faster than 100.00% of Rust online submissions for Remove Duplicates from Sorted Array.

Memory Usage: 2.4 MB, less than 9.03% of Rust online submissions for Remove Duplicates from Sorted Array.

Next challenges:

能不能用stl来解决问题
比如vector是不是有一些很好的函数？
你这样就不用自己造轮子了。
这个问题的本质，就是从头到尾去重重复元素，最后返回一个长度即可。
那vector是否实现了去重的操作呢？

```C++
class Solution{
	int removeDuplicates(vector<int>& nums){
	//Step 0 确定输入输出
	//Step 1 去重
    //Step 2 求距离
	}
}
```

请你开始表演
```C++
//V 0.1
class Solution{
	int removeDuplicates(vector<int>& nums){
	// Step 0 确定输入输出
	int result；
	//Step 1 去重
	uinque(头, 尾）；
    //Step 2 求距离
    distance（头，新的尾）；
    // Step 0.1 返回结果
    result = distance（）；
    return result；
	}
}

```

啊，unique怎么用啊？https://www.cplusplus.com/reference/algorithm/unique/?kw=unique
```C++
// unique algorithm example
#include <iostream>     // std::cout
#include <algorithm>    // std::unique, std::distance
#include <vector>       // std::vector

bool myfunction (int i, int j) {
  return (i==j);
}

int main () {
  int myints[] = {10,20,20,20,30,30,20,20,10};           // 10 20 20 20 30 30 20 20 10
  std::vector<int> myvector (myints,myints+9);

  // using default comparison:
  std::vector<int>::iterator it;
  it = std::unique (myvector.begin(), myvector.end());   // 10 20 30 20 10 ?  ?  ?  ?
                                                         //                ^

  myvector.resize( std::distance(myvector.begin(),it) ); // 10 20 30 20 10

  // using predicate comparison:
  std::unique (myvector.begin(), myvector.end(), myfunction);   // (no changes)

  // print out content:
  std::cout << "myvector contains:";
  for (it=myvector.begin(); it!=myvector.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
```

来抄作业
```C++
//V 0.2
class Solution{
	int removeDuplicates(vector<int>& nums){
	// Step 0 确定输入输出
	int result；
	//Step 1 去重
	std::vector<int>::iterator it;
	it = std::unique(nums.begin(), nums.end())；// 此时，it指向了未删除的最后一位
    //Step 2 求距离
    distance（头，新的尾）；
    // Step 0.1 返回结果
    result = distance（）；
    return result；
	}
}

```

distance怎么用，参考https://en.cppreference.com/w/cpp/iterator/distance，因为cplusplus的广告有点多啊。
```C++
#include <iostream>
#include <iterator>
#include <vector>
 
int main() 
{
    [std::vector](http://en.cppreference.com/w/cpp/container/vector)<int> v{ 3, 1, 4 };
    [std::cout](http://en.cppreference.com/w/cpp/io/cout) << "distance(first, last) = "
              << std::distance(v.begin(), v.end()) << '\n'
              << "distance(last, first) = "
              << std::distance(v.end(), v.begin()) << '\n';
               //the behavior is undefined (until C++11)
 
    static constexpr auto il = { 3, 1, 4 };
    // Since C++17 `distance` can be used in constexpr context.
    static_assert(std::distance(il.begin(), il.end()) == 3);
    static_assert(std::distance(il.end(), il.begin()) == -3);
}
```

```C++
// V 0.3
class Solution{
	int removeDuplicates(vector<int>& nums){
	// Step 0 确定输入输出
	int result；
	//Step 1 去重
	std::vector<int>::iterator it;
	it = std::unique(nums.begin(), nums.end())；// 此时，it指向了未删除的最后一位
    //Step 2 求距离
    distance（nums.begin()，it）；
    // Step 0.1 返回结果
    result = distance（）；
    return result；
	}
}

```

上leetcode后修改了下
```C++
// V 0.4
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	// Step 0 确定输入输出
	int result;
	//Step 1 去重
	std::vector<int>::iterator it;
	it = std::unique(nums.begin(), nums.end());// 此时，it指向了未删除的最后一位
    //Step 2 求距离
    result = distance(nums.begin(),it);
    // Step 0.1 返回结果
    return result;
	}
};
```

代码有点长的啊，是否可以合并同类项
result有点多余啊
```C++
// V 0.5
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
	// Step 0 确定输入输出
	// int result;   //被消除了
	//Step 1 去重
	std::vector<int>::iterator it;
	it = std::unique(nums.begin(), nums.end());// 此时，it指向了未删除的最后一位
    //Step 2 求距离后，直接return distance
    return distance(nums.begin(),it);
    }
};
```

it是一个中间变量是否也可以消除掉？
```C++
// V 0.6
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
    //Step 1 去重
	//std::vector<int>::iterator it;
	//it = std::unique(nums.begin(), nums.end());// 此时，it指向了未删除的最后一位
    //Step 2 求距离后，直接return distance
    return distance(nums.begin(),std::unique(nums.begin(), nums.end()););
    }
};
```

7s一下
```C++
// V 0.7
class Solution{
public:
	int removeDuplicates(vector<int>& nums){
    return distance(nums.begin(),std::unique(nums.begin(), nums.end()));
    }
};
```

冲，提交！
代码短了，但是时间长了
Runtime: 23 ms, faster than 28.38% of C++ online submissions for Remove Duplicates from Sorted Array.

Memory Usage: 18.4 MB, less than 74.65% of C++ online submissions for Remove Duplicates from Sorted Array.

Next challenges:



[Remove Element](https://leetcode.com/problems/remove-element/)[Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)





