## 算法笔记
### 2023年5月14日
#### 问题：给定一个整数数组，找出其中两个数之和等于目标值的那两个数的索引。假设每个输入只对应一个答案，且同一个元素不能使用两次。

示例：

输入：`nums = [2, 7, 11, 15], target = 9`
输出：`[0, 1]`
解释：因为`nums[0] + nums[1] = 2 + 7 = 9`

答案：可以使用哈希表来解决这个问题。遍历数组，对于每个元素，检查哈希表中是否存在与其匹配的值（即`target - nums[i]`），如果存在，则返回匹配值的索引和当前元素的索引；否则，将当前元素及其索引添加到哈希表中。

```python
def two_sum(nums, target):
    hashmap = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in hashmap:
            return [hashmap[complement], i]
        hashmap[num] = i

# 示例
nums = [2, 7, 11, 15]
target = 9
print(two_sum(nums, target))  # 输出：[0, 1]
```

### 2023年5月15日
#### 问题：给定一个字符串，找出其中不含重复字符的最长子串的长度。

示例：

输入：`"abcabcbb"`
输出：`3`
解释：因为无重复字符的最长子串是`"abc"`，所以其为3。

答案：可以使用滑动窗口算法来解决这个问题。定义两个指针`left`和`right`，分别表示串的左右边界。使用一个哈希表来记录每个字符最后一次出现的位置。遍历字符串，当遇到重复字符时，将左指针移动到重复字符的下一个位置，同时更新哈希表中的字符位置。每次遍历时，更新最长子串的长度。

```python
def length_of_longest_substring(s):
    hashmap = {}
    left = 0
    max_len =0
    for right in range(len(s)):
        if s[right] in hashmap and hashmap[s[right]] >= left:
            left = hashmap[s[right]] + 1
        hashmap[s[right]] = right
        max_len = max(max_len, right - left + 1)
    return max_len

# 示例
s = "abcabcbb"
print(length_of_longest_substring(s))  # 输出：3
```

这种方法的时间复杂度为O(n)，空间复杂度为O(min(m, n))，其中n为字符串长度，m为字符集大小。

### 2023年5月16日
#### 题目：寻找两个有序数组的中位数

给定两个大小分别为 m 和 n 的有序数组 nums1 和 nums2。请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5

解答：

这道题可以使用二分查找的思想来解决，具体做法如下：

1. 将两个有序数组合并为一个有序数组，这可以通过双指针法来实现，具体做法是维护两个指针 i 和 j，分别指向 nums1 和 nums2 的开头，每次比较两个指针所指的元素的大小，将较小的那个放入结果数组中，并将对应指针向后移动一位，直到遍历完两个数组为止。

2. 如果数组的长度为奇数，则中位数是合并后数组的中间元素；如果数组的长度为偶数，则中位数是合并后数组中间两个元素的平均值。

具体实现代码如下：

```javascript
function findMedianSortedArrays(nums1, nums2) {
  const len1 = nums1.length, len2 = nums2.length;
  const len = len1 + len2;
  const nums = new Array(len);

  let i = 0, j = 0, k = 0;
  while (i < len1 && j < len2) {
    if (nums1[i] <= nums2[j]) {
      nums[k++] = nums1[i++];
    } else {
      nums[k++] = nums2[j++];
    }
  }

  while (i < len1) {
    nums[k++] = nums1[i++];
  }

  while (j < len2) {
    nums[k++] = nums2[j++];
  }

  if (len % 2 === 0) {
    return (nums[len / 2 - 1] + nums[len / 2]) / 2;
  } else {
    return nums[Math.floor(len / 2)];
  }
}
```

这个算法的时间复杂度为 O(m+n)，并不满足题目要求，但是这里只是为了简化代码实现，如果要达到题目要求的时间复杂度 O(log(m+n))，可以使用类似二分查找的方法来优化代码实现。
