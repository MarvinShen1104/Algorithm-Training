# 前k个高频元素

## 347. Top K Frequent Elements

题目链接：[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/)

视频讲解：[优先级队列正式登场！大顶堆、小顶堆该怎么用？| LeetCode：347.前 K 个高频元素_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Xg41167Lz/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

### My Solution

#### Solution 1

用HashMap记录每个数以及其频率，用set（用来频率去重）和list（由大到小倒序排列频率）来记录频率，再从map中获取前k个最大频率的数

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // 结果数组集
        int[] res = new int[k];
        // 用map记录每个数以及其频率
        HashMap<Integer, Integer> storeMap = new HashMap<>();
        // set用来从map中记录频率，并去掉重复的
        HashSet<Integer> freqs = new HashSet<>();
        // list从set中记录频率，方便后续排序
        ArrayList<Integer> freqList = new ArrayList<>();
        // 遍历数组，构建map
        for (int i: nums) {
            if (!storeMap.containsKey(i)) {
                storeMap.put(i, 1);
            } else {
                storeMap.put(i, storeMap.get(i) + 1);
            }
        }
        // 从map中获取数据，构建set
        for (Map.Entry<Integer, Integer> e: storeMap.entrySet()) {
            freqs.add(e.getValue());
        }
        // 从set中获取数据，构建list
        for (Integer i: freqs) {
            freqList.add(i);
        }
        // 对list进行倒序排序(频率由大到小)
        Collections.sort(freqList);
        Collections.reverse(freqList);
        // 遍历list，选取前k个频率最大的数
        for (int i=0, index=0; i<freqList.size()&&index<k; i++) {
            for (Map.Entry<Integer, Integer> e: storeMap.entrySet()) {
                if (e.getValue() == freqList.get(i)) {
                    res[index++] = e.getKey();
                }
            }
        }
        return res;
    }
}
```

### Standard Solution

可以学习下大/小顶堆的Java写法

#### Solution 1

```java
/*Comparator接口说明:
 * 返回负数，形参中第一个参数排在前面；返回正数，形参中第二个参数排在前面
 * 对于队列：排在前面意味着往队头靠
 * 对于堆（使用PriorityQueue实现）：从队头到队尾按从小到大排就是最小堆（小顶堆），
 *                                从队头到队尾按从大到小排就是最大堆（大顶堆）--->队头元素相当于堆的根节点
 * */
class Solution {
    //解法1：基于大顶堆实现
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();//key为数组元素值,val为对应出现次数
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        //在优先队列中存储二元组(num,cnt),cnt表示元素值num在数组中的出现次数
        //出现次数按从队头到队尾的顺序是从大到小排,出现次数最多的在队头(相当于大顶堆)
        PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2)->pair2[1]-pair1[1]);
        for(Map.Entry<Integer,Integer> entry:map.entrySet()){//大顶堆需要对所有元素进行排序
            pq.add(new int[]{entry.getKey(),entry.getValue()});
        }
        int[] ans = new int[k];
        for(int i=0;i<k;i++){//依次从队头弹出k个,就是出现频率前k高的元素
            ans[i] = pq.poll()[0];
        }
        return ans;
    }
}
```

#### Solution 2

```java
class Solution {  
    //解法2：基于小顶堆实现
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();//key为数组元素值,val为对应出现次数
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        //在优先队列中存储二元组(num,cnt),cnt表示元素值num在数组中的出现次数
        //出现次数按从队头到队尾的顺序是从小到大排,出现次数最低的在队头(相当于小顶堆)
        PriorityQueue<int[]> pq = new PriorityQueue<>((pair1,pair2)->pair1[1]-pair2[1]);
        for(Map.Entry<Integer,Integer> entry:map.entrySet()){//小顶堆只需要维持k个元素有序
            if(pq.size()<k){//小顶堆元素个数小于k个时直接加
                pq.add(new int[]{entry.getKey(),entry.getValue()});
            }else{
                if(entry.getValue()>pq.peek()[1]){//当前元素出现次数大于小顶堆的根结点(这k个元素中出现次数最少的那个)
                    pq.poll();//弹出队头(小顶堆的根结点),即把堆里出现次数最少的那个删除,留下的就是出现次数多的了
                    pq.add(new int[]{entry.getKey(),entry.getValue()});
                }
            }
        }
        int[] ans = new int[k];
        for(int i=k-1;i>=0;i--){//依次弹出小顶堆,先弹出的是堆的根,出现次数少,后面弹出的出现次数多
            ans[i] = pq.poll()[0];
        }
        return ans;
    }
}
```

