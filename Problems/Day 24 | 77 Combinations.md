# Day 24 | 77 Combinations

## 回溯 backtracking

回溯模版：

~~~py
res = []    # 定义全局变量保存最终结果
state = []  # 定义状态变量保存当前状态
p,q,r       # 定义条件变量（一般条件变量就是题目直接给的参数）
def back(状态，条件1，条件2，……):
    if # 不满足合法条件（可以说是剪枝）
        return
    elif # 状态满足最终要求
        res.append(state)   # 加入结果
        return 
    # 主要递归过程，一般是带有 循环体 或者 条件体
    for # 满足执行条件
    if  # 满足执行条件
        back(状态，条件1，条件2，……)
back(状态，条件1，条件2，……)
return res
~~~

## 77 Combinations

Problem: https://leetcode.com/problems/combinations/description/

Solution: https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html

回溯算法：

用递归控制for循环的个数

剪枝：

path.size() ： 已经找的个数

k-path.size() ：还需找的个数

【x, n】的数组长度起码应该是k-path.size()才有继续搜索的可能， 那么就有 n-x+1 = k-path.size()  ， 解方程得 x = n+1 - (k-path.size()), 而且这个x是可以作为起点往下搜的 也就是for(i = s; i<=x; i++) 这里的x是可以取到的

时间复杂度： 在最坏情况下，该算法需要搜索n中所有的组合，因此时间复杂度为O(C(n, k))，其中C(n,k)表示从n个元素中选k个元素的组合数。

空间复杂度： 在最坏情况下，该算法需要维护所有符合要求的组合，因此空间复杂度为O(C(n, k))。

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        # path里装每次迭代出的结果
        path = []
        # 最终结果集 
        res = []
        def backtrack(n, k, start_i):
            # 递归终止条件，判断如果找到符合条件的组合就放入res里
            if len(path) == k:
                res.append(path[:])
                return
            # 剪枝 (for截止每次至多到哪，因为后面的数量不满足要求的k的数量就不用迭代了)
            last_i = n - (k - len(path)) + 1

            for i in range(start_i, last_i + 1):
                path.append(i)
                # 往下递归，直到找到符合k大小的组合，往下递归k-1层
                backtrack(n , k , i + 1)
                # 回溯(因为每次找到需要的组合就把最后一个数扔掉再找下一个组合)
                path.pop()
        
        backtrack(n,k,1)
        return res
```