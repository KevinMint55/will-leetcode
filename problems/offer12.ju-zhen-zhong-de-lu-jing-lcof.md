## 题目地址

https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/

## 题目描述

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

示例 1：
```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

示例 2：
```
输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```

提示：

- 1 <= board.length <= 200
- 1 <= board[i].length <= 200

## 标签

- 深度优先搜索

## 思路

### 算法原理：

- 深度优先搜索： 可以理解为暴力法遍历矩阵中所有字符串可能性。DFS 通过递归，先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。
- 剪枝： 在搜索中，遇到 这条路不可能和目标字符串匹配成功 的情况（例如：此矩阵元素和目标字符不同、此元素已被访问），则应立即返回，称之为 可行性剪枝 。

### 算法流程

- 递归参数：i和j代表行列索引，k表示目标字符在word中的索引
- 终止条件：
  - 返回false：i与j越界、当前矩阵元素与目标字符不同、该矩阵元素已经访问过
  - 返回true：字符串word全部匹配，即k = word.length - 1
- 递推工作：
  - 双重循环矩阵，找到字符与word第一个字符相同开始深度优先搜索
  - 将board[i][j]值存于变量temp，并修改为字符'/'，表示已经访问过
  - 根据上下左右四个方向进行递归，并将结果存在res
  - 递归完毕重置board[i][j]值，并返回结果res

代码实现如下：
```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */

var exist = function(board, word) {
    const yLen = board.length;
    const xLen = board[0].length;
    const dfs = function (i, j, k) {
        if (i < 0 || i > yLen - 1 || j < 0 || j > xLen - 1 || board[i][j] != word[k]) {
            return false;
        }
        if (k == word.length - 1) {
            return true;
        }
        const temp = board[i][j];
        board[i][j] = '/';
        const res = dfs(i-1,j,k+1) || dfs(i+1,j,k+1) || dfs(i,j-1,k+1) || dfs(i,j+1,k+1);
        board[i][j] = temp;
        return res;
    }
    for (let i = 0; i < yLen; i++) {
        for (let j = 0; j < xLen; j++) {
            if (board[i][j] == word[0]) {
                if (dfs(i, j, 0)) {
                    return true;
                }
            }
        }
    }
    return false;
};
```

复杂度分析：

> M,N 分别为矩阵行列大小， KK 为字符串 word 长度。

- 时间复杂度是 O(3<sup>K</sup>MN)。
- 空间复杂度是 O(K)。
