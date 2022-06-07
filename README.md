# AffinityPropagation

This clustering method is like ``hierarchical clustering`` in the sense that you don't have to know the number of clusters beforehand. The main difference is that ``AP Clustering`` tries to find the clusters centers without having to use a dendrogram.

This method uses 4 concepts to find the centers (it works pretty well for small datasets since all the matrices are $(n,n)$):

* similarity (s): Represents the negative euclidean distance between all the data points (distance matrix).

  * For all $i = j$, a value is set manually for all $i,j$. This is called "the preference". The higher it is, the more clusters we will end up having, and the smaller it is, the fewer clusters we will end up having.

* responsibility (r): Represents how much influence a cluster k has on a data point i. It's represented as $r(i,k)=s(i,k) - max_{k' \ne k}(a(i,k')-s(i,k'))$

* availiability (a): Represents how likely the data point i is to choose k as its cluster (representative). It's represented as $a(i,k) = min\{0,r(k,k)+\sum_{i'\ne {i,k}}max(0,r(i',k))), \ i \ne k$
$a(k,k)=\sum_{i'\ne {k}}max(0,r(i',k)), i = k$

  * When updating $a$ and $r$, a "dumping value" $\lambda$ is chosen manually (0.5 is a common value). This prevents oscillations for each iteration $t$:

  <center>$r(i,k)_{t+1}=(1-\lambda)\cdot r(i,k)_{t+1} + \lambda \cdot r(i,k)_{t}$ </center>
 <br>
  <center>$a(i,k)_{t+1}=(1-\lambda)\cdot a(i,k)_{t+1} + \lambda \cdot a(i,k)_{t}$ </center>
* criterion: it represents the sum of $r(i,k)$ and $a(i,k)$ for all $i,k$. The highest value for each row represents the cluster of that row.

The algorithm starts by calculating the ``distance_matrix``, starting the matrix for $a$ and $r$ on zero, and choosing a $\lambda$. Then the responsibility is calculated for all $i,j$, then the ``update formula`` is used. Afterwards, availibility is calculated and updated. Finally, the ``criterion matrix`` is found.

These steps are performed as many times as necessary.
