day7p1 knsapack hai 
day7p2 job sequincing 
#include <stdio.h>
#include <string.h>

struct Job {
    char id[5];
    int deadline, profit;
};

void sortByProfit(struct Job arr[], int n) {
    for (int i = 0; i < n -1; i++) {
        for (int j = 0; j < n - i -1; j++) {
            if (arr[j].profit < arr[j+1].profit) {
                struct Job temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}

void jobSequencing(struct Job arr[], int n) {
    int result[n];
    int slot[n];
    memset(slot, 0, sizeof(slot));

    sortByProfit(arr, n);

    int totalProfit = 0;

    for (int i = 0; i < n; i++) {
        for (int j = arr[i].deadline -1; j >=0; j--) {
            if (slot[j] == 0) {
                result[j] = i;
                slot[j] = 1;
                totalProfit += arr[i].profit;
                break;
            }
        }
    }

    printf("Scheduled Jobs: ");
    for (int i = 0; i < n; i++) {
        if (slot[i])
            printf("%s ", arr[result[i]].id);
    }
    printf("\nTotal Profit: %d\n", totalProfit);
}

int main() {
    int n;
    printf("Enter number of jobs: ");
    scanf("%d", &n);
    struct Job arr[n];
    printf("Enter job id, deadline, profit:\n");
    for (int i = 0; i < n; i++) {
        scanf("%s %d %d", arr[i].id, &arr[i].deadline, &arr[i].profit);
    }
    jobSequencing(arr, n);
    return 0;
}

day6p2 bpf

#include <stdio.h>

#define MAX 5

// Function to perform BFS
void BFS(int adj[MAX][MAX], int n, int start) {
    int visited[MAX] = {0};
    int queue[MAX], front = 0, rear = 0;
    int i;
    
    printf("BFS Traversal: ");
    queue[rear++] = start;
    visited[start] = 1;
    
    while (front < rear) {
        int node = queue[front++];
        printf("%d ", node);
        
        for (i = 0; i < n; i++) {
            if (adj[node][i] == 1 && !visited[i]) {
                queue[rear++] = i;
                visited[i] = 1;
            }
        }
    }
    printf("\n");
}

int main() {
    int n, start;
    int adj[MAX][MAX];
    int i, j;
    
    printf("Enter number of vertices: ");
    scanf("%d", &n);
    
    printf("Enter adjacency matrix:\n");
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            scanf("%d", &adj[i][j]);
        }
    }
    
    printf("Enter the starting vertex: ");
    scanf("%d", &start);
    
    BFS(adj, n, start);
    
    return 0;
}

daylast p1
// frist wala 
#include <stdio.h>
#include <limits.h>

#define V 10

int minDistance(int dist[], int sptSet[], int n) {
    int min = INT_MAX, min_index = -1;

    for (int v = 0; v < n; v++)
        if (sptSet[v] == 0 && dist[v] <= min)
            min = dist[v], min_index = v;

    return min_index;
}

void dijkstra(int graph[V][V], int src, int n) {
    int dist[V];
    int sptSet[V];

    for (int i = 0; i < n; i++)
        dist[i] = INT_MAX, sptSet[i] = 0;

    dist[src] = 0;

    for (int count = 0; count < n - 1; count++) {
        int u = minDistance(dist, sptSet, n);
        if (u == -1) break;  // No reachable vertex
        sptSet[u] = 1;

        for (int v = 0; v < n; v++)
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX &&
                dist[u] + graph[u][v] < dist[v])
                dist[v] = dist[u] + graph[u][v];
    }

    printf("Vertex \t Distance from Source\n");
    for (int i = 0; i < n; i++) {
        if (dist[i] == INT_MAX)
            printf("%d \t INF\n", i);
        else
            printf("%d \t %d\n", i, dist[i]);
    }
}

int main() {
    int n;
    printf("Enter number of vertices (max %d): ", V);
    scanf("%d", &n);

    if (n <= 0 || n > V) {
        printf("Invalid number of vertices! Must be between 1 and %d.\n", V);
        return 1;
    }

    int graph[V][V];
    printf("Enter the adjacency matrix (use 0 if no edge):\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &graph[i][j]);

    int src;
    printf("Enter source vertex (0 to %d): ", n - 1);
    scanf("%d", &src);

    if (src < 0 || src >= n) {
        printf("Invalid source vertex! Must be between 0 and %d.\n", n - 1);
        return 1;
    }

    dijkstra(graph, src, n);

    return 0;
}

// or ye second hai agar sir bole to

#include <stdio.h>
#include <limits.h>

#define V 10

int minDistance(int dist[], int sptSet[], int n) {
    int min = INT_MAX, min_index;

    for (int v = 0; v < n; v++)
        if (sptSet[v] == 0 && dist[v] <= min)
            min = dist[v], min_index = v;

    return min_index;
}

void dijkstra(int graph[V][V], int src, int n) {
    int dist[V];
    int sptSet[V];

    for (int i = 0; i < n; i++)
        dist[i] = INT_MAX, sptSet[i] = 0;

    dist[src] = 0;

    for (int count = 0; count < n - 1; count++) {
        int u = minDistance(dist, sptSet, n);
        sptSet[u] = 1;

        for (int v = 0; v < n; v++)
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX &&
                dist[u] + graph[u][v] < dist[v])
                dist[v] = dist[u] + graph[u][v];
    }

    printf("Vertex \t Distance from Source\n");
    for (int i = 0; i < n; i++)
        printf("%d \t %d\n", i, dist[i]);
}

int main() {
    int n;
    printf("Enter number of vertices: ");
    scanf("%d", &n);

    int graph[V][V];
    printf("Enter the adjacency matrix (use 0 if no edge):\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &graph[i][j]);

    int src;
    printf("Enter source vertex (0 to %d): ", n - 1);
    scanf("%d", &src);

    dijkstra(graph, src, n);

    return 0;
}

daylastp2
#include <stdio.h>
#include <limits.h>

void matrixChainOrder(int p[], int n) {
    int m[n][n];

    for (int i = 1; i < n; i++)
        m[i][i] = 0;

    for (int L = 2; L < n; L++) {
        for (int i = 1; i < n - L + 1; i++) {
            int j = i + L - 1;
            m[i][j] = INT_MAX;
            for (int k = i; k <= j - 1; k++) {
                int q = m[i][k] + m[k + 1][j] + p[i - 1] * p[k] * p[j];
                if (q < m[i][j])
                    m[i][j] = q;
            }
        }
    }

    printf("Minimum number of multiplications: %d\n", m[1][n - 1]);
}

int main() {
    int n;
    printf("Enter number of matrices: ");
    scanf("%d", &n);

    int p[n + 1];
    printf("Enter dimensions array (length %d):\n", n + 1);
    for (int i = 0; i <= n; i++)
        scanf("%d", &p[i]);

    matrixChainOrder(p, n + 1);

    return 0;
}

