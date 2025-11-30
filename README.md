# Network-Delay-Time

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int,int>>> graph(n + 1);
        for (auto &t : times) {
            int u = t[0], v = t[1], w = t[2];
            graph[u].push_back({v, w});
        }
        
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> q;
        q.push({0, k});
        
        vector<int> dist(n + 1, INT_MAX);
        dist[k] = 0;
        
        while (!q.empty()) {
            auto [time, node] = q.top();
            q.pop();
            
            if (time > dist[node]) {
                continue;
            }
            for (auto &edge : graph[node]) {
                int nxt = edge.first;
                int w = edge.second;
                
                if (dist[node] + w < dist[nxt]) {
                    dist[nxt] = dist[node] + w;
                    q.push({dist[nxt], nxt});
                }
            }
        }
        
        int x = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == INT_MAX) return -1;
            x = max(x, dist[i]);
        }
        return x;
    }
};
