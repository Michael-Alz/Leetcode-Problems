# Day 39 | 62, 63 Unique Paths & II

## 62 Unique Paths 

Problem: https://leetcode.com/problems/unique-paths/description/

Solution: https://programmercarl.com/0062.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84.html#%E6%80%9D%E8%B7%AF

- Time complexity: O(N×M)
- Space complexity: O(N×M)

```python
		#  1. 确定dp数组下标含义 dp[i][j] 到每一个坐标可能的路径种类
    #  2. 递推公式 dp[i][j] = dp[i-1][j] dp[i][j-1]
    #  3. 初始化 dp[i][0]=1 dp[0][i]=1 初始化横竖就可
    #  4. 遍历顺序 一行一行遍历
    #  5. 推导结果 
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1 for i in range(n)] for j in range(m)]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i][j - 1] + dp[i - 1][j]
        return dp[m - 1][ n - 1]
```

## 63 Unique Paths II

Problem: https://leetcode.com/problems/unique-paths-ii/description/

Solution: https://programmercarl.com/0063.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84II.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- 时间复杂度：O(n × m)，n、m 分别为obstacleGrid 长度和宽度
- 空间复杂度：O(n × m)

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0] == 1:
            return 0
        # 构造一个DP table m * n
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0 for i in range(n)] for j in range(m)]
        # 第一行
        for i in range(n):
             # 遇到障碍物时，直接退出循环，后面默认都是0
            if obstacleGrid[0][i] == 1:
                break
            dp[0][i] = 1
        # 第一列
        for i in range(m):
            # 遇到障碍物时，直接退出循环，后面默认都是0
            if obstacleGrid[i][0] == 1:
                break
            dp[i][0] = 1
        for i in range(1 , m):
            for j in range(1 , n):
                if obstacleGrid[i][j] != 1:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        return dp[m - 1][n - 1]
```

空间优化版本：

- 时间复杂度：O(n × m)，n、m 分别为obstacleGrid 长度和宽度
- 空间复杂度：O(m)

```python
class Solution:
    """
    使用一维dp数组
    """

    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m, n = len(obstacleGrid), len(obstacleGrid[0])

        # 初始化dp数组
        # 该数组缓存当前行
        curr = [0] * n
        for j in range(n):
            if obstacleGrid[0][j] == 1:
                break
            curr[j] = 1

        for i in range(1, m): # 从第二行开始
            for j in range(n): # 从第一列开始，因为第一列可能有障碍物
                # 有障碍物处无法通行，状态就设成0
                if obstacleGrid[i][j] == 1:
                    curr[j] = 0
                elif j > 0:
                    # 等价于
                    # dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
                    curr[j] = curr[j] + curr[j - 1]
                # 隐含的状态更新
                # dp[i][0] = dp[i - 1][0]

        return curr[n - 1]
```