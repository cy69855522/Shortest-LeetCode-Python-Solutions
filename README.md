# Short-LeetCode-Python-Solutions
  Leet Code 刷题笔记 - - 不求最快最省，但求最短最优雅，Shorter is better here.

# 前言
- 代码精炼是 Python 的核心，同时能够反应对于语言的熟练程度，本博客目的在于汇总 leet code 最短最优雅的解法，拒绝长篇大论，缩短学习周期，掌握各种技巧，祝您在面试中刷三观，立鸡群，给考官留个好印象。
- 博文持续更新中，默认使用 python3，如果已编辑题目中有更短更优雅的解法欢迎联系我更新~  [直接在发issue 或 QQ1272068154  微信ly18597591102，记得留下署名和联系方式🧐]
- 由于CSDN博客更新需要人工审核比较慢，所以迁移到github上。
# 解析
默认已看过题目，没看过的话点标题可以跳转链接🤡
## [1. Two Sum 2行](https://leetcode.com/problems/two-sum/)

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {target - n: i for i, n in enumerate(nums)}
        return [[d[n], i] for i, n in enumerate(nums) if n in d and d[n] != i][0]
```
- O(N)时间效率的快速解法，用字典记录 需要的值:当前索引
- 改成 for 循环加 break 再加动态修改字典能更快一点

	```
	class Solution:
	    def twoSum(self, nums: List[int], target: int) -> List[int]:
	        d = {}
	        for i, n in enumerate(nums): 
	            if n in d: return [d[n], i]
	            d[target-n] = i
	```

## [2. Add Two Numbers 7行](https://leetcode.com/problems/add-two-numbers/)

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode, carry=0) -> ListNode:
        if not (l1 or l2):
            return ListNode(1) if carry else None
        l1, l2 = l1 or ListNode(0), l2 or ListNode(0)
        val = l1.val + l2.val + carry
        res = ListNode(val % 10)
        res.next = self.addTwoNumbers(l1.next, l2.next, int(val > 9))
        return res
```
- int(True) 等于 1
- None or 7 等于 7
- 用 carry 记录是否应该进位
## [3. Longest Substring Without Repeating Characters 3行](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        b, m, d = 0, 0, {}
        for i, l in enumerate(s): b, m, d[l] = max(b, d.get(l, -1) + 1), max(m, i - b), i
        return max(m, len(s) - b)
```
- b代表起始位置，m代表上一步的最大无重复子串，d是一个字典，记录着到当前步骤出现过的字符对应的最大位置
- 每次迭代过程中，遇到遇见过的字符时，b就会变为那个字符上一次出现位置+1，m记录上一次应该达到的全局最大值，所以最后需要再比较一次。

## [7. Reverse Integer 2行](https://leetcode.com/problems/reverse-integer/)

```
class Solution:
    def reverse(self, x):
        r = x // max(1, abs(x)) * int(str(abs(x))[::-1])
        return r if r.bit_length() < 32 or r == -2**31 else 0
```
- x // max(1, abs(x))意味着 0：x为0， 1：x为正， -1：x为负，相当于被废弃的函数cmp
- [::-1]代表序列反转
- 2^31^ 和 -2^31^ 的比特数为32，其中正负号占用了一位
- 32位整数范围 [−2^31^,  2^31^ − 1] 中正数范围小一个是因为0的存在
## [9. Palindrome Number 1行](https://leetcode.com/problems/palindrome-number/)

```
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```
应题目要求不使用字符串的进阶解法：

```
class Solution:
    def isPalindrome(self, x: int) -> bool:
        r = list(map(lambda i: int(10**-i * x % 10), range(int(math.log10(x)), -1, -1))) if x > 0 else [0, x]
        return r == r[::-1]
```
- 思路是一样的，这里把整数转成了列表而不是字符串
- 比如一个整数12321，我想取出百位数可以这么做：<a href="https://www.codecogs.com/eqnedit.php?latex=12321*10^{int(log_{10}12321)}\%10=123\%10=3" target="_blank"><img src="https://latex.codecogs.com/gif.latex?12321*10^{int(log_{10}12321)}\%10=123\%10=3" title="12321*10^{int(log_{10}12321)}\%10=123\%10=3" /></a>
## [13. Roman to Integer 2行](https://leetcode.com/problems/roman-to-integer/)

```
class Solution:
    def romanToInt(self, s: str) -> int:
        d = {'I':1, 'IV':3, 'V':5, 'IX':8, 'X':10, 'XL':30, 'L':50, 'XC':80, 'C':100, 'CD':300, 'D':500, 'CM':800, 'M':1000}
        return sum([d.get(s[max(i-1, 0):i+1], d[n]) for i, n in enumerate(s)])
