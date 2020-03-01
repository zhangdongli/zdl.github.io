# 数组中的K-diff数对
给定一个整数数组和一个整数 k, 你需要在数组里找到不同的 k-diff 数对。这里将 k-diff 数对定义为一个整数对 (i, j), 其中 i 和 j 都是数组中的数字，且两数之差的绝对值是 k.

**示例 1:**
```
输入: [3, 1, 4, 1, 5], k = 2
输出: 2
解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。
```
**示例 2:**
```
输入:[1, 2, 3, 4, 5], k = 1
输出: 4
解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。
```
**示例 3:**
```
输入: [1, 3, 1, 5, 4], k = 0
输出: 1
解释: 数组中只有一个 0-diff 数对，(1, 1)。
```
**注意:**
1. 数对 (i, j) 和数对 (j, i) 被算作同一数对。
2. 数组的长度不超过10,000。
3. 所有输入的整数的范围在 [-1e7, 1e7]。

# 解题思路
首先要理解题意，翻译成大白话，就是要从给定的数组里找出2数相减的绝对值等于k的数对，然后题目里说了数对(i, j)和数对(j, i)被算作同一数对，也就是说假设k=2时，那么3-5和5-3是同一数对。

首先我们要找出来这些绝对值等于k的数对，所以我们可以使用冒泡算法，将数组里的数两两相减，然后找出绝对值等于k的数对。

代码如下：
```java
//冒泡算法两两相比较
for (int i = 0; i < nums.length; i++){
       for(int j = i + 1; j < nums.length; j++){
               //两两相减的绝对值等于k
               if(Math.abs(nums[i] - nums[j]) == k){
              
               }
       }      
}
```
然后我们再想办法怎么排重，假设k=2时，3-5和5-3只保留一个即可，我想到的办法是利用哈希字典，将3-5和5-3当key保存到字典里，如果字典里有这样的数对了，就说明已经重复，可以丢掉。

完整代码如下：
```java
import java.util.HashMap;
import java.util.Map;

public class UuidTest {

    public static void main(String[] args){
        int[] nums = new int[]{3,1,4,1,5};
        System.out.println(findPairs(nums, 2));
    }

    public static int findPairs(int[] nums, int k) {


        //满足条件的数量
        int count = 0;
        
        //排重的字典
        String key1,key2;
        Map<String,String> diffMap = new HashMap<>();

        //冒泡算法两两相比较
        for (int i = 0; i < nums.length; i++){
            for(int j = i + 1; j < nums.length; j++){
                //两两相减的绝对值等于k
                if(Math.abs(nums[i] - nums[j]) == k){
                    
                    key1 = String.format("%d,%d", nums[i], nums[j]);
                    key2 = String.format("%d,%d", nums[j], nums[i]);
                    //如果数对不存在才认为满足
                    if(!diffMap.containsKey(key1) && !diffMap.containsKey(key2)) {
                        diffMap.put(key1, key2);
                        diffMap.put(key2, key1);
                        count++;
                    }
                }
            }
        }
        return count;
    }
}
```