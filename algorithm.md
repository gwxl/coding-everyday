
## 排序

### 选择排序

```python
def selectionSort(A):
  n = len(A)
  
  for i in range(n-1):
    min_index = i
    
    for j in range(i + 1, n):
      if A[min_index] > A[j]:
        min_index = j
        
    A[i], A[min_index] = A[min_index], A[i]
  return A
```


### 冒泡排序

```python
def bubbleSort(A):
  n = len(A)
  
  for i in range(n - 1):
    for j in range(n - i - 1):
      if A[j] > A[j + 1]:
        A[j], A[j + 1] = A[j + 1], A[j]
        
  return A
```

### 插入排序

```python
def insertSort(A):
  n = len(A)
  for i in range(1, n):
    for j in range(i):
      if A[j] > A[i]:
        A[i], A[j] = A[j], A[i]
  return A
```

### 快排

```python
def quick(A):
  n = len(A)
  if n <= 1: return A
  m = A[0]
  left = quick([i for i in A if i < m])
  right = quick([i for i in A if i > m])
  mm = [i for i in A if i == m]
  return left + mm + right

```

### 堆排序

```python
def heap(A, i, n):
  j = 2 * i +1
  while j < n:
    if j + 1 < n and A[j] < A[j+1]:
      j += 1
    if A[i] < A[j]:
      A[i], A[j] = A[j], A[i]
      i = j
      j = 2 * i + 1
    else:
      break
      
def heapSort(A):
  n = len(A)
  for i in range(n // 2 - 1, -1, -1):
    heap(A, i, n)
  for i in range(n - 1, -1, -1):
    A[i], A[0] = A[0], A[i]
    heap(A, 0, i)
  return A
```

### 希尔排序

```python
def shellSort(A):
  n = len(A)
  step = 4
  while step > 0:
    for i in range(step, n):
      p = i
      q = i-step
      while q >= 0:
        if A[q] > A[p]:
          A[p], A[q] = A[q], A[p]
          p = q
          q = p - step
        else:
          break
    step -= 1
  return A
```
### 归并排序

```python
def mergeSort(A):
  n = len(A)
  if n <= 1: return A
  num = n // 2
  
  left = mergeSort(A[:num])
  right = mergeSort(A[num:])
  
  res = []
  i, j = 0, 0
  
  while i < len(left) and j < len(right):
    if left[i] < right[j]:
      res.append(left[i])
      i += 1
    else:
      res.append(right[j])
      j += 1
      
  res += left[i:]
  res += right[j:]
  return res
```

### 计数排序

```python
def countingSort(A):
  mi = min(A)
  ma = max(A)
  res = []
  bu = [0] * (ma - mi + 1)
  
  for i in A:
    bu[i - mi] += 1
    
  for i in range(len(bu)):
    if bu[i]:
      res += [i + mi] * bu[i]
  return res
```

### 基数排序

```python
def radixSort(A):
  n = len(A)
  
  for i in range(4):
    s = [[] for _ in range(10)]
    
    for j in A:
      s[j//(10**i)%10].append(j)
      
    A = [a for b in s for a in b]
  return A
```

### 有序数组合并

```python
def mergeAB(A, B):
  n = len(A)
  m = len(B)
  A.extend([0] * m)
  
  i = n - 1
  j = m - 1
  
  while i >= 0 and j >= 0:
    if A[i] > B[j]:
      A[i+j+1] = A[i]
      i -= 1
    else:
      A[i+j+1]=B[j]
      j -= 1
      
  if j >= 0:
    A[:j+1] = B[:j+1]
  return A
```

### 三色排序问题
```
1. 用三个指针，指针p指向开始，指针q指向末尾，指针i表示当前。
2. 开始时，p向后遍历到不为0的地方，并让 i=p，同时，q向前遍历到不为2的地方。
3. i向后移动，
   若arr[i]等于1, 则i++；
   若arr[i]等于2，则 arr[i]和arr[q]调换位置，q--，i不动（i不动的原因是，被交换活过来的元素有可能是0）；
   若arr[i]等于0，则 arr[i]和arr[p]调换位置，p++，i++。
```
```python
def sortThreeColor(A):
  n = len(A)
  p = 0
  q = n - 1
  
  while A[p] == 0:
    p += 1
  while A[q] == 2:
    q -= 1
  i = p
  
  while i <= q:
    if A[i] == 1:
      i += 1
    elif A[i] == 2:
      A[i], A[q] = A[q], A[i]
      q -= 1
    else:
      A[i], A[p] = A[p], A[i]
      i += 1
      p += 1
  return A
```


