# DS

###Sort


|            | 平均复杂度       | 最优复杂度   | 最坏复杂度        | 辅助空间    | 稳定性   |
| -------------- | ---------------- | ------------ | ----------------- | ----------- | -------- |
| Bubble Sort    | $O(n^2)$         | $O(n)$       | $O(n^2)$          | $O(1)$      | Stable   |
| Selection Sort | $O(n^2)$         | $O(n^2)$     | $O(n^2)$          | $O(1)$      | Unstable |
| Insertion Sort | $O(n^2)$         | $O(n)$       | $O(n^2)$          | $O(1)$      | Stable   |
| Shell Sort     | $O(n^{7\over6})$ | $O(n)$       | $O(n^{4\over 3})$ | $O(1)$      | Unstable |
| Heap Sort      | $O(n\log n)$     | $O(n\log n)$ | $O(n\log n)$      | $O(1)$      | Unstable |
| Merge Sort     | $O(n\log n)$     | $O(n\log n)$ | $O(n\log n)$      | $O(n)$      | Stable   |
| Quick Sort     | $O(n\log n)$     | $O(n\log n)$ | $O(n^2)$          | $O(\log n)$ | Unstable |
| Bucket Sort    | $O(n+{n^2\over k}+k)$         | $O(n)$ | $O(n^2)$ | $O(k+n)$ | Stable   |

通过交换相邻元素的排序算法需要$\Omega(n^2)$

Quick Sort 的辅助空间是递归额外使用的栈空间。

对于归并排序的总体复杂度有：
$$
T(N)=2T(N/2)+N
$$
求解后:
$$
T(N) = N+N\log N   =O(N\log N)
$$
我们总共需要的Merge Times 是$O(\log N)$, 每次Merge的时间开销大概是$O(\log N)$



### 杂项

斐波那契数列：递归：$O(2^n)$   迭代：$O(n)$, 算法：二叉树叶子数就是运行时间，等价于求解$T(n)=T(n-1)+T(n-2)$

### Tree

Nodes  = Degree + 1

### Graph

####拓扑排序

https://blog.csdn.net/qq_41713256/article/details/80805338

邻接矩阵适合稠密图，邻接表适合稀疏图
$$
邻接矩阵复杂度：O(V^2)\\
邻接表的复杂度：O(|V|+|E|)
$$
1. 先统计所有节点的入度，对于入度为0的节点就可以分离出来，然后把这个节点指向的节点的入度减一。
2. 一直做这个操作，直到所有的节点都被分离出来。
3. 如果最后不存在入度为0的节点，那就说明有环，不存在拓扑排序，也就是很多题目的无解的情况。

有些拓扑排序要求字典序最小什么的，那就把队列换成优先队列就好了。

#### 最短路径算法

**Dijkstra**

https://www.cnblogs.com/Glacier-elk/p/9438077.html

1. 遍历没有在最短路中的点，选出一个距离 **已经在最短路集合中的点** 距离最近的点。
2. 把它加入到最短路中。
3. 更新所有点的最短路。
4. 重复，直到所有的点都加入到最短路中。

~~~c
void Dijkstra(Table T)
{
	Vertex V,W;
    while(1)
    {
		V = Smallest unknown distance vertex;
        if(V == NULL)
            break;
		T[V].Known = True;
        for each W adjacent to V
            if(!T[W].Known)
                if(T[V].Dist + Cvw < T[W].Dist)
                {
                    T[W].Dist = T[V].Dist + Cvw;
                    T[W].Path = V;
                }
    }
}
~~~

$$
复杂度:O(V^2)
$$

#### 网络流问题

​	

####最小生成树

https://www.cnblogs.com/JoshuaMK/p/prim_kruskal.html

Prim算法适用于稠密图,Kruskal适用于稀疏图

**Kruskal**

Kruskal算法的步骤包括：

1. 对所有权值进行从小到大排序

2. 然后每次选取最小的权值，如果和已有点集构成环则跳过，否则加到该点集中。最终有所有的点集构成的树就是最佳的。

