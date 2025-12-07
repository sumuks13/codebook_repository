---
{"publish":true,"title":"Dijkstra’s Algorithm","cssclasses":""}
---

tags: [[dsa/data-structures/graph]], [[dsa/data-structures/queue]]

**What is Dijkstra’s Algorithm?**

Dijkstra’s algorithm finds the **shortest path from a source node to all other nodes** in a weighted graph (non-negative weights).

**When is it used?**

- Use Dijkstra when you need the shortest path (or distance) from one node to all others, or to a specific target, in a graph with non‑negative edge weights. 
- Typical applications include GPS navigation, network routing, traffic planning, robot navigation, and many other real‑world route/latency optimization tasks

**How to know a problem needs Dijkstra?**

- You are given a weighted graph (roads with distances, edges with costs, etc.) and must minimize the total weight from a source to others.​    
- All edge weights are non‑negative and you care about exact optimal distances, not just reachability or number of edges.​
- A plain BFS is not enough because “shortest” is by weight, not hops, and Bellman‑Ford/Floyd‑Warshall would be unnecessarily heavy for a sparse graph

**Algorithm**

A standard Dijkstra (for adjacency‑list graph, using a min‑priority queue) works as follows:​

1. Initialize `dist[source] = 0`, and all other `dist[v] = ∞`; mark all vertices unvisited.​
2. Push `(0, source)` into a min‑heap (priority queue) keyed by distance.​
3. While the queue is not empty:
    - Extract the node `u` with smallest tentative distance; if already finalized, skip.
    - For each edge `(u, v, w)`, if `dist[u] + w < dist[v]`, update `dist[v]` and push `(dist[v], v)` into the queue (edge relaxation).​
4. When the queue empties (or you popped the target vertex, if you only care about one destination), `dist[v]` holds the shortest distance from source to every `v`.


```java
class Edge {
    int dest, weight;
    Edge(int d, int w) { dest = d; weight = w; }
}

class Dijkstra {

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
