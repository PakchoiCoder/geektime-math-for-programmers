# 41 | 线性回归（下）：如何使用最小二乘法进行效果验证？

## 基于最小二乘法的求解

...

## 总结

从广义上来说，最小二乘法不仅可以用于线性回归，还可以用于非线性的回归。其主要思想还是要确保误差ε最小，但是由于现在的函数是非线性的，所以不能使用求多元方程求解的办法来得到参数估计值，而需要采用迭代的优化算法来求解，比如梯度下降法、随机梯度下降法和牛顿法。
