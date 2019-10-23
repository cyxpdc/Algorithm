- 问题

在一个无序数组中，有正有负有0，给定一个整数aim，求累加和为aim的最长子数组的长度

- 解析

得到以每个数组元素为结尾的所有最长子数组，再比较谁最长，答案就是谁

使用map，key为以当前元素为结尾时的数组和sum，value为此元素的位置

```java
public class LongestSumSubArrayLength {
    public static int maxLength(int[] arr, int k) {
    	if (arr == null || arr.length == 0) {
    	    return 0;
    	}
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1); //注意，这里不能是(0,0)，因为逻辑不符合
        int len = 0;
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            sum += arr[i];
            if (map.containsKey(sum - k)) {
                len = Math.max(i - map.get(sum - k), len);
            }
            if (!map.containsKey(sum)) {
                map.put(sum, i);
            }
        }
        return len;
    }
}
```
