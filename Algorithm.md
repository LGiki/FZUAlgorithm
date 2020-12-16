# 第一次作业

## YY and Matrix

### ★题目描述

YY 有一个大小为 $n * m$ 的矩阵，现在要对矩阵进行q次操作，操作分为如下三种：

0 x y：交换矩阵的x、y两行。

1 x y：交换矩阵的x、y两列。

2 x y：求当前矩阵第x行第y列的元素。

### ★输入

第一行三个正整数n、m、q，表示矩阵大小和操作次数。

接下来n行，每行m个空格隔开的整数，表示矩阵的元素。

接下来q行，每行三个数op、x、y，表示上述操作中的一种。

对于80%的数据，1 <= n、m、q <= 1000。

对于100%的数据，1 <= n、m <= 1000,1 <= q <= 1000000,矩阵元素大小在int范围内且非负。

### ★输出

对于操作2，输出一个整数，表示对应的元素。

### ★输入样例

```in
  2 2 6
  1 2
  3 4
  0 1 2
  1 1 2
  2 1 1
  2 1 2
  2 2 1
  2 2 2  
```

### ★输出样例

```out
4
3
2
1
```

### 代码

```c++
#include <iostream>

using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n, m, q, op, x, y;
    cin >> n >> m >> q;
    int matrix[n][m], row[n], col[m];
    for (int i = 0; i < n; i++) {
        row[i] = i;
        for (int j = 0; j < m; j++) {
            cin >> matrix[i][j];
        }
    }
    for (int i = 0; i < m; i++) {
        col[i] = i;
    }
    for (int i = 0; i < q; i++) {
        cin >> op >> x >> y;
        if (op == 0) {
            swap(row[x - 1], row[y - 1]);
        } else if (op == 1) {
            swap(col[x - 1], col[y - 1]);
        } else {
            printf("%d\n", matrix[row[x - 1]][col[y - 1]]);
        }
    }
    return 0;
}
```

## YY and Rectangle

### ★题目描述

有 $n$ 张卡片，标记为 $1, 2, ..., n$ 。标记为 $i$ 的卡片上写有正整数 $a_i$ 。

YY 和 ZZ 分别从这 $n$ 张卡片中选择一张卡片取出，假设 YY 和 ZZ 选择的卡片上的数字分别为 $a$ 和 $b$ ，如果 $a*b$ 的矩形的面积大于或等于给定的阈值 $p$ ，则称这次选择得到的矩形是“Perfect Rectangle”。

现在，YY想知道，对于不同的阈值 $p$ ，有多少种的选择使得她们得到的矩形是“Perfect Rectangle”。

**注意**：两人不会选择同一张卡片。

### ★输入

第一行，包含一个正整数 $n$。$(2 \leq n \leq 10^5)$

第二行，包含 $n$ 个正整数 $a_i$，表示卡片上的数字。（$1 \leq a_i \leq 3 *10^5$）

第三行，包含一个正整数 $m$ ，表示询问的次数。（$1 \leq m \leq 10^5$）

接下来有 $m$ 行，每行一个正整数 $p$ ，表示待询问的阈值。（$1 \leq p \leq 3 * 10^5$）

### ★输出

输出包含 $m$ 行，每行一个整数，表示对于对应的阈值，使得她们得到的矩形是“Perfect Rectangle”的选择个数。

### ★输入样例

```in
2
3 3
2
9
10
```

### ★输出样例

```out
2
0
```

### ★样例解释

有 2 张卡片，标记为 1 的卡片上写有正整数 3，标记为 2 的卡片上写有正整数 3

有 2 个询问：

- 对于询问 1，阈值为 9，因此她们可以有 2 种选择：（1, 2）和（2, 1）。

- 对于询问 2，阈值为 10，因此她们可以有 0 种选择。

### 思路

对于每次询问的阈值q：

- 遍历计算有一个乘数＜根号q的情况

- 计算两个乘数都大于等于根号q的情况

- ans *= 2

### 代码

> 时间复杂度$O(m * \text{sqrt}(p))$

```c++
#include <iostream>
#include <cmath>

const int MAX = 3e5 + 5;

using namespace std;

int cnt[MAX];
int sum[MAX];

int main() {
    int n, m, temp;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> temp;
        // 统计每个数字i的出现次数
        cnt[temp]++;
    }
    for (int i = 3e5; i >= 1; i--) {
        // 计算所有牌中，有多少张牌上的数字 >= i
        sum[i] = cnt[i] + sum[i + 1];
    }
    cin >> m;
    double query;
    while (m--) {
        long long result = 0;
        cin >> query;
        int i;
        for (i = 1; i < sqrt(query); i++) {
            result += (long long) cnt[i] * sum[(int) ceil(query / i)];
        }
        result += (long long) sum[i] * (sum[i] - 1) / 2;
        cout << result * 2 << endl;
    }
    return 0;
}
```

## YY and Array

### ★题目描述

YY 最近很头疼，明明学习数组学得头都大了，Z 老师还喜欢出一些丧心病狂的题目。今日份的配方如下：

Z 老师给了 YY 四个长度均为 $n$ （下标为 $1, 2, ..., n$）的数组：$a, b, c, d$ ，以及对应的两种数组操作：

- 操作1：对于所有的 $i \in \{1, 2, ..., n\}$ ，同时将所有的 $a_i$ 改为 $a_i \bigoplus b_i$ ，$\bigoplus$ 表示异或操作。
- 操作2：对于所有的 $i \in \{1, 2, ..., n\}$ ，同时将所有的 $a_i$ 改为 $a_{d_i}+t$ 。注意，数组 $d$ 是 1 到 $n$ 的一个排列。

现在，Z 老师希望 YY 连续进行 $m$ 次操作，使得最后得到的 $\sum_{i=1}^na_ic_i$ 最大，求该最大值。

YY 光是理解题目就已经耗掉所有精力了，帮帮 YY 吧。

### ★输入

第一行包含三个正整数 $n, m, t$ ，分别表示数组长度、操作次数、操作2的参数。（$1 \leq n, m \leq 25, \ 0 \leq t \leq 100$）

接下来四行，每行 $n$ 个整数，分别表示数组 $a, b, c, d$ ，数组 $d$ 是 1 到 $n$ 的一个排列。（$1 \leq a_i, b_i \leq 10^4, \ -10^4 \leq c_i \leq 10^4$）

### ★输出

输出包含一个整数 $s$，表示能得到的 $\sum_{i=1}^na_ic_i$ 的最大值。

### ★输入样例

```in
2 1 0
1 1
2 2
1 -1
2 1
```

### ★输出样例

```out
0
```

### ★样例解释

操作次数为 1，因此只有两种可能：

- 进行一次操作1，$a: \{1, 1\} \Rightarrow \{3, 3\}$，最后得到 $\sum_{i=1}^2a_ic_i=0$
- 进行一次操作2，$a: \{1, 1\} \Rightarrow \{1, 1\}$，最后得到 $\sum_{i=1}^2a_ic_i=0$

因此，最大值为 0。

### ★提示

异或操作的性质：对给定的数A，用同样的运算因子B作两次异或运算后仍得到A本身。即：

$$A \bigoplus B \bigoplus B = A$$

### 代码

```c++
#include <iostream>

using namespace std;

int n, m, t;
int a[26] = {0}, b[26] = {0}, c[26] = {0}, d[26] = {0};
long long result = -0x3f3f3f3f;

void dfs(int f, int leftOperationCount) {
    // leftOperationCount是否是偶数
    if ((leftOperationCount & 1) == 0) {
        long long sum = 0;
        for (int i = 1; i <= n; i++) {
            sum += (long long) a[i] * c[i];
        }
        if (sum > result) {
            result = sum;
        }
    }
    if (leftOperationCount == 0) {
        return;
    }
    int temp[26];
    for (int i = 1; i <= n; i++) {
        temp[i] = a[i];
    }
    if (f == 0) {
        for (int i = 1; i <= n; i++) {
            a[i] ^= b[i];
        }
        dfs(1, leftOperationCount - 1);
    }
    for (int i = 1; i <= n; i++) {
        a[i] = temp[d[i]] + t;
    }
    dfs(0, leftOperationCount - 1);
}

int main() {
    cin >> n >> m >> t;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i <= n; i++) {
        cin >> b[i];
    }
    for (int i = 1; i <= n; i++) {
        cin >> c[i];
    }
    for (int i = 1; i <= n; i++) {
        cin >> d[i];
    }
    dfs(0, m);
    cout << result << endl;
    return 0;
}
```

## YY and Square

### ★题目描述

YY 想找出所有的 $<n, m>$ 正整数对使得 $n$ 行 $m$ 列的表格中恰好包含 $t$ 个正方形。

### ★输入

输入一行，包含一个正整数 $t$。$(1 \leq t \leq 10^{18})$

### ★输出

第一行输出一个整数 $z$，表示 $<n, m>$ 对的个数。

接下来包含 $z$ 行，每行两个正整数 $n, m$，以空格分隔，表示 $n$ 行 $m$ 列的表格中恰好包含 $t$ 个正方形。

注意：输出 $<n, m>$ 对时，以 $n$ 递增的顺序输出，$n$ 相同时以 $m$ 递增的顺序输出。

### ★输入样例

```in
8
```

### ★输出样例

```out
4
1 8
2 3
3 2
8 1
```