```
- 构建一个字典记录所有罗马数字子串，注意长度为2的子串记录的值是（实际值-子串内左边罗马数字代表的数值）。
- 这样一来，遍历整个s的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值。
- 举个例子，遍历经过IV的时候先记录I的对应值1再往前移动一步记录IV的值3，加起来正好是IV的真实值4。max函数在这里是为了防止遍历第一个字符的时候出现[-1:0]的情况。
## [14. Longest Common Prefix 2行](https://leetcode.com/problems/longest-common-prefix/)

```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        r = [len(set(c)) == 1 for c in zip(*strs)] + [0]
        return strs[0][:r.index(0)] if strs else ''
```
- 利用好zip和set
## [20. Valid Parentheses 3行](https://leetcode.com/problems/valid-parentheses/)

```
class Solution:
    def isValid(self, s: str) -> bool:
        while any(('()' in s, '[]' in s, '{}' in s)):
            s = s.replace('()', '').replace('[]', '').replace('{}', '')
        return not s
```
- 不断删除有效括号直到不能删除，思路简单效率低。另外，stack的方法也很简单，而且快多了。

	```
	class Solution:
	    def isValid(self, s: str) -> bool:
	        stack, d = [], {'{': '}', '[': ']', '(': ')'}
	        for p in s:
	            if p in '{[(':
	                stack += [p];
	            elif not (stack and d[stack.pop()] == p):
	                return False
	        return not stack
	```

## [21. Merge Two Sorted Lists 4行](https://leetcode.com/problems/merge-two-sorted-lists/)

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 and l2:
            l1, l2 = sorted((l1, l2), key=lambda x: x.val)
            l1.next = self.mergeTwoLists(l1.next, l2)
        return l1 or l2
```
- 正无穷：float('inf')   负无穷：float('-inf')
- 7 or 9 等于 7
- None and 7 等于 None
- sorted用在这里为了保证 l1 的值小于等于 l2 的值
## [26. Remove Duplicates from Sorted Array 4行](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        for i in range(len(nums)-1, 0, -1):
            if nums[i] == nums[i-1]:
                nums.pop(i)
        return len(nums)
```
- 时间效率O(N)空间效率O(1)，逆遍历可以防止删除某个元素后影响下一步索引的定位。
## [28. Implement strStr() 1行](https://leetcode.com/problems/implement-strstr/)

```
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
		return haystack.find(needle)
```
- 当然了，不用内置函数也可以

	```
	class Solution:
		def strStr(self, haystack: 'str', needle: 'str') -> 'int':
		    for i in range(0, len(haystack) - len(needle) + 1):
		        if haystack[i:i+len(needle)] == needle:
		            return i
	    	return -1
	```

## [38. Count and Say 4行](https://leetcode.com/problems/count-and-say/)

```
class Solution:
    def countAndSay(self, n: int) -> str:
        r = '1'
        for _ in range(n - 1):
            r = re.sub(r'(.)\1*', lambda m: str(len(m.group())) + m.group(1), r)
        return r
```
- 正则表达式 re.sub(正则，替换字符串或函数，被替换字符串，是否区分大小写)
- . 可匹配任意一个除了\n的字符
(.) 匹配任意一个除了\n的字符并把这个匹配结果放进第一组
(.)\1 匹配一个任意字符的二次重复并把那个字符放入数组
(.)\1* 匹配一个任意字符的多次重复并把那个字符放入数组
- group(default=0)可以取匹配文本   group(1)取第一个括号内的文本
## [53. Maximum Subarray 2行](https://leetcode.com/problems/maximum-subarray/)

```
class Solution:
    def maxSubArray(self, nums):
        from functools import reduce
        return reduce(lambda r, x: (max(r[0], r[1]+x), max(r[1]+x,x)), nums, (max(nums), 0))[0]