使用邻接表存储，并查集操作。
$$
复杂度:O(E\log E)
$$



**Prim**

Prim算法求最小生成树的时候和边数无关，和顶点树有关，所以适合求解稠密网的最小生成树。

Prim算法的步骤包括：

1. 将一个图分为两部分，一部分归为点集U，一部分归为点集V，U的初始集合为{V1}，V的初始集合为{ALL-V1}。

2. 针对U开始找U中各节点的所有关联的边的权值最小的那个，然后将关联的节点Vi加入到U中，并且从V中删除（注意不能形成环）。

3. 递归执行步骤2，直到V中的集合为空。

4. U中所有节点构成的树就是最小生成树。

使用邻接矩阵存储。
$$
复杂度:O(V^2)
$$



### Hash Table

Hash Table Find的平均长度

| 处理办法          | 查找成功                                 | 查找失败                                   | 插入 |
| ----------------- | ---------------------------------------- | ------------------------------------------ | ----------------- |
| Separate Chaining | $1+{\lambda \over 2}$ |$\lambda + e^{-\lambda}$| 与失败相同 |
| Linear Probing    | ${1\over 2}{(1+{1\over {(1-\lambda)}})}$                                         |   ${1\over 2}{(1+{1\over {(1-\lambda)^2}})}$                                           | 与失败相同 |
| Quadratic Probing | $-{1\over \lambda}\ln(1-\lambda)$ | $1\over {1-\lambda}$ | 与失败相同 |
| Double Hashing    | $-{1\over \lambda}\ln(1-\lambda)$ | $1\over {1-\lambda}$ | 失败相同 |

Hash表的平均查找与插入时间复杂度为$O(1) $，但真正的复杂度取决于他的冲突解决方法，最坏复杂度为$ O(n)$

查找失败总是比查找成功的平均长度长

**定理**：如果hash table的长度是质数，且表至少有一半是空的，那么使用Quadratic probing 总是能插入一个新的元素。

如果这个素数的形式是4k+3，且使用probing方法是$F(i)=\pm i^2$，那么整个哈希表都可以被探测到。

$F(i)=\pm i^2$的含义是每次探测先加再减

### 程序填空题

**2015-2016秋冬期末**

![image-20200107231121880](D:\Note\DS.assets\image-20200107231121880.png)

![image-20200107231137601](D:\Note\DS.assets\image-20200107231137601.png)

![image-20200107231152382](D:\Note\DS.assets\image-20200107231152382.png)



**2016-2017秋冬期末**

![image-20200109220813508](D:\Note\DS.assets\image-20200109220813508.png)

![image-20200109220915957](D:\Note\DS.assets\image-20200109220915957.png)

**2017-18秋冬期末**

![image-20200110002928539](D:\Note\DS.assets\image-20200110002928539.png)

![image-20200110002951538](D:\Note\DS.assets\image-20200110002951538.png)

**2019-2019秋冬期末**

![image-20200110131922501](D:\Note\DS.assets\image-20200110131922501.png)

![image-20200110131945491](D:\Note\DS.assets\image-20200110131945491.png)

### 函数题

**2015-2016秋冬期末**

![image-20200107231210292](D:\Note\DS.assets\image-20200107231210292.png)

![image-20200107231222800](D:\Note\DS.assets\image-20200107231222800.png)



![image-20200107231233167](D:\Note\DS.assets\image-20200107231233167.png)

不想写了，写了几遍都看错，看成写判断平衡二叉树，艹。

思路应该挺简单的。

**2016-2017秋冬期末**



![image-20200107231418988](D:\Note\DS.assets\image-20200107231418988.png)

![image-20200107231429247](D:\Note\DS.assets\image-20200107231429247.png)

**AC代码**

~~~c
#define MAXN 128

int dfs(Tree T, int X, int *Stack, int *Sp);

