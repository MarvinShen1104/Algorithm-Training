# 四数相加II

## 454. 4Sum II

题目链接：[454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/)

讲解视频：[学透哈希表，map使用有技巧！LeetCode：454.四数相加II_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Md4y1Q7Yh/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

### My Solution

#### Solution 1

没啥思路，看了将视频讲解后尝试实现

主要思路是将原本暴力解法的$O(n^4)$复杂度通过分别遍历nums1、nums2数组和nums3、nums4数组降为$O(n^4)$。将遍历完nums1和nums2数组后的和储存在哈希表中，key为和，value为出现次数（题目要求输出出现次数）。在遍历num3和nums4的时候，通过查找他们和的负数（题目要求四数相加为0）是否在哈希表中出现过来实现输出。

```java
public class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        // 存储计数结果
        int count = 0;
        // 存储nums1和nums2遍历之和以及其出现次数
        HashMap<Integer, Integer> storeMap = new HashMap<>();
        // 遍历nums1和nums2
        for (int i: nums1) {
            for (int j: nums2) {
                int sum = i + j;
                if (!storeMap.containsKey(sum)) {
                    storeMap.put(sum, 1);
                }
                else {
                    storeMap.put(sum, storeMap.get(sum)+1);
                }
            }
        }
        
        // 遍历num3和nums4
        for (int i: nums3) {
            for (int j: nums4) {
                // 查找nums3和nums4中两数之和的负数是否在Map中出现过
                int target = -(i + j);
                if (storeMap.containsKey(target)) {
                    // 若出现，计数累加target的出现次数
                    count += storeMap.get(target);
                }
            }
        }
        return count;
    }
}
```

### Standard Solution

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int temp;
        int res = 0;
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                temp = i + j;
                if (map.containsKey(temp)) {
                    map.put(temp, map.get(temp) + 1);
                } else {
                    map.put(temp, 1);
                }
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                temp = i + j;
                if (map.containsKey(0 - temp)) {
                    res += map.get(0 - temp);
                }
            }
        }
        return res;
    }
}
```

