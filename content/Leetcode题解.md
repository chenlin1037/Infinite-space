# leetcode算法题解
## 第一部分
### 两数之和


```python
from typing import List


class Solution:

    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_dict = {}
        result = []

        for i, num in enumerate(nums):
            complement = target - num
            if complement in num_dict:
                result.append([num_dict[complement], i])  # 使用圆括号修正错误
            num_dict[num] = i

        return result


s = Solution()

print(s.twoSum([2, 7,11,7], 9))
```

    [[0, 1], [0, 3]]


### 最长的无重复字符子串


```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0
        left = 0
        len_max = 0
        num_dict = {}

        for right, char in enumerate(s):
            if char in num_dict and num_dict[char] >= left:
                left = num_dict[char] + 1
            num_dict[char] = right
            len_max = max(len_max,right - left + 1)

        return len_max
s = Solution()
print(s.lengthOfLongestSubstring("abbcds"))
```

    4


### 最长回文子串


```python
class Solution:
    def expand_around_center(self, s, left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left+1:right]

    def longest_palindromic_substring(self, s):
        if len(s) < 1:
            return ""

        longest = ""
        for i in range(len(s)):
            odd_palindrome = self.expand_around_center(s, i, i)
            even_palindrome = self.expand_around_center(s, i, i + 1)

            if len(odd_palindrome) > len(longest):
                longest = odd_palindrome
            if len(even_palindrome) > len(longest):
                longest = even_palindrome

        return longest


# 创建 Solution 类的实例
sol = Solution()

# 调用 longest_palindromic_substring 方法，并传入字符串参数
result = sol.longest_palindromic_substring("abcddf")

# 打印输出结果
print(result)
```

    dd


回文字符串 暴力法


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        str = ""
        if len(s) <= 1:
            return s
        for left in range(len(s)):
            for right in range(left+1, len(s)):
                temp = s[left:right+1]
                reversed_temp = temp[::-1]
                if temp == reversed_temp:
                    if len(temp) > len(str):
                        str = temp
                

        return str
    

# 创建 Solution 类的实例
sol = Solution()

# 调用 longestPalindrome 方法，并传入字符串参数
result = sol.longestPalindrome("abcdddf")

# 打印输出结果
print(result)
```

    ddd


中心扩展法 与第一种方法类似，只是需要考虑奇数和偶数的情况。


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        longest = ""
        for i in range(len(s)):
            # 以s[i]为中心向两边展开，找到最长的回文串（奇数长度）
            left, right = i, i
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            if right - left - 1 > len(longest):
                longest = s[left + 1:right]

            # 以s[i]和s[i+1]为中心向两边展开，找到最长的回文串（偶数长度）
            left, right = i, i + 1
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            if right - left - 1 > len(longest):
                longest = s[left + 1:right]

        return longest


# 创建 Solution 类的实例
sol = Solution()

# 调用 longestPalindrome 方法，并传入字符串参数
result = sol.longestPalindrome("abccdf")

# 打印输出结果
print(result)
```

    cc


### 最接近的三数之和

nums = [-1,2,1,-4], target = 1 # -4-1 0 2 4   3 


```python
from typing import List
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()  # 对数组进行排序
        closest_sum = float('inf')  # 初始化最接近目标值的三数之和

        for i in range(len(nums)):
            left, right = i + 1, len(nums) - 1

            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]  # 当前三数之和
                if current_sum == target:
                    return current_sum
                if abs(current_sum - target) < abs(closest_sum - target):
                    closest_sum = current_sum  # 更新最接近目标值的三数之和

                if current_sum < target:
                    left += 1
                elif current_sum > target:
                    right -= 1

        return closest_sum

sol = Solution()

# 测试代码
nums = [-1, 0, 2, 3, 3, 4]
target = 5
result = sol.threeSumClosest(nums, target)
print(result)
```

    5

### 最接近给定目标值target的值

这段代码定义了一个名为Solution的类，其中包含一个方法threeSumClosest，用于找到一个整数数组nums中三个元素的和最接近给定目标值target的值。整个过程采用双指针法来优化搜索。

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。栈


```python
class Solution:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        else:
            raise IndexError("pop from an empty stack")

    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        else:
            return False

    def isValid(self, s: str) -> bool:
        stack = Solution()
        map = {')': '(', '}': '{', ']': '['}
        for char in s:
            if char in map.values():
                stack.push(char)
            elif char in map.keys():
                if map[char] != stack.peek() or stack.is_empty():
                    return False
                else:
                    stack.pop()

        return stack.is_empty()
```

### 移出数组元素

输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]


```python
from typing import List


class Solution:

    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        

        num_list = []

        for char in nums:
            if char in num_list :
               
                # 如果字符已经出现过，则跳过
                continue
            num_list.append(char)
            

        return num_list


# 创建 Solution 类的实例
sol = Solution()

# 测试代码
nums = [1, 1, 2, 3, 3, 7]
result = sol.removeDuplicates(nums)
print(result)
```

    [1, 2, 3, 7]



