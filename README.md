# Network-Delay-Time
### Dijkstra Algorithm Application to Get Network Delay Time
There are N network nodes, labelled 1 to N.
Given times, a list of travel times as directed edges times[i] = (u, v, w), where u is the source node, v is the target node, and w is the time it takes for a signal to travel from source to target.
Now, we send a signal from a certain node K. How long will it take for all nodes to receive the signal? If it is impossible, return -1.
### Solution
 - Considering that we are looking for the longest path among all shortest paths from the source node, which is K, to all other nodes in the graph, using Dijkstra's algorithm seems a natural choice since it is made for solveing shortest path problems.
 - For a detailed explanation of Dijkstra's algorithm, check wiki page.
 - Here we used a implementation of heap based dijkstra algorithm.
 - Time complexity: O(ElogE) where E is the number of edges in the graph.
 - Space complexity: O(N + E). O(N) for the dist. O(E) for the heap and adjacency list.
 ```java
 class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        int[] time = new int[N];
        Arrays.fill(time, Integer.MAX_VALUE);
        time[K - 1] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b)->a[1] - b[1]);
        pq.offer(new int[]{K - 1, 0});
        while (!pq.isEmpty()) {
            int[] node = pq.poll();
            for (int[] cand : times) {
                if (cand[0] == node[0] + 1) {
                    if (node[1] + cand[2] < time[cand[1] - 1]) {
                        time[cand[1] - 1] = node[1] + cand[2];
                        pq.offer(new int[]{cand[1] - 1, time[cand[1]  -1]});
                    }
                }
            }
        }
        int max = 0;
        for (int i : time) {
            if (i == Integer.MAX_VALUE) {
               return -1;
            }
            max = Math.max (max, i);
        }
        return max;
    }
}
```
