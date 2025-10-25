tags: [[Algorithms/Algorithms]], [[Data Structures/Graph]]
### **What is Dijkstra’s Algorithm?**

Dijkstra’s algorithm finds the **shortest path from a source node to all other nodes** in a weighted graph (non-negative weights).

**Steps:**

1. Initialize distances as infinity, except source = 0.
2. Use a priority queue to pick the minimum distance vertex.
3. Update distances of neighbors.
4. Repeat until all vertices are processed.

```java
import java.util.*;

class Dijkstra {
    static class Edge {
        int dest, weight;
        Edge(int d, int w) { dest = d; weight = w; }
    }

    void shortestPath(List<Edge>[] graph, int src) {
        int V = graph.length;
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        pq.add(new int[]{src, 0});

        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int u = curr[0];
            for (Edge e : graph[u]) {
                if (dist[u] + e.weight < dist[e.dest]) {
                    dist[e.dest] = dist[u] + e.weight;
                    pq.add(new int[]{e.dest, dist[e.dest]});
                }
            }
        }

        System.out.println(Arrays.toString(dist));
    }
}
```