```python
from typing import List
# nums = [1, 1, 2, 3, 3, 7]

class Solution:

    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        current = 1
        while current < len(nums):
            if nums[current] == nums[current - 1]:
                nums.pop(current)
            else:
                current += 1
        

        return nums


# 创建 Solution 类的实例
sol = Solution()

# 测试代码
nums = [1, 1, 2, 3, 3, 7]
result = sol.removeDuplicates(nums)
print(result)
```

    [1, 2, 3, 7]



```python
from typing import List
# nums = [1, 1, 2, 3, 3, 7]


class Solution:

    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        current = 1
        for i in range(1, len(nums)):
            if nums[i] != nums[i - 1]:
                nums[current] = nums[i]
                current += 1
        nums = nums[:current]
        return nums


# 创建 Solution 类的实例
sol = Solution()

# 测试代码
nums = [1, 1, 2, 3, 3, 7]
result = sol.removeDuplicates(nums)
print(result)
```

    [1, 2, 3, 7]


### 给定一组非负整数，要求重新组合成一个最大的数

nums = [3, 30, 34, 5, 9]


```python
from typing import List


class Solution:
    def largestNumber(self, nums: List[int]) -> str:

        nums = sorted(nums, key=lambda x: (str(x) * 3), reverse=True)
        return ''.join(map(str, nums))


nums = [3, 30, 34, 5, 9]
sol = Solution()
result = sol.largestNumber(nums)
print(result)
```

    9534330

### 给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

利用 sorted 函数，通过 key=lambda x: (str(x) * 3), reverse=True 实现上述排序逻辑。这里，key 函数定义了排序依据，即将每个数字转换为字符串并重复三次后转为整数进行比较，reverse=True 表示按降序排序。

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。使用快慢指针


```python
# Definition for singly-linked list.
from typing import Optional, List

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next



class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 虚拟头节点
        dummy = ListNode()
        dummy.next = head
        # 先用双指针找到倒数第n+1个节点
        p1 = dummy
        for i in range(n+1):
            p1 = p1.next
        p2 = dummy
        while p1:
            p1 = p1.next
            p2 = p2.next
        # 找到后把倒数第n+1个节点直接指向倒数第n-1个节点 于是就删除了倒数第n个节点
        p2.next = p2.next.next
        return dummy.next


```

### 逆序一个链表


```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def reverse_list_iteratively(self,head):
        prev = None
        current = head
        while current:
            next_temp = current.next
            current.next = prev
            prev = current
            current = next_temp
        return prev



head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
sol = Solution()
result = sol.reverse_list_iteratively(head)
current = result
while current:
    print(current.val, end=" ")
    current = current.next
```

    3 2 1 

递归方法逆序


```python
def reverse_list_recursively(head):
    if not head or not head.next:
        return head
    new_head = reverse_list_recursively(head.next)
    head.next.next = head
    head.next = None
    return new_head
```

### 递归逆序数组


```python
def reverse_array(arr, start, end):
    if start >= end:
        return
    arr[start], arr[end] = arr[end], arr[start]
    reverse_array(arr, start + 1, end - 1)


# 测试代码
arr = [1, 2, 3, 4, 5]
print("Original Array:", arr)
reverse_array(arr, 0, len(arr) - 1)
print("Reversed Array:", arr)
```

    Original Array: [1, 2, 3, 4, 5]
    Reversed Array: [5, 4, 3, 2, 1]


输入：sales = [-2,1,-3,4,-1,2,1,-5,4]
-1  -4 0 -1 1 2 -3 1
输出：6
解释：[4,-1,2,1] 此连续四天的销售总额最高，为 6。


```python
from typing import List


class Solution:
    def maxSales(self, sales: List[int]) -> int:
        current_sum = sales[0]
        max_sum = sales[0]

        for i in range(1, len(sales)):
            current_sum = max(current_sum + sales[i], sales[i])
            max_sum = max(max_sum, current_sum)

        return max_sum


# 测试
if __name__ == '__main__':
    s = Solution()
    sales = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
    print(s.maxSales(sales))
```

    6


### 给链表排序，使用归并排序，时间复杂度O(nlogn)，空间复杂度O(logn)


```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head

        # 找到链表中点
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        mid = slow.next
        slow.next = None

        # 递归分割链表
        left = self.sortList(head)
        right = self.sortList(mid)

        # 合并有序链表
        dummy = ListNode(0)
        curr = dummy
        while left and right:
            if left.val < right.val:
                curr.next = left
                left = left.next
            else:
                curr.next = right
                right = right.next
            curr = curr.next

        curr.next = left if left else right

        return dummy.next


# 测试
head = ListNode(4)
head.next = ListNode(2)
head.next.next = ListNode(1)
head.next.next.next = ListNode(3)

solution = Solution()
sorted_head = solution.sortList(head)

current = sorted_head
while current:
    print(current.val, end=" ")
    current = current.next
```

    1 2 3 4 

### 保留非重复值


```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


def deleteDuplicates(head: ListNode) -> ListNode:
    if not head or not head.next:
        return head

    dummy = ListNode(0)
    dummy.next = head
    curr = dummy

    while curr.next and curr.next.next:
        if curr.next.val == curr.next.next.val:
            
            curr.next = curr.next.next
        else:
            curr = curr.next

    return dummy.next


# 创建链表
head = ListNode(1)
head.next = ListNode(1)
head.next.next = ListNode(2)
head.next.next.next = ListNode(3)
head.next.next.next.next = ListNode(3)

# 删除重复节点
new_head = deleteDuplicates(head)

# 打印删除重复节点后的链表
while new_head:
    print(new_head.val, end=" ")
    new_head = new_head.next
```

    1 2 3 

