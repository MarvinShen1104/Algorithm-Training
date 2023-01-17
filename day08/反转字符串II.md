# 反转字符串II

## 344. Reverse String II

题目链接：[541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/)

视频讲解：[字符串操作进阶！ | LeetCode：541. 反转字符串II_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1dT411j7NN/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

### My Solution

#### Solution 1

使用两个嵌套循环解决

```java
class Solution {
    public String reverseStr(String s, int k) {
        // 用StringBuffer来存储结果输出
        StringBuffer res = new StringBuffer();
        // 遍历数组，step=2*k
        for (int i=0; i<s.length(); i+=2*k) {
            // 每2k个数，反转前k个数
            res.append(reverseWindow(s, i, k));
            // 剩下的数按序加入buffer中
            for (int j=i+k; j<i+2*k && j<s.length(); j++) {
                res.append(s.charAt(j));
            }
        }
        return res.toString();
    }
    public String reverseWindow(String s, int start, int k) {
        StringBuffer sb = new StringBuffer();
        for (int i=start; i<start+k && i<s.length(); i++) {
            sb.append(s.charAt(i));
        }
        return sb.reverse().toString();
    }
}
```

### Standard Solution

#### Solution 1

```java
//解法一
class Solution {
    public String reverseStr(String s, int k) {
        StringBuffer res = new StringBuffer();
        int length = s.length();
        int start = 0;
        while (start < length) {
            // 找到k处和2k处
            StringBuffer temp = new StringBuffer();
            // 与length进行判断，如果大于length了，那就将其置为length
            int firstK = (start + k > length) ? length : start + k;
            int secondK = (start + (2 * k) > length) ? length : start + (2 * k);

            //无论start所处位置，至少会反转一次
            temp.append(s.substring(start, firstK));
            res.append(temp.reverse());

            // 如果firstK到secondK之间有元素，这些元素直接放入res里即可。
            if (firstK < secondK) { //此时剩余长度一定大于k。
                res.append(s.substring(firstK, secondK));
            }
            start += (2 * k);
        }
        return res.toString();
    }
}
```

#### Solution 2

```java
//解法二（似乎更容易理解点）
//题目的意思其实概括为 每隔2k个反转前k个，尾数不够k个时候全部反转
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0; i < ch.length; i += 2 * k){
            int start = i;
            //这里是判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1, start + k - 1);
            //用异或运算反转 
            while(start < end){
                ch[start] ^= ch[end];
                ch[end] ^= ch[start];
                ch[start] ^= ch[end];
                start++;
                end--;
            }
        }
        return new String(ch);
    }
}

// 解法二还可以用temp来交换数值，会的人更多些
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        for(int i = 0;i < ch.length;i += 2 * k){
            int start = i;
            // 判断尾数够不够k个来取决end指针的位置
            int end = Math.min(ch.length - 1,start + k - 1);
            while(start < end){
                
                char temp = ch[start];
                ch[start] = ch[end];
                ch[end] = temp;

                start++;
                end--;
            }
        }
        return new String(ch);
    }
}
```

#### Solution 3

```java
class Solution {
    public String reverseStr(String s, int k) {
        char[] ch = s.toCharArray();
        // 1. 每隔 2k 个字符的前 k 个字符进行反转
        for (int i = 0; i< ch.length; i += 2 * k) {
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= ch.length) {
                reverse(ch, i, i + k -1);
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转
            reverse(ch, i, ch.length - 1);
        }
        return  new String(ch);

    }
    // 定义翻转函数
    public void reverse(char[] ch, int i, int j) {
        for (; i < j; i++, j--) {
            char temp  = ch[i];
            ch[i] = ch[j];
            ch[j] = temp;
        }
    }
}
```

