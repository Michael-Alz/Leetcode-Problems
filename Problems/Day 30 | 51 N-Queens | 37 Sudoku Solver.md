# Day 30 | 51 N-Queens | 37 Sudoku Solver

## 51 N-Queens

Problem: https://leetcode.com/problems/n-queens/description/

Solution:https://programmercarl.com/0051.N%E7%9A%87%E5%90%8E.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E8%A1%A5%E5%85%85

回溯，判断合法位置需要注意

Time complexity: O(N!)

Space complexity: O(N^2)

```python
class Solution:
    def __init__(self):
        self.res = []

    def solveNQueens(self, n: int):
        if not n:
            return []
        # 生成n*n棋盘 需要用range生成棋盘，不能直接乘，因为直接乘每一列就指向一个内存
        chessboard = [['.'] * n for _ in range(n)]
        self.backtracking(chessboard, n, 0)
        return self.res

    def isValid(self, row, col, chessboard, n):
        # 单层搜索的过程中，每一层递归，只会选for循环（也就是同一行）里的一个元素，所以不用去重行
        # 不用检查左下角和右下角，因为是从上往下递归，下面的一定没有皇后
        # 检查列
        for j in range(0, n):
            if chessboard[j][col] == 'Q':
                return False
        # 检查左上角
        i = row - 1
        j = col - 1
        while i >= 0 and j >= 0:
            if chessboard[i][j] == 'Q':
                return False
            i -= 1
            j -= 1

        # 检查右上角
        i = row - 1
        j = col + 1
        while i >= 0 and j < n:
            if chessboard[i][j] == 'Q':
                return False
            i -= 1
            j += 1

        return True

    def backtracking(self, chessboard, n, row):
        # Base Case， 当找到最后一行的结果时，记录结果
        if row == n:
            temp_res = []
            for temp in chessboard:
                temp_str = "".join(temp)
                temp_res.append(temp_str)
            self.res.append(temp_res)
        # 遍历行
        for i in range(0, n):
            # 判断当前位置是否合法
            if not self.isValid(row, i, chessboard, n):
                continue
            chessboard[row][i] = 'Q'
            # 递归遍历到下一行
            self.backtracking(chessboard, n, row + 1)
            # 回溯
            chessboard[row][i] = '.'
```

## 37 Sudoku Solver

Problem: https://leetcode.com/problems/sudoku-solver/description/

Solution: https://programmercarl.com/0037.%E8%A7%A3%E6%95%B0%E7%8B%AC.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

Time Complexity: O(9^(m * n))

Space Complexity: O(m*n)

```python
class Solution:
    def __init__(self):
        self.board = None

    def solveSudoku(self, board) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        self.backtracking(board)
        self.board = board

    def is_valid(self, col, row, val, board):
        # 判断同一行是否冲突
        for i in range(9):
            if board[row][i] == str(val):
                return False

        # 判断同一列是否冲突
        for i in range(9):
            if board[i][col] == str(val):
                return False

        # 判断九宫格是否冲突
        start_row = (row // 3) * 3
        start_col = (col // 3) * 3
        for i in range(start_row, start_row + 3):
            for j in range(start_col, start_col + 3):
                if board[i][j] == str(val):
                    return False

        return True

    def backtracking(self, board):
        # 如果有解，返回True，如果没有则返回False
        for row in range(len(board)):
            for col in range(len(board[0])):
                # 如果空格内有数字，跳过
                if board[row][col] != '.':
                    continue
                for num in range(1, 10):
                    if self.is_valid(col, row, num, board):
                        board[row][col] = str(num)
                        if self.backtracking(board):
                            return True
                        # 回溯
                        board[row][col] = '.'
                # 如果数字1-9都不合法，说明无解，返回False
                return False
        # 如果最终有解，则返回 True，通过递归一层一层返回上去
        return True

s = Solution()
s.solveSudoku(board=[["5", "3", ".", ".", "7", ".", ".", ".", "."],
                     ["6", ".", ".", "1", "9", "5", ".", ".", "."],
                     [".", "9", "8", ".", ".", ".", ".", "6", "."],
                     ["8", ".", ".", ".", "6", ".", ".", ".", "3"],
                     ["4", ".", ".", "8", ".", "3", ".", ".", "1"],
                     ["7", ".", ".", ".", "2", ".", ".", ".", "6"],
                     [".", "6", ".", ".", ".", ".", "2", "8", "."],
                     [".", ".", ".", "4", "1", "9", ".", ".", "5"],
                     [".", ".", ".", ".", "8", ".", ".", "7", "9"]])
					

											"""
											['5', '3', '4', '6', '7', '8', '9', '1', '2']
											['6', '7', '2', '1', '9', '5', '3', '4', '8']
											['1', '9', '8', '3', '4', '2', '5', '6', '7']
											['8', '5', '9', '7', '6', '1', '4', '2', '3']
											['4', '2', '6', '8', '5', '3', '7', '9', '1']
											['7', '1', '3', '9', '2', '4', '8', '5', '6']
											['9', '6', '1', '5', '3', '7', '2', '8', '4']
											['2', '8', '7', '4', '1', '9', '6', '3', '5']
											['3', '4', '5', '2', '8', '6', '1', '7', '9']
											"""
						
```

## 回溯算法总结

https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html