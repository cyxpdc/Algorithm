- 问题

在一个无序数组中，有正有负有0，给定一个整数aim，求累加和为aim的最长子数组的长度

- 解析

getMaxLength1:得到以每个数组元素为结尾的所有最长子数组，再比较谁最长，答案就是谁

使用map，key为以当前元素为结尾时的数组和sum，value为此元素的位置

getMaxLength2（此方法假设数组全为正数，可以使空间复杂度变为O（1））：双指针L和R，sum小于等于（等于的时候L右移也行，因为都是正数，所以如果L右，sum比aim大，下一次L必往右；如果L右，sum比aim小，下一次R必往右，得到的数组相同）aim，R右移，等于时查看是否更新max；sum大于，L右移，缩小sum。R到尾时结束

```java
public class LongestSumSubArrayLength {
    public static int getMaxLength1(int[] arr, int k) {
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

    public static int getMaxLength2(int[] arr, int aim) {
        if (arr == null || arr.length == 0 || aim <= 0) {
            return 0;
        }
        int L = 0;
        int R = 0;
        int sum = arr[0];
        int len = 0;
        while (R < arr.length) {
            if (sum == aim) {//大于等于L右移
                len = Math.max(len, R - L + 1);
                sum -= arr[L++];//出窗口的数
            } else if (sum < aim) {
                R++;
                if (R == arr.length) {//保证R不越界，避免下面入窗口操作出错
                    break;
                }
                sum += arr[R];//入窗口
            } else {
                sum -= arr[L++];//出窗口
            }
        }
        return len;
    }
}
```
