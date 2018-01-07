## DFS notes
_update Jan 7, 2017  16:05_

---
### Basic Idea
DFS 是一种搜索算法，一个非常好的应用是用来解决排列组合问题。

写 DFS 的时候要注意两个要点：

1.  What does it store on each level, 每层的意义以及总的递归深度；
2.  How many different states should we try to put on each level，即每层有多少个不同的分支需要继续；

<br>

### 经典题目：
#### 1. [Subsets of a set](https://will-gxz.gitbooks.io/xiaozheng_algo/content/dfs/permutation-and-combination/subsets.html)

按照之前的两点考虑，例如输入是一个包含三个元素的set：{'a', 'b', 'c'}， 则：

  *  一共有三层 dfs，每层考虑该位置的元素是否包含；
  *  每层有两个分支，即包含或者不包含；

**时间复杂度** 为 `O(2^n)`, 因为一共有 n 层，每层每次拓展出 2 个分支；

以上的两种情况可以由如下方式来表示：
```
    'a'  'b'  'c'
     0    0    0
     1    1    1
```
即有三层dfs，分别对应 `'a', 'b', 'c'`，每层中又有两个分支，分别对应 `0, 1` 即包含或者不包含当前字符。

于是，根据之前的表示法，我们可以对有重复元素的 input set 进行表示, 例如input：{'a', 'b', 'b', 'b', 'c'}：
```
     'a' 'b' 'c'
      0   0   0
      1   1   1
          2
          3
```
即对于 `'b'` 来说，有四个分支，分别对应不选择 `'b'`，选第一个，第二个和选第三个；
          
          