```
- [reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)
- r[0]代表以当前位置为结尾的局部最优解
- r[1]代表全局最优解
- 直接DP的解法更好理解一些

	```
	class Solution:
	    def maxSubArray(self, nums: List[int]) -> int:
	        for i in range(1, len(nums)):
	            nums[i] = max(nums[i], nums[i] + nums[i-1])
	        return max(nums)
	```

## [62. Unique Paths 1行](https://leetcode.com/problems/unique-paths/)

```
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        return int(math.factorial(m+n-2)/math.factorial(m-1)/math.factorial(n-1))
```

- 题目可以转换为排列组合问题，解是C(min(m,n), m+n)，从m+n个中选出m个下移或n个右移。
- 用DP做也很快，以后自己算 C(a, b) 也可以用算这题的DP法代替
- math.factorial 的速度不亚于DP，可能内部有优化，不是直接连乘
## [66. Plus One 1行](https://leetcode.com/problems/plus-one/)

```
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        return list(map(int, str(int(''.join(map(str, digits))) + 1)))
```
## [69. Sqrt(x) 1行](https://leetcode.com/problems/sqrtx/)
```
class Solution:
    def mySqrt(self, x: int) -> int:
        return int(x ** 0.5)
```
出题者应该是希望看到下面的答案：
```
class Solution:
    def mySqrt(self, x: int) -> int:
        r = x
        while r*r > x:
            r = (r + x/r) // 2
        return int(r)
```
- 基本不等式(a+b)/2 >=√ab 推导自 (a-b)^2 >= 0，注意 a>0 且 b>0
- r 代表 result
## [70. Climbing Stairs 2行](https://leetcode.com/problems/climbing-stairs/)

```
class Solution:
    def climbStairs(self, n: int) -> int:
        from functools import reduce
        return reduce(lambda r, _: (r[1], sum(r)), range(n), (1, 1))[0]
```
- dp递归方程：到达当前楼梯的路径数 = 到达上个楼梯的路径数 + 到达上上个楼梯的路径数
- 这里用一个元组 r 来储存（当前楼梯路径数，下一层楼梯路径数）
- 利用 reduce 来代替for循环，[reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)。
## [88. Merge Sorted Array 1行](https://leetcode.com/problems/merge-sorted-array/)

```
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        while n > 0: nums1[m+n-1], m, n = (nums1[m-1], m-1, n) if m and nums1[m-1] > nums2[n-1] else (nums2[n-1], m, n-1)
```
- 这种题倒着算更容易
- 上面那行代码其实就相当于：
	
	```
	class Solution:
	    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
	        """
	        Do not return anything, modify nums1 in-place instead.
	        """
	        while n > 0:
	            if m and nums1[m-1] > nums2[n-1]:
	                nums1[m+n-1], m, n = nums1[m-1], m-1, n
	            else:
	                nums1[m+n-1], m, n = nums2[n - 1], m, n-1
	```

## [104. Maximum Depth of Binary Tree 1行](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        return max(map(self.maxDepth,(root.left, root.right))) + 1 if root else 0
```
- 利用map函数递归左右节点获取最大值，map函数会将参数一所指向的函数应用于参数二里的所有对象并返回所有结果。
## [136. Single Number 2行](https://leetcode.com/problems/single-number/)
```
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        from functools import reduce
        return reduce(int.__xor__, nums)
```
- 这里用到了异或（xor），相同的数字异或后为0，0异或任何数都等于那个数，用reduce在列表所有元素之间使用异或^，那么留下的就是那个单独的数字了。
## [198. House Robber 2行](https://leetcode.com/problems/house-robber/)
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        from functools import reduce
        return reduce(lambda r, n: (max(r[0], n + r[1]), r[0]), nums, (0, 0))[0]
```
- DP递归方程：一直偷到这家的钱 = max（一直偷到上一家的钱，一直偷到上上家的钱 + 这家的钱）😃有点小绕
- 以上为下面代码的化简版，[reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)。
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        last, now = 0, 0
        for i in nums:
            last, now = now, max(last + i, now)
        return now
```
- DP不一定要数组，这里两个变量就够了，空间复杂度为O(1)
## [268. Missing Number 1行](https://leetcode.com/problems/missing-number/)
```
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return int(len(nums) * (len(nums) + 1) / 2 - sum(nums))
```
- 等差数列求和公式
## [344. Reverse String 1行](https://leetcode.com/problems/reverse-string/)

```
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s.reverse()
```
## [412. Fizz Buzz 1行](https://leetcode.com/problems/fizz-buzz/)

```
class Solution:
    def fizzBuzz(self, n):
        return ['Fizz' * (not i % 3) + 'Buzz' * (not i % 5) or str(i) for i in range(1, n+1)]
```
- 7 or 8 = 7
- 0 or 8 = 8
# 解法汇总贡献者
注：此处贡献名单仅代表汇总搜集贡献，不代表全部原创，欢迎所有更短的解法🤓
- Knife丶[QQ1272068154  微信ly18597591102]