### 删除1链表中在2链表中出现的节点


```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


def deleteNodesWithSameValue(head1: ListNode, head2: ListNode) -> ListNode:
    dummy = ListNode(0)
    dummy.next = head1
    prev = dummy  # 初始指向头部节点
    curr = dummy.next  # 初始指向头部的下一个节点

    while curr:
        # 判断当前节点的值是否在链表2中存在
        if isValueInList(curr.val, head2):
            prev.next = curr.next
        else:
            prev = curr
        curr = curr.next

    return dummy.next


def isValueInList(value, head: ListNode) -> bool:
    curr = head
    while curr:
        if curr.val == value:
            return True
        curr = curr.next
    return False


# 创建链表1：1 -> 2 -> 3 -> 4 -> 5
head1 = ListNode(1)
head1.next = ListNode(2)
head1.next.next = ListNode(3)
head1.next.next.next = ListNode(4)
head1.next.next.next.next = ListNode(5)

# 创建链表2：3 -> 5
head2 = ListNode(1)
head2.next = ListNode(5)

# 删除链表1中节点的值在链表2中存在的节点
new_head = deleteNodesWithSameValue(head1, head2)

# 打印删除节点后的链表1
while new_head:
    print(new_head.val, end=" ")
    new_head = new_head.next
```

    2 3 4 

### 找出那个只出现了一次的元素

给你一个 非空 整数数组 nums ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 

示例 1 ：

输入：nums = [2,2,1]
输出：1


```python
from typing import List


class Solution:
    

    def singleNumber(self, nums: List[int]) -> int:
        list = []
        for num in nums:
            if num in list:
                list.remove(num)
            else:
                list.append(num)
        val = list[0]
        return val


# 测试
if __name__ == '__main__':
    nums = [4, 1, 2, 1, 2]
    print(Solution().singleNumber(nums))
```

    4

### 只出现了一次的元素

给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法且使用常数级空间来解决此问题。
输入：nums = [2,2,3,2]
输出：3


```python
class Solution:
    def find(self,dict):
        for k,v in dict.items():
            if v == 1:
                return k
    
    
    def singleNumber(self, nums: List[int]) -> int:
        dict = {}
        for num in nums:
            if num in dict:
                dict[num] += 1
                
            else:
                dict[num] = 1
        return self.find(dict)
       
##测试
s = Solution()
nums = [2, 2, 3, 2,1,3,3]
print(s.singleNumber(nums))
```

    1


### 给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

如果链表中存在环 ，则返回 true 。 否则，返回 false 。


```python

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast = slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True
        return False

```

### 给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 
如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。


```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    

    def detectCycle(self, head: ListNode) -> ListNode:
        fast = slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                break
        if not fast or not fast.next:
            return None  # 无环

        slow = head
        while slow != fast:
            slow = slow.next
            fast = fast.next

        return slow
```


```python
#得到链表的第n个节点
def getNthFromLast(head,n):
    # Code here
    temp=head
    count=0
    while temp:
        count+=1
        temp=temp.next
    if n>count:
        return -1
    temp=head
    for i in range(count-n):
        temp=temp.next
    return temp.data
```
### 删除重复出现的元素
给你一个 非严格递增排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

考虑 nums 的唯一元素的数量为 k ，你需要做以下事情确保你的题解可以被通过：

更改数组 nums ，使 nums 的前 k 个元素包含唯一元素，并按照它们最初在 nums 中出现的顺序排列。nums 的其余元素与 nums 的大小不重要。
返回 k 。


```python
from typing import List


class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)

        new_nums = []
        for num in nums:
            if num not in new_nums:
                new_nums.append(num)

        nums.clear()
        nums.extend(new_nums)

        return len(new_nums)
s = Solution()
print(s.removeDuplicates([0,0,1,1,1,2,2,3,3,4]))
```

    5


### 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 


```python
from typing import List


class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[slow] = nums[i]
                slow += 1
        return slow


# 测试
nums = [3, 2, 2, 3]
val = 3
print(Solution().removeElement(nums, val))  # 输出 2
```

    2


### 整数数组的一个 排列  就是将其所有成员以序列或线性顺序排列。

例如，arr = [1,2,3] ，以下这些都可以视作 arr 的排列：[1,2,3]、[1,3,2]、[3,1,2]、[2,3,1] 。
整数数组的 下一个排列 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 下一个排列 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

例如，arr = [1,2,3] 的下一个排列是 [1,3,2] 。
类似地，arr = [2,3,1] 的下一个排列是 [3,1,2] 。
而 arr = [3,2,1] 的下一个排列是 [1,2,3] ，因为 [3,2,1] 不存在一个字典序更大的排列。
给你一个整数数组 nums ，找出 nums 的下一个排列。

必须 原地 修改，只允许使用额外常数空间。

 


