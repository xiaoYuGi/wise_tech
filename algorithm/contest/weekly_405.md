T2:

3211\. 生成不含相邻零的二进制字符串
---------------------

给你一个正整数 `n`。

如果一个二进制字符串 `x` 的所有长度为 2 的

子字符串

中包含 **至少** 一个 `"1"`，则称 `x` 是一个 **有效** 字符串。

返回所有长度为 `n` 的 **有效** 字符串，可以以任意顺序排列。

**示例 1：**

**输入：** n = 3

**输出：** \["010","011","101","110","111"\]

**解释：**

长度为 3 的有效字符串有：`"010"`、`"011"`、`"101"`、`"110"` 和 `"111"`。

**示例 2：**

**输入：** n = 1

**输出：** \["0","1"\]

**解释：**

长度为 1 的有效字符串有：`"0"` 和 `"1"`。

**提示：**

*   `1 <= n <= 18`

[https://leetcode.cn/problems/generate-binary-strings-without-adjacent-zeros/description/](https://leetcode.cn/problems/generate-binary-strings-without-adjacent-zeros/description/)

```java
public List<String> validStrings(int n) {
    List<String> ans = new ArrayList<>();

    // 长度为n的二进制字符串，对应的int树最大不能超过1 << n, 直接左移n位，再大长度就不符合了
    int mask = (1 << n) - 1;
    for (int i = 0; i < (1 << n); i++) {
        int x = mask ^ i;
        if (((x >> 1) & x) == 0) {
            // toBinaryString方法不会保留数字的高位0，所以可以使用(1 << n) | i).substring(1) 来保留高位前导0
            ans.add(Integer.toBinaryString((1 << n) | i).substring(1));
        }
    }
    return ans;
}
```