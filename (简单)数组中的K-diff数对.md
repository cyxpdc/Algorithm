给定一个整数数组和一个整数 k, 你需要在数组里找到不同的 k-diff 数对。

这里将 k-diff 数对定义为一个整数对 (i, j), 其中 i 和 j 都是数组中的数字，且两数之差的绝对值是 k.

>示例 1:

输入: [3, 1, 4, 1, 5], k = 2

输出: 2

解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。

尽管数组中有两个1，但我们只应返回不同的数对的数量。

>示例 2:

输入:[1, 2, 3, 4, 5], k = 1

输出: 4

解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。

>示例 3:

输入: [1, 3, 1, 5, 4], k = 0

输出: 1

解释: 数组中只有一个 0-diff 数对，(1, 1)。

注意:

数对 (i, j) 和数对 (j, i) 被算作同一数对。

数组的长度不超过10,000。

所有输入的整数的范围在 [-1e7, 1e7]。

>https://leetcode.com/problems/k-diff-pairs-in-an-array/discuss/100098/Java-O(n)-solution-one-Hashmap-easy-to-understand
```java
public class Solution {
    public int findPairs(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k < 0) return 0;
        
        Map<Integer, Integer> map = new HashMap<>();
        int count = 0;
        for (int i : nums) {//记录每个数出现的次数
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (k == 0) {//若k为0，则每个出现两次的数都能成对
                if (entry.getValue() >= 2) {
                    count++;
                } 
            } else {//若不为0，则每个数与其加上k的数能成对
                if (map.containsKey(entry.getKey() + k)) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