### ★样例解释

 1行8列、2行3列、3行2列、8行1列的表格均恰好包含8个正方形

下图以2行3列为例：

![](https://msk-picgo.oss-cn-shanghai.aliyuncs.com/img/20200828161854.png)

### 思路

- 原有矩形的长度每增加1，所包含的小正方形的个数将增加1+2+…+m个（1个边长为m的+2个边长为m-1的+…+m个边长为1的）即m\*(m+1)/2个
- 因为长宽互换结果是一样的所以只考虑一种情况
- 已知m\*n的矩阵有m\*n+(m-1)\*(n-1)+…（直到m或n为1为止）个小正方形，那么以n为边的正方形必定有n\*(n+1)\*(2n+1)/6个正方形（平方和公式：n\*n+(n-1)\*(n-1)+…+2\*2+1\*1= n\*(n+1)\*(2n+1)/6 ）
- 我们假设矩阵的初始状态为一个边长i已知的正方形，计算每次增加一列时增加的小正方形的个数，即increase=i\*(i+1)/2
- 我们假定正方形矩阵的边长i从1开始每次增加1，当i\*(i+1)\*(2\*i+1)/6>t时结束，因为此时以i为边的正方形矩形包含的小正方形个数已满足条件
- 计算特定正方形数目t减去边长为i的正方形矩阵包含的正方形数目i\*(i+1)\*(2\*i+1)/6得到剩余的小正方形数目remain=t-i\*(i+1)\*(2\*i+1)/6
- 如果remain能整除increase，记j=remain/increase + i，则说明以i为宽j为长的矩阵能够满足条件（以j为宽i为长的矩阵也行）
- 记录每组<i,j>和<j,i>，统计答案的个数输出，最后对结果排序并依次输出

### 代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    unsigned long long t;
    cin >> t;
    vector<vector<unsigned long long> > ans;
    //i是行数，从1到n*(n+1)*(2n+1)/6 ，因为以n为边的正方形必定有t个正方形已满足条件
    for (unsigned long long i = 1; i * (i + 1) * (2 * i + 1) / 6 <= t; i++) {
        //平方和公式：n*(n+1)*(2n+1)/6
        unsigned long long remain = (t - i * (i + 1) * (2 * i + 1) / 6);
        //计算除去所求正方形个数减去以i为边长的正方形所提供的正方形个数
        //每增加一列，增加的正方形个数为1+2+3+...+i，即i*(i+1)/2
        unsigned long long increase = (i * (i + 1) / 2);
        //如果剩余的正方形数能整除增加一列所带来的新增正方形数目
        if (remain % increase == 0) {
            //那么列数就等于行数加上增加的列数
            unsigned long long j = remain / increase + i;
            ans.push_back(vector<unsigned long long>{i, j});
            if (i == j) {
                break;
            }
            ans.push_back(vector<unsigned long long>{j, i});
        }
    }
    sort(ans.begin(), ans.end());
    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i++) {
        cout << ans[i][0] << ' ' << ans[i][1] << endl;
    }
    return 0;
}
```

## YY and Sequence

### ★题目描述

"12345678910111213..."，YY 回味着今天 Z 老师教授的知识点——这样的数字序列的第 $k$ 位数字是什么。

YY 突发奇想，如果把所有的这种数字序列排放在一起，那么序列的第 $k$ 位数字是什么。

总而言之，题目是这样的：

给定一个数字序列 "1121231234..."，序列是由"1", "12", "123", "1234" ... 拼接而成的，第 $i$ 个拼接的子序列由数字 1 到 $i$ 组成。现在有 $q$ 个询问，每个询问包含一个正整数 $k$，YY 想知道上述的数字序列的第 $k$ 位数字是什么。

### ★输入

第一行，包含一个正整数 $q$ 。（$1 \leq q \leq 500$）

接下来 $q$ 行，每行一个正整数 $k$ 。（$1 \leq k \leq 10^9$）

### ★输出

输出包含 $q$ 行，每行一个正整数 $x$，表示对应询问的第 $k$ 位数字。($0 \leq x \leq 9$)

### ★输入样例

```in
5
1
2
3
4
5
```

### ★输出样例

```out
1
1
2
1
2
```

### 思路

枚举第k层所含有的字符数total\[i\] -> 记录前n层所含有的总字符数sum\[n\] -> 二分查找第k位数所在的层 -> 定位第k位数的具体字符

### 代码

```c++
#include <iostream>

using namespace std;

int total[250005];
int sum[250005];

// 统计某个数字的位数
// e.g. 123456 -> 6
int numberLength(int num) {
    int count = 0;
    while (num) {
        count++;
        num /= 10;
    }
    return count;
}

/*
 * 1
 * 12
 * 123
 * 1234
 * total: 每一层的字符数
 * sum[n]: 前n层的总字符数
 */
int main() {
    int q, k;
    cin >> q;
    for (int i = 1; i <= 25000; i++) {
        total[i] = total[i - 1] + numberLength(i);
        sum[i] = sum[i - 1] + total[i];
    }
    string str;
    for (int i = 0; i < 23000; i++) {
        str += to_string(i);
    }
    while (q--) {
        cin >> k;
        int left = 1;
        int right = 25000;
        int index;  // k在第几层
        while (true) {
            int middle = (left + right) / 2;
            if (sum[middle] < k && sum[middle + 1] >= k) {
                index = middle;
                break;
            }
            if (sum[middle + 1] < k) {
                left = middle + 1;
            }
            if (sum[middle] >= k) {
                right = middle - 1;
            }
        }
        cout << str[k - sum[index]] << endl;
    }
    return 0;
}
```

# 第二次作业

## YY and Lucky Number

### ★题目描述

YY 的幸运数字是......，是什么数字我也不知道，但是已知这个数字的十进制表示（不含前导零）中只包含不超过两个不同的数字。

给定一个数 $n$ ，请计算出不超过 $n$ 的所有正整数中，有可能是 YY 的幸运数字的个数。

### ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^5$

对于 100% 的数据：

$1 \leq n \leq 10^9$

### ★输出

输出一个整数，表示不超过 $n$ 的所有正整数中，有可能是 YY 的幸运数字的个数。

### ★输入样例

```in
103
```

### ★输出样例

```out
101
```

### ★样例解释

 不超过103的所有正整数中，只有102、103不可能是 YY 的幸运数字。

### 代码

```c++
#include <iostream>

using namespace std;

long long n;
int ans;

// A -> 第一位数， B -> 第二位数, now -> 当前数值
void dfs(int A, int B, long long now) {
    // 如果当前数值 > n直接返回
    if (now > n) {
        return;
    }
    ans++;
    // 若前两位数字A、B不同，则分别搜索在当前数值后追加A和追加B的答案
    if (A != B) {
        dfs(A, B, now * 10 + A);
        dfs(A, B, now * 10 + B);
    } else {
        // 若前两位数字相同，则从0开始枚举后一位数字，继续搜索
        for (int i = 0; i <= 9; i++) {
            dfs(B, i, now * 10 + i);
        }
    }
}