<br/>

## 动态规划

### 01背包问题
```
dp[x][y]表示前x件物品，不超过重量y的时候的最大价值。
有两种可能：
（1）若选择x件物品，则前x-1件物品的重量不能超过y-w[x]。
（2）若不选择x件物品，则前x-1件物品的重量不能超过y。

所以dp[x][y]可能等于dp[x-1][y]，也就是不取第x件物品。也可能是dp[x-1][y-w[x]]+v[x]，二者取较大值。
```
```python
def bag(w, v, cap):
  n = len(w)
  dp = [[0 for i in range(cap + 1)] for j in range(n + 1)]
  for i in range(1, n + 1):
    for j in range(1, cap + 1):
      dp[i][j] = dp[i - 1][j]
      if j >= w[i -1] and dp[i-1][j-w[i-1]] + v[i-1] > dp[i-1][j]:
        dp[i][j] = dp[i-1][j-w[i-1]] + v[i-1]
  return dp[-1][-1]
```

### 最长公共子序列LCS
```
1. dp[i][j]的含义是字符串arr1[0...i]和arr2[0...j]的最长公共子序列。
2. 矩阵dp的第一列，即dp[i][0]，为arr1[0..j]和arr2[0]的最长公共子序列长度，
   由于arr2[0]长度为1，所以dp[i][0]最大为1。dp[i][0]一旦被设为1，那么之后的dp[i+1..M]都为1。
3. 同理，矩阵的第一行，dp[0][j]，也是相同的规律。
4. 若 arr1[i]==arr2[j]，则 dp[i][j]=dp[i-1][j-1]+1，否则 dp[i][j] = max(dp[i-1][j], dp[i][j-1])。
```
```python
def LCS(A, B):
  n = len(A)
  m = len(B)
  dp =[[0 for i in range(m)]for j in range(n)]
  dp[0][0] = 1 if A[0] == B[0] else 0
  for i in range(1, n):
    dp[i][0] = 1 if A[i] == B[0] else dp[i-1][0]
  for j in range(1, m):
    dp[0][j] = 1 if A[0] == B[j] else dp[0][j-1]
  for i in range(1, n):
    for j in range(1, m):
      if A[i] == B[j]:
        dp[i][j] = dp[i-1][j-1] + 1
      else:
        dp[i][j] = max(dp[i-1][j], dp[i][j-1])
  return dp[-1][-1]
```

### 最长上升子序列 LIS
```
dp[i]表示以arr[i]结尾的情况下，arr[0...i]中最大递增子序列长度。
dp[i]=max{ dp[j] + 1 }，其中 0<=j<i，并且 arr[j]<arr[i]
```
```python
def LIS(A):
  n = len(A)
  dp = [0 for i in range(n)]
  dp[0] = 1
  
  for i in range(1, n):
    temp = 0
    
    for j in range(i):
      if A[j] < A[i] and temp < dp[j]:
        temp = dp[j]
    dp[i] = temp + 1
    
  return max(dp) # 注意是取最大的，不是最后一个
```

### 找零钱问题
```
dp[i]是目标面值为i时，可以换钱的方法数。
dp[j] += dp[j - penny[i]]，比如，当 penny[i] 为5时，比5大的目标，都应该加上5 + (剩余的面值)的方法数。
```
```python
def exchange(penny, aim):
  n = len(penny)
  dp = [0 for i in range(aim + 1)]
  dp[0] = 1
  for i in range(n):
    j = penny[i]
    while j <= aim:
      dp[j] += dp[j - penny[i]]
      j += 1
  return dp[-1]
```

<br/>
## 二叉树

### 二叉树的实现

