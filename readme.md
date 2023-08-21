



## 1.数组

### 解体思路：

双指针

###  [27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```python3
# 双循环 第一层找对应值 第二层将值后面的元素依次前移
class Solution:
    def removeElement(nums, val: int) -> int:
        ans = len(nums)
        idx = 0
        while idx<ans:
            item=nums[idx]
            if item == val:
                ans-=1
                for idx2 in range(idx, len(nums) - 1):
                    nums[idx2] = nums[idx2 + 1]
            else:
                idx+=1
        return ans
```

```python3
# 一次遍历/双指针 快指针向后遍历 当与值不等时 慢指针对应位置为该值
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        if len(nums) == 0:
            return 0
        # 初始化快慢指针
        fast = slow = 0

        while fast < len(nums):
            # 如果快指针指向的值不等于 val
            # fast 指向位置的元素向 slow 指向的位置赋值
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            
            # 如果快指针指向的值等于 val
            # 只 fast 指针移动，不向 slow 指针指向的位置赋值
            fast += 1

        return slow
```

### [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```python
# 模拟法 注意边界条件 需要细心
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        num = n * n
        matrix = [[0 for i in range(n)] for j in range(n)]
        up, down, left, right = 0,n,0, n
        step=0
        while step<num:
            for i in range(left,right):
                step += 1
                matrix[up][i] = step
                
            up += 1
            for j in range(up, down):
                step += 1
                matrix[j][right-1] = step
            right-=1

            for k in range(right-1, left-1, -1):
                step += 1
                matrix[down-1][k] = step
            down-=1
            for l in range(down-1, up-1, -1):
                step += 1
                matrix[l][left] = step
            left+=1
        return matrix
        
```

### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]

```python
# 平方+快排 超时 n+logn
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        a = [i * i for i in nums]
        def quick_sort(nums):
            if nums==[]:
                return []
            num = nums[0]
            left=[]
            right=[]
            for item in nums[1:]:
                if item>=num:
                    right.append(item)
                else:
                    left.append(item)
            return quick_sort(left)+[num]+quick_sort(right)
        return quick_sort(a)
```

```python
# 双指针 维护一个答案数组 注意site
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        ans = [0]*len(nums)
        l,r=0,len(nums)-1
        site=len(nums)-1
        while l<=r:
            lv=nums[l]*nums[l]
            rv=nums[r]*nums[r]
            if lv>=rv:
                ans[site]=lv
                l+=1
            else:
                ans[site]=rv
                r-=1
            site-=1
        return ans
```

## 2.链表

### 解题思路：

虚拟头节点

### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

```python
# 迭代双指针
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        hair=None
        cur = head
        while cur:
            nxt = cur.next
            cur.next=hair
            hair=cur
            cur = nxt
        return hair
```



### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

```python
# ！！！第一没思路 注意 head 可能被删掉 所以返回dummy.next
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy=ListNode(0,head)
        first=head
        second=dummy
        for i in range(n):
            first=first.next
        while first:
            first=first.next
            second=second.next
        second.next=second.next.next
        return dummy.next
```

### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

```python
# 没做出来！！！ 构建虚拟头节点 三步走 右移
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        dummyHead=ListNode(0,head)
        pre=dummyHead
        p=head
        
        while p and p.next:
            q=p.next
            pre.next=q
            p.next=q.next
            q.next=p
            # 指针右移
            pre = p
            p = p.next
        # 返回链表
        return dummyHead.next
```

### [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 

```python
# 困难题 最好在草纸上模拟一下 大厂会考
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverse(self,head,tail):
        prev=tail.next
        p=head
        while prev!=tail:
            nxt=p.next
            p.next=prev
            prev=p
            p=nxt
        return tail,head
        
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        hair=ListNode(0)
        hair.next=head
        pre=hair
        while head:
            tail=pre
            for i in range(k):
                tail=tail.next
                if not tail:
                    return hair.next
            nxt=tail.next
            head,tail = self.reverse(head,tail)
            pre.next=head
            tail.next=nxt
            pre=tail
            head=tail.next
        return hair.next
```



## 3.栈和队列

解题思路：栈 先入后出  队列先入先出



### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

```python
# 左括号入栈 碰到右括号 弹出最后一个元素 匹配 
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        pair = {"(":")","[":"]","{":"}"}

        for item in s:
            if item in pair.values():
                if stack==[] or pair[stack.pop()]!=item:
                    return False
            else:
                stack.append(item)

        return not stack
