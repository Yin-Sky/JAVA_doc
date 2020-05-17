# 数组中重复的数字

**题目描述**

找出数组中重复的数字。

在一个长度为 n 的数组nums里的所有数字都在0～n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：

[2, 3, 1, 0, 2, 5, 3]

输出：2 或 3 

**解题思路**

如果没有重复数字，那么正常排序后，数字i应该在下标为i的位置，所以思路是重头扫描数组，遇到下标为i的数字如果不是i的话，（假设为m),那么我们就拿与下标m的数字交换。在交换过程中，如果有重复的数字发生，那么终止返回ture

**参考代码**

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int temp;
        for(int i=0;i<nums.length;i++){
            while (nums[i]!=i){
                if(nums[i]==nums[nums[i]]){
                    return nums[i];
                }
                temp=nums[i];
                nums[i]=nums[temp];
                nums[temp]=temp;
            }
        }
        return -1;
    }
}

```

> [2, 3, 1, 0, 2, 5, 3]	nums[i]!=nums[nums[i]] 不等就交换
>
> [1, 3, 2, 0, 2, 5, 3]
>
> [1, 0, 2, 3, 2, 5, 3]	nums[i]==i，所以while不执行
>
> [1, 0, 2, 3, 2, 5, 3]	nums[i]==i，所以while不执行
>
> [1, 0, 2, 3, 2, 5, 3]	找到了！
>
> [1, 0, 2, 3, 2, 5, 3]	nums[i]==i，所以while不执行
>
> [1, 0, 2, 3, 2, 5, 3]	找到了！

**总结**

1. 遍历数组；

2. 下标为i的数等于i，那么不执行；

3. 下标为i的数不等于i，检查下标为i的数是否等于下标为 “下标为i的数 ”的数（有点绕）。如果等于，那我们找到了；

4. 如果不等于，交换他们。


参考链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/yuan-di-zhi-huan-shi-jian-kong-jian-100-by-derrick/

# 二维数组中的查找

**题目描述**

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数

实例

现有矩阵 matrix 如下：

[ [1,  4,  7, 11, 15],

 [2,  5,  8, 12, 19],

 [3,  6,  9, 16, 22],

 [10, 13, 14, 17, 24],

 [18, 21, 23, 26, 30]]

**解题思路**

> 若使用暴力法遍历矩阵 matrix ，则时间复杂度为 O(N*M)O(N∗M) 。暴力法未利用矩阵 “从上到下递增、从左到右递增” 的特点，显然不是最优解法。
>
> 本题解利用矩阵特点引入标志数，并通过标志数性质降低算法时间复杂度。 

标志数引入： 此类矩阵中左下角和右上角元素有特殊性，称为标志数。 

- 左下角元素： 为所在列最大元素，所在行最小元素。

- 右上角元素： 为所在行最大元素，所在列最小元素。

标志数性质： 将 matrix 中的左下角元素（标志数）记作 flag ，则有: 

1. 若 flag > target ，则 target 一定在 flag 所在行的上方，即 flag 所在行可被消去。

2. 若 flag < target ，则 target 一定在 flag 所在列的右方，即 flag 所在列可被消去。

- 本题解以左下角元素为例，同理，右上角元素 也具有行（列）消去的性质。

算法流程： 根据以上性质，设计算法在每轮对比时消去一行（列）元素，以降低时间复杂度。 

1. 从矩阵 matrix 左下角元素（索引设为 (i, j) ）开始遍历，并与目标值对比：

- 当 matrix[i][j] > target 时： 行索引向上移动一格（即 i--），即消去矩阵第 i 行元素；

- 当 matrix[i][j] < target 时： 列索引向右移动一格（即 j++），即消去矩阵第 j 列元素；

- 当 matrix[i][j] == target 时： 返回 truetrue 。

2. 若行索引或列索引越界，则代表矩阵中无目标值，返回 falsefalse 。

> 算法本质：每轮 i 或 j 移动后，相当于生成了“消去一行（列）的新矩阵”， 索引(i,j) 指向新矩阵的左下角元素（标志数），因此可重复使用以上性质消去行（列）。

**复杂度分析**

时间复杂度 O(M+N)O(M+N) ：其中，NN 和 MM 分别为矩阵行数和列数，此算法最多循环 M+NM+N 次。

空间复杂度 O(1)O(1) : i, j 指针使用常数大小额外空间。

**参考代码**

PYTHON

```python
class Solution:
    def findNumberIn2DArray(self, matrix: List[List[int]], target: int) -> bool:
        i, j = len(matrix) - 1, 0
        while i >= 0 and j < len(matrix[0]):
            if matrix[i][j] > target: i -= 1
            elif matrix[i][j] < target: j += 1
            else: return True
        return False
```

JAVA

```JAVA
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        int i = matrix.length - 1, j = 0;
        while(i >= 0 && j < matrix[0].length)
        {
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else return true;
        }
        return false;
    }
}
```


参考链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/mian-shi-ti-04-er-wei-shu-zu-zhong-de-cha-zhao-zuo/

# 替换空格

https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/

**题目描述**

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例 1：

输入：s = "We are happy."

输出："We%20are%20happy."

**解题思路**

1. 初始化一个 StringBuilder ，记为 res ；

2. 遍历字符串 s 中的每个字符 c ：

- 当 c 为空格时：向 res 后添加字符串 "%20"；

- 当 c 不为空格时：向 res 后添加字符 c ；

3. 将 res 转化为 String 类型并返回。

**参考代码**

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder();
        for(Character c : s.toCharArray())
        {
            if(c == ' ') res.append("%20");
            else res.append(c);
        }
        return res.toString();
    }
}


```

 参考链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/solution/mian-shi-ti-05-ti-huan-kong-ge-ji-jian-qing-xi-tu-/