```python
class Node:
  def __init__(self, x):
    self.val = x
    self.left = None
    self.right = None

class Tree:
  def __init__(self):
    self.root = None
  
  def add(self, x):
    node = Node(x)
    if not self.root:
      self.root = node
    else:
      q = [self.root]
      while True:
        p = q.pop(0)
        if p.left is None:
          p.left = node
          return
        elif p.right is None:
          p.right = node
          return
        else:
          q.append(p.left)
          q.append(p.right)
```

### 二叉树的层次遍历
1. 队列，先放入根节点。
2. 然后每次弹出一个来，放入res中，并且遍历左右孩子，放入队列中。
```python
def traverseTree(root):
  if not root: return []
  res = []
  q = [root]
  while q:
    p = q.pop(0)
    res.append(p.val)
    if p.left:
      q.append(p.left)
    if p.right:
      q.append(p.right)
  return res

```
### 二叉树的前序、中序、后序遍历（递归）

```python
def preOrder(root):
  if not root: return []
  res = [root.val]
  left = preOrder(root.left)
  right = preOrder(root.right)
  return res + left + right

def inOrder(root):
  if not root: return []
  res = [root.val]
  left = inOrder(root.left)
  right = inOrder(root.right)
  return left + res + right

def postOrder(root):
  if not root: return []
  res = [root.val]
  left = postOrder(root.left)
  right = postOrder(root.right)
  return left + right + res
```
### 二叉树的前序、中序、后序遍历（非递归）

#### 前序遍历
1. 申请一个栈，将根节点推入。
2. 每次从栈中弹出一个元素，并放入res中，并且依次遍历它的右、左孩子，如果存在，将其放入栈中。
3. 重复步骤2，直到栈为空。
```python
def preOrder2(root):
  if not root: return []
  res = []
  q = [root]
  
  while q:
    node = q.pop()
    res.append(node.val)
    
    if node.right:
      q.append(node.right)
    if node.left:
      q.append(node.left)
  return res
```
#### 中序遍历
1. 申请一个栈，和一个变量cur，指向根节点。
2. 只要cur不为空，就不断将其压入到栈中，并不断将cur指向它的左孩子。
3. 如果cur为空，就从栈中弹出一个元素node，并打印，将cur指向node的右孩子。
4. 重复步骤2和3，直到栈和cur都为空。
```python
def inOrder2(root):
  if not root: return []
  res = []
  q = []
  cur = root
  
  while q or cur:
    if cur:
      q.append(cur)
      cur = cur.left
    else:
      node = q.pop()
      res.append(node.val)
      cur = node.right
  return res
```
#### 后序遍历
1. 申请两个栈，s1 和 s2，s1先推入根节点
2. 每次s1弹出一个节点，将其放入s2中，并且依次遍历其左右孩子，将其推入s1中
3. 重复步骤2，直到s1为空
4. 从s2中依次弹出节点，并打印。
```
def postOrder2(root):
  if not root: return []
  res = []
  s1 = [root]
  s2 = []
  
  while s1:
    node = s1.pop()
    s2.append(node)
    if node.left: s1.append(node.left)
    if node.right: s1.append(node.right)
  
  while s2:
    res.append(s2.pop().val)
  return res
```

### 二叉树的按层打印
1. 用队列不断推入当前层的节点，每次遍历的时候用一个变量记录当前层的节点个数。
2. 依次遍历队列，将值放入temp中，遍历完一层后，将temp放入res中。

```python
def levelOrder(root):
  if not root: return 
  res = []
  q = [root]
  
  while q:
    curLen = len(q)
    temp = []
    for i in range(curLen):
      node = q.pop(0)
      temp.append(node.val)
      if node.left: q.append(node.left)
      if node.right: q.append(node.right)
    res.append(temp)
  return res
```



### 二叉树之字形打印
1. 和上题类似，多了一个标志位。
2. python的三元表达式形式：
```python
res = 'haha' if True else 'hehe'

# 或者
res.append(temp if True else temp[::-1])
```
3. python用not来取反

```python
def levelReverseOrder(root):
  if not root: return 
  res = []
  q = [root]
  positiveFlag = True
  
  while q:
    curLen = len(q)
    temp = []
    
    for i in range(curLen):
      node = q.pop(0)
      temp.append(node.val)
      if node.left: q.append(node.left)
      if node.right: q.append(node.right)
      
    res.append(temp if positiveFlag else temp[::-1])
    positiveFlag = not positiveFlag
  return res
```

