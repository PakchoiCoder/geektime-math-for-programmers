# 08 | 组合：如何让计算机安排世界杯的赛程？

组合：跟排列类似，但是不考虑每个元素出现的顺序。

对于 N 个元素中取出 M 个值的组合的全集合，就叫做全组合。比如对于集合{1, 2, 3}而言，全组合就是{空集, {1}, {2}, {3}, {1, 2}, {1,3} {2, 3}, {1, 2, 3}}。

## 组合有关运算

- n 个元素里取出 m 个的组合，可能性数量就是 n 个里取 m 个的排列数量，除以 m 个全排列的数量，也就是 n! / m!(n-m)!
- 对于全组合而言，可能性为 2^n 种。例如，当 n=3 的时候，全组合包括了 8 种情况

首先按照排列的方式获取所有的数量，因为组合是没有顺序的，所以需要去掉相同内容不同顺序的内容，因此除以 m!。

## 程序实现

相比之前的排列，只需要在生成组合的时候，用去掉当前值剩余的集合去计算。

## 应用

处理数组的多元文法。一篇文章中，拆解词语的时候，可能会把单词进行合并形成新的词组，但是当用户检索的时候，可能不按照词组顺序进行搜索，此时可以将之前的词组进行标准化处理，形成固定的序号，将用户的输入也进行标准化处理。相当于用组合的方式去匹配。

## 思考题

可以先从 100 个人里面取出 14 个人的组合，再对所有的组合再进行运算，从 14 个人中取出 10 个，然后再把计算之后的组合再进行运算，从 4 个取 3 个。

也可以先从 100 个人里面取出 1 个，然后继续基于已有的 99 个数据分别取 3 个，在基于已有的数据再取 10 个。