```



### [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

 

**示例 1：**

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        use = ["+", "-", "/", "*"]
        for item in tokens:
            if item not in use:
                stack.append(item)
            else:
                left, right = int(stack.pop()), int(stack.pop())
                if item == "+":
                    stack.append(right + left)
                elif item == "-":
                    stack.append(right - left)
                elif item == "*":
                    stack.append(right * left)
                elif item == "/":
                    stack.append(int(right / left))
        return int(stack[-1])
```

### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**说明：**

- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 

**示例 1：**

```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

```python
# 双端队列模拟栈
class MyQueue:

    def __init__(self):
        self.stack_in = []
        self.stack_out = []

    def push(self, x: int) -> None:
        self.stack_in.append(x)

    def pop(self) -> int:
        if self.empty():
            return None

        if self.stack_out:
            return self.stack_out.pop()
        else:
            for i in range(len(self.stack_in)):
                self.stack_out.append(self.stack_in.pop())
        return self.stack_out.pop()

    def peek(self) -> int:

        if self.stack_out:
            return self.stack_out[-1]
        else:
            return self.stack_in[0]

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)
```

### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```python
# 单调双向队列 
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        ans = []
        que=[]
        for i,item in enumerate(nums):
            if que!=[] and que[0]==i-k:
                que.pop(0)
            while que!=[] and item > nums[que[-1]]:
                que.pop()
            que.append(i)

            if i>=k-1:
                ans.append(nums[que[0]])

        return ans
```

### [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```

```python
# 简单题
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack=[]

        for item in s:
            if stack and item==stack[-1]:
                stack.pop()
            else:
                stack.append(item)
        return ''.join(stack)
```



## 4.字符串

解题思路：python 中一般字符串不可变 转化为列表 配合双指针解决

### [151.反转字符串中的单词](151. 反转字符串中的单词)

给你一个字符串 s ，请你反转字符串中 单词 的顺序。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

返回 单词 顺序颠倒且 单词 之间用单个空格连接的结果字符串。

注意：输入字符串 s中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

示例 1：

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

```python
# 调用库
class Solution:
    def reverseWords(self, s: str) -> str:
        return " ".join(s.split()[::-1])
```

```python
# 先去除多余空格 整体反转 单词反转
class Solution:
    def reverseWords(self, s: str) -> str:
        def remove_space(s):
            l,r=0,len(s)-1
            while l<r and s[l]==" ":
                l+=1
            while l<r and s[r]==" ":
                r-=1
            new_s = []
            for i in range(l,r+1):
                if s[i]!=" ":
                    new_s.append(s[i])
                elif s[i-1]!=" ":
                    new_s.append(s[i])
            return "".join(new_s)

        def reverse_str(s):
            new_s = []
            for i in range(len(s)-1,-1,-1):
                new_s.append(s[i])
            return "".join(new_s)

        s  = remove_space(s)
        s = reverse_str(s)
        slow,fast = 0,0
        ans = ""
        while fast<len(s)+1:
            if fast!=len(s) and s[fast]!=" " :
                fast+=1
            else:
                ans += reverse_str(s[slow:fast])+" "
                slow=fast+1
                fast=fast+1

        return ans[:-1]
```

## 

### [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

**示例 1：**

```
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

```python
# 写好反转函数 模拟反转
class Solution:
    def reverseString(self,s):
        # 初始化双指针
        left = 0
        right = len(s) - 1

        # 这种方法可以不用判断元素奇偶
        while left < right:
            s[left], s[right] = s[right], s[left]

            #交换后，左指针右移，右指针左移
            left += 1
            right -= 1

        return s

    # 列表反转
    def reverseStr(self, s: str, k: int) -> str:
        res = list(s)

        for i in range(0, len(s), 2 * k):
            res[i : i + k] = self.reverseString(res[i : i + k])

        return ''.join(res)