### 折纸问题
把纸条从下往上对折，此时向下突起，再折一次，从上到下突起方向分别为：下、下、上，那么折n次突起顺序？
```
       下
      /  \
    下    上
   / \   /  \
  下  上 下  上 
```
- 第一次折痕向`下`，第二次`下、下、上`，可以发现是二叉树结构，每个折痕的上面是`上`，下面是`下`
- 那就是二叉树的`左-根-右`遍历，即中序遍历。

```python
def fold(i, n, down, res):
  if i > n: return 
  fold(i + 1, n, True, res)
  res.append('down') if down else res.append('up')
  fold(i + 1, n, False, res)
  
def foldPaper(n):
  res = []
  fold(1, n, True, res)
  return res
```

### 求二叉树的镜像
1. 只要当前节点不为空，交换其左、右孩子
2. 对左、右孩子进行递归。
```python
def mirror(root):
  if root:
    root.left, root.right = root.right, root.left
    mirror(root.left)
    mirror(root.right)
```

### 判断是否是平衡树
1. 如果根节点为空，返回True。
2. 否则递归比较根节点的左、右孩子，以及左、右孩子的高度差是否小于1。

获取树的高度的函数
 - 如果根节点为空，返回0
 - 递归获取左右子树的高度
 - 返回`max(左右子树的高度)+1`

```python
def checkBalance(root):
  if not root: return True
  return checkBalance(root.left) and \
         checkBalance(root.right) and \
         abs(getHeight(root.left) - getHeight(root.right)) <= 1

# 获取树的高度
def getHeight(root):
  if not root: return 0
  left = getHeight(root.left)
  right = getHeight(root.right)
  return left + 1 if left > right else right + 1

```
## 链表
### 翻转链表
```
输入一个链表，翻转链表后，输出新链表的表头。
1->2->3->4->5->null
5->4->3->2->1->null
```
```python
def reverseList(head):
  if not head or not head.next: return head
  pre = None
  while head:
    nxt = head.next
    head.next = pre
    pre = head
    head = nxt
  return pre
```

### K 个一组翻转链表 [Leetcode - 25]
```
1. 设置一个头结点的前一个节点 hair，设置 tail，含义是指向子链表的尾部，一开始等于 hair。
2. 每k个一组，找到tail的位置，并记下tail.next，设为nxt。
3. 翻转head和tail之间的子链表
4. 翻转完后，将 pre.next 指向子链表新的 head，将 tail.next 指向记录的 nxt。
5. pre 指向 tail，head 指向 tail.next，进行下一次循环。


翻转子链表：
1. 首先要找到 tail.next，记为 prev，让 p(head) 指向 prev，并移动 p。
2. 终止条件是，prev 等于 tail。
```
```python
def reverseK(head, k):
  hair = ListNode(0)
  hair.next = head
  pre = hair
  
  while head:
    tail = pre
    # 查看剩余部分长度是否大于等于 k
    for i in range(k):
      tail = tail.next
      if not tail:
        return hair.next
    nxt = tail.next
    head, tail = reverse(head, tail)
    
    # 把子链表重新接回原链表
    pre.next = head
    tail.next = nxt
    pre = tail
    head = tail.next

# 翻转一个子链表，并且返回新的头与尾
def reverse(head, tail):
  prev = tail.next
  p = head
  
  while prev != tail:
    nxt = p.next
    p.next = prev
    prev = p
    p = nxt
  return tail, head
```
### 两两交换链表中的节点
```
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例: 给定 1->2->3->4, 你应该返回 2->1->4->3.
```
```
node1表示子链表的第1个节点，node2表示子链表的第2个节点，nxt表示node2的下一个节点。
```
```python
def reverseTwo(head):
  hair = ListNode(0)
  hair.next = head
  p = hair
  
  while p.next and p.next.next:
    node1 = p.next
    node2 = node1.next
    nxt = node2.next
    
    node2.next = node1
    node1.next = nxt
    
    p.next = node2
    p = node1
    
  return hair.next
```



## 编程基础
### 质数因子