```python
from typing import List


class Solution:

    def nextPermutation(self, nums: List[int]) -> None:
            # 从右往左找到第一个降序的位置 i
        i = len(nums) - 2
        while i >= 0 and nums[i] >= nums[i + 1]:
            i -= 1
        
        # 如果 i >= 0，说明找到了这样一个位置
        if i >= 0:
            # 从右往左找到第一个比 nums[i] 大的位置 j
            j = len(nums) - 1
            while j >= 0 and nums[j] <= nums[i]:
                j -= 1
            # 交换 nums[i] 和 nums[j]
            nums[i], nums[j] = nums[j], nums[i]
        
        # 将 i 后面的部分逆序
        left, right = i + 1, len(nums) - 1
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
            right -= 1



# 测试样例
nums = [1, 2, 3]
Solution().nextPermutation(nums)
print(nums)
```

    [1, 3, 2]


### 给你一个整数数组 nums 。如果任一值在数组中出现 至少两次 ，返回 true ；如果数组中每个元素互不相同，返回 false 。
输入：nums = [1,2,3,1]
输出：true


```python
from typing import List
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        if len(nums) <= 1:
            return False

        new_nums = []
        for num in nums:
            if num in new_nums:
                return True
            else:
                new_nums.append(num)

        return False
##测试
nums = [1,2,3,4]
print(Solution().containsDuplicate(nums))
```

    False



```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums.sort()
        for i in range(len(nums)-1):
            if nums[i] == nums[i+1]:
                return True
        return False
###测试
nums = [1,2,3,1]
print(Solution().containsDuplicate(nums))

##复杂度是O(nlogn)
```

    True


### 给你一个整数数组 nums 和一个整数 k ，判断数组中是否存在两个 不同的索引 i 和 j ，满足 nums[i] == nums[j] 且 abs(i - j) <= k 。如果存在，返回 true ；否则，返回 false 。
输入：nums = [1,2,3,1], k = 3
输出：true


```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        dict = {}
        for num in nums:
            if num in dict:
                dict[num] += 1

            else:
                dict[num] = 1
        
```

### 给定一个会议时间安排的数组 intervals ，每个会议时间都会包括开始和结束的时间 intervals[i] = [starti, endi] ，请你判断一个人是否能够参加这里面的全部会议。


```python
from typing import List
class Solution:

    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        intervals.sort(key=lambda x: x[0])  # 按开始时间排序
        n = len(intervals)
        for i in range(1, n):
            if intervals[i][0] < intervals[i-1][1]:
                return False  # 存在重叠的会议时间
        return True
##测试
intervals = [[0,30],[5,10],[15,20]]
s = Solution()
print(s.canAttendMeetings(intervals))
```

    False



```python
from typing import List



class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        intervals.sort(key=lambda x: x[0])  # 按开始时间排序
        intervals_list = []
        for num in range(len(intervals)):  # 遍历排序后的数组
            intervals_list.append(intervals[num][0])
            intervals_list.append(intervals[num][1])  # 将开始时间和结束时间添加到数组中
        for i in range(1, len(intervals_list) - 2, 2):  # 遍历数组，步长为2
            if intervals_list[i] >= intervals_list[i + 1]:  # 如果开始时间大于等于结束时间
                return False
        return True


# 测试
intervals = [[0, 30], [5, 10], [15, 20]]
print(Solution().canAttendMeetings(intervals))  # 应返回 False，因为第二个和第三个会议时间有重叠


```

    False

### 中心对称
中心对称数是指一个数字在旋转了 180 度之后看起来依旧相同的数字（或者上下颠倒地看）。

请写一个函数来判断该数字是否是中心对称数，其输入将会以一个字符串的形式来表达数字。


```python
class Solution:
    def isStrobogrammatic(self, num: str) -> bool:
        for i in range(len(num)):
            if num[i] not in ["1", '0', '6', '8', '9']:
                return False
            elif num[i] == '0' or num[i] == '8' or num[i] == '1':
                if num[i] != num[-i - 1]:
                    return False
            elif num[i] == '6':
                if num[-i - 1] != '9':
                    return False
            elif num[i] == '9':
                if num[-i - 1] != '6':
                    return False
        return True


# 测试
s = Solution()
print(s.isStrobogrammatic("69"))  # 应返回 True
```

    True


给定一个大小为 n 的数组 nums ，返回其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。
输入：nums = [3,2,3]
输出：3
输入：nums = [2,2,1,1,1,2,2]
输出：2


```python
from typing import List
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums_new:dict = {}
        for num in nums:
            if num in nums_new:
                nums_new[num] += 1
            else:
                nums_new[num] = 1
        if len(nums) == 1:
            return nums[0]
        for num in nums_new:
            if nums_new[num] > len(nums) / 2:
                return num
##测试
s = Solution()
print(s.majorityElement([3,2,3,3,2,2,2]))

        
```

    2


排序法：排序后，出现次数大于一半的肯定在中间


```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        return nums[len(nums)//2]
s = Solution()
print(s.majorityElement([3,2,3,3,2,2,2]))
```

    2


摩尔投票算法：摩尔投票算法的时间和空间都很低，时间复杂度为O(n)，空间复杂度为O(1)
可以用来求绝对众数，即大于一半的数。原理把第一个数看作众数，然后遍历数组，如果遇到和众数不同的数，则抵消一个数，如果遇到和众数相同的数，则抵消一个数，最后剩下的数就是绝对众数。


