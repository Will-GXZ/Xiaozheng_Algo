## KMP Search Algorithm

---

KMP 的基本思想就是对O\(n^2\) 的brute force的匹配算法进行改进，改进的办法是对pattern字符串进行预处理，生成一个next数组，存放当前位置字符失配之后pattern指针要跳到的位置。

#### 如果我们已经得到了next\[\]数组

我们需要进行匹配，匹配的过程非常简单。维持两个指针**i** , **j**， **i** 对应source字符串，**j **对应pattern。每次失配后 i 不动，移动 j 指向next\[j\] 位置的pattern character， 继续匹配。（这里所讲只是一个思路，实际上应该指向的是next\[i - 1\]，因为这里的next数组指的其实只是当前公共前后缀最大长度，而非其他文章中所说的左移一位，然后把next\[0\] 设为 -1 的版本。我认为我的方法更易理解和实现，而其他文章中的方法或许效率更高，但毕竟是优化后的，不容易记忆。）匹配代码如下：

```java
    public int strStr(String source, String target) {
        if (source == null || target == null) return -1;
        if (target.length() == 0) return 0;
        int[] next = getNextArr(target);
        // start to do kmp search
        int i = 0, j = 0;
        while (i < source.length() && j < target.length()) {
            if (source.charAt(i) == target.charAt(j)) {
                i++;
                j++;
            } else {
                if (j == 0) i++;
                else j = next[j - 1];
            }
        }
        return j == target.length() ? i - j : -1;
    }
```

#### 下面我们来看如何获得next\[\] 数组

这个**next\[\] **数组的本质是当前位置之前段 **pattern**有一定长度相同前缀和后缀，而数组中所存储的就是这个长度。于是我们从这里出发，可以** set next\[0\] to 0**, 然后令 **i = 0, j = 1 **， 然后用ij右移进行匹配。这其实就是kmp算法本身的思想的应用，用**pattern**自己匹配自己。详情见下方的code。

```java
    public static void main(String[] args) {
        char[] p = "abcgabcfabcgabcg".toCharArray();;
        int[] next = new int[p.length];
        next[0] = 0;
        int i = 0, j = 1;
        while (j < next.length) {
            if (p[i] == p[j]) {
                next[j++] = ++i;
            } else {
                if (i == 0) next[j++] = 0; // 如果最左边的都不匹配，则只把j+1继续匹配。
                else i = next[i - 1]; // 如果在中段失配，则尝试更短的同样前缀去匹配，关键步骤，
                                      // 解释见下面。
            }
        }
        Arrays.stream(next).forEach(System.out::println);
    }
```

```java
    // output : 0000123012345674
```

上面的code用了一个比较有说服力的 example：

| a | b | c | g | a | b | c | f | a | b | c | g | a | b | c | g |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 1 | 2 | 3 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 4 |

注意最右边g对应的4. 当 **i **在 **next\[7\] **失配之后，我们让** i = next\[i - 1\] **即 **i = next\[6\] = 3** , 继续让 **i **和 **j **比较，发现 **p\[i\] == p\[j\] \(p\[3\] = p\[15\]\), **所以 **next\[j\] = ++i == 3 + 1 = 4 。**

一些同学可能会觉得这里的 **i = next\[i - 1\]** 很不好理解，起初我也觉得费解，所以在这里解释一下。要想理解这一步，首先要看 **i 、i - 1 **和 **next\[i - 1\] **三者之间的关系（有点绕哈哈）：

* **i : **当前需要和 **j** 匹配的字符的index，其实也是当前最大公共前缀后面的第一个需要匹配的字符的index。
* **i - 1 : **当前需要匹配字符之前最大公共前缀的长度。
* **next\[i - 1\] : **前一个最大公共前缀的长度，因为index是 0-based，所以**next\[i - 1\] **也是上一个最大公共前缀之后的第一个需要匹配的字符的index。

现在我们来用一句话解释。因为当前最大长度公共前缀后面的第一个字符 **i **失配，我们需要考虑上一个公共前缀后面的第一个字符也就是 **next\[i - 1\]， **看它是否和 p\[ **j \] **匹配**.** 

（如果理解不了，就写出这个例子，一定可以理解。）

#### Next\[\] 数组的优化

可以发现，依照上述方法构造的**next\[\] **数组是有缺陷的，比如下面这个pattern：

| a | b | c | d | a | b | c | d | a | b | c | d | a |
|--|
| 0 | 0 | 0 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |

当p**\[12\] **失配时，**i **会移动到 **next\[i - 1\] = next\[11\] = 8 , **而 **p\[8\] **和 **p\[12\] **都是 **a**，所以必然继续失配。为了避免这种不必要的情况发生，我们可以检查 **if \(p[i] == p(next[i - 1])\) { i = next[next[i - 1]] } **。优化之后的code:


```java
    asdasdsdfksldfjsldkfjl
```













