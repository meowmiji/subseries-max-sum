# 连续子序列最大和

给出一个序列，找出其中的一段连续子序列，使得该子序列拥有所有连续子序列中最大和。\
显然该问题在序列元素有正有负时才有意义，最直观的对应场景是已知股票的历史涨跌，找出获得最大收益的买入卖出时间。

这里以下图中的一个序列：nums = [5, -3, 6, 1, -8, 2, -5, -2, 3, 1, -1, 8, -4] 为例，给出两种复杂度最低的算法：

![](https://github.com/meowmiji/subseries-max-sum/blob/master/images/series.png)

## 算法一

- 步骤1：\
令dp[i]表示以nums[i]作为结尾的连续子序列的最大和
- 步骤2：\
因为dp[i]是要求必须以nums[i]结尾的连续序列，那么只有两种情况：\
&nbsp;&nbsp;&nbsp;&nbsp;(1). 这个最大连续序列只有一个元素，即以nums[i]开始，以nums[i]结尾\
&nbsp;&nbsp;&nbsp;&nbsp;(2). 这个最大和的连续序列有多个元素，即以nums[p]开始（p<i），以nums[i]结束\
对于情况(1)，最大和就是nums[i]本身\
对于情况(2)，最大和是dp[i-1]+nums[i]\
于是得到状态转移方程：\
dp[i]=max{nums[i],dp[i-1]+nums[i]}
- 步骤3：\
最大连续子序列的和为max{dp[i]} (1<=i<=n)

```bash
def maxSumAndSub1(nums):
    dp = [nums[0]]
    start = [0]  # 这里的开始和结束位置指的是以其值做序列切片，即nums[start:end]
    for i in range(1, len(nums)):
        # 最大序列只有nums[i]一个元素的情况，此时的最大序列开始位置为i
        if nums[i] >= dp[-1] + nums[i]:
            dp.append(nums[i])
            start.append(i)
        # 最大序列为[以nums[i-1]为结尾的最大序列, nums[i]]的情况，此时最大序列开始位置与上一最大序列开始位置保持不变
        else:
            dp.append(dp[-1] + nums[i])
            start.append(start[-1])

    print('nums:', nums)
    print('dp:', dp)
    print('start:', start)
    # 上为辅助计算数值，下为最大和和对应的子序列
    print('max sum:', max(dp))
    max_index = dp.index(max(dp))
    print('max sub:', nums[start[max_index]:max_index + 1])  # 结束位置永远是i+1，由dp的定义决定
```

![](https://github.com/meowmiji/subseries-max-sum/blob/master/images/method_1_illustration.png)   

## 算法二

- 步骤1：\
以左端第一个元素作为局部最低点，局部最大累加集合LM为[]
- 步骤2：\
累加和s=0，局部最大累加lm=0
- 步骤3：\
向右累加一个元素，s+=nums[i]\
若s为正且s>lm，lm=s
- 步骤4：\
重复步骤3，直到s非正，来到了新的局部低点，LM.append(lm)
- 步骤5：\
重复步骤2-4，直到来到序列最右端，最大连续子序列的和为max(LM)

```bash
def maxSumAndSub2(nums):
    s = [0]  # 从上一低点到下一低点前的当前位置的子序列的和，一旦非正，以该位置为新的低点，同时s归零
    lm = [0]  # 从上一低点到下一低点前的当前位置过程中达到过的最大增幅（局部子序列和），local maximum
    start = [0]  # 记录（两个连续低点间）最大和局部子序列的开始位置
    end = [0]  # 记录最大和局部子序列的结束位置
    for i in range(len(nums)):
        new_s = s[-1] + nums[i]  # 从上一低点到当前位置的累加s
        # 一旦非正，该位置作为新的最低点，累加清零，最大增幅也清零，重置局部最大序列开始结束位置
        if new_s <= 0:
            new_s = 0
            new_lm = 0
            new_start = i + 1
            new_end = i + 1
        else:  # 在下一个新的低点前
            new_start = start[-1]  # 这一段的局部最大和子序列的开始始终是这个低点
            if new_s > lm[-1]:  # s为局部最大时，更新局部最大和结束位置
                new_lm = new_s
                new_end = i + 1
            else:  # 否则维持lm和结束位置
                new_lm = lm[-1]
                new_end = end[-1]
        s.append(new_s)
        lm.append(new_lm)
        start.append(new_start)
        end.append(new_end)

    print('nums:', nums)
    print('s:', s)
    print('lm:', lm)
    print('start:', start)
    print('end:', end)
    # 上为辅助计算数值，下为最大和和对应的子序列
    print('max sum:', max(lm))
    max_index = lm.index(max(lm))
    print('max sub:', nums[start[max_index]:end[max_index]])
```

![](https://github.com/meowmiji/subseries-max-sum/blob/master/images/method_2_illustration.png)
