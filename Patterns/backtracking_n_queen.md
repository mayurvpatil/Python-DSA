

### **N-Queens Problems**

#### **Easy**
1. **[N-Queens](https://leetcode.com/problems/n-queens/)**
   - **Problem**: Place `n` queens on an `n x n` chessboard such that no two queens attack each other.
   - **Approach**:
     - Brute Force: Try all possible placements of queens and check for conflicts.
     - Optimized: Use backtracking to place queens row by row, ensuring no conflicts.
   - **Time Complexity**:
     - Brute Force: \(O(n^n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\) (chessboard)
     - Optimized: \(O(n^2)\) (chessboard)

   ```python
   def solveNQueens(n):
       def backtrack(row):
           if row == n:
               result.append(["".join(row) for row in board])
               return
           for col in range(n):
               if not is_under_attack(row, col):
                   board[row][col] = 'Q'
                   backtrack(row + 1)
                   board[row][col] = '.'
       def is_under_attack(row, col):
           for i in range(row):
               if board[i][col] == 'Q':
                   return True
               if col - (row - i) >= 0 and board[i][col - (row - i)] == 'Q':
                   return True
               if col + (row - i) < n and board[i][col + (row - i)] == 'Q':
                   return True
           return False
       result = []
       board = [['.' for _ in range(n)] for _ in range(n)]
       backtrack(0)
       return result
   ```

2. **[N-Queens II](https://leetcode.com/problems/n-queens-ii/)**
   - **Problem**: Count the number of distinct solutions to the N-Queens problem.
   - **Approach**:
     - Brute Force: Try all possible placements of queens and count valid configurations.
     - Optimized: Use backtracking to count valid configurations without storing the board.
   - **Time Complexity**:
     - Brute Force: \(O(n^n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\) (chessboard)
     - Optimized: \(O(n)\) (recursion stack)

   ```python
   def totalNQueens(n):
       def backtrack(row):
           if row == n:
               nonlocal count
               count += 1
               return
           for col in range(n):
               if not is_under_attack(row, col):
                   board[row][col] = 'Q'
                   backtrack(row + 1)
                   board[row][col] = '.'
       def is_under_attack(row, col):
           for i in range(row):
               if board[i][col] == 'Q':
                   return True
               if col - (row - i) >= 0 and board[i][col - (row - i)] == 'Q':
                   return True
               if col + (row - i) < n and board[i][col + (row - i)] == 'Q':
                   return True
           return False
       count = 0
       board = [['.' for _ in range(n)] for _ in range(n)]
       backtrack(0)
       return count
   ```

3. **[Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)**
   - **Problem**: Determine if a `9x9` Sudoku board is valid.
   - **Approach**:
     - Brute Force: Check all rows, columns, and subgrids for duplicates.
     - Optimized: Use sets to track seen numbers in rows, columns, and subgrids.
   - **Time Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2)\)
   - **Space Complexity**:
     - Brute Force: \(O(1)\)
     - Optimized: \(O(n)\)

   ```python
   def isValidSudoku(board):
       rows = [set() for _ in range(9)]
       cols = [set() for _ in range(9)]
       boxes = [set() for _ in range(9)]
       for i in range(9):
           for j in range(9):
               num = board[i][j]
               if num == '.':
                   continue
               if num in rows[i] or num in cols[j] or num in boxes[(i // 3) * 3 + (j // 3)]:
                   return False
               rows[i].add(num)
               cols[j].add(num)
               boxes[(i // 3) * 3 + (j // 3)].add(num)
       return True
   ```

---

#### **Medium**
4. **[Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)**
   - **Problem**: Solve a `9x9` Sudoku board.
   - **Approach**:
     - Brute Force: Try all possible numbers for each empty cell.
     - Optimized: Use backtracking to fill cells efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(9^{n^2})\)
     - Optimized: \(O(9^{n^2})\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2)\)

   ```python
   def solveSudoku(board):
       def is_valid(row, col, num):
           for i in range(9):
               if board[row][i] == num or board[i][col] == num or board[(row // 3) * 3 + i // 3][(col // 3) * 3 + i % 3] == num:
                   return False
           return True
       def backtrack():
           for i in range(9):
               for j in range(9):
                   if board[i][j] == '.':
                       for num in '123456789':
                           if is_valid(i, j, num):
                               board[i][j] = num
                               if backtrack():
                                   return True
                               board[i][j] = '.'
                       return False
           return True
       backtrack()
   ```

5. **[Word Search](https://leetcode.com/problems/word-search/)**
   - **Problem**: Given a 2D board and a word, determine if the word exists in the grid.
   - **Approach**:
     - Brute Force: Try all possible paths in the grid.
     - Optimized: Use backtracking to explore paths efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(m \cdot n \cdot 4^k)\)
     - Optimized: \(O(m \cdot n \cdot 4^k)\)
   - **Space Complexity**:
     - Brute Force: \(O(k)\) (recursion stack)
     - Optimized: \(O(k)\) (recursion stack)

   ```python
   def exist(board, word):
       def backtrack(i, j, index):
           if index == len(word):
               return True
           if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or board[i][j] != word[index]:
               return False
           temp = board[i][j]
           board[i][j] = '#'
           for dx, dy in [(0, 1), (1, 0), (0, -1), (-1, 0)]:
               if backtrack(i + dx, j + dy, index + 1):
                   return True
           board[i][j] = temp
           return False
       for i in range(len(board)):
           for j in range(len(board[0])):
               if backtrack(i, j, 0):
                   return True
       return False
   ```