```python
from typing import List


class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        # 初始化当前元素为列表的第一个元素，并设定计数为1
        current = nums[0]
        count = 1

        # 从第二个元素开始遍历整个列表
        for i in range(1, len(nums)):
            # 如果当前遍历到的元素与我们跟踪的元素相同，则增加计数
            if nums[i] == current:
                count += 1
            else:
                # 遇到不同的元素时，减少计数
                # 当计数减至0时，更换跟踪的元素为当前遍历到的元素，并重置计数为1
                count -= 1
                if count == 0:
                    current = nums[i]
                    count = 1

        # 遍历结束后，current变量保存的就是可能的多数元素。
        # 根据题目假设（存在多数元素），我们直接返回current作为结果。
        # 实际上，为了严格验证这个元素确实是多数元素，理论上还需要额外的计数步骤，
        # 但在本题的特定条件下，根据算法逻辑，current已经是正确的多数元素。

        return current


# 创建Solution类的实例
s = Solution()
# 调用实例方法并打印结果
print(s.majorityElement([1, 2,3,4,3,1,3,3]))
```

    3


### 给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。
输入: [3,2,1,5,6,4], k = 2
输出: 5
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4


```python

from typing import List
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[-k]
##测试
nums = [3,2,1,5,6,4]
k = 2
print(Solution().findKthLargest(nums,k))
```

    5


### 找出所有相加之和为 n 的 k 个数的组合
且满足下列条件：

只使用数字1到9
每个数字 最多使用一次 
返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。


```python
from typing import List

class Solution:
    def combinationSum(self, k: int, n: int) -> List[List[int]]:
        def backtrack(start, target, path):
            if target == 0 and len(path) == k:
                res.append(path[:])
                return
            if target < 0 or len(path) == k:
                return
            for i in range(start, 10):
                path.append(i)
                backtrack(i + 1, target - i, path)
                path.pop()

        res = []
        backtrack(1, n, [])
        return res

# 测试
s = Solution()
print(s.combinationSum(3, 7))  # 应返回 [[1,2,4]]
```

    [[1, 2, 4]]


### 给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
123456      321


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head

        prev = None
        while current is not None:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node

        return prev
```
### 判断回文
如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 回文串 。

字母和数字都属于字母数字字符。

给你一个字符串 s，如果它是 回文串 ，返回 true ；否则，返回 false 。


```python
import re
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = s.lower()
        s = re.sub('[^a-z0-9]', '', s)
        #return s
        return s == s[::-1]
s = Solution()
print(s.isPalindrome("A man, a plan, a canal: Panama"))
```

    True


### 给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。


```python
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        current = head
        prev = None

        while current:
            if current.val == val:
                if not prev:
                    head = current.next
                else:
                    prev.next = current.next
            else:
                prev = current

            current = current.next

        return head


# 构建链表
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(6)
head.next.next.next = ListNode(3)
head.next.next.next.next = ListNode(4)
head.next.next.next.next.next = ListNode(5)
head.next.next.next.next.next.next = ListNode(6)

solution = Solution()
result = solution.removeElements(head, 6)

# 输出结果
while result:
    print(result.val)
    result = result.next
```

    1
    2
    3
    4
    5


### 前序遍历二叉树


```python
##二叉树前序排列
# Definition for a binary tree node.
from typing import List
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        result = []
        stack = [root]

        while stack:
            node = stack.pop()
            result.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)

        return result
##测试
root = TreeNode(1)
root.right = TreeNode(2)
root.right.left = TreeNode(3)
print(Solution().preorderTraversal(root))
```

    [1, 2, 3]


### 中序遍历二叉树


```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = []
        current = root

        while stack or current:
            while current:
                stack.append(current)
                current = current.left

            current = stack.pop()
            result.append(current.val)
            current = current.right

        return result
```


```python
##后序遍历
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = []
        current = root
        last_visited = None

        while stack or current:
            while current:
                stack.append(current)
                current = current.left

            peek_node = stack[-1]

            if peek_node.right and last_visited != peek_node.right:
                current = peek_node.right
            else:
                node = stack.pop()
                result.append(node.val)
                last_visited = node

        return result
```

### 给你一个整数数组 A，请找出并返回在该数组中仅出现一次的最大整数。

如果不存在这个只出现一次的整数，则返回 -1。


```python
from typing import List


class Solution:
    def largestUniqueNumber(self, nums: List[int]) -> int:
        num_dict = {}
        nums.sort()
        new_list = []

        for num in nums:
            if num not in num_dict:
                num_dict[num] = 1
            else:
                num_dict[num] += 1

        for num in num_dict:
            if num_dict[num] == 1:
                new_list.append(num)

        if not new_list:
            return -1

        return new_list[-1]


s = Solution()
print(s.largestUniqueNumber([1, 2, 3, 3, 4, 4, 5]))
```

    5

### 动态规划之苹果
你有一些苹果和一个可以承载 5000 单位重量的篮子。

给定一个整数数组 weight ，其中 weight[i] 是第 i 个苹果的重量，返回 你可以放入篮子的最大苹果数量 。
输入：weight = [100,200,150,1000]
输出：4
解释：所有 4 个苹果都可以装进去，因为它们的重量之和为 1450。


```python
class Solution:
    def maxNumberOfApples(self, weight: List[int]) -> int:
        weight.sort()
        current = 0
        sum = 0
        while current < len(weight) and sum + weight[current] <= 5000:
            sum += weight[current]
            current += 1
        
        return current
