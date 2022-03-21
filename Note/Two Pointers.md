# 双指针

| 题目                                                         | 题解                                         | 难度 | 标签 | 时间      | 备注 |
| ------------------------------------------------------------ | -------------------------------------------- | ---- | ---- | --------- | ---- |
| [15. 三数之和](https://leetcode-cn.com/problems/3sum)        | [15. 三数之和](#15-三数之和)                 | 中等 |      | 2022-3-21 |      |
| [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array) | [88. 合并两个有序数组](#88-合并两个有序数组) | 中等 |      | 2022-3-21 |      |
| [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water) | [42. 接雨水](#42-接雨水)                     | 困难 |      | 2022-3-21 |      |
| [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list) | [234. 回文链表](#234-回文链表)               | 简单 |      | 2022-3-21 |      |
| [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/) | [283. 移动零](#283-移动零)                   | 简单 |      | 2022-3-21 |      |



## 15. 三数之和

三指针，一个先不动，剩下两个动。剩下两个动完再动第一个。

记得去重

```Java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums.length < 3) return res;
    Arrays.sort(nums);
    int k = 0;
    while (k < nums.length - 2) {
        if (k > 0 && k < nums.length && nums[k] == nums[k - 1]) k++;
        if (k >= nums.length || nums[k] > 0) break;
        int i = k + 1, j = nums.length - 1;
        while (i < j) {
            int sum = nums[k] + nums[i] + nums[j];
            if (sum == 0) {
                List<Integer> tmp = new ArrayList<>();
                tmp.add(nums[k]);
                tmp.add(nums[i]);
                tmp.add(nums[j]);
                res.add(tmp);
            }
            if (sum >= 0) {
                i++;
                while (i < j && nums[i] == nums[i - 1]) i++;
            } else {
                j--;
                while (j > i && nums[j] == nums[j + 1]) j--;
            }
        }
        k++;
    }
    return res;
}
```

## 88. 合并两个有序数组

为了`O(1)`，就在原数组上操作了。

按照以往的想法，直接复制在`nums1`上面的话，会覆盖后面的值。

所以就从后面开始来，把比较大的加上去。这样无论如何都不会覆盖原数组。

```Java
public void merge(int[] nums1, int m, int[] nums2, int n){
    int i = m - 1, j = n - 1;
    int loc = nums1.length - 1;
    for (; loc >= 0; loc--) {
        if (i == -1) nums1[loc] = nums2[j--];
        else if (j == -1) nums1[loc] = nums1[i--];
        else if (nums1[i] > nums2[j]) nums1[loc] = nums1[i--];
        else if (nums1[i] <= nums2[j]) nums1[loc] = nums2[j--];
    }
}
```

## 42. 接雨水

记录`leftMax`和`rightMax`，两个指针分别指向数组开头和结尾，然后一步一步移动，移动策略如下。

- 先更新`leftMax`和`rightMax`
- 如果`height[left]>=height[right]`，说明`leftMax>rightMax`，记下在`right`这一列的雨水，然后`right`向左移一步。
- 如果`height[left]<height[right]`，同上，然后`left`向右移一步。

```Java
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int leftMax = height[left], rightMax = height[right];
    int ans = 0;
    while (left < right) {
        leftMax = Math.max(height[left], leftMax);
        rightMax = Math.max(height[right], rightMax);
        if (height[left] <= height[right]) {
            ans += rightMax - height[right--];
        } else {
            ans += leftMax - height[left++];
        }
    }
    return ans;
}
```



## 234. 回文链表

先用快慢指针找到链表中间，然后将后半段翻转。判断回文后再把后半部分链表反转回来。

```Java
public boolean isPalindrome(ListNode head){
    if (head == null || head.next == null) return true;
    ListNode fast = head.next.next;
    ListNode slow = head.next;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
    }
    ListNode last = slow;
    ListNode rightHead = null;
    while (slow != null) {
        ListNode next = slow.next;
        slow.next = last;
        last = slow;
        if (next == null) rightHead = slow;
        slow = next;
    }
    ListNode left = head, right = rightHead;
    boolean ans = true;
    if (left.val != right.val) ans = false;
    last = null;
    while (left != right && left.next != right) {
        left = left.next;
        ListNode next = right.next;
        right.next = last;
        last = right;
        right = next;
        if (left.val != right.val) ans = false;
    }

    return ans;
}
```

## 283. 移动零

这道题我小丑了，直接把所有非零放在开头，然后把剩下的置零就好了。

```Java
public void moveZeros(int[] nums) {
    int left = 0, right = 0;
    while (left < nums.length && right < nums.length) {
        while (left < nums.length && nums[left] != 0) left++;
        while (right < nums.length && nums[right] == 0) right++;
        if (left < right && left < nums.length && right < nums.length) {
            int tmp = nums[left];
            nums[left] = nums[right];
            nums[right] = tmp;
            left++;
        }
        right++;
    }
}
```

上面的是我的低能代码，后面发现这道题做过而且代码只有四五行。

```Java
public void moveZeroes(int[] nums){
    int index = 0;
    for(int i=0;i<nums.length;i++){
        if(nums[i]!=0) nums[index++] = nums[i];
    }
    for(int i=index;i<nums.length;i++) nums[i] = 0;
}
```

