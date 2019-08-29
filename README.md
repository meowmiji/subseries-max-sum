# 连续子序列最大和

给出一个序列，找出其中的一段连续子序列，使得该子序列拥有所有连续子序列中最大和。
显然该问题在序列元素有正有负时才有意义，最直观的对应场景是已知股票的历史涨跌，找出获得最大收益的买入卖出时间。

这里以下图中的一个序列：nums = [5, -3, 6, 1, -8, 2, -5, -2, 3, 1, -1, 8, -4] 为例，给出两种复杂度最低的算法：

![alt text](https://github.com/meowmiji/subseries-max-sum/blob/master/images/series.png)

## 算法一

'''bash
def maxSumAndSub1(nums):
	'''
	步骤1：
	令dp[i]表示以nums[i]作为结尾的连续子序列的最大和
	步骤2：
	因为dp[i]是要求必须以nums[i]结尾的连续序列，那么只有两种情况：
		1.这个最大连续序列只有一个元素，即以nums[i]开始，以nums[i]结尾
		2.这个最大和的连续序列有多个元素，即以nums[p]开始（p<i），以nums[i]结尾
	对于情况1，最大和就是nums[i]本身
	对于情况2，最大和是dp[i-1]+nums[i]
	于是得到状态转移方程：
	dp[i]=max{nums[i],dp[i-1]+nums[i]} 
	步骤3：
	最大连续子序列的和为max{dp[i]} (1<=i<=n)
	'''
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
'''

![alt text](https://github.com/meowmiji/subseries-max-sum/blob/master/images/method_1_illustration.png)
![alt text](https://github.com/meowmiji/subseries-max-sum/blob/master/images/method_2_illustration.png)
