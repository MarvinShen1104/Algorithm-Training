# 删除链表的倒数第N个节点

## 19. Remove Nth Node From End of List

题目链接：[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

视频讲解：[链表遍历学清楚！ | LeetCode：19.删除链表倒数第N个节点_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vW4y1U7Gf/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

### My Solution

```java
public class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode p = dummy;
        int size = 0; // 记录链表长度
        // 遍历链表计算链表长度
        while (p.next != null) {
            size++;
            p = p.next;
        }
        int index = size - n; // 要删除节点的index(从0计数)
        p = dummy;
        // 找到要删除节点的前一个节点
        for (int i=0; i<index; i++) {
            p = p.next;
        }
        p.next = p.next.next;
        return dummy.next;
    }
}
```

### Standard Solution

快慢指针，让二者相差n个节点，则当快指针指向null时，慢指针指向待删除节点

```java
public class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyNode = new ListNode(0);
        dummyNode.next = head;

        ListNode fastIndex = dummyNode;
        ListNode slowIndex = dummyNode;

        //只要快慢指针相差n个结点即可
        for (int i = 0; i < n; i++) {
            fastIndex = fastIndex.next;
        }

        // 为了得到待删除节点的前一个节点，当fast.next==null时就退出循环
        while (fastIndex.next != null) {
            fastIndex = fastIndex.next;
            slowIndex = slowIndex.next;
        }

        //此时 slowIndex 的位置就是待删除元素的前一个位置。
        //具体情况可自己画一个链表长度为 3 的图来模拟代码来理解
        slowIndex.next = slowIndex.next.next;
        return dummyNode.next;
    }
}
```

