## 从树到图：如何让计算机学会看地图？

### 基于广度优先或深度优先搜索的方法
假设你关心的是路上所花费的时间，那么权重就是从一点到另一点所花费的时间；如果你关心的是距离，那么权重就是两点之间的物理距离。

从出发点开始，广度优先遍历每个点，当遍历到某个点的时候，如果该点还没有耗时的记录，记下当前这条通路的耗时。如果该点之前已经有耗时记录了，那就比较当前这条通路的耗时是不是比之前少。如果是，那就用当前的替换掉之前的记录。

在遍历所有可能的路线时，有几个问题需要注意
- 由于要遍历所有可能的通路，因此一个点可能会被访问多次。且要避免回路
- 如果某个结点 x 和起始点 s 之间存在多个通路，每当 x 到 s 之间的最优路线被更新之后，我们还需要更新所有和 x 相邻的结点之最优路线，计算复杂度会很高

### Dijkstra 算法
无论是广度优先还是深度优先的实现，算法对每个结点的访问都可能多于一次。
而访问多次，就意味着要消耗更多的计算机资源。那么，有没有可能在保证最终结果是正确的情况下，尽可能地减少访问结点的次数来提升算法的效率
Dijkstra 算法的核心思想是，对于某个结点，如果我们已经发现了最优的通路，那么就无需在将来的步骤中，再次考虑这个结点。Dijkstra 算法很巧妙地找到这种点，而且能确保已经为它找到了最优路径。

- 第一个是 source，我们用它表示图中的起始点，缩写是 s
- 然后是 weight，表示二维数组，保存了任意边的权重，缩写为 w。w[m, n] 表示从结点 m 到结点 n 的有向边之权重，大于等于 0。如果 m 到 n 有多条边，而且权重各自不同，那么取权重最小的那条边。
- 接下来是 min_weight，表示一维数组，保存了从 s 到任意结点的最小权重，缩写为 mw。假设从 s 到某个结点 m 有多条通路，而每条通路的权重是这条通路上所有边的权重之和，那么 mw[m] 就表示这些通路权重中的最小值。mw[s]=0，表示起始点到自己的最小权重为 0。
- 最后是 Finish，表示已经找到最小权重的结点之集合，缩写为 F。一旦结点被放入集合 F，这个结点就不再参与将来的计算。
- 初始的时候，Dijkstra 算法会做三件事情。第一，把起始点 s 的最小权重赋为 0，也就是 mw[s] = 0。第二，往集合 F 里添加结点 s，F 包含且仅包含 s。第三，假设结点 s 能直接到达的边集合为 M，对于其中的每一条边 m，则把 mw[m] 设为 w[s, m]，同时对于所有其他 s 不能直接到达的结点，将通路的权重设为无穷大。
- Dijkstra 算法会重复 查找最小 mw ， 更新权重
  - 查找最小 mw。从 mw 数组选择最小值，则这个值就是起始点 s 到所对应的结点的最小权重，并且把这个点加入到 F 中，针对这个点的计算就算完成了。比如，当前 mw 中最小的值是 mw[x]=10，那么结点 s 到结点 x 的最小权重就是 10，并且把结点 x 放入集合 F，将来没有必要再考虑点 x，mw[x] 可能的最小值也就确定为 10 了。
  - 更新权重。然后，我们看看，新加入 F 的结点 x，是不是可以直接到达其他结点。如果是，看看通过 x 到达其他点的通路权重，是否比这些点当前的 mw 更小，如果是，那么就替换这些点在 mw 中的值。例如，x 可以直接到达 y，那么把 (mw[x] + w[x, y]) 和 mw[y] 比较，如果 (mw[x] + w[x, y]) 的值更小，那么把 mw[y] 更新为这个更小的值，而我们把 x 称为 y 的前驱结点。

- 再次从 mw 中找出最小值，此时要求 mw 对应的结点不属于 F，重复上述动作，直到集合 F 包含了图的所有结点，也就是说，没有结点需要处理了

最小的、非无穷大的 mw 值，对应的结点是还没有加入 F 集合的、且和 s 有通路的那些结点。假设当前 mw 数组中最小的值是 mw[x]，对应的结点是 x。如果边的权重都是正值，那么通路上的权重之和是单调递增的，所以其他通路的权重之和一定大于当前的 mw[x]，因此即使存在其他的通路，其权重也会比 mw[x] 大。

这就是为什么每次都要选择最小的 mw 值，并认为对应的结点已经完成了计算。和广度优先或者深度优先的搜索相比，Dijkstra 算法可以避免对某些结点，重复而且无效的访问。因此，每次选择最小的 mw，就可以提升了搜索的效率

### 小结

除了时间之外，你也可以对图的边设置其他类型的权重，比如距离、价格，这样 Dijkstra 算法可以让用户找到地图任意两点之间的最短路线，或者出行的最低价格等等。有的时候，边的权重越大越好，比如观光车开过某条路线的车票收入。对于这种情况，Dijkstra 算法就需要调整一下，每次找到最大的 mw，更新邻近结点时也要找更大的值。