s = Solution()
print(s.maxNumberOfApples([100, 200, 150, 1000]))
```

    4

### 最小绝对差的元素对
给你个整数数组 arr，其中每个元素都 不相同。

请你找到所有具有最小绝对差的元素对，并且按升序的顺序返回。

每对元素对 [a,b] 如下：

a , b 均为数组 arr 中的元素
a < b
b - a 等于 arr 中任意两个元素的最小绝对差


```python
class Solution:
    def minimumAbsDifference(self, arr: List[int]) -> List[List[int]]:
        arr.sort()
        min = 999999
        new_list = []
        for i in range(len(arr)-1):
            current = arr[i+1] - arr[i]
            if current < min:
                min = current
        for j in range(len(arr)-1):
            current = arr[j+1] - arr[j]
            if current == min:
                result = [arr[j], arr[j+1]]
                new_list.append(result)

        return new_list
#测试
s = Solution()
print(s.minimumAbsDifference([4,7,10,3]))
```

    [[3, 4]]


### 给你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。

你可以 任意多次交换 在 pairs 中任意一对索引处的字符。

返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。
输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释： 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"


```python
from collections import defaultdict


class UnionFind:
    def __init__(self, n):
        self.parent = [i for i in range(n)]

    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x != root_y:
            self.parent[root_x] = root_y


s = "dcab"
pairs = [[0, 3], [1, 2], [0, 2]]


class Solution:
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        uf = UnionFind(len(s))

        for pair in pairs:
            uf.union(pair[0], pair[1])

        groups = defaultdict(list)
        for i in range(len(s)):
            root = uf.find(i)
            groups[root].append(i)

        s_list = list(s)
        for group in groups.values():
            chars = [s_list[i] for i in group]
            chars.sort()
            for i, char in zip(sorted(group), chars):
                s_list[i] = char

        return "".join(s_list)


print(Solution().smallestStringWithSwaps(s, pairs))
```

    abcd



```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        num1: int = int(num1)
        num2: int = int(num2)
        return str(num1 * num2)
```

### 数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。


```python
from typing import List
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def generate_helper(current, left, right):
            if len(current) == 2 * n:
                if left == right == n:
                    result.append(current)
                return
            if left < n:
                generate_helper(current + '(', left + 1, right)
            if right < left:
                generate_helper(current + ')', left, right + 1)

        result = []
        generate_helper("", 0, 0)
        return result
```

### 给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串的第一个匹配项的下标（下标从 0 开始）。如果 needle 不是 haystack 的一部分，则返回  -1 。


```python
class Solution:
    def build_lps_array(self,pattern):
        m = len(pattern)
        lps = [0] * m
        j = 0
        i = 1
        while i < m:
            if pattern[i] == pattern[j]:
                j += 1
                lps[i] = j
                i += 1
            else:
                if j != 0:
                    j = lps[j-1]
                else:
                    lps[i] = 0
                    i += 1
        return lps
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0
    
        m = len(haystack)
        n = len(needle)
        lps = self.build_lps_array(needle)
        
        i = 0
        j = 0
        while i < m:
            if haystack[i] == needle[j]:
                i += 1
                j += 1
                if j == n:
                    return i - n
            else:
                if j != 0:
                    j = lps[j-1]
                else:
                    i += 1
        
        return -1
##测试样例
if __name__ == '__main__':
    s = Solution()
    haystack = "hello"
    needle = "ll"
    print(s.strStr(haystack, needle))
        
```

    2


### 字串是否包括回文串


```python
class Solution:
    def list_all_substrings(self,s):
        substrings = []
        for i in range(len(s)):
            for j in range(i+2, len(s)+1):
                substrings.append(s[i:j])
        return substrings



    def canPermutePalindrome(self, s: str) -> bool:
        if len(s) == 1:
            return True
        else:
            substrings = self.list_all_substrings(s)
            for substring in substrings:
                if substring == substring[::-1]:
                    return True
            return False
##测试样例
s = Solution()
print(s.canPermutePalindrome("ab"))

```

    False


### 给你一个字符串 s ，如果该字符串的某个排列是 回文串，则返回 true ；否则，返回 false 。就是元素重新排列，可以组成回文串


```python
class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        # 对字符串中字母进行计数，保证至多有一个字母出现次数为奇数
        count = {}
        for i in s:
            if i in count:
                count[i] = count[i]+1
            else:
                count[i] = 1
        j = 0
        for i in count:
            if count[i] % 2 == 1:
                j = j+1
        if j > 1:
            return False
        else:
            return True
```


```python
from typing import List


class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        start = 0
        end = len(s) - 1
        while start < end:
            s[start], s[end] = s[end], s[start]
            start += 1
            end -= 1