int LCA(Tree T, int u, int v) {
    int stack1[MAXN];
    int stack2[MAXN];
    int sp1 = -1;
    int sp2 = -1;
    int f1 = dfs(T, u, stack1, &sp1);
    int f2 = dfs(T, v, stack2, &sp2);
    if (f1 * f2 == 0) {
        return ERROR;
    }
    int min = (sp1 - sp2 < 0) ? sp1 : sp2;
    for (int i = 0; i <min+1; ++i) {
        if (stack1[i]!=stack2[i])
        {
            return stack1[i-1];
        }
    }
    return stack1[min];


}

int dfs(Tree T, int X, int *Stack, int *Sp) {
    Tree cur = T;
    while (1) {
        if (cur == NULL) {
            return 0;
        }
        Stack[++*Sp] = cur->Key;
        if (cur->Key > X) {
            cur = cur->Left;
        } else if (cur->Key < X) {
            cur = cur->Right;
        } else {
            return 1;
        }
    }

}
~~~

**2017-18秋冬期末**

![image-20200110110926817](D:\Note\DS.assets\image-20200110110926817.png)

![image-20200110110940149](D:\Note\DS.assets\image-20200110110940149.png)![image-20200110110949010](D:\Note\DS.assets\image-20200110110949010.png)

**AC代码**

~~~c
void ShortestDist(MGraph Graph, int dist[], Vertex S) {
    int vis[MaxVertexNum];
    int min_dist;
    int min_dist_v;
    for (int i = 0; i < Graph->Nv; ++i) {
        vis[i] = 0;
        dist[i] = INFINITY;
    }
    dist[S] = 0;

    while (1) {
        min_dist = -1;
        min_dist_v = INFINITY;
        for (int i = 0; i < Graph->Nv; ++i) {
            if (min_dist_v > dist[i] && !vis[i]) {
                min_dist_v = dist[i];
                min_dist = i;
            }
        }
        if (min_dist == -1) {
            break;
        }
        vis[min_dist] = 1;
        for (int i = 0; i < Graph->Nv; ++i) {
            if (Graph->G[min_dist][i] > 0 && Graph->G[min_dist][i] + dist[min_dist] < dist[i]) {
                dist[i] = Graph->G[min_dist][i] + dist[min_dist];
            }
        }

    }

    for (int i = 0; i < Graph->Nv; ++i) {
        if (dist[i] == INFINITY)
            dist[i] = -1;
    }

}
~~~

**2018-2019秋冬期末**

![image-20200110131616495](D:\Note\DS.assets\image-20200110131616495.png)

![image-20200110131654763](D:\Note\DS.assets\image-20200110131654763.png)

![image-20200110131709508](D:\Note\DS.assets\image-20200110131709508.png)

**AC代码**

~~~c
typedef struct Queue {
    Vertex Arr[128];
    int front;
    int rear;
} QueueADT;

typedef QueueADT *Queue;

Queue CreateQueue() {
    Queue Q = malloc(sizeof(QueueADT));
    Q->front = 0;
    Q->rear = -1;
    return Q;
}

void Enqueue(Queue Q, Vertex V) {
    Q->Arr[++Q->rear] = V;
}

Vertex Dequeue(Queue Q) {
    return Q->Arr[Q->front--];
}

int isEmpty(Queue Q) {
    return Q->rear + 1 == Q->front;
}

int isInQueue(Queue Q, Vertex X) {
    for (int i = Q->front; i <= Q->rear; i++) {
        if (Q->Arr[i] == X)
            return 1;
    }
    return 0;
}

/* Your function will be put here */
bool IsTopSeq(Vertex Seq[], LGraph G) {
    Queue Q = CreateQueue();
    for (int i = 0; i < G->N_v; ++i) {
        int cur = Seq[i] - 1;
        for (int j = 0; j < G->N_v; ++j) {
            if (isInQueue(Q, j) || j == cur) {
                continue;
            }

            PtrToAdjVNode temp = G->G[j].FirstEdge;
            while (temp != NULL) {
                if (temp->AdjV == cur) {
                    return false;
                }
                temp = temp->Next;
            }
        }
        Enqueue(Q, cur);
    }
    return true;
}
~~~

