# 排序

| 题目                                                         | 题解                                                         | 难度 | 标签 | 时间      | 备注                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- | ---- | --------- | ---------------------------------------------- |
| [912. 排序数组](https://leetcode-cn.com/problems/sort-an-array/) | [912. 排序数组](#912-排序数组)                               | 中等 |      | 2022-3-22 | 手撕快速排序<br />手撕堆排序<br />手撕归并排序 |
| [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals) | [56. 合并区间](#56-合并区间)                                 | 中等 |      | 2022-3-22 |                                                |
| [面试题 17.14. 最小K个数](https://leetcode-cn.com/problems/smallest-k-lcci/) | [面试题 17.14. 最小K个数](#最小K个数)                        | 中等 |      | 2022-3-22 |                                                |
| [179. 最大数](https://leetcode-cn.com/problems/largest-number) | [179. 最大数](#179-最大数)                                   | 中等 |      | 2022-3-22 |                                                |
| [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors) | [75. 颜色分类](#75-颜色分类)                                 | 中等 |      | 2022-3-22 |                                                |
| [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays) | [349. 两个数组的交集](#349-两个数组的交集)                   | 简单 |      | 2022-3-23 |                                                |
| [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof) | [剑指 Offer 45. 把数组排成最小的数](#45-把数组排成最小的数)  | 中点 |      | 2022-3-23 |                                                |
| [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram) | [242. 有效的字母异位词](#242-有效的字母异位词)               | 简单 |      | 2022-3-23 |                                                |
| [315. 计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/) | [315. 计算右侧小于当前元素的个数](#315-计算右侧小于当前元素的个数) | 困难 |      | 2022-3-24 |                                                |
| [148. 排序链表](https://leetcode-cn.com/problems/sort-list)  | [148. 排序链表](#148-排序链表)                               | 中点 |      | 2022-3-24 |                                                |

## 912. 排序数组

这道题可以考察手撕各种排序，目前先写了几种比较常考的。

### 快速排序

每次确定一个数的最终位置，即把该区间内所有比`pivot`的放在它前面，比`pivot`大的放在它后面。为了避免最坏情况，`pivot`随机选取。

```Java
public int[] sortArray(int[] nums) {
    if (nums.length < 2) return nums;
    process(nums, 0, nums.length - 1);
    return nums;
}

public void process(int[] nums, int left, int right) {
    if (left >= right) return;
    int rand = new Random().nextInt(right - left + 1) + left;
    swap(nums, right, rand);
    int loc = partition(nums, left, right);
    process(nums, left, loc - 1);
    process(nums, loc + 1, right);
}

public int partition(int[] nums, int left, int right) {
    int pivot = nums[right];
    int index = left - 1;
    for (int i = left; i < right; i++) {
        if (nums[i] <= pivot) {
            index++;
            swap(nums, i, index);
        }
    }
    swap(nums, index + 1, right);
    return index + 1;
}
```

### 堆排序

很适合用来做`topK`问题

```Java
public int[] sortArrayHeap(int[] nums) {
    int[] heap = new int[nums.length];
    for (int i = 0; i < nums.length; i++) {
        heapInsert(heap, nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        swap(heap, 0, nums.length - 1 - i);
        heapify(heap, nums.length - 2 - i);
    }
    return heap;
}

public void heapInsert(int[] nums, int num, int len) {
    nums[len] = num;
    while (len > 0 && nums[len] > nums[(len - 1) / 2]) {
        swap(nums, len, (len - 1) / 2);
        len = (len - 1) / 2;
    }
}

public void heapify(int[] nums, int len) {
    int cur = 0;
    int left = cur * 2 + 1;
    int right = cur * 2 + 2;
    while (left <= len) {
        int maxIndex = nums[cur] < nums[left] ? left : cur;
        maxIndex = right <= len && nums[maxIndex] < nums[right] ? right : maxIndex;
        if (cur == maxIndex) break;
        swap(nums, cur, maxIndex);
        cur = maxIndex;
        left = cur * 2 + 1;
        right = cur * 2 + 2;
    }
}
```

### 归并排序

```Java
public int[] sortArrayMerge(int[] nums) {
    mergeSort(nums, 0, nums.length - 1);
    return nums;
}

public void mergeSort(int[] nums, int left, int right) {
    if (left >= right) return;
    int mid = left + ((right - left) >> 1);
    mergeSort(nums, left, mid);
    mergeSort(nums, mid + 1, right);
    merge(nums, left, right);
}

public void merge(int[] nums, int left, int right) {
    int mid = left + ((right - left) >> 1);
    int i = left, j = mid + 1;
    int[] cpy = new int[right - left + 1];
    int index = 0;
    while (i <= mid && j <= right) {
        if (nums[i] < nums[j]) {
            cpy[index++] = nums[i++];
        } else {
            cpy[index++] = nums[j++];
        }
    }
    if (i > mid) {
        while (index <= right - left) cpy[index++] = nums[j++];
    } else if (j > right) {
        while (index <= right - left) cpy[index++] = nums[i++];
    }
    for (int k = left; k <= right; k++) {
        nums[k] = cpy[k - left];
    }
}
```

## 56. 合并区间

从左往右遍历并更新`left`和`right`

```Java
public int[][] merge(int[][] intervals) {
        List<int[]> ans = new ArrayList<>();
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if (o1[0] == o2[0]) return o1[1] - o2[1];
                return o1[0] - o2[0];
            }
        });
        int right = 0, left = intervals[0][0];
        for (int i = 0; i < intervals.length; i++) {
            if (intervals[i][0] > right) {
                ans.add(new int[]{left, right});
                left = intervals[i][0];
                right = intervals[i][1];
            } else {
                right = Math.max(right, intervals[i][1]);
            }
        }
        int[][] res = new int[ans.size()][2];
        for (int i = 0; i < ans.size(); i++) res[i] = ans.get(i);
        return res;
    }
```

## 最小K个数

堆排序，维护小顶堆，`heapify`进行`K`次

```Java
public int[] smallestK(int[] arr, int k) {
        int[] heap = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            heapInsert(heap, i, arr[i]);
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = heap[0];
            swap(heap, 0, heap.length - 1 - i);
            heapify(heap, heap.length - 2 - i);
        }
        return res;
    }

    public void heapInsert(int[] heap, int len, int num) {
        heap[len] = num;
        while (len > 0 && heap[len] < heap[(len - 1) / 2]) {
            swap(heap, len, (len - 1) / 2);
            len = (len - 1) / 2;
        }
    }

    public void heapify(int[] heap, int len) {
        int left = 1, cur = 0;
        while (left <= len) {
            int minIndex = heap[left] < heap[cur] ? left : cur;
            minIndex = left + 1 <= len && heap[minIndex] > heap[left + 1] ? left + 1 : minIndex;
            if (cur == minIndex) break;
            swap(heap, cur, minIndex);
            cur = minIndex;
            left = minIndex * 2 + 1;
        }
    }

    public void swap(int[] nums, int a, int b) {
        int tmp = nums[a];
        nums[a] = nums[b];
        nums[b] = tmp;
    }
```

## 179. 最大数

自定义排序，比较策略是`o1+o2`和`o2+o1`去比较。

我傻逼地进行了很多细节的比较最后都有问题。

再判断一下是否为`0`

```Java
public String largestNumber(int[] nums) {
    String[] strs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) strs[i] = String.valueOf(nums[i]);
    StringBuilder res = new StringBuilder();
    Arrays.sort(strs, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return (o2 + o1).compareTo(o1 + o2);
        }
    });
    for (int i = 0; i < strs.length; i++) {
        res.append(strs[i]);
    }
    return res.charAt(0) == '0' ? "0" : res.toString();
}
```

## 75. 颜色分类

`partition`问题

```Java
public void sortColors(int[] nums) {
    int left = -1;
    int right = nums.length;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 0) {
            swap(nums, i, left++);
        } else if (nums[i] == 2) {
            swap(nums, i--, right--);
        }
    }
}
```

## 349. 两个数组的交集

排序之后双指针

```Java
public int[] intersection(int[] nums1, int[] nums2) {
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    int i = 0, j = 0;
    List<Integer> res = new ArrayList<>();
    int prev = -1;
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] == nums2[j] && nums1[i] != prev) {
            res.add(nums1[i]);
            prev = nums1[i];
            i++;
            j++;
        } else if (nums1[i] < nums2[j]) {
            i++;
        } else if (nums1[i] > nums2[j]) {
            j++;
        }
    }
    int[] ans = new int[res.size()];
    for (i = 0; i < res.size(); i++) ans[i] = res.get(i);
    return ans;
}
```

## 45. 把数组拍成最小的数

和[179.最大数](#179-最大数)一样

记得要防一手零开头

```Java
public String largestNumber(int[] nums) {
    String[] strs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) strs[i] = String.valueOf(nums[i]);
    StringBuilder res = new StringBuilder();
    Arrays.sort(strs, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            return (o2 + o1).compareTo(o1 + o2);
        }
    });
    for (int i = 0; i < strs.length; i++) {
        res.append(strs[i]);
    }
    return res.charAt(0) == '0' ? "0" : res.toString();
}
```

## 242. 有效的字母异位词

排序之后一个一个比对

```Java
 public boolean isAnagram(String s, String t) {
     char[] ss = s.toCharArray();
     char[] tt = t.toCharArray();
     Arrays.sort(ss);
     Arrays.sort(tt);
     if (s.length() != t.length()) return false;
     for (int i = 0; i < s.length(); i++) if (ss[i] != tt[i]) return false;
     return true;
 }
```

## 315. 计算右侧小于当前元素的个数

用归并排序，每次归并的时候，是分区间的，在归并的时候对于左边每个位置加上右边比它大的数量就可以了。

不过过程中每个数的位置一直变化，所以要用一个`helper`数组来记录位置的变化

```Java
public List<Integer> countSmaller(int[] nums) {
    int[] counts = new int[nums.length];
    int[] index = new int[nums.length];
    for (int i = 0; i < nums.length; i++) index[i] = i;
    mergeSort(nums, counts, 0, nums.length - 1, index);
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < counts.length; i++) res.add(counts[i]);
    return res;
}

public void mergeSort(int[] nums, int[] counts, int left, int right, int[] index) {
    if (right - left < 1) return;
    int mid = left + ((right - left) >> 1);
    mergeSort(nums, counts, left, mid, index);
    mergeSort(nums, counts, mid + 1, right, index);
    merge(nums, counts, left, mid, right, index);
}

public void merge(int[] nums, int[] counts, int left, int mid, int right, int[] helper) {
    int[] cpy = new int[right - left + 1];
    int[] cpyIndex = new int[right - left + 1];
    int index = 0;
    int i = left, j = mid + 1;
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) {
            cpy[index] = nums[j];
            cpyIndex[index] = helper[j];
            j++;
        } else {
            cpy[index] = nums[i];
            cpyIndex[index] = helper[i];
            counts[helper[i]] += right - j + 1;
            i++;
        }
        index++;
    }
    for (; i <= mid; i++) {
        cpy[index] = nums[i];
        cpyIndex[index] = helper[i];
        index++;
    }
    for (; j <= right; j++) {
        cpy[index] = nums[j];
        cpyIndex[index] = helper[j];
        index++;
    }
    for (i = 0; i < index; i++) {
        nums[i + left] = cpy[i];
        helper[i + left] = cpyIndex[i];
    }
}
```

## 148. 排序链表

同样是归并排序，之前做过一次是自底向上的（迭代），这次做自顶向下的（递归）

一开始被每个区间在归并时和上一个区间和下一个区间的连接烦了好久，后面看到题解说**直接快慢指针找中点，把两个要处理的区间断出来，最后归并的时候会重新连接的。**

```java
public ListNode sortList(ListNode head) {
        return mergeSort(head);
    }

    public ListNode mergeSort(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode next = slow.next;
        slow.next = null;

        return merge(mergeSort(head), mergeSort(next));
    }

    public ListNode merge(ListNode l1, ListNode l2) {
        ListNode res = new ListNode();
        ListNode cur = res;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            } else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 == null ? l2 : l1;
        return res.next;
    }
```

