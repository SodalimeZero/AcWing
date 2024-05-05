## 最长上升子序列

给定一个长度为 $N$ 的数列，求数值严格单调递增的子序列的长度最长是多少。

## 输入格式

第一行包含整数 $N$

第二行包含 $N$ 个整数，表示完整序列。

## 输出格式
输出一个整数，表示最大长度。

## 数据范围

$1\le N\le 100000$

$−10^9≤数列中的数≤10^9$

## 输入样例

```
7
3 1 2 1 8 5 6
```

## 输出样例：

```
4
```

## 解题思路

此时如果沿用上一题的做法，时间会超时，因此需要

以给到的 ```3 1 2 1 8 5 6``` 为例

可以得到， $f[0]=1, f[1]=1, f[2]=2, f[3]=2, f[4]=2, f[5]=3, f[6]=4$

可以看到 $f[0]=f[1]=1$ ，对于子序列 $3\ 1\ 8$ 来说，3 1和3 8都是单调递增的，可以观察到有1的存在，3就没有必要出现了，因为1比3更小，所以更有机会接触到更大的数来形成更长的子序列。

因此我们可以用数组 $f[i]$ 来存储当最大子序列长度为 $i$ 时，这些子序列里最后一项的最小值。

然后对于数组中的每一项，用**二分查找**去找到小于这一项的那些值里的最大者，再+1即可。

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

const int N = 100010;
int w[N], f[N];

int main()
{
    int n; 
    cin >> n;
    for (int i = 0; i < n; i++) {
        scanf("%d", &w[i]);
    }
    
    f[0] = -2e9;
    int max_len = 0;
    for (int i = 0; i < n; i++) {
        int left = 0, right = max_len;
        while (left < right) {
            int mid = left + right + 1 >> 1;
            if (w[i] > f[mid]) {
             // 分界点在右侧
             left = mid;
            } else {
                right = mid - 1;
            }
        }
        max_len = max(max_len, right + 1);
        f[right + 1] = w[i];
    }
    cout << max_len << endl;
    return 0;
}
```
