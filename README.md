# :snake: Shortest-LeetCode-Python-Solutions
  Leet Code 刷题笔记 - - 不求最快最省，但求最短最优雅 :herb:，Shorter is better here.

# 前言
- 代码精炼是 Python 的核心，同时能够反应对于语言的熟练程度，本项目目的在于汇总 leet code 最短最优雅的解法，拒绝长篇大论，缩短学习周期，掌握各种技巧，助您在面试中写出令人眼前一亮的解答，给考官留个好印象。
- 为什么我们追求最短?1.短代码更pythonic，而且通常能够避免一些冗余过程。2.除了刷题掌握算法思路之外，我们更追求深入理解和掌握python,学会套用技巧，举一反三。3.真正面试的时候不一定要这么短，可以适当展开几行(O_o 除非你就是想秀其他人一脸 😅)，保证思路更清晰，相信就算展开几行也会比其他题解短很多吧。4.刷题很累，找点乐子，送给自己一些成就感吧。5.所有已收录代码都是优中选优，有利于缩短学习周期，除了短代码外，另有常规解法作为补充。
- 项目持续更新中，优先使用 python3，不支持的题目使用 python2 代替，如果您有更短更优雅的解法希望分享的话欢迎联系更新~  [直接发issue 或 fork，记得留下署名和联系方式 :panda_face:] 鉴于追求的主题，此项目收录 1.在代码量(不是行数)上明显优于现有解的最短代码 2.思路更高效的一般解法（作为补充放在首选解之后） [题目自带的代码不计入代码量]
- 如果您对当前解析有任何疑问，咱们 issue 见~
- 由于CSDN博客更新需要人工审核比较慢，所以迁移到github上，优先更新github内容。
- 为了快速找到题目可以按 [**Ctrl键 + F键**] 输入题目序号或名字定位。
# :trophy: 里程碑
- [:penguin: 腾讯精选练习](https://leetcode-cn.com/problemset/all/?listId=ex0k24j)（50题: 25简单 21中等 4困难） 代码行数 总计：140行 平均：2.8行 [:bookmark_tabs: 题目详情](tencent50.png) :calendar: 2019/05/05
# 推荐
- 与本项目有关联的，是一个[ C++最清晰题解汇总 ](https://github.com/cy69855522/Clearest-LeetCode-Cpp-Solutions)👻。Python篇注重熟悉语言特性，充分利用高级语言提供的已内置的功能避免冗余编码，最低成本地解决问题。C++篇注重通用思想，分专题逐个击破，深入探究算法流程。俩者同时服用效果更佳，只想学一门也不必担心，俩个项目相辅相成，Python篇会在题解之后添加常规解法作为补充，并聚合C++篇精华总结一套运用python独特技巧的专题，C++篇会利用python题解的思想优化代码，保证代码简洁，可读性高。
- 🌟 推荐刷题路线：[`专题剖析`](#专题) → [腾讯精选50题](https://leetcode-cn.com/problemset/all/?listId=ex0k24j) → [全题解析](#解析)

# 解析
默认已看过题目，🤡 没看过的话点标题可以跳转链接
## [1. Two Sum 4行](https://leetcode.com/problems/two-sum/)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
	d = {}
	for i, n in enumerate(nums): 
	    if n in d: return [d[n], i]
	    d[target-n] = i
```
- O(N)时间效率的快速解法，用字典记录 ｛需要的值:当前索引｝
## [2. Add Two Numbers 5行](https://leetcode.com/problems/add-two-numbers/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode, carry=0) -> ListNode:
        if not (l1 or l2): return ListNode(1) if carry else None
        l1, l2 = l1 or ListNode(0), l2 or ListNode(0)
        val = l1.val + l2.val + carry
        l1.val, l1.next = val % 10, self.addTwoNumbers(l1.next, l2.next, val > 9)
        return l1
```
- int(True) 等于 1
- None or 7 等于 7
- 用 carry 记录是否应该进位
## [3. Longest Substring Without Repeating Characters 3行](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        b, m, d = 0, 0, {}
        for i, l in enumerate(s): b, m, d[l] = max(b, d.get(l, -1) + 1), max(m, i - b), i
        return max(m, len(s) - b)
```
- b代表起始位置，m代表上一步的最大无重复子串，d是一个字典，记录着到当前步骤出现过的字符对应的最大位置
- 每次迭代过程中，遇到遇见过的字符时，b就会变为那个字符上一次出现位置+1，m记录上一次应该达到的全局最大值，所以最后需要再比较一次
## [4. Median of Two Sorted Arrays 5行](https://leetcode.com/problems/median-of-two-sorted-arrays/)

```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        a, b, m = *sorted((nums1, nums2), key=len), (len(nums1) + len(nums2) - 1) // 2
        self.__class__.__getitem__ = lambda self, i: m-i-1 < 0 or a[i] >= b[m-i-1]
        i = bisect.bisect_left(self, True, 0, len(a))
        r = sorted(a[i:i+2] + b[m-i:m-i+2])
        return (r[0] + r[1 - (len(a) + len(b)) % 2]) / 2
```
- 本题思路与官方题解类似，时间复杂度O(log(min(m, n)))，没看过的话建议先大体了解一下
- python 中 bisect 模块针对的是 list, 如果直接构造 list，时间复杂度为 O(min(m, n))，因此我们修改当前类的魔法方法伪造 list
- 在一个有序递增数列中，中位数左边的那部分的最大值一定小于或等于右边部分的最小值
- 如果总数组长度为奇数，m 代表中位数的索引，否则 m 代表用于计算中位数的那两个数字的左边一个。比如输入为[1,2]，[3]，那么m应该为[1,2,3]中位数2的索引1，如果输入为[1,3]，[2,4]，那么m应该为[1,2,3,4]中2的索引1
- 使用二分搜索找到 m 对应的值在a或b中对应的索引，也就是说，我们要找的中位数或中位数左部应该是 a[i] 或者 b[m-i]
- bisect.bisect_left 搜索列表中保持列表升序的情况下，True应该插入的位置（从左侧），比如 [F,F,T] 返回 2，[F,F] 返回 2
- 这里保证 a 是 nums1 和 nums2 中较短的那个，是为了防止二分搜索的时候索引越界
- sorted返回一个list，假设返回值是 [nums1, nums2]，那么前面加个 * 号就代表取出列表的所有内容，相当于一个迭代器，结果相当于直接写 nums1, nums2
## [5. Longest Palindromic Substring 5行](https://leetcode.com/problems/longest-palindromic-substring/)
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        r = ''
        for i, j in [(i, j) for i in range(len(s)) for j in (0, 1)]:
            while i > -1 and i + j < len(s) and s[i] == s[i + j]: i, j = i - 1, j + 2
            r = max(r, s[i + 1:i + j], key=len)
        return '' if not s else r
```
- 遍历字符串的每个索引 i，判断能否以 s[i] 或 s[i:i+j+1] 为中心向往拓展回文字符串
## [7. Reverse Integer 2行](https://leetcode.com/problems/reverse-integer/)

```python
class Solution:
    def reverse(self, x):
        r = x // max(1, abs(x)) * int(str(abs(x))[::-1])
        return r if r.bit_length() < 32 or r == -2**31 else 0
```
- x // max(1, abs(x))意味着 0：x为0， 1：x为正， -1：x为负，相当于被废弃的函数cmp
- [::-1]代表序列反转
- 2^31 和 -2^31 的比特数为32，其中正负号占用了一位
- 32位整数范围 [−2^31,  2^31 − 1] 中正数范围小一个是因为0的存在
## [8. String to Integer (atoi) 1行](https://leetcode.com/problems/string-to-integer-atoi/)
```python
class Solution:
    def myAtoi(self, s: str) -> int:
        return max(min(int(*re.findall('^[\+\-]?\d+', s.lstrip())), 2**31 - 1), -2**31)
```
- 使用正则表达式 `^：匹配字符串开头，[\+\-]：代表一个+字符或-字符，?：前面一个字符可有可无，\d：一个数字，+：前面一个字符的一个或多个，\D：一个非数字字符，*：前面一个字符的0个或多个`
- `max(min(数字, 2**31 - 1), -2**31)` 用来防止结果越界
## [9. Palindrome Number 1行](https://leetcode.com/problems/palindrome-number/)

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```
不使用字符串的进阶解法：

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        r = list(map(lambda i: int(10**-i * x % 10), range(int(math.log10(x)), -1, -1))) if x > 0 else [0, x]
        return r == r[::-1]
```
- 思路是一样的，这里把整数转成了列表而不是字符串
- 比如一个整数12321，我想取出百位数可以这么做：12321 * 10^{int(log_{10}12321)} % 10 = 123 % 10 = 3
## [11. Container With Most Water 3行](https://leetcode.com/problems/container-with-most-water/)
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        res, l, r = 0, 0, len(height) - 1
        while l < r: res, l, r = (max(res,  height[l] * (r - l)), l + 1, r) if height[l] < height[r] else (max(res,  height[r] * (r - l)), l, r - 1)
        return res
```
- 双指针 O(N) 解法
- res：结果，l：容器左壁索引，r：容器右壁索引
- 如果 height[l] < height[r] 那么 l += 1 否则 r -= 1，说明：如果 height[0] < height[3] 那么(0, 1), (0, 2)对应的容器体积一定小于(0, 3)的，因为此时计算体积的时候高为 height(0)，容器的宽减少而高不增加，面积必然缩小
## [13. Roman to Integer 2行](https://leetcode.com/problems/roman-to-integer/)

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        d = {'I':1, 'IV':3, 'V':5, 'IX':8, 'X':10, 'XL':30, 'L':50, 'XC':80, 'C':100, 'CD':300, 'D':500, 'CM':800, 'M':1000}
        return sum([d.get(s[max(i-1, 0):i+1], d[n]) for i, n in enumerate(s)])
```
- 构建一个字典记录所有罗马数字子串，注意长度为2的子串记录的值是（实际值-子串内左边罗马数字代表的数值）
- 这样一来，遍历整个s的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值
- 举个例子，遍历经过IV的时候先记录I的对应值1再往前移动一步记录IV的值3，加起来正好是IV的真实值4。max函数在这里是为了防止遍历第一个字符的时候出现[-1:0]的情况
## [14. Longest Common Prefix 2行](https://leetcode.com/problems/longest-common-prefix/)

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        r = [len(set(c)) == 1 for c in zip(*strs)] + [0]
        return strs[0][:r.index(0)] if strs else ''
```
- 利用好zip和set
## [15. 3Sum 5行](https://leetcode.com/problems/3sum/)
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums, r = sorted(nums), set()
        for i in [i for i in range(len(nums)-2) if i < 1 or nums[i] > nums[i-1]]:
            d = {-nums[i]-n: j for j, n in enumerate(nums[i + 1:])}
            r.update([(nums[i], n, -nums[i]-n) for j, n in enumerate(nums[i+1:]) if n in d and d[n] > j])
        return list(map(list, r))
```
- 时间复杂度：O(N^2)
- 这里 sort 一是为了避免重复，这一点可以体现在我们输出的结果都是升序的，如果不这么做 set 无法排除一些相同结果，而是为了节省计算，防止超时
- for 循环内部的代码思想同` 第一题 Two Sum`，用字典记录｛需要的值:当前索引｝，如果字典中存在相同的数字，那么将会记录比较大的那个索引，因此可以用`d[n] > i`来避免一个元素重复选择
- `(nums[i], n, -nums[i]-n)`保证了列表升序
## [16. 3Sum Closest 7行](https://leetcode.com/problems/3sum-closest/)
```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums, r, end = sorted(nums), float('inf'), len(nums) - 1
        for c in range(len(nums) - 2):
            i, j = max(c + 1, bisect.bisect_left(nums, target - nums[end] - nums[c], c + 1, end) - 1), end
            while r != target and i < j:
                s = nums[c] + nums[i] + nums[j]
                r, i, j = min(r, s, key=lambda x: abs(x - target)), i + (s < target), j - (s > target)
        return r
```
- float('inf') = 正无穷
- 排序，遍历，双指针，O(N^2) 时间复杂度，二分法初始化
- 排序是为了使用双指针，首先遍历得到索引 c，然后计算 c，左指针 i，右指针 j 对应数字之和，如果大于 target，j 向内移动，否则 i 向内移动
- i 的初始值不是 c + 1，是为了减少计算量，用二分法得到一个合理的初始值
## [20. Valid Parentheses 2行](https://leetcode.com/problems/valid-parentheses/)

```python
class Solution:
    def isValid(self, s: str) -> bool:
        while any(('()' in s, '[]' in s, '{}' in s)): s = s.replace('()', '').replace('[]', '').replace('{}', '')
        return not s
```
- 不断删除有效括号直到不能删除，思路简单效率低。另外，stack的方法也很简单，而且快多了。

	```python
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

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 and l2:
            if l1.val > l2.val: l1, l2 = l2, l1
            l1.next = self.mergeTwoLists(l1.next, l2)
        return l1 or l2
```
- 7 or 9 等于 7
- None and 7 等于 None
- sorted用在这里为了保证 l1 的值小于等于 l2 的值
## [23. Merge k Sorted Lists 4行](https://leetcode.com/problems/merge-k-sorted-lists/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        r, n, p = [], lists and lists.pop(), None
        while lists or n: r[len(r):], n = ([n], n.next or lists and lists.pop()) if n else ([], lists.pop())
        for n in sorted(r, key=lambda x: x.val, reverse=True): n.next, p = p, n
        return n if r else []
```
- 本题思路：
	1. 把题目给的所有链表中的所有节点放进一个列表 r。
	2. 对这个列表 r 中的所有节点进行从大到小的排序。O(NlogN)
	3. 把每个节点的指针指向前一个节点。（第一个节点，也就是最大的那个，指向None。）
	4. 返回最后一个节点，也就是整个新链表的开头。

- 如何把所有节点放进 r(result link)？

	我们首先初始化 r 为空列表，初始化 n(node) 为题目所给的第一个链表的开头节点，并删除lists中的这个节点，接着进入while循环，如果 n 不为空，那么 r += [n]，这里使用了切片的技巧（r[len(r):]=[n]相当于r=r+[n]），n=n.next，如果n是第一个链表的最后一个节点的话n.next就是None，下一次while的时候如果lists不为空就说明还有别的链表，此时n为None，我们让 r 不变，n=lists.pop()，也就是从lists中再取下一个节点赋值给n，重复以上步骤直到 lists 为空，我们就把所有节点放进 r 了。
	
- 怎么对 r 排序？

	用了sorted函数，其中key定义了排序时用来比较的是每个元素的val属性，同时设置reverse为True代表降序排序。
	
- 如何修改每个节点的指针？

	我们初始化 p(previous node) 为None。遍历降序排好的列表 r，r中的第一个元素就是值最大的元素，也就是我们应该返回的链表的结尾，我们设置它指向None，然后让p=这个节点，继续for循环。之后每经过一个节点 n 就把这个节点的next属性设置为上一个节点 p，遍历完成之后的 n，也就是我们遍历经过的最后一个元素，拥有最小的值，自然就是整个新链表的起始节点，我们将其作为输出值，函数返回。
	
## [26. Remove Duplicates from Sorted Array 3行](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        for i in range(len(nums)-1, 0, -1):
            if nums[i] == nums[i-1]: nums.pop(i)
        return len(nums)
```
- 时间效率O(N)空间效率O(1)，逆遍历可以防止删除某个元素后影响下一步索引的定位
- 每次删除数组元素会引发大量的数据迁移操作，使用以下算法解题效率更高
	```python
	class Solution:
	    def removeDuplicates(self, nums: List[int]) -> int:
		i = 0
		for j in range(1, len(nums)):
		    if nums[j] != nums[i]:
			nums[i + 1] = nums[j]
			i += 1
		return i + 1 if nums else 0
	```
	- 此解法思路同官方题解
	- 数组完成排序后，我们可以放置两个指针 i 和 j，其中 i 是慢指针，而 j 是快指针。只要 nums[i] = nums[j]，我们就增加 j 以跳过重复项。当我们遇到 nums[j] != nums[i]时，跳过重复项的运行已经结束，因此我们必须把它（nums[j]）的值复制到 nums[i + 1]。然后递增 i，接着我们将再次重复相同的过程，直到 j 到达数组的末尾为止
## [28. Implement strStr() 1行](https://leetcode.com/problems/implement-strstr/)

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
		return haystack.find(needle)
```
- 不用内置函数也可以

	```python
	class Solution:
		def strStr(self, haystack: 'str', needle: 'str') -> 'int':
		    for i in range(0, len(haystack) - len(needle) + 1):
		        if haystack[i:i+len(needle)] == needle:
		            return i
	    	return -1
	```
## [33. Search in Rotated Sorted Array 3行](https://leetcode.com/problems/search-in-rotated-sorted-array/)
```python
class Solution:
    def search(self, nums, target):
        self.__class__.__getitem__ = lambda self, m: not(target < nums[0] <= nums[m] or nums[0] <= nums[m] < target or nums[m] < target <= nums[-1])
        i = bisect.bisect_left(self, True, 0, len(nums))
        return i if target in nums[i:i+1] else -1
```
- 作出数列的函数图像，可以看作是一个含断点的局部递增函数，形如:zap:，前面一段总是比较高
- python 中 bisect 模块针对的是 list, 如果直接构造 list，相当于遍历所有元素，时间复杂度为 O(N) 而不是 O(logN)，因此我们修改当前类的魔法方法伪造 list，然后用当前类代替list
- 用二分搜索时，m 代表 middle，low 代表 low，hi 代表 high，当满足任一条件｛① targe < middle 且 middle 在前一段上 且 target < nums[0] ② target > middle 且 middle 在第一段上 ③ target > middle 且 middle 在第二段上 且 target <= nums[-1]｝时，应该向右搜索，因此 getitem 返回 False。
- 另外还有一种简单的思路：二分法找到断点的位置恢复原始数组，然后正常二分法即可
	```python
	class Solution:
	    def search(self, nums, target):
		lo, hi, k = 0, len(nums) - 1, -1
		while lo <= hi:
		    m = (lo + hi) // 2
		    if m == len(nums) - 1 or nums[m] > nums[m+1]:
			k = m + 1
			break
		    elif m == 0 or nums[m] < nums[m-1]:
			k = m
			break
		    if nums[m] > nums[0]:
			lo = m + 1
		    else:
			hi = m - 1
		i = (bisect.bisect_left(nums[k:] + nums[:k], target) + k) % max(len(nums), 1)
		return i if nums and nums[i] == target else -1
	```
## [38. Count and Say 1行](https://leetcode.com/problems/count-and-say/)

```python
class Solution:
    def countAndSay(self, n: int) -> str:
        return '1' * (n is 1) or re.sub(r'(.)\1*', lambda m: str(len(m.group())) + m.group(1), self.countAndSay(n - 1))
```
- 正则表达式 re.sub(正则，替换字符串或函数，被替换字符串，是否区分大小写)
- . 可匹配任意一个除了\n的字符
(.) 匹配任意一个除了\n的字符并把这个匹配结果放进第一组
(.)\1 匹配一个任意字符的二次重复并把那个字符放入数组
(.)\1* 匹配一个任意字符的多次重复并把那个字符放入数组
- group(default=0)可以取匹配文本   group(1)取第一个括号内的文本
## [43. Multiply Strings 5行](https://leetcode.com/problems/multiply-strings/)
```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        d = {}
        for i, n1 in enumerate(num1[::-1]):
            for j, n2 in enumerate(num2[::-1]): d[i + j] = d.get(i + j, 0) + int(n1) * int(n2)
        for k in [*d]: d[k + 1], d[k] = d.get(k + 1, 0) + int(d[k] * 0.1), d[k] % 10
        return re.sub('^0*', '', ''.join(map(str, d.values()))[::-1]) or '0'
```
- 本题的难点在于计算整数的时候不能超过32bits，因此使用竖式计算
- 我们遍历num1中的每个数字n1，然后带着这个数字遍历num2中的每个数字n2做乘法，所得乘积放进 d 中相应的位置然后逐位计算结果
- i + j 正好对应俩个数字相乘后所在的位置，比如 0 + 0 就应该是个位， 0 + 1 就是十位， 1 + 1 百位。这里所说的位置可以参考[这篇博客中的过程图](https://blog.csdn.net/Give_me_the_who/article/details/80313860)
## [46. Permutations 1行](https://leetcode.com/problems/permutations/)
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return [[n] + sub for i, n in enumerate(nums) for sub in self.permute(nums[:i] + nums[i+1:])] or [nums]
```
- 每次固定第一个数字递归地排列数组剩余部分
- python 有内置函数可以直接实现

	```python
	class Solution:
	    def permute(self, nums: List[int]) -> List[List[int]]:
		from itertools import permutations
		return list(permutations(nums))
	```
## [53. Maximum Subarray 2行](https://leetcode.com/problems/maximum-subarray/)

```python
class Solution:
    def maxSubArray(self, nums):
        from functools import reduce
        return reduce(lambda r, x: (max(r[0], r[1]+x), max(r[1]+x,x)), nums, (max(nums), 0))[0]
```
- [reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)
- r[0]代表以当前位置为结尾的局部最优解
- r[1]代表全局最优解
- 直接DP的解法更好理解一些

	```python
	class Solution:
	    def maxSubArray(self, nums: List[int]) -> int:
	        for i in range(1, len(nums)):
	            nums[i] = max(nums[i], nums[i] + nums[i-1])
	        return max(nums)
	```
## [54. Spiral Matrix 1行](https://leetcode.com/problems/spiral-matrix/)
```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        return matrix and [*matrix.pop(0)] + self.spiralOrder([*zip(*matrix)][::-1])
```
- 为什么是`[*matrix.pop(0)]`而不是`matrix.pop(0)`？因为对于后面的递归，传进来的列表中元素是tuple
## [58. Length of Last Word 1行](https://leetcode.com/problems/length-of-last-word/)
```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.strip(' ').split(' ')[-1])
```
## [59. Spiral Matrix II 3行](https://leetcode.com/problems/spiral-matrix-ii/)
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        r, n = [[n**2]], n**2
        while n > 1: n, r = n - len(r), [[*range(n - len(r), n)]] + [*zip(*r[::-1])]
        return r
```
- 流程图
```
||  =>  |9|  =>  |8|      |6 7|      |4 5|      |1 2 3|
		 |9|  =>  |9 8|  =>  |9 6|  =>  |8 9 4|
				     |8 7|      |7 6 5|
```
## [61. Rotate List 4行](https://leetcode.com/problems/rotate-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        l = []
        while head: l[len(l):], head = [head], head.next
        if l: l[-1].next, l[-1 - k % len(l)].next = l[0], None
        return l[- k % len(l)] if l else None
```
## [62. Unique Paths 1行](https://leetcode.com/problems/unique-paths/)

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        return int(math.factorial(m+n-2)/math.factorial(m-1)/math.factorial(n-1))
```

- 题目可以转换为排列组合问题，解是C(min(m,n), m+n)，从m+n个中选出m个下移或n个右移。
- 用DP做也很快，以后自己算 C(a, b) 也可以用算这题的DP法代替
- math.factorial 的速度不亚于DP，可能内部有优化
- 0的阶乘为1
## [66. Plus One 1行](https://leetcode.com/problems/plus-one/)

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        return list(map(int, str(int(''.join(map(str, digits))) + 1)))
```
## [69. Sqrt(x) 1行](https://leetcode.com/problems/sqrtx/)
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        return int(x ** 0.5)
```
出题者应该是希望看到下面的答案：
```python
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

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        from functools import reduce
        return reduce(lambda r, _: (r[1], sum(r)), range(n), (1, 1))[0]
```
- dp递归方程：到达当前楼梯的路径数 = 到达上个楼梯的路径数 + 到达上上个楼梯的路径数
- 这里用一个元组 r 来储存（当前楼梯路径数，下一层楼梯路径数）
- 利用 reduce 来代替for循环。[reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)
## [78. Subsets 2行](https://leetcode.com/problems/subsets/)
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        from itertools import combinations
        return sum([list(combinations(nums, i)) for i in range(len(nums) + 1)], [])
```
## [88. Merge Sorted Array 1行](https://leetcode.com/problems/merge-sorted-array/)

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        while n > 0: nums1[m+n-1], m, n = (nums1[m-1], m-1, n) if m and nums1[m-1] > nums2[n-1] else (nums2[n-1], m, n-1)
```
- 这种题倒着算更容易
- 上面那行代码其实就相当于：
	
	```python
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
## [89. Gray Code 1行](https://leetcode.com/problems/gray-code/)
```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        return (lambda r: r + [x | 1<<n-1 for x in r[::-1]])(self.grayCode(n-1)) if n else [0]
```
- 前4个结果：
	```
	[0]
	[0 1]
	[00 01 11 10]
	[000 001 011 010 110 111 101 100]
	```
- 递归方程：这一步结果 = 上一步结果 + 上一步结果的镜像并在每个二进制数字前面加一位1
- << 左移符号，即在二进制表示后加一位 0 ，例子：3<<1 等于 6`（011 → 110）`，相当于 3 * 2的1次方
- x | 1<<n-1 就是在十进制数字 x 的二进制前面加一位1之后的十进制结果，比如 x = 1，有 1 | 10 等于 110
- 循环可以避免一些不必要的操作，会比递归快一些：
	```python
	class Solution:
	    def grayCode(self, n: int) -> List[int]:
		r = [0]
		for i in range(n):
		    r.extend([x | 1<<i for x in r[::-1]])
		return r
	```
- 或者直接背格雷码的公式🥶吧：
```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        return [i ^ i >> 1  for i in range(1 << n)]
```
## [94. Binary Tree Inorder Traversal 2行](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        f = self.inorderTraversal
        return f(root.left) + [root.val] + f(root.right) if root else []
```
- 递归
```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        r, stack = [], []
        while True:
            while root:
                stack.append(root)
                root = root.left
            if not stack:
                return r
            node = stack.pop()
            r.append(node.val)
            root = node.right
        return r
```
- 迭代
## [104. Maximum Depth of Binary Tree 1行](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
```python
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
- 利用map函数递归左右节点获取最大值，map函数会将参数一所指向的函数应用于参数二里的所有对象并返回所有结果构成的迭代器
## [121. Best Time to Buy and Sell Stock 2行](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        from functools import reduce
        return reduce(lambda r, p: (max(r[0], p-r[1]), min(r[1], p)), prices, (0, float('inf')))[0]
```
- r = (结果，之前遍历过的所有元素中的最小值)
- [reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        r, m = 0, float('inf')
        for p in prices:
            r, m = max(r, p - m), min(m, p)
        return r
```
## [122. Best Time to Buy and Sell Stock II 2行](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        return sum(b - a for a, b in zip(prices, prices[1:]) if b > a)
```
- 本题可以在同一天买入和卖出，因此只要当天票价比昨天的高就可以卖出
## [124. Binary Tree Maximum Path Sum 4行](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def maxPathSum(self, root: TreeNode, ok=True) -> int:
        if not root: return 0
        l, r = self.maxPathSum(root.left, False), self.maxPathSum(root.right, False)
        self.max = max(getattr(self, 'max', float('-inf')), l + root.val + r)
        return self.max if ok else max(root.val + max(l, r), 0)
```
- 使用 self.max 记录全局最大值，getattr 返回自身 max 属性的值或预定义的负无穷
- 本题思路是：递归每一个节点，返回`max(以当前节点为结尾的最大路径和,0)`。并更新最大值`全局最大路径和=max(全局最大路径和，当前节点值+左子树返回结果+右子树返回结果)`
- 用ok判断是不是第一次递归，是就返回全局最大值，否则照常
## [136. Single Number 2行](https://leetcode.com/problems/single-number/)
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        from functools import reduce
        return reduce(int.__xor__, nums)
```
- 这里用到了异或（xor），相同的数字异或后为0，0异或任何数都等于那个数，用reduce在列表所有元素之间使用异或^，那么留下的就是那个单独的数字了。
## [141. Linked List Cycle 2行](https://leetcode.com/problems/linked-list-cycle/)
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        while head and head.val != None: head.val, head = None, head.next
        return head != None
```
- 这题不支持python3，所以用pyhton2解法代替，下题记得调回来 :baby_chick:
- 破坏走过的所有节点，下次再遇到就知道了
- 不过以上方法会丢失原有信息，一般解法为快慢指针
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        slow = fast = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if slow == fast:
                return True
        return False
```
## [142. Linked List Cycle II 5行](https://leetcode.com/problems/linked-list-cycle-ii/)
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        s = {None}
        while head not in s:
            s.add(head)
            head = head.next
        return head
```
- 把见过的节点丢集合里，下次再遇见就是环的开始
- 还有一个纯数学的快慢指针解法，设环的起始节点为 E，快慢指针从 head 出发，快指针速度为 2，设相交节点为 X，head 到 E 的距离为 H，E 到 X 的距离为 D，环的长度为 L，那么有：快指针走过的距离等于慢指针走过的距离加快指针多走的距离（多走了 n 圈的 L） `2(H + D) = H + D + nL`，因此可以推出 `H = nL - D`，这意味着如果我们让俩个慢指针一个从 head 出发，一个从 X 出发的话，他们一定会在节点 E 相遇
	```
				  _____
				 /     \
		 head___________E       \
				\       /
				 X_____/ 
	```
	```python
	class Solution(object):
	    def detectCycle(self, head):
		slow = fast = head
		while fast and fast.next:
		    fast = fast.next.next
		    slow = slow.next
		    if slow == fast:
			break
		else:
		    return None
		while head is not slow:
		    head = head.next
		    slow = slow.next
		return head
	```
## [146. LRU Cache 7行](https://leetcode.com/problems/lru-cache/)
```python
class LRUCache(object):

    def __init__(self, capacity):
        self.od, self.cap = collections.OrderedDict(), capacity

    def get(self, key):
        if key not in self.od: return -1
        self.od.move_to_end(key)
        return self.od[key]

    def put(self, key, value):
        if key in self.od: del self.od[key]
        elif len(self.od) == self.cap: self.od.popitem(False)
        self.od[key] = value


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
## [148. Sort List 10行](https://leetcode.com/problems/sort-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not (head and head.next): return head
        pre, slow, fast = None, head, head
        while fast and fast.next: pre, slow, fast = slow, slow.next, fast.next.next
        pre.next = None
        return self.mergeTwoLists(*map(self.sortList, (head, slow)))
    
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 and l2:
            if l1.val > l2.val: l1, l2 = l2, l1
            l1.next = self.mergeTwoLists(l1.next, l2)
        return l1 or l2
```
- 使用快慢指针寻找链表中点，并分解链表
- 递归融合俩个有序链表，详解见 21 题
- 此处忽略了递归开栈导致的非 常数级空间复杂度（想太多了吧:laughing:），如果一定要抬杠，推荐使用quicksort
	```python
	class Solution(object):
	    def sortList(self, head):
		"""
		:type head: ListNode
		:rtype: ListNode
		"""
		def partition(start, end):
		    node = start.next.next
		    pivotPrev = start.next
		    pivotPrev.next = end
		    pivotPost = pivotPrev
		    while node != end:
			temp = node.next
			if node.val > pivotPrev.val:
			    node.next = pivotPost.next
			    pivotPost.next = node
			elif node.val < pivotPrev.val:
			    node.next = start.next
			    start.next = node
			else:
			    node.next = pivotPost.next
			    pivotPost.next = node
			    pivotPost = pivotPost.next
			node = temp
		    return [pivotPrev, pivotPost]

		def quicksort(start, end):
		    if start.next != end:
			prev, post = partition(start, end)
			quicksort(start, prev)
			quicksort(post, end)

		newHead = ListNode(0)
		newHead.next = head
		quicksort(newHead, None)
		return newHead.next
	```
## [155. Min Stack 每个1行](https://leetcode.com/problems/min-stack/)
```python
class MinStack:
    
    def __init__(self):
        self.data = [(None, float('inf'))]

    def push(self, x: 'int') -> 'None':
        self.data.append((x, min(x, self.data[-1][1])))

    def pop(self) -> 'None':
        if len(self.data) > 1: self.data.pop()

    def top(self) -> 'int':
        return self.data[-1][0]

    def getMin(self) -> 'int':
        return self.data[-1][1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```
## [160. Intersection of Two Linked Lists 3行](https://leetcode.com/problems/intersection-of-two-linked-lists/)
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        a, b = (headA, headB) if headA and headB else (None, None)
        while a != b: a, b = not a and headB or a.next, not b and headA or b.next
        return a
```
- 这题不支持 Python3 所以只能用 Python2 做了
- 把第一条链表的尾部接到第二条链表的开头，第二条接到第一条的开头，就能消除俩条链表的长度差，并在某一时刻在第一个交叉点相遇，或在走完俩条链表长度的时候同时为 None
## [162. Find Peak Element 2行](https://leetcode.com/problems/find-peak-element/)
```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        self.__class__.__getitem__ = lambda self, i: i and nums[i - 1] > nums[i]
        return bisect.bisect_left(self, True, 0, len(nums)) - 1
```
- [二分查找套路](#二分查找)
- 如果当前位置的左边是更大的数字，则当前位置置为True，继续向左搜索，最后应该插入的位置 = 我们寻找的位置 + 1，因此最后 - 1
## [169. Majority Element 1行](https://leetcode.com/problems/majority-element/)
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        return sorted(nums)[len(nums) // 2]
```
## [198. House Robber 2行](https://leetcode.com/problems/house-robber/)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        from functools import reduce
        return reduce(lambda r, n: (max(r[0], n + r[1]), r[0]), nums, (0, 0))[0]
```
- DP递归方程：一直偷到这家的钱 = max（一直偷到上一家的钱，一直偷到上上家的钱 + 这家的钱）😃有点小绕
- 以上为下面代码的化简版，[reduce 函数详解](https://www.cnblogs.com/XXCXY/p/5180245.html)
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        last, now = 0, 0
        for i in nums:
            last, now = now, max(last + i, now)
        return now
```
- DP不一定要数组，这里两个变量就够了，空间复杂度为O(1)
## [206. Reverse Linked List 2行](https://leetcode.com/problems/reverse-linked-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode, tail=None) -> ListNode:
        if head: head.next, tail, head = tail, head, head.next
        return self.reverseList(head, tail) if head else tail
```
- 递归解法
- 此解法为尾递归，即直接以递归返回值作为结果，一般编译器会做优化，避免多余的函数开栈操作，实现效果相当于迭代
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        p = None
        while head: head.next, p, head = p, head, head.next
        return p
```
- 迭代解法
## [215. Kth Largest Element in an Array 1行](https://leetcode.com/problems/kth-largest-element-in-an-array/)
```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        return sorted(nums)[-k]
```
- O(NlogN)调库
- 面试官一般不会接受以上答案的，可以参考下面这个O(N)的quick-selection，思路借鉴的quick-sort
	```python
	class Solution:
	    def findKthLargest(self, nums: List[int], k: int) -> int:
		l = [x for x in nums if x > nums[0]]
		m = [x for x in nums if x == nums[0]]
		r = [x for x in nums if x < nums[0]]
		f = self.findKthLargest

		if k <= len(l):
		    return f(l, k)
		elif k <= len(l) + len(m):
		    return nums[0]
		return f(r, k - len(l) - len(m))
	```
## [217. Contains Duplicate 1行](https://leetcode.com/problems/contains-duplicate/)
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        return len(nums) != len(set(nums))
```
## [230. Kth Smallest Element in a BST 3行](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def kthSmallest(self, root, k):
        from itertools import chain, islice
        def gen(x): yield from chain(gen(x.left), [x.val], gen(x.right)) if x else ()
        return next(islice(gen(root), k - 1, k))
```
- 本题利用迭代器骚了一波:grinning:，不太了解的话看这里 [yield 推荐阅读博客](https://blog.csdn.net/mieleizhi0522/article/details/82142856)
- chain 函数可以组合多个迭代器，islice 函数对迭代器做切片操作
- 此题常规解法 中序遍历 还是需要了解下的
	```python
	# Definition for a binary tree node.
	# class TreeNode(object):
	#     def __init__(self, x):
	#         self.val = x
	#         self.left = None
	#         self.right = None

	class Solution(object):
	    def kthSmallest(self, root, k):
		"""
		:type root: TreeNode
		:type k: int
		:rtype: int
		"""
		res = []
		self.visitNode(root, res)
		return res[k - 1]

	    # 中序遍历
	    def visitNode(self, root, res):
		if root is None:
		    return
		self.visitNode(root.left, res)
		res.append(root.val)
		self.visitNode(root.right, res)
	```
## [231. 2的幂 1行](https://leetcode.com/problems/power-of-two/)
```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
	"""
	:type n: int
	:rtype: bool
	"""
        return n > 0 and n & n - 1 == 0
```
- 2 的幂的二进制形式最高位一定是1，其余为0
- 用常规思路也行
	```python
	class Solution(object):
	    def isPowerOfTwo(self, n):
		return n > 0 and 2**int(math.log2(n)) == n
	```
## [235. Lowest Common Ancestor of a Binary Search Tree 2行](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root, p, q):
        while (root.val - p.val) * (root.val - q.val) > 0: root = (root.left, root.right)[p.val > root.val]
        return root
```
- 最近公共祖先的值一定介于p、q值之间(闭区间)
## [236. Lowest Common Ancestor of a Binary Tree 2行](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        l, r = map(lambda x: x and self.lowestCommonAncestor(x, p, q), (root.left, root.right))
        return (root in (p, q) or l and r) and root or l or r
```
- 递归全部节点，p 的祖先节点全部返回 p，q 的祖先节点全部返回 q，除非它同时是俩个节点的最近祖先，也就是 p，q 分别位于左右子树，那么返回自身
- 这思路用在[235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)也行
## [237. Delete Node in a Linked List 1行](https://leetcode.com/problems/delete-node-in-a-linked-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val, node.next = node.next.val, node.next.next
```
- `node = node.next`是不行的，因为这里只是改了函数参数引用的对象，而原来传进来的 node 没有任何改变
- 详细说明下：如果Python的函数得到的参数是可变对象（比如list，set，这样的，内部属性可以改变的），那么我们实际得到的是这个对象的浅拷贝。比如这个函数刚刚开始的时候题目传进来一个参数node，我们设这个节点为A，那么实际上得到的参数node是一个对于A的一个浅拷贝，你可以想象node是一把钥匙，它可以打开真正的节点A的门，如果我们现在让`node = node.next`，那么我们只是换了钥匙，变成了打开 A.next 的门的对应的钥匙，因此链表没有被修改， A没有被修改，只是我们手里的钥匙变了。而如果我们直接写`node.val, node.next = node.next.val, node.next.next`，就相当于我们先用钥匙找到 A 的门，然后修改了 A 的属性，链表发生变化
- 此题考查python函数的传参形式为“传对象引用”，相当于浅拷贝（对于可变对象来说）
## [238. Product of Array Except Self 5行](https://leetcode.com/problems/product-of-array-except-self/)
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        res, l, r = [1] * len(nums), 1, 1
        for i, j in zip(range(len(nums)), reversed(range(len(nums)))):
            res[i], l = res[i] * l, l * nums[i]
            res[j], r = res[j] * r, r * nums[j]
        return res
```
- O(N)双指针双向遍历
## [240. Search a 2D Matrix II 1行](https://leetcode.com/problems/search-a-2d-matrix-ii/)
```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        return any(target in row for row in matrix)
```
- 以下为 O(m+n) 解法：
	```python
	class Solution:
	    def searchMatrix(self, matrix, target):
		"""
		:type matrix: List[List[int]]
		:type target: int
		:rtype: bool
		"""
		j = -1
		for row in matrix:
		    while j > -len(row) and row[j] > target:
			j -= 1
		    if row and row[j] == target:
			return True
		return False
	```
	- 从矩阵右上角开始，若值比 target 大则证明这一列的值都比 target 大，继续搜索前面的列；若比 target 小说明 target 可能在后面的行中，进入下一行
## [268. Missing Number 1行](https://leetcode.com/problems/missing-number/)
```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return int(len(nums) * (len(nums) + 1) / 2 - sum(nums))
```
- 等差数列求和公式
## [283. Move Zeroes 1行](https://leetcode.com/problems/move-zeroes/)
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums.sort(key=bool, reverse=True)
```
- sort 时间复杂度为O(NlogN), 直接遍历可以达到 O(N)
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = 0
        for i, n in enumerate(filter(lambda x: x, nums)):
            nums[i] = n
        for i in range(i + 1, len(nums)):
            nums[i] = 0
```
- 直接使用 filter 迭代器可以避免交换操作，思路更简单
## [292. Nim Game 1行](https://leetcode.com/problems/nim-game/)
```python
class Solution:
    def canWinNim(self, n: int) -> bool:
        return bool(n % 4)
```
- 只要轮到你的时候剩余石头数量不是 4 的倍数都是完胜，因为你有办法使得每次轮到对方的时候剩余石头数量都为 4 的倍数
## [328. Odd Even Linked List 6行](https://leetcode.com/problems/odd-even-linked-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head or not head.next: return head
        r, odd, p, head = head, head, head.next, head.next.next
        while head:
            odd.next, head.next, p.next = head, odd.next, head.next
            p, odd, head = p.next, head, p.next and p.next.next
        return r
```
- odd 记录上一个奇数位节点，p 记录前一个节点
- 从第3个位置开始循环，每次都把当前节点接到 odd 后面，然后跳到下一个奇数位节点继续循环
## [344. Reverse String 1行](https://leetcode.com/problems/reverse-string/)

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        s.reverse()
```
## [367. Valid Perfect Square 4行](https://leetcode.com/problems/valid-perfect-square/)
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        r = num
        while r * r > num:
            r = (r + num / r) // 2
        return r * r == num
```
- 基本不等式(a+b)/2 >=√ab 推导自 (a-b)^2 >= 0 → a^2 + b^2 >= 2ab → (a+b)/2 >=√ab（换元），注意 a>0 且 b>0
- `(r + num / r) / 2` >= √num 而 r > num / r 保证每次迭代 r 在不断减小,而`//`的存在保证最接近的时候能够逃离循环体
## [412. Fizz Buzz 1行](https://leetcode.com/problems/fizz-buzz/)

```python
class Solution:
    def fizzBuzz(self, n):
        return ['Fizz' * (not i % 3) + 'Buzz' * (not i % 5) or str(i) for i in range(1, n+1)]
```
- 7 or 8 = 7
- 0 or 8 = 8
## [414. Third Maximum Number 3行](https://leetcode.com/problems/third-maximum-number/)
```python
class Solution:
    def thirdMax(self, nums: List[int]) -> int:
        nums = set(nums)
        for _ in range((2, 0)[len(nums) < 3]): nums.remove(max(nums))
        return max(nums)
```
## [448. Find All Numbers Disappeared in an Array 1行](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        s = set(nums)
        return [i for i in range(1, len(nums) + 1) if i not in s]
```
- set 的内部实现为 dict，in 操作时间复杂度为 O(1)
- 应题目进阶要求，以下解为 O(N) 时间效率，无额外空间（除了返回数组和中间变量）
	```python
	class Solution:
	    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
		for n in nums:
		    nums[abs(n) - 1] = -abs(nums[abs(n) - 1])
		return [i + 1 for i, n in enumerate(nums) if n > 0]
	```
	- 此解实际上是利用索引把数组自身当作哈希表处理
	- 将 nums 中所有正数作为索引i，置 nums[i] 为负值。那么，仍为正数的位置即为（未出现过）消失的数字
	    - 原始数组：[4,3,2,7,8,2,3,1]
	    - 重置后为：[-4,-3,-2,-7,`8`,`2`,-3,-1]
	    - 结论：[8,2] 分别对应的index为[5,6]（消失的数字）
## [461. Hamming Distance 1行](https://leetcode.com/problems/hamming-distance/)
```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        return bin(x ^ y).count('1')
```
## [557. Reverse Words in a String III 1行](https://leetcode.com/problems/reverse-words-in-a-string-iii/)
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        return ' '.join(s.split(' ')[::-1])[::-1]
```
## [771. Jewels and Stones 1行](https://leetcode.com/problems/jewels-and-stones/)
```python
class Solution:
    def numJewelsInStones(self, J: str, S: str) -> int:
        return sum(S.count(i) for i in J)
```
- 时间复杂度O(N^2)，另附O(N)解法（set内部实现为dict，in操作时间复杂度为O(N)）
	```python
	class Solution:
	    def numJewelsInStones(self, J: str, S: str) -> int:
		j = set(J)
		return sum(s in j for s in S)
	```
## [867. Transpose Matrix 1行](https://leetcode.com/problems/transpose-matrix/)
```python
class Solution:
    def transpose(self, A: List[List[int]]) -> List[List[int]]:
        return [*zip(*A)]
```
## [938. Range Sum of BST 1行](https://leetcode.com/problems/range-sum-of-bst/)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
        return root and root.val * (L <= root.val <= R) + self.rangeSumBST(root.left, L, R) + self.rangeSumBST(root.right, L, R) or 0
```
## [953. Verifying an Alien Dictionary 1行](https://leetcode.com/problems/verifying-an-alien-dictionary/)
```python
class Solution(object):
    def isAlienSorted(self, words, order):
        return words == sorted(words, key=lambda w: [order.index(x) for x in w])
```
- 充分利用 python 序列比较的特点，sorted 的参数 key 可传入一个函数，sorted 函数会将每个元素作为输入，输入到 key 函数并获得返回值，整个序列将按此值的大小来排序。此处 key 函数为`lambda w: [order.index(x) for x in w]`，其为words中每个单词 word 返回一个 list，list 中每个元素为单词中字母 x 在 order 中的索引。比如当 order 为 ‘abcde……’ 时，单词 ‘cab’ 将返回 [3, 2, 1]。关于俩个 list 的大小比较，服从 python 序列比较的特性，请参考官方文档教程 5.8 节内容。
- 另外一个通用的方法是简单的数学计算，给每个单词赋予一个数字然后排序对比和原来的数组是否一致即可，每个字母的价值按字母表顺序，第几个就代表几，每进一位需要`*10^-2`避免冲突，比如字母表是`abcde……`，单词 cab 的价值就是 `3 * 1 + 1 * 0.01 + 2 * 0.0001`，价值越小的单词位置应该越靠前

	```python
	class Solution:
	    def isAlienSorted(self, words: List[str], order: str) -> bool:
		d = {c: i + 1 for i, c in enumerate(order)}
		return sorted(words, key=lambda x: sum(d[c] * 10**(-2 * i) for i, c in enumerate(x))) == words
	```
# 专题
- 相比于解析部分追求代码的绝对精简，本专题追求以高可读性呈现各大专题的常规思路。俩部分题目可能重复，但专题部分会有更详细的解析，且可能运用不同解法。为降低学习成本，🛫 每个方向会根据[C++篇](https://github.com/cy69855522/Clearest-LeetCode-Cpp-Solutions)收录少数优选的经典题目，若觉得不够请转[解析部分](#解析)，若想更改例题，欢迎 issue 提出您的建议或意见。

## 数组
### [238. Product of Array Except Self 双指针](https://leetcode.com/problems/product-of-array-except-self/)
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        res = [1] * n
        
        l = 1
        for i in range(n):
            res[i] *= l
            l *= nums[i]
        
        r = 1
        for j in range(n - 1, -1, -1):
            res[j] *= r
            r *= nums[j]
        
        return res
```
- 本题利用双指针，新数组每个位置上的值应该等于数组左边所有数字的乘积 × 数组右边所有数字的乘积
- 1.初始化一个新的数组res（result），包含n个1

  2.初始化变量l（left）代表左边的乘积，从左到右遍历数组，每次都让新数组的值乘以它左边数字的乘积l，然后更新l。此时新数组里的所有数字就代表了nums数组中对应位置左边所有数字的乘积
  
  3.再从右往左做一遍同样的操作，最终`res[i] = 1 * nums中i左边所有数字的乘积 * nums中i右边所有数字的乘积`
### [448. Find All Numbers Disappeared in an Array 逆遍历](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        r = [*range(1, len(nums) + 1)]
        for n in nums:
            r[n - 1] = 0
        for i in range(len(r) - 1, -1, -1):
            if not r[i]:
                r.pop(i)
        return r
```
- 应题目进阶要求，此解为 O(N) 时间效率，无额外空间（除了返回数组和中间变量）
- 初始化返回列表为数字 1 ~ n，值=索引+1
- 遍历nums，并把r中出现在nums中的值全部置0，这里实际上利用r为递增序列的特性，近似为一个哈希表
- 删除r中所有0元素
- 为什么不在遍历nums到时候直接删除r中的值？直接删除会影响后续索引的定位，因此需要逆遍历数组
## 哈希表
### [1. Two Sum 字典](https://leetcode.com/problems/two-sum/)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
	d = {}
	for i, n in enumerate(nums): 
	    if n in d: return [d[n], i]
	    d[target-n] = i
```
- 建立一个字典，每次遍历过一个值就记录与其匹配的值（设置d[匹配值]=当前索引），今后遇见匹配值即可直接返回结果
## 链表
### [206. Reverse Linked List 前后指针](https://leetcode.com/problems/reverse-linked-list/)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        p = None
        while head:
            head.next, head, p = p, head.next, head
        return p
```
- 此处利用 python 多重赋值表达式的特性（例：`a, b = b, a`），python 中多变量同时赋值时，右手边的表达式在任何赋值发生之前就被求值了。右手边的表达式是从左到右被求值的
## 数学
### [268. Missing Number 等差数列](https://leetcode.com/problems/missing-number/)
```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        s = sum(nums)
        n += 1
        return n * (n - 1) // 2 - s
```
- 缺失数字 = 0 加到 n+1 的总和 - 数组中所有数字的总和
- 计算 0 加到 n+1 的总和，可利用等差数列求和公式，此题可理解为`总和 = (元素个数 / 2) * (首尾两数字之和)`
## 双指针
### [344. Reverse String 双向指针](https://leetcode.com/problems/reverse-string/)
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        i, j = 0, len(s) - 1
        while i < j:
            s[i], s[j] = s[j], s[i]
            i += 1
            j -= 1
```
## 字符串
### [13. Roman to Integer 哈希表](https://leetcode.com/problems/roman-to-integer/)
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        d = {'I':1, 'IV':3, 'V':5, 'IX':8, 'X':10, 'XL':30, 'L':50, 'XC':80, 'C':100, 'CD':300, 'D':500, 'CM':800, 'M':1000}
        r = d[s[0]]
        for i in range(1, len(s)):
            r += d.get(s[i-1:i+1], d[s[i]])
        return r
```
- 构建一个字典记录所有罗马数字子串，注意长度为2的子串记录的值是（实际值-子串内左边罗马数字代表的数值）
- dict.get(key, default)：判断字典中键key是否存在，存在返回对应的value否则返回default（预定义的值）
- 遍历整个s的时候判断当前位置和前一个位置的两个字符组成的字符串是否在字典内，如果在就记录值，不在就说明当前位置不存在小数字在前面的情况，直接记录当前位置字符对应值。整个过程时间复杂度为O(N)
- 举个例子，遍历经过IV的时候先记录I的对应值1再往前移动一步记录IV的值3，加起来正好是IV的真实值4
## 二分查找
- :paperclip:【知识卡片】二分查找利用已排好序的数组，每一次查找都可以将查找范围减半。查找范围内只剩一个数据时查找结束。数据量为 n 的数组，将其长度减半 log2n 次后，其中便只剩一个数据了。也就是说，在二分查找中重复执行“将目标数据和数组中间的数据进行比较后将查找范围减半”的操作 log2n 次后，就能找到目标数据（若没找到则可以得出数据不存在的结论），因此它的时间复杂度为 O(logn)
- :tophat:【套路】
	```python
	self.__class__.__getitem__ = lambda self, x: 向左搜索的条件
	寻找的索引 = bisect.bisect_left(self, True, 0, len(nums)) - 1
	```
	- python 中 bisect 模块针对的是 list, 如果直接构造 list，时间复杂度为 O(N)，因此我们修改当前类的魔法方法伪造 list
	- bisect.left(用于比较的数组，搜索的值，起始范围(包括)，终止范围(不包括))：返回应该插入的位置
### [162. Find Peak Element 二分查找](https://leetcode.com/problems/find-peak-element/)
```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        l = 0
        r = len(nums) - 1
        
        while l < r:
            m = (l + r) // 2
            if m > 0 and nums[m - 1] > nums[m]:
                r = m - 1
            elif m < len(nums) and nums[m + 1] > nums[m]:
                l = m + 1
            else:
                return m
        return l
```
- 初始化搜索范围为[0, len(nums)-1]，初始搜索位置为中间位置 m，如果 m 左边存在值比 nums[m] 大，说明[0, m-1]一定存在峰值，我们缩小搜索范围；否则如果 m 右边存在值比 nums[m] 大，说明[m+1, len(nums)-1]一定存在峰值，我们缩小范围；否则 m 就是峰值
- 自古真情留不住，唯有套路得人心，[2行题解](#162-Find-Peak-Element-2行)
## 位运算
### [461. Hamming Distance 异或](https://leetcode.com/problems/hamming-distance/)
```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        return bin(x ^ y).count('1')
```
# 解法汇总贡献者
注：此处贡献名单仅代表汇总搜集贡献，不代表全部原创，欢迎所有更短的解法🤓
- [Knife丶](https://github.com/cy69855522)[QQ1272068154  微信ly18597591102]
- [zdylzdyl](https://github.com/zdylzdyl)