```python
def getPrime(n):
  i = 2
  res = []
  while n != 1:
    if n%i == 0:
      res.append(i)
      n = n//i
    else:
      i += 1
  return res
```

### 扫描所有视频文件
```javascript
const path = require('path')
const fs = require('fs')

// 要遍历的根目录
// const firstPath = path.join(__dirname, '.')
const firstPath = 'D:'
const saveFileLocation = path.join(__dirname, 'video_list.txt')
readDir(firstPath)

let count = 1
// 遍历目录
function readDir(rootPath) {
  fs.readdir(rootPath, {encoding: 'utf8'}, (err, files) => {
    if(!files || !files.length) return
    
    files.forEach(file => {
      const filePath = `${rootPath}/${file}`
      
      fs.stat(filePath, (err, info) => {
        if (info && info.isDirectory()) {
          readDir(filePath)
        } else {
          const extName = path.extname(filePath)
          if (['.mp4', '.avi', '.mkv', '.rmvb'].includes(extName)){
            writeToFile(saveFileLocation, `${count}. ${filePath}\n`)
            count += 1
          }
        }
      })
    }) 
  })
}

function writeToFile(file, data) {
  fs.writeFile(file, data, {flag: 'a'}, (err,a,b) =>{
  })
}

```


## LeetCode
### 移动零 [Leetcode - 283 ]

```
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]

说明:
1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。
```

```
1. 用一个变量k，表示数组的前k项都是非0元素，初始值为0
2. 依次遍历数组，如果当前值item不为0，赋值给nums[k]，同时k++。
3. 再从k遍历到数组尾部，将k位置以后的全部赋值为0。
```

```javascript
var moveZeroes = function(nums) {
    // let _list = []
    
    // for (let item of nums) {
    //     if (item != 0) {
    //         _list.push(item)
    //     }
    // }
    // for (let i = 0; i < nums.length; i++) {
    //     nums[i] = _list[i] || 0
    // }

    // 原地(in place)解决该问题
    // 空间复杂度O(n) => O(1)
    let k = 0 // nums中, [0...k)的元素均为非0元素

    for (let item of nums) {
        if (item) {
            nums[k++] = item
        }
    }
    for (let i = k; i < nums.length; i ++) {
        nums[i] = 0
    }
};
```
python实现：
```python
def fn(nums):
  k = 0
  
  for item in nums:
    if item:
      nums[k]=item
      k+=1
      
  for i in range(k, len(nums)):
    nums[i]=0
  return nums
```
### 长度最小的子数组 [Leetcode - 209]
```
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的子数组，
并返回其长度。如果不存在符合条件的子数组，返回 0。

示例：

输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

```
1. 思路：滑动窗口
2. 设置左右指针`l`和`r`，一开始都设置为0。设置变量sum，记录当前窗口内的值的和。
3. 依次遍历数组，如果右指针小于n，并且sum小于s，那么sum加上右指针指向的值，同时右指针向后移动，否则，左指针向后移动，sum减去左指针指向的值。
4. 遍历的过程中，如果满足条件，即sum>=s，那么更新res为原来的res和`r-l`的较小值。
```

```javascript
var minSubArrayLen = function(s, nums) {
    const n = nums.length
    let l = 0
    let r = 0 // nums[l...r]为我们的滑动窗口

    let res = n + 1
    let sum = 0

    while (l < n){ // 窗口的左边界在数组范围内,则循环继续
        if (r < n && sum < s) {
            sum += nums[r++]
        }
        
        else { // r已经到头 或者 sum >= s
            sum -= nums[l++]
        }
        
        if (sum >= s) {
            res=Math.min(res, r-l)
        }
    }
    if (res == n+1) return 0
    return res
};
```
python实现：
```python
def fn(s, nums):
  n = len(nums)
  l = 0
  r = 0
  
  res = n + 1
  sum = 0
  
  while l < n:
    if r < n and sum < s:
      sum += nums[r]
      r += 1
    else:
      sum -= nums[l]
      l -= 1
    
    if sum >= s:
      res = min(res, r - l)
      
  if res == n+1: return 0
  return res