```

### [2337. 移动片段得到字符串](https://leetcode.cn/problems/move-pieces-to-obtain-a-string/)

给你两个字符串 `start` 和 `target` ，长度均为 `n` 。每个字符串 **仅** 由字符 `'L'`、`'R'` 和 `'_'` 组成，其中：

- 字符 `'L'` 和 `'R'` 表示片段，其中片段 `'L'` 只有在其左侧直接存在一个 **空位** 时才能向 **左** 移动，而片段 `'R'` 只有在其右侧直接存在一个 **空位** 时才能向 **右** 移动。
- 字符 `'_'` 表示可以被 **任意** `'L'` 或 `'R'` 片段占据的空位。

如果在移动字符串 `start` 中的片段任意次之后可以得到字符串 `target` ，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：start = "_L__R__R_", target = "L______RR"
输出：true
解释：可以从字符串 start 获得 target ，需要进行下面的移动：
- 将第一个片段向左移动一步，字符串现在变为 "L___R__R_" 。
- 将最后一个片段向右移动一步，字符串现在变为 "L___R___R" 。
- 将第二个片段向右移动三步，字符串现在变为 "L______RR" 。
可以从字符串 start 得到 target ，所以返回 true 。
```

**示例 2：**

```
输入：start = "R_L_", target = "__LR"
输出：false
解释：字符串 start 中的 'R' 片段可以向右移动一步得到 "_RL_" 。
但是，在这一步之后，不存在可以移动的片段，所以无法从字符串 start 得到 target 。
```



```python
# 第一次没做出来（想用栈的...） 双指针 
class Solution:
    def canChange(self, start: str, target: str) -> bool:
        n = len(start)
        i = j = 0
        while i < n and j < n:
            while i < n and start[i] == '_':
                i += 1
            while j < n and target[j] == '_':
                j += 1
            if i < n and j < n:
                if start[i] != target[j]:
                    return False
                c = start[i]
                if c == 'L' and i < j or c == 'R' and i > j:
                    return False
                i += 1
                j += 1
        while i < n:
            if start[i] != '_':
                return False
            i += 1
        while j < n:
            if target[j] != '_':
                return False
            j += 1
        return True
```



## 5.哈希表



### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash=dict()
        for index,item in enumerate(nums):
            if target-item in hash:
                return hash[target - item], index
            else:
                hash[item] = index
```

### [15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

```python
# 哈希表 无优化 set 和 sort 是为了去重 将两个元素的和存入hash表中
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        ans = set()
        nums.sort()
        for i in range(len(nums)):
            hash=dict()
            for j in range(i+1,len(nums)):
                if nums[j] in hash:
                    ans.add((nums[i],-nums[i]-nums[j],nums[j]))
                else:
                    hash[-nums[i]-nums[j]]=1
        return list(ans)
```

```python
# 双指针 注意优化重复项
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n= len(nums)
        nums.sort()
        ans = list()
        for first in range(n):
            if first > 0 and nums[first] == nums[first-1]:
                continue
            third = n -1
            target = -nums[first]
            for second in range(first+1,n):
                if second >first+1 and nums[second]==nums[second-1]:
                    continue
                while second<third and nums[second]+nums[third]> target:
                    third-=1
                if second == third:
                    break
                if nums[second]+nums[third]==target:
                    ans.append([nums[first],nums[second],nums[third]])
        return ans
```

### [202.快乐数](https://leetcode.cn/problems/happy-number/description/)

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

```
示例 1：

输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

```python
# 哈希表
class Solution:
    def isHappy(self, n: int) -> bool:
        def get_num(num):
            new = 0
            while num > 0:
                new += (num % 10) * (num % 10)
                num = num // 10
            return new
        stack = []
        while 1:
            stack.append(n)
            n =  get_num(num=n)
            if n == 1:
                return True
            if n in stack:
                return False
```

```python
# 快慢指针 和判断链表有无环一样
class Solution:
    def isHappy(self, n: int) -> bool:
        def get_next(number):
            total_sum = 0
            while number > 0:
                number, digit = divmod(number, 10)
                total_sum += digit ** 2
            return total_sum

        slow_runner = n
        fast_runner = get_next(n)
        while fast_runner != 1 and slow_runner != fast_runner:
            slow_runner = get_next(slow_runner)
            fast_runner = get_next(get_next(fast_runner))
        return fast_runner == 1
```

### [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/description/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。



```
示例 1:

输入: s = "anagram", t = "nagaram"
输出: true
示例 2:

输入: s = "rat", t = "car"
输出: false
```



