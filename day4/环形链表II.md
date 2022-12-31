# 环形链表II

## 142. Linked List Cycle II

题目链接：[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/)

视频讲解：[把环形链表讲清楚！ 如何判断环形链表？如何找到环形链表的入口？ LeetCode：142.环形链表II_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1if4y1d7ob/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

### My Solution

#### Solution 1

用HashMap记录节点

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Map<ListNode, Integer> storeMap = new HashMap<>();
        ListNode p = head;
        while (p != null) {
            if (storeMap.get(p) == null) {
                storeMap.put(p, 1);
                p = p.next;
            }
            else {
                return p;
            }
        }
        return null;
    }
}
```

#### Solution 2

看了视频讲解后尝试实现

运用快慢指针，快指针一次走2步，慢指针一次走1步。

判断是否有环：当有环时，快慢指针遍历链表时一定会相遇，因为快指针相对于慢指针走1步，因此不存在快指针跳过慢指针的情况；若快指针一次走3步，想对于慢指针走2步，则有可能在绕圈时跳过慢指针而不相遇。

确定环的起始节点：视频有详细讲解[把环形链表讲清楚！ 如何判断环形链表？如何找到环形链表的入口？ LeetCode：142.环形链表II_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1if4y1d7ob/?vd_source=304d6ccf436c6c746de182aef4e17fe6)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) break;
        }
        if (fast == slow) {
            ListNode p = head;
            while (p != slow) {
                p = p.next;
                slow = slow.next;
            }
            return p;
        }
        return null;
    }
}

```

### Standard Solution 

快慢指针

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {// 有环
                ListNode index1 = fast;
                ListNode index2 = head;
                // 两个指针，从头结点和相遇结点，各走一步，直到相遇，相遇点即为环入口
                while (index1 != index2) {
                    index1 = index1.next;
                    index2 = index2.next;
                }
                return index1;
            }
        }
        return null;
    }
}
```

