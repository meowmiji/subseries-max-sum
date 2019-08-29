# 连续子序列最大和

给出一个序列，找出其中的一段连续子序列，使得该子序列拥有所有连续子序列中最大和。
显然该问题在序列元素有正有负时才有意义，最直观的对应场景是已知股票的历史涨跌，找出获得最大收益的买入卖出时间。

这里以下图中的一个序列：nums = [5, -3, 6, 1, -8, 2, -5, -2, 3, 1, -1, 8, -4]为例，给出两种复杂度最低的算法：

![alt text](https://github.com/meowmiji/subseries-max-sum/blob/master/images/series.png)

具体算法思路见main.py中的注释。下面两图是对应算法中间计算过程的演示。

![alt text](htps://github.com/meowmiji/subseries-max-sum/blob/master/images/method_1_illustration.png)
![alt text](htps://github.com/meowmiji/subseries-max-sum/blob/master/images/method_2_illustration.png)
