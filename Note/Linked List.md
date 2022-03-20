# 链表

| 题目                                                         | 题解                                                       | 难度 | 标签     | 时间      | 备注 |
| ------------------------------------------------------------ | ---------------------------------------------------------- | ---- | -------- | --------- | ---- |
| [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list) | [206. 反转链表](#206-反转链表)                             | 简单 |          | 2022-3-19 |      |
| [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group) | [25. K 个一组翻转链表](#25-K个一组翻转链表)                | 困难 |          | 2022-3-19 |      |
| [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists) | [21. 合并两个有序链表](#21-合并两个有序链表)               | 简单 |          | 2022-3-19 |      |
| [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle) | [141. 环形链表](#141-环形链表)                             | 简单 | 快慢指针 | 2022-3-19 |      |
| [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists) | [160. 相交链表](#160-相交链表)                             | 简单 |          | 2022-3-19 |      |
| [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists) | [23. 合并K个排序链表](#23-合并K个排序链表)                 | 困难 |          | 2022-3-20 |      |
| [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii) | [92. 反转链表 II](#92-反转链表)                            | 中等 |          | 2022-3-20 |      |
| [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii) | [142. 环形链表 II](#142-环形链表)                          | 中等 |          | 2022-3-20 |      |
| [143. 重排链表](https://leetcode-cn.com/problems/reorder-list) | [143. 重排链表](#143-重排链表)                             | 中等 |          | 2022-3-20 |      |
| [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list) | [19. 删除链表的倒数第N个节点](#19-删除链表的倒数第N个节点) | 中等 |          | 2022-3-20 |      |

## 206. 反转链表

[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list)

迭代方式容易实现，不做过多说明。

```Java
public ListNode reverseList1(ListNode head) {
    if (head == null) return head;
    ListNode last = null;
    ListNode next = null;
    while (head.next != null) {
        next = head.next;
        head.next = last;
        last = head;
        head = next;
    }
    head.next = last;
    return head;
}
```

递归方式稍微抽象。

首先一直往最后一个节点递归，当`next`节点为`null`时，返回当前节点作为最终结果的头节点。

而在返回递归上一层的时候，将该节点的`next`的`next`指向自己，再将自己的`next`指向`null`。会有如下的效果。

![Snipaste_2022-03-19_09-48-05](../images/LinkedList/Snipaste_2022-03-19_09-48-05.png)

![Snipaste_2022-03-19_09-49-35](../images/LinkedList/Snipaste_2022-03-19_09-49-35.png)

将自己的`next`指向`null`是为了使原来的头节点最终指向`null`。

```Java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode ans = reverseList(head.next);
    head.next.next = head;
    // 以达到原来的头节点的next指向null
    head.next = null;
    return ans;
}
```



------

## 25. K个一组翻转链表

[25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group)

模拟题，没有什么奇妙的算法，只有亿些细节要注意。

- 翻转后，每个翻转块的第一个节点是原来的链表中的最后一个节点，而指向下一个翻转块的节点是原来的第一个节点。
- 先判断能否凑成一个K大小的翻转块，再进行翻转。
- 遇到不能凑成翻转块要返回结果的时候，记得把这剩下的也连上去。

```Java
public ListNode reverseKGroup(ListNode head, int k) {
    if(k==1) return head;
    ListNode last = null;
    ListNode next = null;
    ListNode ans = null;
    ListNode cur = head;
    ListNode hair = null;
    ListNode hairCpy = null;
    while(cur!=null){
        ListNode tmp = cur;
        last = null;
        for(int i=0;i<k;i++){
            if(tmp==null){
                if(ans==null) return head;
                hair.next = cur;
                return ans;
            }
            tmp = tmp.next;
        }
        for(int i=0;i<k;i++){
            next = cur.next;
            if(i==0) {
                hairCpy = cur;
            }else if(i==k-1){
                if(hair!=null) hair.next = cur;
                if(ans==null) ans = cur;
                hair = hairCpy;
            }
            cur.next = last;
            last = cur;
            cur = next;
        }
    }
    return ans;
}
```

我为我写出这和屎一样的代码感到羞愧，几乎就是缝缝补补。这边发现有问题就改一下，那边发现有问题改一下。到最后发现改着改着能通过了，但是代码却一点也看不懂了。注释也写不了了。

但是不想改了，开摆🥰



------

## 21. 合并两个有序链表

```Java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode ans = new ListNode();
    ListNode cur = ans;
    while (list1 != null && list2 != null) {
        if (list1.val < list2.val) {
            cur.next = list1;
            list1 = list1.next;
        } else {
            cur.next = list2;
            list2 = list2.next;
        }
        cur = cur.next;
    }
    cur.next = list1 == null ? list2 : list1;
    return ans.next;
}
```

------

## 141. 环形链表

快慢指针，`fast`每次走两步，`slow`每次走一步，如果有环就会遇到。

因为循环判断条件是`fast != slow`，所以初始化`fast`和slow`就`分别是各走一次的结果，那么还得判断一下特殊情况`head`或`head.next`为空。

```Java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) return false;
    ListNode fast = head.next.next;
    ListNode slow = head.next;
    while (fast != slow) {
        if (fast == null || fast.next == null) return false;
        fast = fast.next.next;
        slow = slow.next;
    }
    return true;
}
```

------

## 160. 相交链表

两个指针，当一个走到null时，指向另一个的头重新走。这样直到最后两个指针走的距离都是两个链表的长度之和，所以一定会相遇。

如果相遇时同时指向null，就是没有相交，否则就是相交在第一次相遇的节点。

```Java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode a = headA, b = headB;
    while (a != b) {
        a = a.next;
        b = b.next;
        if(a == b) return a;
        if (a == null) a = headB;
        if (b == null) b = headA;
    }
    return a;
}
```



## 23. 合并K个排序链表

用归并排序

```Java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists.length < 1) return null;
    else if (lists.length == 1) return lists[0];
    return mergeKLists(lists, 0, lists.length - 1);
}

public ListNode mergeKLists(ListNode[] lists, int left, int right) {
    if (left + 1 == right) {
        return merge(lists[left], lists[right]);
    } else if (left + 1 < right) {
        int mid = left + ((right - left) >> 1);
        return merge(mergeKLists(lists, left, mid), mergeKLists(lists, mid + 1, right));
    } else {
        return lists[left];
    }
}

public ListNode merge(ListNode l1, ListNode l2) {
    ListNode ans = new ListNode();
    ListNode res = ans;
    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            ans.next = l1;
            l1 = l1.next;
        } else {
            ans.next = l2;
            l2 = l2.next;
        }
        ans = ans.next;
    }
    ans.next = l1 == null ? l2 : l1;
    return res.next;
}
```



## 92. 反转链表

模拟，用一个辅助头节点来避免从第一个节点就开始翻转的情况

```Java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (left == right) return head;
    ListNode res = new ListNode();
    res.next = head;
    ListNode begin = res;
    for (int i = 0; i < left - 1; i++) {
        begin = begin.next;
    }
    ListNode cur = begin.next;
    ListNode last = null;

    for (int i = 0; i <= right - left; i++) {
        ListNode next = cur.next;
        cur.next = last;
        last = cur;
        cur = next;
    }
    begin.next.next = cur;
    begin.next = last;
    return res.next;
}
```

## 142. 环形链表

快慢指针确认有环，有环的情况下第一次相遇的时候，让其中一个指针重新指向头节点，然后接下来快慢指针都同时走一步，再次相遇就是环入口。

```Java
public ListNode detectCycle(ListNode head) {
    if(head==null||head.next==null) return null;
    ListNode slow = head.next;
    ListNode fast = head.next.next;
    while(fast!=slow){
        if(fast==null||fast.next==null) return null;
        fast = fast.next.next;
        slow = slow.next;
    }
    fast = head;
    while(fast!=slow){
        fast = fast.next;
        slow = slow.next;
    }
    return fast;
}
```

## 143. 重排链表

用栈按顺序压入所有节点，再重新遍历头节点一个一个接上去。

```Java
public void reorderListStack(ListNode head) {
    if (head.next == null) return;
    Stack<ListNode> stack = new Stack<>();
    ListNode cur = head;
    while (cur != null) {
        stack.push(cur);
        cur = cur.next;
    }
    cur = head;
    while (cur != stack.peek() && cur.next != stack.peek()) {
        // !!!
        ListNode order = stack.pop();
        ListNode next = cur.next;
        cur.next = order;
        order.next = next;
        cur = next;
    }
    cur = stack.pop();
    cur.next = null;
}
```

还有更优的办法，先用快慢指针把链表分成两半。然后对后面那一半进行链表反转，再合并两个链表。

这个做法要注意`next`值的链接，不然很容易出现接错成环了。

例如上面最后一个`while`循环，不仅要备份前半部分节点的`next`，还要备份后半部分节点的`next`。

## 19. 删除链表的倒数第N个节点

之前做过，之前的做法就是先找到倒数第N个节点，然后再重新遍历删掉这个节点。这种做法的好处是比较好操作删除节点为头节点的情况。

后面题意说进阶做法是用一次遍历的做法，其实和前一种做法大同小异。

```Java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode res = new ListNode();
    res.next = head;
    ListNode cur = res;
    ListNode helper = res;
    while (cur.next != null) {
        n--;
        if (n < 0) helper = helper.next;
        cur = cur.next;
    }
    helper.next = helper.next.next;
    return res.next;
}
```

不过要注意，最好用一个辅助头节点，也比较好操作。