```python
# 数量差
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s)!=len(t):
            return False
        hash=collections.defaultdict(int)
        for i,j in zip(s,t):
            hash[i]+=1
            hash[j]-=1

        for v in hash.values():
            if v!=0:
                return False
        return True
```

```python
# 调库
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return collections.Counter(s)==collections.Counter(t)
```

```python
# 排序
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
         s1=self.qsort(s)
         t1=self.qsort(t)
         return s1 == t1
    
    def qsort(self, l: str):
        list1 = list(l)
        list1.sort()
        k=''.join(list1)
        return k
```

### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

```python
# 哈希表
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        ans = []
        if not nums1 or not nums2:
            return []
        hash = {}
        for item in nums1:
            if not hash.get(item):
                hash[item]=1
        for item in nums2:
            if hash.get(item):
                ans.append(item)
                hash[item]=0
        return ans
```



```python
# 二分查找
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums2 = sorted(nums2)
        ans = set()
        for num in nums1:
            l,r = 0,len(nums2)-1
            while l<=r:
                mid = l+(r-l)//2
                if num<nums2[mid]:
                    r = mid-1
                elif num>nums2[mid]:
                    l = mid +1
                else:
                    ans.add(num)
                    break
        return list(ans)
```

### [383.赎金信](https://leetcode.cn/problems/ransom-note/description/)

给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

 

```
示例 1：

输入：ransomNote = "a", magazine = "b"
输出：false
示例 2：

输入：ransomNote = "aa", magazine = "ab"
输出：false
示例 3：

输入：ransomNote = "aa", magazine = "aab"
输出：true
```



```python
# 计数哈希表
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        if len(ransomNote)>len(magazine):
            return False
        hash=dict()
        for item in magazine:
            hash[item]=hash.get(item,0)+1
        for item in ransomNote:      
            if item not in hash:
                return False
            hash[item]-=1
            if  hash[item]<0:
                return False
        return True
```

### [454.四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0

```
示例 1：

输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：

1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
示例 2：

输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```



```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        # 初始化哈希表
        hash = {}
        cnt = 0

        # 首先存储前两个数组之和
        for n1 in nums1:
            for n2 in nums2:
                # 如果在哈希表中，则对应哈希值 +1
                if n1 + n2 in hash:
                    hash[n1 + n2] += 1
                # 如果不在哈希表中，放入哈希表
                else:
                    hash[n1 + n2] = 1
        # 统计剩余两个数组的和，在哈希表中找是否存在相加为 0 的情况。
        for n3 in nums3:
            for n4 in nums4:
                if -(n3 + n4) in hash:
                    cnt += hash[-(n3 + n4)]

        return cnt
```





1388. 3n 块披萨
      提示
      困难
      191
      相关企业
      给你一个披萨，它由 3n 块不同大小的部分组成，现在你和你的朋友们需要按照如下规则来分披萨：

你挑选 任意 一块披萨。
Alice 将会挑选你所选择的披萨逆时针方向的下一块披萨。
Bob 将会挑选你所选择的披萨顺时针方向的下一块披萨。
重复上述过程直到没有披萨剩下。
每一块披萨的大小按顺时针方向由循环数组 slices 表示。

请你返回你可以获得的披萨大小总和的最大值。

 

示例 1：



输入：slices = [1,2,3,4,5,6]
输出：10
解释：选择大小为 4 的披萨，Alice 和 Bob 分别挑选大小为 3 和 5 的披萨。然后你选择大小为 6 的披萨，Alice 和 Bob 分别挑选大小为 2 和 1 的披萨。你获得的披萨总大小为 4 + 6 = 10 。
示例 2：



输入：slices = [8,9,8,6,1,1]
输出：16
解释：两轮都选大小为 8 的披萨。如果你选择大小为 9 的披萨，你的朋友们就会选择大小为 8 的披萨，这种情况下你的总和不是最大的。


提示：

1 <= slices.length <= 500
slices.length % 3 == 0
1 <= slices[i] <= 1000





## 6.动态规划

### [70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/description/)

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

```
示例 1：

输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。

1. 1 阶 + 1 阶
2. 2 阶

示例 2：

输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。

1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```



```python
class Solution:
    def climbStairs(self, n: int) -> int:
        first=1
        second = 2
        if n==1:
            return first
        if n==2:
            return second
        ans =0
        for i in range(2,n):
            ans =first+second
            first = second
            second=ans
        return ans
```

