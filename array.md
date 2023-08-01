# 数组
## 双指针
若数组是有序的，可以尝试双指针解题
## 差分数组
![](./images/1690602822636_image.png)
假如给你输入一个数组 nums，然后又要求给区间 nums[2..6] 全部加 1，再给 nums[3..9] 全部减 3，再给 nums[0..4] 全部加 2，再给...
结果是什么？如果直接使用for循环暴力，会对nums频繁修改。所以可以构建一个差分数组

diff[i] = nums[i] - nums[i-1];
diff[2] = nums[2] - nums[1] = 6 - 2 = 4
而且通过diff差分数组也可以反推出原数组：
nums[i] = nums[i-1] + diff[i];
因为第一位是一样的，所以从第二位开始
nums[1] = nums[0] + diff[1] = 8 + (-6) = 2
转换为diff数组后，如果我们需要在nums[i...j]区间内全部+3操作，我们只需要在diff[i] += 3
然后再让 diff[j+1] -= 3 即可

因为在diff[i]处+3，那么后面所有的数都会+3，公式里nums[i]是依赖于diff[i]的。但是我们是一个区间，所以要在diff[j]处，后面的元素都不加3了，因此还得在diff[j+1]处减去3，那么后面的元素也都-3了。
伪代码code：

```
// 初始化diff
int[] diff = new int[nums.length];
diff[0] = nums[0];
for (int i = 1; i < nums.lenth; i++) {
    diff[i] = nums[i] - nums[i-1];
}

// 做加减操作
public void increment(int i, int j, int val) {
    diff[i] += val;
    if (j + 1 < diff.length) {
        diff[j+1] -= val;
    }
}

// 然后再恢复数组
public int[] result() {
    int[] res = new int[diff.length];
    res[0] = diff[0];
    for (int i = 1; i < diff.length; i++) {
        res[i] = diff[i] + res[i-1];
    }
    return res;
}

```
## 数组滑动窗口
维护一个窗口，时间复杂度O（N）
```
    public void slidingWindow(String originStr, String matchStr) {
        // 窗口
        HashMap<Character, Integer> window = new HashMap<>();
        // 需要匹配的部分
        HashMap<Character, Integer> need = new HashMap<>();
        for (char c : matchStr.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }
        int left = 0;
        int right = 0;
        while (right < originStr.length()) {
            // 窗口右移
            char c = originStr.charAt(right);
            right++;
            window.put(c, window.getOrDefault(c, 0) + 1);
            // 窗口数据更新...
            ...
            // 什么条件需要缩小窗口？
            while (left < right && window need shrink) {
                // 要移出去的字符
                char d = originStr.charAt(left);
                left++;
                window.put(d, window.get(d) - 1);
                // 窗口数据更新...
                ...
            }
        }
    }
```