6. **[Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)**
   - **Problem**: Partition a string such that every substring is a palindrome.
   - **Approach**:
     - Brute Force: Generate all possible partitions and check for palindromes.
     - Optimized: Use backtracking to generate valid partitions.
   - **Time Complexity**:
     - Brute Force: \(O(2^n)\)
     - Optimized: \(O(n \cdot 2^n)\)
   - **Space Complexity**:
     - Brute Force: \(O(n)\) (recursion stack)
     - Optimized: \(O(n)\) (recursion stack)

   ```python
   def partition(s):
       def is_palindrome(s):
           return s == s[::-1]
       def backtrack(start, path):
           if start == len(s):
               result.append(path)
               return
           for end in range(start + 1, len(s) + 1):
               if is_palindrome(s[start:end]):
                   backtrack(end, path + [s[start:end]])
       result = []
       backtrack(0, [])
       return result
   ```

---

#### **Hard**
7. **[N-Queens Puzzle](https://leetcode.com/problems/n-queens/)**
   - **Problem**: Place `n` queens on an `n x n` chessboard such that no two queens attack each other.
   - **Approach**:
     - Brute Force: Try all possible placements of queens and check for conflicts.
     - Optimized: Use backtracking to place queens row by row, ensuring no conflicts.
   - **Time Complexity**:
     - Brute Force: \(O(n^n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\) (chessboard)
     - Optimized: \(O(n^2)\) (chessboard)

   ```python
   def solveNQueens(n):
       def backtrack(row):
           if row == n:
               result.append(["".join(row) for row in board])
               return
           for col in range(n):
               if not is_under_attack(row, col):
                   board[row][col] = 'Q'
                   backtrack(row + 1)
                   board[row][col] = '.'
       def is_under_attack(row, col):
           for i in range(row):
               if board[i][col] == 'Q':
                   return True
               if col - (row - i) >= 0 and board[i][col - (row - i)] == 'Q':
                   return True
               if col + (row - i) < n and board[i][col + (row - i)] == 'Q':
                   return True
           return False
       result = []
       board = [['.' for _ in range(n)] for _ in range(n)]
       backtrack(0)
       return result
   ```

8. **[N-Queens II](https://leetcode.com/problems/n-queens-ii/)**
   - **Problem**: Count the number of distinct solutions to the N-Queens problem.
   - **Approach**:
     - Brute Force: Try all possible placements of queens and count valid configurations.
     - Optimized: Use backtracking to count valid configurations without storing the board.
   - **Time Complexity**:
     - Brute Force: \(O(n^n)\)
     - Optimized: \(O(n!)\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\) (chessboard)
     - Optimized: \(O(n)\) (recursion stack)

   ```python
   def totalNQueens(n):
       def backtrack(row):
           if row == n:
               nonlocal count
               count += 1
               return
           for col in range(n):
               if not is_under_attack(row, col):
                   board[row][col] = 'Q'
                   backtrack(row + 1)
                   board[row][col] = '.'
       def is_under_attack(row, col):
           for i in range(row):
               if board[i][col] == 'Q':
                   return True
               if col - (row - i) >= 0 and board[i][col - (row - i)] == 'Q':
                   return True
               if col + (row - i) < n and board[i][col + (row - i)] == 'Q':
                   return True
           return False
       count = 0
       board = [['.' for _ in range(n)] for _ in range(n)]
       backtrack(0)
       return count
   ```

9. **[Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)**
   - **Problem**: Solve a `9x9` Sudoku board.
   - **Approach**:
     - Brute Force: Try all possible numbers for each empty cell.
     - Optimized: Use backtracking to fill cells efficiently.
   - **Time Complexity**:
     - Brute Force: \(O(9^{n^2})\)
     - Optimized: \(O(9^{n^2})\)
   - **Space Complexity**:
     - Brute Force: \(O(n^2)\)
     - Optimized: \(O(n^2)\)

   ```python
   def solveSudoku(board):
       def is_valid(row, col, num):
           for i in range(9):
               if board[row][i] == num or board[i][col] == num or board[(row // 3) * 3 + i // 3][(col // 3) * 3 + i % 3] == num:
                   return False
           return True
       def backtrack():
           for i in range(9):
               for j in range(9):
                   if board[i][j] == '.':
                       for num in '123456789':
                           if is_valid(i, j, num):
                               board[i][j] = num
                               if backtrack():
                                   return True
                               board[i][j] = '.'
                       return False
           return True
       backtrack()
   ```

---

### **Summary of Approaches**
1. **Brute Force**:
   - Try all possible configurations and check for validity.
   - Time: \(O(n^n)\), Space: \(O(n^2)\).

2. **Optimized Backtracking**:
   - Use backtracking to explore valid configurations efficiently.
   - Time: \(O(n!)\), Space: \(O(n^2)\).

3. **Pruning**:
   - Skip invalid configurations early to reduce the search space.
   - Time: \(O(n!)\), Space: \(O(n^2)\).

---
