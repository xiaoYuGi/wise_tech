



## Q2 2039. 网络空闲的时刻
https://leetcode.cn/problems/the-time-when-the-network-becomes-idle/submissions/581908899/

> 本题知识点

1. 链式前向星的建图方式，并且细节设置；
2. 取模的小技巧使用；

```java
int[] h, e, ne;
boolean[] status;
int idx, N = 100010;

void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}

void init(int[][] edges) {
    h = new int[N];

    // `1 <= edges.length <= min(10^5, n * (n - 1) / 2)`  根据题目提示，最多100000条边，这里边的双向都要存一次，因此2倍节点长度即可
    e = new int[N * 2];
    // 每个节点的边链表长度为m的话，那么该节点的下一条边应该是m - 1, 因此 n * 2
    ne = new int[N * 2];
    status = new boolean[N];
    status[0] = true;
    idx = 0;
    Arrays.fill(h, -1);
    for (int[] edge : edges) {
        add(edge[0], edge[1]);
        add(edge[1], edge[0]);
    }
}

public int networkBecomesIdle(int[][] edges, int[] patience) {
    init(edges);

    Queue<Integer> q = new ArrayDeque<>();
    q.offer(0);
    int dis = 1;
    int ans = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            Integer cur = q.poll();
            if (cur == null) continue;
            for (int nextIdx = h[cur]; nextIdx != -1; nextIdx = ne[nextIdx]) {
                int nextNode = e[nextIdx];
                if (status[nextNode]) continue;
                status[nextNode] = true;
                int di = dis * 2, t = patience[nextNode];
                // (di - 1) / t * t + di   1.如果di - 1导致模数 -1,则说明原来di可以整除，这里直接去除一倍t， 如果di - 1不会导致模数-1，那么舍弃余数. 是这里场景最简洁的方式
                int curTime = di <= t ? di : (di - 1) / t * t + di;
                ans = Math.max(ans, curTime);
                q.offer(nextNode);
            }
        }
        ++dis;
    }
    return ans + 1;
}
```