# 测试
s = ["h", "e", "l", "l", "o"]
solution = Solution()
solution.reverseString(s)
print(s)  # Output: ["o", "l", "l", "e", "h"]
```

    ['o', 'l', 'l', 'e', 'h']


### 给定一个已排序的链表的头 head ， 删除所有重复的元素，使每个元素只出现一次 。返回 已排序的链表 。
1223456


```python
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        
        curr = head

        while curr and curr.next:
            if curr.val == curr.next.val:
                curr.next = curr.next.next
            else:
                curr = curr.next

        return head


# 测试
head = ListNode(1)
head.next = ListNode(1)
head.next.next = ListNode(2)
head.next.next.next = ListNode(3)
head.next.next.next.next = ListNode(3)

solution = Solution()
result = solution.deleteDuplicates(head)
while result:
    print(result.val)
    result = result.next
```

    1
    2
    3



```python
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        current = head
        prev = None

        while current:
            if current.val == val:
                if not prev:
                    head = current.next
                else:
                    prev.next = current.next
            else:
                prev = current

            current = current.next

        return head


# 构建链表
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(6)
head.next.next.next = ListNode(3)
head.next.next.next.next = ListNode(4)
head.next.next.next.next.next = ListNode(5)
head.next.next.next.next.next.next = ListNode(6)

solution = Solution()
result = solution.removeElements(head, 6)

# 输出结果
while result:
    print(result.val)
    result = result.next
```

    1
    2
    3
    4
    5



```python
from typing import Optional


class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    

    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head

        dummy = ListNode(0)
        dummy.next = head
        prev = dummy
        curr = head

        while curr and curr.next:
            if curr.val == curr.next.val:
                val = curr.val
                while curr and curr.val == val:
                    curr = curr.next
                prev.next = curr
            else:
                prev = curr
                curr = curr.next

        return dummy.next


# 测试
head = ListNode(1)
head.next = ListNode(1)
head.next.next = ListNode(2)
head.next.next.next = ListNode(3)
head.next.next.next.next = ListNode(3)

solution = Solution()
result = solution.deleteDuplicates(head)
while result:
    print(result.val, end=" ")
    result = result.next
```

    2 

### 给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素只出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

下面代码新建了一个数组


```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        
        i = 0
        result = []
        if len(nums)<= 2:
            return len(nums)
        for i in range(len(nums)):
        
            while i< len(nums)-2:
                if nums[i] == nums[i+1] and nums[i] == nums[i+2]:
                    result.append(nums[i])
                    result.append(nums[i])
                    val = nums[i]
                    while i< len(nums)-2 and nums[i] == val:
                        i+=1
                else:
                    result.append(nums[i])
                    i+=1
            result.append(nums[-2])
            result.append(nums[-1])
            return len(result)


nums = [1, 1, 1, 2, 2, 3]
s = Solution()
print(s.removeDuplicates(nums))
```

    5


下面代码在原地址修改


```python
from typing import List


class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 2:
            return len(nums)

        i = 2
        while i < len(nums):
            if nums[i] == nums[i - 2]:
                nums.pop(i)
            else:
                i += 1

        return len(nums)
```

### 给你一个非空数组，返回此数组中 第三大的数 。如果不存在，则返回数组中最大的数。

12333345


```python
from typing import List


class Solution:
    def thirdMax(self, nums: List[int]) -> int:
        nums.sort(reverse=True)
        

        i = 1
        while i < len(nums):
            if nums[i] == nums[i - 1]:
                nums.pop(i)
            else:
                i += 1

        if len(nums) <3:
            return nums[0]
        else:
            return nums[2]
s = Solution()
print(s.thirdMax([1,1,1,2,2,3,7,6,4]))
```

    4


11233
第一次
slow = 0 f = 1
1
第二次
slow = 1 f = 2
12
nums[1] = nums[2]
第三次
slow = 2 f = 3
nums[2] = nums[3]
123
第四次
1233 + 3
所以还需要去重末尾多余的部分


### 双指针去重


```python
from typing import List


class Solution:
    def thirdMax(self, nums: List[int]) -> int:
        nums.sort(reverse=True)
        if not nums:
            return nums

        # 双指针
        slow = 0
        for fast in range(1, len(nums)):
            if nums[fast] != nums[slow]:
                slow += 1
                nums[slow] = nums[fast]
        # nums.pop()
        # 删除重复元素的尾部部分
        nums = nums[:slow + 1]

        # 返回删除重复元素后的数组
        if len(nums) <3:
                return nums[0]
        else:
            return nums[2]



nums = [1, 1, 2, 3, 3, 5]
print(Solution().thirdMax(nums))

```

    2


### 给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 BigInteger）， 也不能直接将输入的字符串转换为整数形式。


```python
class Solution:

    def addStrings(self, num1: str, num2: str) -> str:
        max_len = max(len(num1), len(num2))
        carry = 0
        result = []

        # 补齐较短字符串的前导零，使两者长度相同
        num1 = '0' * (max_len - len(num1)) + num1
        num2 = '0' * (max_len - len(num2)) + num2

        for i in range(max_len - 1, -1, -1):
            digit_sum = carry + int(num1[i]) + int(num2[i])
            carry, digit = divmod(digit_sum, 10)
            result.append(str(digit))

        if carry > 0:
            result.append(str(carry))

        # 反转结果列表并拼接成字符串
        return ''.join(result[::-1])