int main() {
    cin >> n;
    if (n <= 9) {
        cout << n << endl;
        return 0;
    }
    ans = 9;
    // 枚举前两位数字
    for (int i = 1; i <= 9; i++) {
        for (int j = 0; j <= 9; j++) {
            dfs(i, j, i * 10 + j);
        }
    }
    cout << ans << endl;
    return 0;
}
```

## YY and Triangle

### ★输入

一个正整数 $n$ ，表示三角形的规模。$(1 <= n <= 10)$

### ★输出

画出对应的三角形，注意最后一个有效字符后直接回车，且不要输出多余的空格。

规模为 1 的三角形如样例所示。

规模为 $n$ 的三角形由三个规模为 $n-1$ 的三角形拼接而成。

### ★样例输入1

```
1
```

### ★样例输出1

![avatar](data:img/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAA1CAIAAACBRl8ZAAACFElEQVRoBe2YIW/CQBSAywwkM7hKwh+onBtqFUsgKBRkiixZhoJMTLA/sIBCopaikGWqDjlJMrsEicOQMLcWGO8Kvfbd61vCyKGux929L+++S+5dZrVaGaf6uzhVsIBLw1F35xwzN3eqxe50vU2J/3F57y6p+ZHPo2Vu5g4WnVopt123UG7WR463kEch/kOBW0/HPbNVsfYh83aj/eF4830HU4MAt/Scvt0sFwSCXKnWWQzcmdDF0VSHm0+Go8eGnQ9Htyotszf+tTD8H/lLGc7XbdYuX+10g7i+eHbf8ViPhSJcoJsBRwHYDCMQzxtOOMVTgwt0s8SjINLxi6cEF+hWDx8Fkc7gFk8FbqPb0VEQ8ZjFw8PF6AZ8vOKh4WJ1AzpW8bBwSboBHqN4GX0ThrwqtbDbqrQo12ANR82kzpzOHDUD1HnaOZ05agao8/6tc+tpt1h10lQsKR8qYjIXUTyr7k/Khwo5nH+99NqxFQMCNd1DhRQu/FaD4IgekuraLoELqpm8rECNxpD1pri2R8NtqpnGtfhWI4ud3E+vFyPh8NVMMpo/glwvRsElF88oJhhEFe8YDlU8Q2BciybeERyyeMYxwSiSeIdwzLoBHUW8Azh23YCOIF4I7k90Azxl8VDPEZ+994cXCCK0sndP32+vQgc0s89fN7cmfBNaKDjCuixTQtvKsiLjIhqOmkydubPM3A9rNOQZ25nCzwAAAABJRU5ErkJggg==)

### ★样例输入2

```in
2
```

### ★样例输出2

![avatar](data:img/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAFQAAAB5CAIAAAAoO5F+AAAFBUlEQVR4Ae2dv2sUQRTHN1oYCELAIliF+AdcYRFsTOUV4oU0psphFQQxWCRaWMR/QC6FXJlKLoVEsLizukJImcIiYCGIkEquC0ggFoI7czGzczs/N+/NbHZfqt2Z3Xnzed/3NvBedjN1enqa1PXnWl3BGTfB11V9Up6Ur6EHKOxrKDpHJuVJ+Rp6gMK+hqLTA4/CPlDYH/dWFrYPzsbW0pOZp/2TQKbVZgIqf9TvjrZWl6bHG5lvra/t9YYj9bbCjAaDPzvY78xtLDcusGab7c3D3vD4YiD8QSj4k2Fvp7nems8QTi+tbo26/aPMUODDQPDHg9295+3mrEzXWN6Y6+z/fwrIcyHOwsCn6X602Vo8T3fBlSZ+c6c3jPXYCwHP0j0RjzrBniQs8Ye7g0iJHwCepXsj+6jL0kdNfHx4lu5r8qMuS59ETHx0eJ7uuUddFj9e4iPDG9Jd8EdLfFx4Y7oL+liJjwpvS3eBHyfxp6hRKSSo1RFq2JfdkwRfdoWw9kfKY3m27OuS8mVXCGt/pDyWZ8u+LilfdoWw9kfKg3pWakoVWfnsYHvhXjdEOR9cebkpVQSelTaSzvCwyL1+9wDD55pSfrs5vzotbTS6ffxmBiy8oilViD5QTRMUXt2UKoIfpqYJCa9rShWhD1LThIN3qlJ7+CFATRMM3rFK7UGPn/hQ8O5Vand89MQHgrc3pdyZxZXYiQ8CD53uAh838SHg4dNd0KMmPgA8RroLeszEx2tX/Xg38/2jgMgctW62Br8HmQFx2Lr96cPdW+Ic9wgPHnffIKsDhD3IPqIsQvBR3F4Co6R8CUSIsgVSPorbS2CUlC+BCFG2QMpHcXsJjJLyJRAhyhYKKi815FDfEmOdu5Xe+G0E6C5eMXi5IZdWW9DeEpN6QNBdvCLwuYYcq7bgvCWWVomGm5m/1oft4hWAl8QY5ypWmVWOMGYLtKbnD69uyGGUWVmEzU6+nANZ0/OG1zXkQCXh8cSLwu372fcQ2ThglHnCGyr0kJIwSH1RGCzK/OCNFXpASVJ2Uw8IKsq84PViMKkSwLfEDBHGDAFFmQ+8SQy2JbhnsTHCmCGYKHOHt4jBtgQliS3CmCWIxHeGt4rBdgQjiT3CmCWAxHeFdxGDbenykjhFGDN0+cSndhXzYx1/XMO+kr4h+ErK6gBFyjs4qZKXkPKVlNUBipR3cFIlLyHlKymrAxQpr3FStlOkucQy7NzIktpflkWV04U6WQblWfli4hN2SruGwbTg4NTISssXmY8kGhbUTrG6lvf7aHr4tHwhdYq0dk0TrOBgbWSx8oX0kUTTitq5Ap0sLfzlxeDbdKg0AkQYN+Vf19LAMzFynSKt040TtkojK5DlP5JoXFIz6V3XUsPzamW+U6Qxahk2K8KrlYqPJFoWVU47hJl0nxLevVopraU7MSjiXK3UrT0xbgsz+XIVvFvpWF7HeKZVxLEeblxcnjSHmXyt4v/YQIvBDaoVAY4wbskQZhPoin/iAy8Gt6lSBDzCuCVtmOXYc8pjiMGt5hRBiTBuSh1mefZJeBwxuN0JRZAijJtShZmCXYbHE4ObziqCFmHcUi7MVOhJ4tSu+tb5/OyN8v4bT179ef9WPfX654OHc8op/eDo68s7v9QfjFi9nuz/Vd75+MujF4vKGcugE7xljSs7rfo9f2VhfDdO8L4eq8r1pHxVlPTlIOV9PVaV62ut/D9xq5q3e3vCWwAAAABJRU5ErkJggg==)

### 代码

```c++
#include <iostream>

using namespace std;

string triangle[1024] = {" /\\", "/__\\"};

void draw(int n) {
    // step = 2^(n-1)
    int step = 1 << (n - 1);
    for (int i = 2 * step - 1; i >= step; i--) {
        triangle[i] = triangle[i - step];
        for (int j = 1; j < 2 * step - i; j++) {
            triangle[i] += " ";
        }
        triangle[i] += triangle[i - step];
    }
    string temp;
    for (int i = step - 1; i >= 0; i--) {
        temp = "";
        for (int j = 1; j <= step; j++) {
            temp += " ";
        }
        triangle[i] = temp + triangle[i];
    }
}

int main() {
    int n;
    cin >> n;
    for (int i = 2; i <= n; i++) {
        draw(i);
    }
    for (int i = 0; i <= (1 << n) - 1; i++) {
        cout << triangle[i] << endl;
    }
    return 0;
}
```

## YY and Pair

### ★题目描述

对于给定的一对正整数 $(x,y)$ ，可以进行若干次的如下操作：

1. 将 $(x, y)$ 修改为新的一对正整数 $(x + y, y)$
2. 将 $(x, y)$ 修改为新的一对正整数 $(x, y + x)$

YY 想知道，对于初始正整数对 $(1, 1)$ ，最少需要经过多少次操作使得最终得到的正整数对中包含至少一个正整数 $n$ 。

### ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^3$

对于 100% 的数据：

$1 \leq n \leq 10^6$

### ★输出

输出一个整数，表示最少需要的操作次数。

### ★输入样例

```in
7
```

### ★输出样例

```out
4
```

### ★样例解释

$n=7$ 时的一个最优解： $(1, 1) \rightarrow (1, 2) \rightarrow (3,2) \rightarrow (5,2) \rightarrow (7,2)$ ，需要四次操作。

### 思路

假设最优解的为(n, t)，那么最优解上一个状态为(max(n-t, t), min(n-t, t))，再上一个为(max(max1-min1,min1),min(max1-min1,min1))

当n>t>1时，F(n, t) = F(max(n-t, t), min(n-t,t), t) + 1；当n=t=1时，F(1, 1) = 0；t=0时，不符合规则，舍去答案

优化：如果整数对位(999999, 2)，上一个(999999-2, 2)经过无数次-2，终于到了(2,1)再到(1,1)

从F(999999,2)到F(2,1)其实就是减了(999999/2)次，得到F(2, 999999%2) -> F(n, t) = F(t, n%t) + n/t

### 代码

> 时间复杂度O(nlogn)

```c++
#include <iostream>

using namespace std;