```


### 无重复字符的最长子串 [Leetcode - 3]
```
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
```
1. 思路：滑动窗口
2. 设置左右指针`l`和`r`，一开始都设置为0。设置对象freq，记录某字符是否出现过。
3. 依次遍历字符串，如果右指针小于n，并且freq不存在右指针的值，那么将右指针的值加入到freq中，同时右指针向后移动，否则，左指针向后移动，sum减去左指针指向的值。
```
```javascript
var lengthOfLongestSubstring = function(s) {
    let freq = {}
    let l = 0
    let r = 0  //滑动窗口为s[l...r]
    const n = s.length
    let res = 0

    // 在每次循环里逐渐改变窗口, 维护freq, 并记录当前窗口中是否找到了一个新的最优值
    while (l < n) {
        if (r < n && !freq[s[r]]) {
            freq[s[r]] = 1
            r+=1
        } 
        else {   //r已经到头 || freq[s[r+1]] == 1
            freq[s[l++]]--
        }
        res = Math.max(res, r-l)
    }
    return res
};
```
python实现：
```python
def fn(s):
  n = len(s)
  l = 0
  r = 0
  res = 0
  freq = {}
  
  while l < n:
    if r < n and s[r] not in freq:
      freq[s[r]] = 1
      r += 1
    else:
      del freq[s[l]]
      l += 1
    res = max(res, r - l)
  return res
```

### 完全平方数 [Leetcode - 279]
```
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。
你需要让组成和的完全平方数的个数最少。

示例 1:
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.

示例 2:
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```

