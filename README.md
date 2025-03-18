 #include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

// Graph class representing the water distribution network
class WaterDistribution {
private:
    int numNodes; // Number of outlets
    vector<vector<pair<int, int>>> adjList; // Adjacency List (node, pipe capacity)

public:
    // Constructor
    WaterDistribution(int nodes) {
        numNodes = nodes;
        adjList.resize(numNodes);
    }

    // Function to add pipes between outlets
    void addPipe(int u, int v, int capacity) {
        adjList[u].push_back({v, capacity});
        adjList[v].push_back({u, capacity}); // Undirected graph (bidirectional flow)
    }

    // Dijkstra’s Algorithm to find shortest path from the source
    void shortestPath(int source) {
        vector<int> dist(numNodes, INT_MAX); // Distance vector initialized to infinity
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;

        // Start from the source
        dist[source] = 0;
        pq.push({0, source});

        while (!pq.empty()) {
            int currentDist = pq.top().first;
            int currentNode = pq.top().second;
            pq.pop();

            // Process neighbors
            for (auto &neighbor : adjList[currentNode]) {
                int nextNode = neighbor.first;
                int weight = neighbor.second;

                // Relaxation step
                if (currentDist + weight < dist[nextNode]) {
                    dist[nextNode] = currentDist + weight;
                    pq.push({dist[nextNode], nextNode});
                }
            }
        }

        // Display shortest paths from source
        cout << "Shortest path from Main Water Source (Node " << source << "):\n";
        for (int i = 0; i < numNodes; i++) {
            cout << "To Node " << i << " - Distance: " << (dist[i] == INT_MAX ? -1 : dist[i]) << " units\n";
        }
    }
};

int main() {
    int nodes = 5; // Example: 5 water outlets
    WaterDistribution waterSystem(nodes);

    // Adding pipes between outlets (node1, node2, capacity)
    waterSystem.addPipe(0, 1, 10); // Pipe from Source to Kitchen
    waterSystem.addPipe(0, 2, 20); // Pipe from Source to Bathroom
    waterSystem.addPipe(1, 3, 5);  // Kitchen to Garden
    waterSystem.addPipe(2, 3, 15); // Bathroom to Garden
    waterSystem.addPipe(3, 4, 10); // Garden to Washing Machine

    // Find shortest paths from the Main Water Source (Node 0)
    waterSystem.shortestPath(0);

    return 0;
}