int main() {
    int n, ans, count, min, max, temp;
    cin >> n;
    ans = n - 1;
    for (int i = 2; i <= n / 2; i++) {
        count = 0;
        min = i;
        max = n;
        // min == 0时舍去答案
        while (min != 0 && min != 1) {
            temp = max % min;
            count += max / min;
            max = min;
            min = temp;
        }
        // min == 1时可直接得出答案
        if (min == 1) {
            count += max - 1;
            if (count < ans) {
                ans = count;
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```

## YY and Point

### ★题目描述

二维空间中散落着数不胜数的点，YY 可以将每个点的坐标 $(x_i, y_i)$ 修改为以下四种之一：

1. $(x_i, y_i)$
2. $(-x_i, y_i)$
3. $(x_i. -y_i)$
4. $(-x_i, -y_i)$

YY 想知道经过修改之后能得到的最小两点“Y式距离”是多少。

两点“Y式距离”定义为：若两点坐标分别为 $(x_1, y_1)$ 和 $(x_2, y_2)$ ，则两点“Y式距离”为 $D_Y = \sqrt{(x_1+x_2)^2+(y_1+y_2)^2}$

### ★输入

第一行，包含一个正整数 $n$。

接下来有 $n$ 行，每行两个整数 $x_i$ 和 $y_i$ ，表示点的坐标。

对于 60% 的数据：

$2 \leq n \leq 10^3, -10^6 \leq x_i, y_i \leq 10^6$

对于 100% 的数据：

$2 \leq n \leq 10^5, -10^6 \leq x_i, y_i \leq 10^6$

### ★输出

令得到的最小两点“Y式距离”为 $D_Y$ ，输出一个整数，表示 ${|D_Y|}^2$。

### ★输入样例

```in
4
2 4
-3 4
-5 -8
1 -7
```

### ★输出样例

```out
1
```

### ★样例解释

将点 $(2,4)$ 和点 $(-3, 4)$ 分别修改为 $(-2, 4)$ 和 $(3, -4)$ ，得到 $|D_Y|^2=((-2)+3)^2+(4+(-4))^2=1$

### 代码

```c++
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;
const long long inf = 0x3f3f3f3f;
const int N = 1e5 + 10;
int n;
struct Point {
    int x, y;
} a[N], b[N];

bool cmpx(const Point a, const Point b) {
    return a.x < b.x;
}

bool cmpy(const Point a, const Point b) {
    return a.y < b.y;
}

long long dis(Point a, Point b) {
    return (pow(a.x - b.x, 2) + pow(a.y - b.y, 2));
}

long long solve(int l, int r) {
    if (l == r) {
        return inf;
    }
    int mid = (l + r) / 2, tail = 0;
    // 递归求出左右两边的最小距离
    long long d = min(solve(l, mid), solve(mid + 1, r));
    // 选出左半边与中间位置的距离在d以内的点，存入b[]数组中
    for (int i = mid; i >= l && a[mid].x - a[i].x < d; i--) {
        b[tail++] = a[i];
    }
    // 选出右半边与中间位置的距离在d以内的点，存入b[]数组中
    for (int i = mid + 1; i <= r && a[i].x - a[mid].x < d; i++) {
        b[tail++] = a[i];
    }
    // 将b[]数组（即左右两边与中间位置距离在d以内的点）按照y升序的方式进行排序
    sort(b, b + tail, cmpy);
    for (int i = 0; i < tail; i++) {
        for (int j = i + 1; j < tail && b[j].y - b[i].y < d; j++) {
            // 寻找是否存在两个点在左右半边，且两点间距离小于d
            d = min(d, dis(b[i], b[j]));
        }
    }
    return d;
}

// 在递归函数内的排序需要O(nlogn)，二分法递归本身的复杂度为O(logn)，所以这个算法的复杂度是O(nlognlogn)
int main() {
    int x, y;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> x >> y;
        a[i].x = abs(x);
        a[i].y = abs(y);
    }
    sort(a + 1, a + n + 1, cmpx);
    cout << solve(1, n) << endl;
    return 0;
}
```

## YY and One

### ★题目描述

YY 很享受一个人独处的时光。

这可能也是 YY 对 1 这个数字情有独钟的原因。

现在，对于给定的一个正整数 $n$，YY 想把它表示为若干个加数之和，其中每个加数都是仅包含 1 的整数。YY 想知道这些加数最少可以包含多少个 1 。

例如：对于正整数 10，可以表示为：$10=11+(-1)$ ，包含 3 个 1 。

### ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^5$

对于 100% 的数据：

$1 \leq n \leq 10^{12}$

### ★输出

输出一个整数，表示所有加数包含 1 的个数的最小值。

### ★输入样例

```in
1232
```

### ★输出样例

```out
10
```

### ★样例解释

最优解为：$1232=1111+111+11+(-1)$ ，包含 10 个 1 。

### 代码

```c++
#include <iostream>
#include <cmath>

using namespace std;

int sumOne = 0;
long long ones[] = {0, 1, 11, 111, 1111, 11111, 111111, 1111111, 11111111, 111111111, 1111111111, 11111111111,
                    111111111111, 1111111111111};
/*
 *         1232
 *     1111 /  \ 11111
 *         /    \
 *       121  |1232-11111|
 *   111 /  \ 1111
 *      /    \
 *     10  |121-1111|
 * 11 /  \ 111
 *   1  |10-111|
 */
void countSumOne(long long n) {
    if (n == 0) {
        return;
    }
    // digits表示n是几位数
    int digits = (int) log10(n) + 1;
    if (fabs(n - ones[digits]) <= fabs(n - ones[digits + 1])) {
        // 左分支
        sumOne += digits;
        countSumOne(fabs(n - ones[digits]));
    } else {
        // 右分支
        sumOne += digits + 1;
        countSumOne(fabs(n - ones[digits + 1]));
    }
}

int main() {
    long long n;
    cin >> n;
    countSumOne(n);
    cout << sumOne << endl;
    return 0;
}
```

# 第三次作业

## YY and Shop II

### ★题目描述

喜迎国庆中秋“双节”，周边的商店也迎来了促销。YY 在本期促销活动中抽到了特等奖！

本期活动中，共有 $n$ 件商品参与促销，编号分别为 $1, 2, ..., n$ ，第 $i$ 件商品的价值为 $v_i$ ，YY 可以选择 $m$ 件拿走。但是，拿走商品有一定的限制，某些商品不能被直接拿走，如果想要拿走它，必须要先拿走它指定的另一件特定商品。

YY 想知道，最多能拿走总价值为多少的商品。

### ★输入

第一行，包含两个正整数 $n$ 和 $m$ 。

接下来有 $n$ 行，每行包含两个整数 $f_i$ 和 $v_i$。若 $f_i=0$ ，表示该商品可以直接拿走，否则，表示拿走该商品前需要先拿走第 $f_i$ 个商品。$v_i$ 表示该商品的价值。

对于 100% 的数据：

$1 \leq n, m \leq 300, \ 0 \leq f_i \leq n, \ 0 \leq v_i \leq 10^6$

数据保证商品的需求关系不存在环。

### ★输出

输出一个整数，表示能拿走的最大商品总价值。

### ★输入样例

```in
3 2
0 1
0 2
0 3
```

### ★输出样例

```out
5
```

### ★样例解释

3个商品都可以直接拿走，因此选择编号为 2 和 3 的商品，得到的总价值为 5。

### 思路

- 树状DP
- f\[i\]\[j\]：以i为根节点的子树取j个节点时，获得的最大价值
- 状态转移方程：f\[father\]\[j\] = max(f\[father\]\[j\], f\[son\]\[k\] + f\[father\]\[j-k\])

### 代码

> 时间复杂度：O($n^2$)

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int n, m, f[305][305];
vector<int> G[305];

void dfs(int x) {
    // 遍历G的所有子节点
    for (int i = 0; i < G[x].size(); i++) {
        // son是x的第i个子节点
        int son = G[x][i];
        // 递归到叶子节点y，G[y].size()=0
        dfs(son);
        for (int j = m + 1; j >= 1; j--) {
            for (int k = 0; k < j; k++) {
                f[x][j] = max(f[x][j], f[son][k] + f[x][j - k]);
            }
        }
    }
}

/*
 *  输入：
 *  6 3           0
 *  0 2          / \
 *  5 1         1   5
 *  4 3        /   / \
 *  1 5       4   2   6
 *  0 4      /
 *  5 3     3
 */
int main() {
    int u;
    cin >> n >> m;
    for (int i = 1; i <= n; i++) // u = father
    {
        // f[i][j]: 以节点i为根，选了j件商品的最大价值
        cin >> u >> f[i][1];
        // G[u]中存放了节点u的子节点的编号，u=0时，FS[0]是根节点编号
        G[u].push_back(i);
    }
    dfs(0);
    // 多加一个编号0的商品
    printf("%d", f[0][m + 1]);
}
```

## YY and Inverse

### ★题目描述

逆序对的定义为：对于序列的第 $i$ 个和第 $j$ 个元素，如果满足 $i<j$ 且 $a_i>a_j$ ，则其为一个逆序对；否则不是。

YY 想知道 $1$ 到 $n$ 的全排列中，恰好拥有逆序对个数为 $t$ 的排列个数。

### ★输入

输入一行，包含两个正整数 $n$ 和 $t$ 。

对于 30% 的数据：

$2 \leq n \leq 8, 0 \leq t \leq 20$

对于80% 的数据：

$2 \leq n \leq 100, 0 \leq t \leq 100$

对于 100% 的数据：

$2 \leq n \leq 1000, 0 \leq t \leq 1000$

### ★输出

若恰好拥有逆序对个数为 $t$ 的排列个数为 $X$ ，输出 $X \ mod \ 1000000007$ 。

### ★输入样例

```in
3 1
```

### ★输出样例

```out
2
```

### ★样例解释

$n=3$ 时的全排列为：$[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]$

其中 $[1,3,2]$ 和 $[2,1,3]$ 有 $1$ 个逆序对。

### 思路

dp\[i\]\[j\]表示从1到i的全排列中有j个逆序对的排列个数

若已知1...i-1的全排列，将i放到全排列的任意位置：

- 当i放到最后一个位置，增加了0个逆序对 -> dp\[i-1\]\[j-0\]
- 当i放到倒数第二个位置，增加了1个逆序对 -> dp\[i-1\]\[j-1\]
- 当i放到第一个位置，增加了i-1个逆序对 -> dp\[i-1\]\[j-(i-1)\]

所以，dp\[i\]\[j\] = dp\[i-1\]\[j\] + dp\[i-1\]\[j-1\] + ... + dp\[i-1\]\[j-(i-1)\]

用dp\[i\]\[j+1\] = dp\[i-1\]\[j+1\] + dp\[i-1\]\[j\] + ... + dp\[i-1\]\[j+1-(i-1)\]减去上式可得：

dp\[i\]\[j+1\] - dp\[i\]\[j\] = dp\[i-1\]\[j+1\] - dp\[i-1\]\[j-i+1\]

即dp\[i\]\[j+1\] = dp\[i\]\[j\] + dp\[i-1\]\[j+1\] - dp\[i-1\]\[j-i+1\]
即dp\[i\]\[j\] = dp\[i\]\[j-1\] + dp\[i-1\]\[j\] - dp\[i-1\]\[j-i\]

### 代码

时间复杂度：O(nt)

```c++
#include <iostream>

using namespace std;

int dp[1010][1010];

int main() {
    int n, t;
    int modNum = 1e9 + 7;
    dp[1][0] = 1;
    cin >> n >> t;
    for (int i = 2; i <= n; i++) {
        // 1到i的全排列中逆序对的数量为0只有1种(即1...i)
        dp[i][0] = 1;
        for (int j = 1; j <= t; j++) {
            dp[i][j] = (dp[i][j - 1] + dp[i - 1][j]) % modNum;
            // 只有当 j >= i时，最有一项才有值才要减去
            if (i <= j) {
                dp[i][j] = ((dp[i][j] - dp[i - 1][j - i]) % modNum + modNum) % modNum;
            }
        }
    }
    cout << dp[n][t] << endl;
    return 0;
}
```

## YY and Fibonacci

### ★ 题目描述

如果一个序列 $a_1,a_2,...,a_m$ 满足：

* m≥3
* 对于所有 i≥3 ，都有 $a_i=a_{i−1}+a_{i−2}$

则称序列 $a_1,a_2,...,a_m$ 具有 Fibonacci 性。

现在给定一个严格递增的序列 $a_1,a_2,...,a_n$，YY 想知道其中最长的具有 Fibonacci 性的子序列的长度。

### ★ 输入

第一行，包含一个正整数 n。

第二行，包含 n 个正整数，表示序列 $a_1,a_2,...,a_n$。

对于 60% 的数据：

3≤n≤100,1≤a[1]<a[2]<...<a[n]≤$10^9$

对于 100% 的数据：

3≤n≤3000,1≤a[1]<a[2]<...<a[n]≤$10^9$

### ★ 输出

输出一个整数，表示最长的具有 Fibonacci 性的子序列的长度。如果不存在这样的子序列，输出 −1 。

### ★ 输入样例

```in
5
1 2 3 4 5
```

### ★ 输出样例

```out
4
```

### ★ 样例解释

最长的具有 Fibonacci 性的子序列为：1,2,3,5

### ★ 提示

可能会用到的数据结构：[c++ - unordered_map](http://www.cplusplus.com/reference/unordered_map/unordered_map/)
### 代码

> 时间复杂度：O($n^2$)

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

/*
 * e.g.:
 * +───────+───+───+───+────+────+────+────+────+
 * | Value | 3 | 4 | 7 | 11 | 14 | 15 | 18 | 29 |
 * +───────+───+───+───+────+────+────+────+────+
 * | Index | 0 | 1 | 2 | 3  | 4  | 5  | 6  | 7  |
 * +───────+───+───+───+────+────+────+────+────+
 * 如果最长的斐波那契子序列以18，29为最后两个数，那么根据斐波那契数列的性质，这个序列一定包含11
 * 要想知道这个以11，18，29为最后三个数的斐波那契序列有多长，那么我们需要知道以11，18为最后两个数的最长斐波那契子序列有多长
 * 假设dp[i][j]代表依次以第j个数字和第i个数字为最后两个数的斐波那契子序列的长度，那么可以得到状态转移方程
 * dp[i][j] = dp[j][id] + 1
 * 其中a[id],a[j],a[i]组成了一组斐波那契数列
 */
int main() {
    unordered_map<long long, int> index;
    unordered_map<int, int> dp;
    int n, ans = 0;
    cin >> n;
    int a[n];
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        index[a[i]] = i;
    }
    for (int j = 2; j < n; j++) {
        for (int i = 1; i < j; i++) {
            long long first = a[j] - a[i];
            if (first < a[i] && index.count(first)) {
                int id = index[first];
                dp[i * n + j] = dp[id * n + i] + 1;
                ans = max(ans, dp[i * n + j] + 2);
            }
        }
    }
    if (ans >= 3) {
        cout << ans << endl;
    } else {
        cout << -1 << endl;
    }
    return 0;
}
```

## YY and Card

### ★题目描述

YY 有 $n$ 张卡片，第 $i$ 张卡片上写有数字 $a_i$ 。

YY 可以修改卡片上数字的符号，如将 $5$ 修改为 $-5$ ，也可以保持卡片上的数字不变

YY 想知道一共有多少种方案，使得最终所有卡片上的数字之和为 $T$ 。

### ★输入

第一行，包含两个整数 $n$ 和 $T$。

第二行，包含 $n$ 个正整数，表示卡片上的数字 $a_1, a_2, ..., a_n$。

对于 60% 的数据：

$1 \leq n \leq 20, \ a_i > 0, \ \sum a_i \leq 1000, \ |T| \leq 1000$

对于 100% 的数据：

$1 \leq n \leq 1000, \ a_i > 0, \ \sum a_i \leq 1000, \ |T| \leq 1000$

### ★输出

若方案数为 $X$ ，输出 $X \ mod \ 1000000007$ 。

### ★输入样例

```in
5 3
1 1 1 1 1
```

### ★输出样例

```out
5
```

### ★样例解释

$-1+1+1+1+1 = 3$

$+1-1+1+1+1 = 3$

$+1+1-1+1+1 = 3$

$+1+1+1-1+1 = 3$

$+1+1+1+1-1 = 3$

### 思路

n个数字肯定一部分取+，另一部分取-，将取+的数放到P集合，取-的数放到N集合，则
$$
\begin{align}
\text{SUM}(\text{P})-\text{SUM}(\text{N})&=t\\
\text{SUM}(\text{P})+\text{SUM}(\text{N})&=sum
\end{align}
$$
t是给定的数，sum是输入的n个数字的和，将两个式子相加消去SUM(N)，得到$\text{SUM}(\text{P})=\frac{t+sum}{2}$，所以题目就简化为从n个数中选出一部分数，使其的和为$\frac{t+sum}{2}$的方案数（0-1背包问题）

令dp\[i\]\[j\]表示前i个物品放进重量为j的背包有几种方案，转移方程为：dp\[i\]\[j\]=dp\[i-1\]\[j\]+dp\[i-1\]\[j-nums\[i\]\]，其中dp\[i-1\]\[j\]表示不把第i个物品放进背包，dp\[i-1\]\[j-nums\[i\]\]表示把第j件物品放入背包中。初始化：dp\[0\]\[0\]=1，把0个物品放进重量为0的背包方案数为1。

dp\[i\]\[j\]只和前一行的dp\[i-1\]\[...\]有关，将二维转换为一维：dp\[j\]=dp\[j\]+dp\[j-nums\[i\]\]。

时间复杂度O(n*$\frac{sum+t}{2}$)，空间复杂度O($\frac{sum+t}{2}$)

### 代码

```c++
#include <iostream>
#include <algorithm>

using namespace std;
const int MOD_NUM = 1e9 + 7;

int main() {
    int num[1001], dp[1001], n, t, sum = 0, target;
    cin >> n >> t;
    for (int i = 0; i < n; i++) {
        cin >> num[i];
        sum += num[i];
    }
    dp[0] = 1;
    target = (sum + t) / 2;
    // 如果目标大于数字总和直接输出0
    // (sum+t)/2肯定是偶数，当不是偶数的时候直接输出0
    if (sum < abs(t) || (sum + t) % 2 != 0) {
        cout << 0 << endl;
        return 0;
    }
    for (int i = 0; i < n; i++) {
        for (int j = target; j >= num[i]; j--) {
            dp[j] = (dp[j] + dp[j - num[i]]) % MOD_NUM;
        }
    }
    cout << dp[target] << endl;
    return 0;
}
```

## YY and Shop I

### ★题目描述

喜迎国庆中秋“双节”，周边的商店也迎来了促销。

本期活动中，共有 $n$ 种商品参与促销，编号分别为 $0, 1, ..., n-1$ ，第 $i$ 种商品的促销价为 $c_i$ ，原价为 $v_i$ ，限购数量为 $k_i$ 。但是事情并没有这么简单，店家会在活动当天宣布有一种商品退出促销，无法购买。

为了获取最大利益，YY 想提前计算好 $q$ 种情况，即在给定携带资金 $x_i$ 和无法购买第 $y_i$ 种商品的情况下，最多能够买走原价总和是多少的商品。注意：商品论件出售，不允许拆分。

### ★输入

第一行，包含一个正整数 $n$ 。

接下来有 $n$ 行，每行包含三个整数 $c_i$ ，$v_i$ 和 $k_i$。

接下来一行，包含一个正整数 $q$ 。

之后 $q$ 行，每行包含两个整数 $x_i$ 和 $y_i$ 。

对于 60% 的数据：

$1 \leq n\leq 50, \ 1 \leq q \leq 20$

$1 \leq c_i, v_i, k_i \leq 100$

$0 \leq x_i \leq 1000, 0 \leq y_i < n$

对于 100% 的数据：

$1 \leq n\leq 800, \ 1 \leq q \leq 50000$

$1 \leq c_i, v_i, k_i \leq 100$

$0 \leq x_i \leq 1000, 0 \leq y_i < n$

### ★输出

输出包含 $q$ 行，每行包含一个整数，表示对应的情况下 YY 能买走的最大原价总和。

### ★输入样例

```in
3
1 3 1
1 4 1
1 5 1
1
1 2
```

### ★输出样例

```out
4
```

### ★样例解释

资金为 1 且不能购买编号为 2 的商品，因此选择购买 1 件编号为 1 的商品，得到的原价总和为 4 。

### 思路

front\[i\]\[j\]表示只考虑前i种商品，花费j元所能获得的最大原价总和

behind\[i\]\[j\]表示只考虑后i种商品，花费j元所能获得的最大原价总和

front\[i\]\[cost\] = max(front\[i\]\[cost\], front\[i-1\]\[cost-c\[i\]\*j\]+j\*v\[i\])
behind\[i\]\[cost\] = max(behind\[i\]\[cost\], behind\[i+1\]\[cost-c\[i\]\*j\]+j\*v\[i\])

其中c\[i\]为第i件商品的促销价，v\[i\]为第i件商品的原价，j为第i件商品买的件数，$0\leq j\leq k[i]$，k\[i\]为第i件商品的限购数量

因为商品$y_i$不能购买，对于每次查询以第$y_i$件商品为分界线，问题转换为花费$x_i$元购买前$y_i-1$种商品和后$n-y_i$件商品所能得到的最大原价总和。

maxSum = max(maxSum, front\[y\[i\]-1\]\[cost\]+behind\[y\[i\]+1\]\[x\[i\]-cost\])

表示花费cost元购买前y\[i\]-1件商品和花费x\[i\]-cost元购买后y\[i\]+1件商品，所能买到的最大原价总和，$0\leq \text{cost}\leq x[i]$

时间复杂度：O($\text{maxX}*\sum k[i]$)

### 代码

```c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int front[810][1010], behind[810][1010];
    int n, q;
    cin >> n;
    int c[n + 1], v[n + 1], k[n + 1];
    for (int i = 1; i <= n; i++) {
        cin >> c[i] >> v[i] >> k[i];
    }
    cin >> q;
    int x[q], y[q], maxX = -0x3f3f3f3f;
    for (int i = 0; i < q; i++) {
        cin >> x[i] >> y[i];
        y[i]++;
        if (x[i] > maxX) {
            maxX = x[i];
        }
    }
    // 考虑第i个商品的情况
    for (int i = 1; i <= n; i++) {
        // 考虑第i个商品购买j个
        for (int j = 0; j <= k[i]; j++) {
            for (int cost = c[i] * j; cost <= maxX; cost++) {
                front[i][cost] = max(front[i][cost], front[i - 1][cost - c[i] * j] + j * v[i]);
            }
        }
    }
    for (int i = n; i > 0; i--) {
        for (int j = 0; j <= k[i]; j++) {
            for (int cost = c[i] * j; cost <= maxX; cost++) {
                behind[i][cost] = max(behind[i][cost], behind[i + 1][cost - c[i] * j] + j * v[i]);
            }
        }
    }
    for (int i = 0; i < q; i++) {
        int maxSum = 0;
        for (int cost = 0; cost <= x[i]; cost++) {
            maxSum = max(maxSum, front[y[i] - 1][cost] + behind[y[i] + 1][x[i] - cost]);
        }
        cout << maxSum << endl;
    }
    return 0;
}
```

# 第四次作业

## 算法设计与分析 4.1 小明和果子

### ★题目描述

果园里有 $n$ 堆果子，每堆果子有 $a_i$ 个。小明要把所有果子合并成一堆。小明每次可以合并两堆果子，合并的代价是合并后果子的个数。问将所有果子合并成一堆的最小代价是多少。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 堆果子。

接下来一行有 $n$ 个正整数，用空格隔开，行末无空格，表示每堆果子的个数。

对于60%的数据，$1 \le n \le 10^3$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i \le 10^6$。

### ★输出格式
输出一个正整数，表示最小花费。

### ★样例输入
```wiki
3
1 2 4
```
### ★样例输出
```wiki
10
```
### 代码

> 堆：建堆(n)、删除(log n)、插入(log n)
>
> 遍历：n
>
> 时间复杂度：O(nlogn)

```c++
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

// 每次都选择最小的两个数，通过优先队列实现
int main() {
    int n;
    long long num;
    priority_queue<long long> q;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> num;
        q.push(-num);
    }
    long long sum = 0, a, b;
    while (q.size() > 1) {
        a = q.top();
        q.pop();
        b = q.top();
        q.pop();
        sum += a + b;
        q.push(a + b);
    }
    cout << -sum << endl;
    return 0;
}
```

## 算法设计与分析 4.2 小明卖股票

### ★ 题目描述

有 n 天，第 i 天小明会获得 $a_i$ 支股票。第 i 天每支股票可以卖 $b_i$ 元，小明最多能卖 $c_i$ 支股票。请问 n 天结束后小明最多能赚多少钱。

### ★ 输入格式

第一行为一个正整数 n ，表示有 n 天。

接下来 n 行，每行有三个正整数 $a_i\ b_i\ c_i$ ，用空格隔开，行末无空格。

对于 60% 的数据，1≤n≤$10^3$。

对于 100% 的数据，1≤n≤$10^6$，1≤$a_i,b_i,c_i$≤$10^6$。

### ★ 输出格式

输出一个正整数，表示最多能赚多少钱。

### ★ 样例输入

```wiki
3
1 2 3
4 5 6
7 8 9
```

### ★ 样例输出

```wiki
87
```
### 代码

```c++
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

struct Stock {
    int prize, cup;

    Stock(int _p = 0, int _c = 0) {
        prize = _p;
        cup = _c;
    }

    bool operator<(const Stock &t) const {
        return prize < t.prize;
    }
};

// 选择卖出价格尽可能高的时间点卖出去
int main() {
    int n;
    cin >> n;
    int a[n + 1], b[n + 1], c[n + 1];
    for (int i = 1; i <= n; i++) {
        cin >> a[i] >> b[i] >> c[i];
    }
    priority_queue<Stock> q;
    long long result = 0;
    for (int i = n; i > 0; i--) {
        q.push(Stock(b[i], c[i]));
        // 当队非空且有可卖股票的时候循环
        while (!q.empty() && a[i]) {
            // 选择优先队列中可卖价格最大的股票
            Stock temp = q.top();
            q.pop();
            // 可卖大于现有的
            if (temp.cup >= a[i]) {
                result += 1ll * a[i] * temp.prize;
                // 更新可卖数量
                temp.cup -= a[i];
                if (temp.cup) {
                    q.push(temp);
                }
                // 第i填无可卖股票
                a[i] = 0;
            } else {
                result += 1ll * temp.cup * temp.prize;
                a[i] -= temp.cup;
            }
        }
    }
    cout << result << endl;
    return 0;
}
```
## 算法设计与分析 4.3 小明借书

### ★题目描述

图书馆里有 $n$ 本书，借第 $i$ 本书要付 $a_i$ 元押金，$b_i$ 元租金，还书的时候会退押金，但是租金不会退。小明有 $s$ 元，他想看尽量多的书，请问他最多能看几本？

PS：小明不会借两本相同的书。

### ★输入格式

第一行为两个正整数 $n\ s$ ，用空格隔开，行末无空格，表示图书馆有 $n$ 本书，小明有 $s$ 元。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i$ ，用空格隔开，行末无空格。

对于30%的数据，$1 \le n \le 20$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i,b_i \le 10^6$，$1 \le s \le 2 \times 10^8$。

### ★输出格式
输出一个正整数，表示最多能看几本书。

### ★样例输入
```wiki
3 10
1 2
3 4
5 6
```
### ★样例输出
```wiki
2
```

### 代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <cmath>

using namespace std;

const int SIZE = 1e6 + 1;

struct Book {
    int a;
    int b;

    friend bool operator<(const Book &A, const Book &B) {
        return A.b < B.b;
    }
} book[SIZE];

bool cmp(Book b1, Book b2) {
    return b1.a == b2.a ? b1.b > b2.b : b1.a > b2.a;
}

/**
 * 1. 输入数据，将总金额小于等于初始资金的书本存入数组，之后按照押金递减排序；
 *    当押金相等时，按照租金递减排序
 * 2. 设置优先队列，按照租金递减排序，将目前借得到的书存入优先队列
 * 3. 从头开始借，每借一本书时，先加上最后借到的书的租金，再将剩余资金与借这
 *    本书所需的总金额进行比较，能借就存入队列，不能借的话就和队头书本的租金进
 *    行比较，如果目前这本书的租金比队头租金少，意味着我们可以花更少的钱，借到同等数量的书
 */
int main() {
    priority_queue<Book> q;
    int n, bor = 0;
    bool flag = false;
    long long s, left;
    cin >> n >> s;
    left = s;
    int x, y;
    x = y = 0;
    int j = 0;
    for (int i = 0; i < n; i++) {
        cin >> x >> y;
        if (x + y <= s) {
            book[j].a = x;
            book[j].b = y;
            j++;
        }
    }
    sort(book, book + j, cmp);
    for (int i = 0; i < j; i++) {
        if (flag) {
            left += bor;
        }
        if (left >= book[i].a + book[i].b) {
            left -= (book[i].a + book[i].b);
            bor = book[i].a;
            q.push(book[i]);
            flag = true;
        } else if (q.top().b > book[i].b && q.top().b + left >= book[i].a + book[i].b) {
            left += q.top().b;
            q.pop();
            left -= book[i].a + book[i].b;
            bor = book[i].a;
            q.push(book[i]);
            flag = true;
        } else {
            flag = false;
        }
    }
    cout << q.size() << endl;
    return 0;
}
```

## 算法设计与分析 4.4 小明上课

### ★题目描述

有 $n$ 节课，每节课开始和结束的时间分别是 $a_i\ b_i$ 。小明无法选两节有冲突的课。如果两节课的时间有交集（端点相交也算），这两节课就有冲突。请问小明最多能选几门课。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 节课。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i$ ，用空格隔开，行末无空格。

对于60%的数据，$1 \le n \le 10^3$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i \le b_i \le 10^6$。

### ★输出格式
输出一个正整数，表示最多能选几门课。

### ★样例输入
```wiki
3
1 2
3 4
5 6
```
### ★样例输出
```wiki
3
```
### 代码

> 时间复杂度：O(nlogn)

```c++
#include <iostream>
#include <algorithm>

using namespace std;

typedef struct {
    int begin;
    int end;
} lesson;

// 按照结束时间升序
bool cmp(lesson a, lesson b) {
    return a.end < b.end;
}

int main() {
    int n;
    cin >> n;
    lesson lessonList[n];
    for (int i = 0; i < n; i++) {
        cin >> lessonList[i].begin >> lessonList[i].end;
    }
    sort(lessonList, lessonList + n, cmp);
    // 第一个课程必选
    int result = 1;
    // lastIndex：已选课程中最迟结束的课程
    int lastIndex = 0;
    for (int i = 1; i < n; i++) {
        if (lessonList[i].begin > lessonList[lastIndex].end) {
            lastIndex = i;
            result++;
        }
    }
    cout << result << endl;
    return 0;
}
```

## 算法设计与分析 4.5 小明和 Johnson's rules

### ★题目描述

有 $n$ 个病人，两位医生（我们叫他们小易和小胜）。每个病人都要找两位医生看病，有的病人要先找小易看病，再找小胜看病；有的病人要先找小胜看病，再找小易看病。第 $i$ 个病人会找小易看 $a_i$ 分钟，找小胜看 $b_i$ 分钟。每个医生每次只能看一位病人，每位病人每次也只能找一个医生，两位医生同时上班，同时下班，并且他们会看完所有病人再下班。请问他们最早下班时间。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 个病人。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i\ c_i$ ，用空格隔开，行末无空格，表示第 $i$ 个病人会找小易看 $a_i$ 分钟，找小胜看 $b_i$ 分钟。

如果 $c_i = 0$，则病人会先找小易看病。

如果 $c_i = 1$，则病人会先找小胜看病。

对于60%的数据，$c_i = 0$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i, b_i \le 10^6$，$0 \le c_i \le 1$。

### ★输出格式
输出一个正整数，表示最早下班时间。

### ★样例输入
```wiki
3
1 2 0
3 4 0
5 6 1
```
### ★样例输出
```wiki
12
```

### 算法实现

用pair<int,int>表示病人，用两个deque<pair<int, int> >表示看诊病人队列。首先用两个pair数组初步记录病人情况，并进行johnson’s rules规则的排序。然后将数组结果存入deque。

当两个队列不为空时，有五种情况。

第一种指q1为空，q2不空时。总时间T加上q2队首病人的first，如果该病人的second不为0，表示该病人是“一手病人”，就将该病人的second值赋值给first,second为0，加入q1队列的队尾。

第二种指q2为空，q1不为空时。同上处理。

第三、四种指两队都不为空，队首病人的first不等时。判断first更小的病人是哪种病人，分类处理。而first更大的病人需将first减去更小的first重新插入到原队列队首。

第五种，如果first相等时，就用第一种情况的方法同时处理两个队首病人。

### 代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;
vector<pair<int, int> > v[2];
deque<pair<int, int> > pa[2];

int cmp(pair<int, int> a, pair<int, int> b) {
    int aa = a.first - a.second;
    int bb = b.first - b.second;
    if (aa <= 0 && bb <= 0) {
        return a.first < b.first;
    } else if (aa > 0 && bb > 0) {
        return a.second > b.second;
    } else {
        return aa < bb;
    }
}

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        if (c)swap(a, b);
        v[c].push_back(make_pair(a, b));
    }
    for (int i = 0; i < 2; i++) {
        sort(v[i].begin(), v[i].end(), cmp);
    }
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < v[i].size(); j++) {
            pa[i].push_back(v[i][j]);
        }
    }
    long long ans = 0;
    while (!pa[0].empty() || !pa[1].empty()) {
        if (pa[0].empty()) {
            pair<int, int> t = pa[1].front();
            pa[1].pop_front();
            ans += t.first;
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[0].push_back(t);
            }
        } else if (pa[1].empty()) {
            pair<int, int> t = pa[0].front();
            pa[0].pop_front();
            ans += t.first;
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[1].push_back(t);
            }
        } else if (pa[0].front().first < pa[1].front().first) {
            pair<int, int> t = pa[0].front();
            pa[0].pop_front();
            int cost = t.first;
            ans += cost;
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[1].push_back(t);
            }
            t = pa[1].front();
            pa[1].pop_front();
            t.first -= cost;
            pa[1].push_front(t);
        } else if (pa[0].front().first > pa[1].front().first) {
            pair<int, int> t = pa[1].front();
            pa[1].pop_front();
            int cost = t.first;
            ans += cost;
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[0].push_back(t);
            }
            t = pa[0].front();
            pa[0].pop_front();
            t.first -= cost;
            pa[0].push_front(t);
        } else {
            pair<int, int> t = pa[1].front();
            pa[1].pop_front();
            int cost = t.first;
            ans += cost;
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[0].push_back(t);
            }
            t = pa[0].front();
            pa[0].pop_front();
            t.first = 0;
            if (t.second) {
                swap(t.first, t.second);
                pa[1].push_back(t);
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```

# 第五次作业

## 算法设计与分析 5.1 小明爱数数

### ★题目描述

给定一个自然数 $N$ ，找出一个 $M$ ，使得 $M > 0$ 且 $M$ 是 $N$ 的倍数，并且 $M$ 的十进制表示只包含 $0$ 或  $1$ 。求符合条件的最小的 $M$ 。

### ★输入格式

第一行为一个正整数 $N$ 。

对于100%的数据，$1 \le N \le 10^6$。

### ★输出格式

输出符合条件的最小的 $M$ 。

### ★样例输入
```wiki
4
```
### ★样例输出
```wiki
100
```
### 代码

```c++
#include <iostream>
#include <queue>
#include "stdio.h"

using namespace std;
const int maxrem = 1e6;
bool repeat[maxrem];

struct Node {
    string number;
    int remainder;

    Node(string num, int rem) {
        number = num;
        remainder = rem;
    }
};

void MingAndCounting2(int n) {
    queue<Node> q;
    q.push(Node("1", 1 % n));
    while (!q.empty()) {
        Node cur = q.front();
        q.pop();
        int rem = cur.remainder;
        if (rem == 0) {
            cout << cur.number;
            break;
        } else if (!repeat[rem]) {
            repeat[rem] = 1;
            q.push(Node(cur.number + '0', cur.remainder * 10 % n));
            q.push(Node(cur.number + '1', (cur.remainder * 10 + 1) % n));
        }
    }
}

int main() {
    int n;
    scanf("%d", &n);
    MingAndCounting2(n);
    return 0;
}
```

## 算法设计与分析 5.2 小明和序列

### ★题目描述

给你一个长度为 $n$ 的数列，此数列共有 $2^n- 1$ 个非空子序列，把每个子序列的元素和列出来共有 $2^n- 1$ 个数字，求当中第 $k$ 小的数。 

### ★输入格式

第一行为一个正整数 $n$。

接下来一行，有 $n$ 个正整数 $a_i$ ，用空格隔开，行末无空格。

对于30%的数据，$1 \le n \le 20$。

对于100%的数据，$1 \le n \le 10^4$，$1 \le k \le min(10^6, 2 ^ n - 1)$，$1 \le a_i \le 10^8$。

### ★输出格式

输出一个数，表示第 $k$ 小的数。

### ★样例输入
```wiki
5 30
4 2 1 16 8
```
### ★样例输出
```wiki
30
```

### 思路

> We list the sums one by one by maintaining a priority queue of sums. We start with the empty set. Assume that we added the sum induced by $I\subset [n]$ (that is, $\sum_{i\in I} x_i$ into the output, let $j=1+\max I$. Now we can consider two possibilities by extending the current solution: the sum induced by $I\cup \{j\}$ or the sum induced by $I\cup \{k\}$ where k>j. We will add both possibilities to the queue so that one can inspect them later. We can avoid storing the sets, only the values are required.

### 代码

> 时间复杂度：O(k log k)

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

using namespace std;

struct Node {
    long value;
    int currentIndex;

    bool operator<(const Node &right) const {
        return value > right.value;
    }

    bool operator>(const Node &right) const {
        return value < right.value;
    }
};

int main() {
    int n, k;
    cin >> n >> k;
    int ai[n];
    for (int i = 0; i < n; i++) {
        cin >> ai[i];
    }
    sort(ai, ai + n);
    priority_queue<Node> q;
    Node node;
    node.value = ai[0];
    node.currentIndex = 0;
    q.push(node);
    int currentResultCount = 0;
    long result;
    while (!q.empty() && currentResultCount < k) {
        node = q.top();
        q.pop();
        currentResultCount++;
        result = node.value;
        if (node.currentIndex + 1 < n) {
            Node tempNode;
            tempNode.currentIndex = node.currentIndex + 1;
            tempNode.value = node.value + ai[node.currentIndex + 1];
            q.push(tempNode);
            tempNode.currentIndex = node.currentIndex + 1;
            tempNode.value = node.value - ai[node.currentIndex] + ai[node.currentIndex + 1];
            q.push(tempNode);
        }
    }
    cout << result << endl;
    return 0;
}
```
## 算法设计与分析 5.3 小明坐地铁

### ★ 题目描述

一维数轴上有 n 个地铁站，假设第 i 个地铁站的坐标在 $x_i$ 。小明从第 $a_0$ 站出发，坐到第 $a_1$ 站，再从第 $a_1$ 站出发，坐到第 $a_2$ 站，...，再从第 $a_{m-1}$ 站出发，坐到第 $a_m$ 站。现在你不知道 $a_i$ 具体的值，只知道 $d_i=|x_{a_{i-1}}-x_{a_i}|$。请问 n 最小可能是多少？

### ★ 输入格式

第一行为一个正整数 m。

接下来一行，有 m 个正整数 $d_i$，用空格隔开，行末无空格。

对于 30% 的数据，1≤m≤20。

对于 100% 的数据，1≤m≤25，$1\leq d_i\leq 10^5$。

### ★ 输出格式

输出一个数，表示 n 最小可能是多少。

### ★ 样例输入

```wiki
2
3 3
```

### ★ 样例输出

```wiki
2
```

### 思路

- 用数轴上的坐标来表示站点，假设初始位置坐标为0
- 对于每次移动，距离为$d_i$，假设当前位置的坐标为$x_i$，可以在当前位置向左或者向右移动$d_i$，移动后的坐标为$x_i$−$d_i$,或者$x_i$+$d_i$
- 每次到达一个坐标（即站点）时，判断该坐标是否被访问过：
  - 如果没有访问过，站点数量+1，继续移动
  - 如果访问过，站点数量不变，继续移动
- 由于每次移动的方向不同，会有不同的移动方案，用二叉树来构造移动方案的解树
- 通过深度遍历所有情况，取站点数最小的移动方式

优化：

- 在初始站点，由于之前没有经历过其他站点，选择向左走或者向右走的路径中各站点的绝对值关于初始位置是对称的，所以可以进行剪枝，默认剪掉根节点的左子树
- 根据题目，小明一共走了 m 段路程，所以最多会有m+1个站点，初始化站点数 res=m+1
- 在深度优先搜索过程中，如果经过的站点数>res，则停止对该节点分支的搜索，进行回溯

### 代码

> 时间复杂度：o($2^{m-1}$-1)

```c++
#include <iostream>

using namespace std;

// d数组用来存储25段距离
int d[26];
// 用来记录某一站点的坐标是否被访问过
bool position[5000001];
int m, res;

// i表示走到第几个站点，temp为下一个站点处的坐标，step表示某一次遍历时已经经过的站点
void dfs(int i, int temp, int step) {
    if (step > res) {
        return;
    }
    // 表示已经走完最后一层
    if (i == m + 1) {
        if (!position[temp]) {
            step++;
        }
        if (res > step) {
            res = step;
        }
        return;
    }
    if (!position[temp]) {
        // 如果某个坐标位置没有被访问过
        step++;
        position[temp] = true;
        dfs(i + 1, temp - d[i + 1], step);
        dfs(i + 1, temp + d[i + 1], step);
        position[temp] = false;
    } else {
        // 如果某个坐标位置被访问过
        dfs(i + 1, temp - d[i + 1], step);
        dfs(i + 1, temp + d[i + 1], step);
    }
}

int main() {
    cin >> m;
    // 可能的最大结果为m+1
    res = m + 1;
    for (int i = 1; i <= m; i++) {
        cin >> d[i];
    }
    if (m == 1) {
        cout << 2 << endl;
        return 0;
    }
    // j用来记录站点坐标
    int j = 0 + 2500000;
    // 初始化起始点的坐标访问位为true
    position[j] = true;
    dfs(1, j + d[1], 1);
    cout << res << endl;
    return 0;
}
```

## 算法设计与分析 5.4 小明和最小点覆盖

### 题目描述

### ★题目描述

给定 $n$ 个点 $m$ 条边的无向图，无重边无自环，至少要选几个点才能使得所有边所连接的两个点至少都有一个点被选到？若所需点数超过 $\lceil log_2n \rceil$ ，则输出 $-1$。

### ★输入格式

第一行为两个正整数 $n$，$m$ ，用空格隔开，行末无空格。

接下来 $m$ 行，每行有两个正整数 $u$，$v$ ，用空格隔开，行末无空格，表示边 $(u, v)$ 。

对于30%的数据，$1 \le n \le 16$。

对于100%的数据，$1 \le n \le 512$，$1 \le m \le \frac{n(n - 1)}{2}$。

### ★输出格式

输出一个数，表示所需的最少点数。若所需点数超过 $\lceil log_2n \rceil$ ，则输出 $-1$。

### ★样例输入
```wiki
3 2
1 2
1 3
```
### ★样例输出
```wiki
1
```

### 思路

使用两个数组x和y来代表每条边对应的两个端点，使用visited数组来代表某个端点是否被访问过，即是否加入了结果集。遍历无向图中的所有边：

1. 如果某条边的两个端点之一被访问过了，则表明该边所连接的两个点已经有一个点被选到了，则该边连接的另一个点就不需要加入结果集，这时候就遍历下一条边，但结果集中的点数不增加

2. 如果某条边的两个端点都没被访问过，则需要选取该边中的任意一个端点加入结果集，结果集中的点数加一，遍历下一条边

重复(1)，(2)步骤，直至遍历完所有边或者当前结果集超过了最小结果集

### 代码

> 算法复杂度：
>
> 广度：$2^{\log_2N}+1$
>
> 深度：$(N-1)\lceil\log_2N\rceil$
>
> $O(N^2\log_2N)$

```c++
#include <iostream>
#include <cmath>

using namespace std;

int n, m, res;
int x[1000000], y[1000000];
int visited[1000] = {0};

void dfs(int id, int point) {
    if (point >= res) {
        return;
    }
    if (id == m) {
        if (res > point) {
            res = point;
        }
        return;
    }
    if (visited[x[id]] || visited[y[id]]) {
        dfs(id + 1, point);
    } else {
        visited[x[id]] = 1;
        dfs(id + 1, point + 1);
        visited[x[id]] = 0;
        visited[y[id]] = 1;
        dfs(id + 1, point + 1);
        visited[y[id]] = 0;
    }
}

int main() {
    cin >> n >> m;
    int minPoint = log(n - 1) / log(2) + 1;
    res = minPoint + 1;
    for (int i = 0; i < m; i++) {
        cin >> x[i] >> y[i];
    }
    dfs(0, 0);
    cout << (res > minPoint ? -1 : res) << endl;
    return 0;
}
```

## 算法设计与分析 5.5 小明和第K小带权匹配

### ★题目描述

给你 $2n$ 个点，$m$ 条边的二分图，二分图的两边各有 $n$ 个点，每条边只会从左边的点连向右边的点，没有重边，求权重和第 $k$ 小的匹配的值。

注：一个匹配是指给定图的一个边的子集（空集是任意集合的子集），其中任意两条边都没有公共的顶点。

### ★输入格式

第一行为两个正整数 $n$，$m$，$k$ ，用空格隔开，行末无空格。

接下来 $m$ 行，每行有三个正整数 $u$，$v$，$w$ ，用空格隔开，行末无空格，表示边 $(u, v)$，权值为$w$ 。

对于30%的数据，$1 \le m \le 20$。

对于100%的数据，$1 \le n \le 20$，$1 \le m \le 100$，$1 \le k \le 10^5$，$1 \le u, v \le n$，$1 \le w \le 1000$。

### ★输出格式

输出一个数，表示权重和第 $k$ 小的匹配的值。无解输出 $-1$ 。

### ★样例输入
```wiki
2 4 7
1 1 1
1 2 2
2 1 3
2 2 4
```
### ★样例输出
```wiki
5
```

### 代码

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

const int maxn = 25;
priority_queue<long long, vector<long long>, less<long long> > pq;

int n, m, k;
// 二维数组e存放二分图
int e[maxn][maxn];
// vis存放已访问过的右边的节点
bool vis[maxn];

void dfs(int idx, long long sum) {
    // 优先队列已满，中间值大于优先队列的最大值
    if (pq.size() == k && sum >= pq.top()) {
        return;
    }
    if (idx == n + 1) {
        pq.push(sum);
        if (pq.size() > k) {
            pq.pop();
        }
        return;
    }
    dfs(idx + 1, sum);
    for (int i = 1; i <= n; i++) {
        if (e[idx][i] != 0 && !vis[i]) {
            vis[i] = true;
            dfs(idx + 1, sum + e[idx][i]);
            vis[i] = false;
        }
    }
}

int main() {
    cin >> n >> m >> k;
    int u, v, w;
    for (int i = 1; i <= m; i++) {
        cin >> u >> v >> w;
        e[u][v] = w;
    }
    dfs(1, 0);
    if (pq.size() != k) {
        cout << -1 << endl;
    } else {
        cout << pq.top() << endl;
    }
    return 0;
}
```