# 测试
s = Solution()
print(s.addStrings("123", "456"))  # 输出："579"
```

    579



```python
from typing import List
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        # 创建一个包含索引、姓名、身高的元组列表
        indexed_data = [(i, name, height)
                        for i, (name, height) in enumerate(zip(names, heights))]

        # 按照身高（第三个元素）降序对元组列表进行排序
        sorted_indexed_data = sorted(
            indexed_data, key=lambda x: x[2], reverse=True)

        # 根据排序后的索引，从原始 names 列表中提取并返回对应姓名
        sorted_names = [names[index] for _, index, _ in sorted_indexed_data]

        return sorted_names
```

### 给你一个字符串数组 names ，和一个由 互不相同 的正整数组成的数组 heights 。两个数组的长度均为 n 。

对于每个下标 i，names[i] 和 heights[i] 表示第 i 个人的名字和身高。

请按身高 降序 顺序返回对应的名字数组 names 。
输入：names = ["Alice","Bob","Bob"], heights = [155,185,150]
输出：["Bob","Alice","Bob"]
解释：第一个 Bob 最高，然后是 Alice 和第二个 Bob 。


```python
class Solution:

    def new_list(self,names):
        new_list = []
        for i in range(len(names)):
            if names[i] in new_list:
                new_list.append(names[i]+" "*i)
            else:
                new_list.append(names[i])
        return new_list
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        names = self.new_list(names)
        # 合并列表为字典，list1的元素作为键，list2的元素作为值
        merged_dict = dict(zip(names, heights))

        # 按照值（value）降序排列字典
        names = sorted(merged_dict.keys(), key=lambda k: merged_dict[k], reverse=True)
        for i, name in enumerate(names):
            names[i] = name.rstrip()

        return names
##测试
if __name__ == '__main__':
    names = ["Alice", "Bob", "Bob"]
    heights = [155,185,150]
    print(Solution().sortPeople(names,heights))
        
```

    ['Bob', 'Alice', 'Bob']


### 最小公倍数


```python
class Solution:
    def smallestEvenMultiple(self, n: int) -> int:
        if n%2==0:
            return n
        else:
            return n*2
        
```

### 给你一个由小写英文字母组成的字符串 s ，请你找出并返回第一个出现 两次 的字母。

注意：

如果 a 的 第二次 出现比 b 的 第二次 出现在字符串中的位置更靠前，则认为字母 a 在字母 b 之前出现两次。
s 包含至少一个出现两次的字母。
输入：s = "abccbaacz"
输出："c"


```python
class Solution:
    def repeatedCharacter(self, s: str) -> str:
        new_list = []
        s = list(s)
        for i in s:
            if i in new_list:
                return i
            else:
                new_list.append(i)
s = Solution()
print(s.repeatedCharacter("abccbaacz"))
```

    c


用集合实现


```python
from typing import Set


class Solution:
    def repeatedCharacter(self, s: str) -> str:
        seen_chars: Set[str] = set()

        for char in s:
            if char in seen_chars:
                return char
            seen_chars.add(char)

        # 如果未找到重复字符，返回特定值（这里选择 None）
        return None
```

### 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if list1 is None:
            return list2
        if list2 is None:
            return list1
        if list1.val < list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```


```python
from typing import Optional
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
class Solution:

    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        
        current = dummy

        while list1 is not None and list2 is not None:
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next  # 移动到下一个合并节点

        # 将剩余非空链表接到合并链表末尾
        if list1 is not None:
            current.next = list1
        elif list2 is not None:
            current.next = list2

        return dummy.next  # 返回合并后链表的实际头节点（跳过虚拟头节点）
##测试
s = Solution()
list1 = ListNode(1, ListNode(2, ListNode(4)))
list2 = ListNode(1, ListNode(3, ListNode(4)))
merged_list = s.mergeTwoLists(list1, list2)
while merged_list:
    print(merged_list.val, end=" ")
    merged_list = merged_list.next
```

    1 1 2 3 4 4 

### 合并两个有序数组


```python
class Solution:

    def mergeTwoLists(self, list1: List[int],list2: List[int]) -> List:
        
        i = 0
        j = 0
        merged_list = []
        max_num = max(list1[-1],list2[-1])

        while i<len(list1) and j<len(list2): 
                if list1[i]<list2[j]:
                     merged_list.append(list1[i])
                     i+=1
                else:
                     merged_list.append(list2[j])
                     j+=1
        merged_list.append(max_num)
        return merged_list
##测试
s = Solution()
list1 = [1,2,5]
list2 = [1,3,4]
print(s.mergeTwoLists(list1,list2))
```

    [1, 1, 2, 3, 4, 5]


递归实现


```python
from typing import List
class Solution:

    def mergeTwoLists(self, list1: List[int], list2: List[int]) -> List[int]:
        if not list1 or not list2:
            return list2 or list1

        if list1[0] < list2[0]:
            return [list1[0]] + self.mergeTwoLists(list1[1:], list2)
        else:
            return [list2[0]] + self.mergeTwoLists(list1, list2[1:])


# 测试
s = Solution()
list1 = [1, 2, 5]
list2 = [1, 3, 4]
print(s.mergeTwoLists(list1, list2))
```

    [1, 1, 2, 3, 4, 5]