```
1. 动态规划解法，`dp[n`]存储了组成n的最少平方的个数
2. 求平方根，python：`x**0.5`，js：`Math.sqrt(x)`
3. 无穷大，python：`float('inf')`，js：`Infinity`
```

```python
def numSquares(n):
  square_nums = []
  for i in range(int(n ** 0.5) + 1):
    square_nums.append(i ** 2)
  
  dp = [0]
  for i in range(1, n+1):
    dp.append(float('inf'))
  
  for i in range(1, n+1):
    for square in square_nums:
      if i < square: break
      dp[i] = min(dp[i], dp[i-square] + 1)
      
  return dp[-1]
```

### 存在重复元素 II [Leetcode - 219]
```
给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，
并且 i 和 j 的差的 绝对值 至多为 k。 

示例:
输入: nums = [1,2,3,1], k = 3
输出: true
```

```
1. 思路：滑动窗口+查找表
2. 判读元素是否在数组中，python：`x in list`，js：`list.includes(x)`
```

```python
def fn(nums, k):
  if len(nums) <= 1 or k <= 0: return 
  d = []
  
  for i in range(len(nums)):
    if nums[i] in d: return True
    
    d.append(nums[i])
    
    if (len(d) > k):
      d.pop(0)
  return False
```

### 存在重复元素 III [ Leetcode - 220]
```
在整数数组 nums 中，是否存在两个下标 i 和 j，使得 nums [i] 和 nums [j] 的差的绝对值小于等于 t ，
且满足 i 和 j 的差的绝对值也小于等于 ķ 。

如果存在则返回 true，不存在返回 false。

示例 1:
输入: nums = [1,2,3,1], k = 3, t = 0
输出: true

示例 2:
输入: nums = [1,0,1,1], k = 1, t = 2
输出: true

示例 3:
输入: nums = [1,5,9,1,5,9], k = 2, t = 3
输出: false
```

```
1. 桶作为一个窗口，桶的大小是t+1，也就是值的最大距离+1，那么x的编号是x/(t+1)。
2. 桶中只放一个元素，如果有两个，说明满足条件了，返回True。
3. 依次遍历数组，获得当前值x的id，如果这个桶里有值，返回True。否则，看与它相邻的两个桶，如果它们中有值，并且，其值和x之差小于等于t，返回True。都不满足，将x放入桶id中。
4. 桶的个数是k，遍历的过程中，如果索引>=k，删除之前的桶。

注意：
1. python中，如果id不在d中，直接`d[id]`会报错。
2. python中，删除dict中的元素：`del d[id]`，如果id不在d中，会报错。
```

```python
import math

def getId(x, t): # 获取桶的编号
  return math.floor(x / (t + 1))

def fn(nums, k, t):
  if t < 0: return False
  d = {} # 桶
  
  for i in range(len(nums)):
    id = getId(nums[i], t)
    if id in d: return True
    elif id+1 in d and abs(d[id+1] - nums[i]) <= t: return True
    elif id-1 in d and abs(d[id-1] - nums[i]) <= t: return True
  
    d[id] = nums[i]
    if i >= k: del d[getId(nums[i-k], t)]
  return False
```


### 回旋镖的数量 [Leetcode - 447]
```
给定平面上 n 对不同的点，“回旋镖” 是由点表示的元组 (i, j, k) ，其中 i 和 j 之间的距离和 i 和 k 
之间的距离相等（需要考虑元组的顺序）。

找到所有回旋镖的数量。你可以假设 n 最大为 500，所有点的坐标在闭区间 [-10000, 10000] 中。

示例:

输入: [[0,0],[1,0],[2,0]]

输出: 2

解释: 两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```

```
1. 依次遍历数组，针对每一个点都建立一个字典，存储它和其他点的距离的平方（注意不要开根号）。
2. 然后求组合总数，比如某个距离有6个，那就是5*6中组合方式(C62)。
```

```python
def getDis(a, b):
  return (a[0] - b[0])**2 + (a[1] - b[1])**2

def fn(points):
  res = 0
  
  for item in points:
    d = {}
    
    for it in points:
      if item[0] != it[0] or item[1] != it[1]:
        dis = getDis(item, it)
        
        if dis in d:
          d[dis] += 1
        else: 
          d[dis] = 1
      
    for it in d:
      res += d[it]*(d[it]-1)
  return res
```

### 丑数 [Leetcode - 263]
```
思路：不断除以2、3、5，如果最后等于1，说明是丑数，如果最后小于1，说明不是丑数。
```
```python
def isUgly(num):
  while True:
    if num < 1: return False
    elif num == 1: return True
    elif num % 2 == 0: num = num // 2
    elif num % 3 == 0: num = num // 3
    elif num % 5 == 0: num = num // 5
    else: return False
```


### 丑数 II [Leetcode - 264]
```
编写一个程序，找出第 n 个丑数。
丑数就是质因数只包含 2, 3, 5 的正整数。

示例:
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

说明:  
1 是丑数。
n 不超过1690。
通过次数30,264提交次数56,833
```
```
1. 思路：动态规划
2. 从数组中只包含一个丑数数字 1 开始，使用三个指针`i2、i3、i5`标记所指向丑数要乘以的因子。
3. 在`nums[i2]*2, nums[i3]*3, nums[i5]*5`中选出最小的丑数并添加到数组中。并将该丑数对应的因子指针往前走一步。重复该步骤直到计算完 1690 个丑数。
```
```python
def nthUglyNumber(n):
  res = [1]
  i2 = i3 = i5 = 0
  for i in range(1, n):
    ugly = min(res[i2]*2, res[i3]*3, res[i5]*5)
    res.append(ugly)
    
    if ugly == res[i2]*2:
      i2 += 1
    if ugly == res[i3]*3:
      i3 += 1
    if ugly == res[i5]*5:
      i5 += 1
  return res[-1]
```

### 超级丑数 [Leetcode - 313]
```
编写一段程序来查找第 n 个超级丑数。
超级丑数是指其所有质因数都是长度为 k 的质数列表 primes 中的正整数。

示例:
输入: n = 12, primes = [2,7,13,19]
输出: 32 
解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。

说明:
(1) 1 是任何给定 primes 的超级丑数。
(2) 给定 primes 中的数字以升序排列。
(3) 0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000 。
(4) 第 n 个超级丑数确保在 32 位有符整数范围内。
```
```
1. 和上一题类似，L存放质数的指针，初始值都是0。
2. 然后求出质数列表中，每个质数乘以相应指针的最小值。并让相应的指针向后移动1位。
```
```python
def ulgy(n, primes):
  num = len(primes)
  L = [0 for i in range(num)]
  res = [0] * n
  res[0]=1
  
  for i in range(1, n):
    min_u = min(res[L[j]] * primes[j] for j in range(num))
    res[i] = min_u
    
    for k in range(num):
      if res[L[k]] * primes[k] == min_u:
        L[k] += 1
        
  return res[-1]
```
