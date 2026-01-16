# Maximum-Gold-Collection-in-a-Kingdom

You are exploring a kingdom made up of n cities connected by roads. Each city contains a certain amount of gold, but you can collect the gold from a city only the first time you visit it.

The kingdom is represented as an undirected graph:

Each city is a node.
Each road connects two cities and can be traveled in both directions.

You may start your journey from any city of your choice. From there, you can move to any adjacent connected city, and you are allowed to visit the same city multiple times, but gold is collected only once per city.
Your task is to determine the maximum total gold you can collect by choosing an optimal starting city and moving through the kingdom.

#include <bits/stdc++.h>
using namespace std;

vector<long long> gold;
vector<vector<int>> graph;
vector<bool> visited;

long long dfs(int node) {
    visited[node] = true;
    long long total = gold[node];
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            total += dfs(neighbor);
        }
    }
    return total;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;

    gold.resize(n);
    for (int i = 0; i < n; i++) {
        cin >> gold[i];
    }

    graph.resize(n);
    visited.assign(n, false);

    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        u--; v--; // converting to 0-based index
        graph[u].push_back(v);
        graph[v].push_back(u);
    }

    long long maxGold = 0;

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            long long componentGold = dfs(i);
            maxGold = max(maxGold, componentGold);
        }
    }

    cout << maxGold << "\n";
    return 0;
}
