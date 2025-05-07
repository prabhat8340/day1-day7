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

#define MAX 8

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

strassen matrix 

#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define MAX 4

// Matrix addition
void addMatrix(int size, int A[MAX][MAX], int B[MAX][MAX], int result[MAX][MAX]) {
    int i, j;
	for (i = 0; i < size; i++)
        for (j = 0; j < size; j++)
            result[i][j] = A[i][j] + B[i][j];
}

// Matrix subtraction
void subtractMatrix(int size, int A[MAX][MAX], int B[MAX][MAX], int result[MAX][MAX]) {
    int i, j;
	for (i = 0; i < size; i++)
        for (j = 0; j < size; j++)
            result[i][j] = A[i][j] - B[i][j];
}

// Divide & Conquer multiplication
void divideConquerMultiply(int size, int A[MAX][MAX], int B[MAX][MAX], int C[MAX][MAX]) {
    if (size == 1) {
        C[0][0] = A[0][0] * B[0][0];
        return;
    }

    int newSize = size / 2;
    int A11[MAX][MAX], A12[MAX][MAX], A21[MAX][MAX], A22[MAX][MAX];
    int B11[MAX][MAX], B12[MAX][MAX], B21[MAX][MAX], B22[MAX][MAX];
    int C11[MAX][MAX], C12[MAX][MAX], C21[MAX][MAX], C22[MAX][MAX];
    int temp1[MAX][MAX], temp2[MAX][MAX];

    // Initialize
    int i, j;
    for (i = 0; i < MAX; i++)
        for (j = 0; j < MAX; j++)
            C[i][j] = 0;

    // Divide
    for (i = 0; i < newSize; i++)
        for (j = 0; j < newSize; j++) {
            A11[i][j] = A[i][j];
            A12[i][j] = A[i][j + newSize];
            A21[i][j] = A[i + newSize][j];
            A22[i][j] = A[i + newSize][j + newSize];
            B11[i][j] = B[i][j];
            B12[i][j] = B[i][j + newSize];
            B21[i][j] = B[i + newSize][j];
            B22[i][j] = B[i + newSize][j + newSize];
        }

    // Recursive calls and combining
    divideConquerMultiply(newSize, A11, B11, C11);
    divideConquerMultiply(newSize, A12, B21, temp1);
    addMatrix(newSize, C11, temp1, C11);

    divideConquerMultiply(newSize, A11, B12, C12);
    divideConquerMultiply(newSize, A12, B22, temp1);
    addMatrix(newSize, C12, temp1, C12);

    divideConquerMultiply(newSize, A21, B11, C21);
    divideConquerMultiply(newSize, A22, B21, temp1);
    addMatrix(newSize, C21, temp1, C21);

    divideConquerMultiply(newSize, A21, B12, C22);
    divideConquerMultiply(newSize, A22, B22, temp1);
    addMatrix(newSize, C22, temp1, C22);

    // Combine results into C
    for (i = 0; i < newSize; i++)
        for (j = 0; j < newSize; j++) {
            C[i][j] = C11[i][j];
            C[i][j + newSize] = C12[i][j];
            C[i + newSize][j] = C21[i][j];
            C[i + newSize][j + newSize] = C22[i][j];
        }
}

// Strassen's Multiplication (already optimized)
void strassenMultiply(int size, int A[MAX][MAX], int B[MAX][MAX], int C[MAX][MAX]) {
    if (size == 1) {
        C[0][0] = A[0][0] * B[0][0];
        return;
    }

    int newSize = size / 2;
    int A11[MAX][MAX], A12[MAX][MAX], A21[MAX][MAX], A22[MAX][MAX];
    int B11[MAX][MAX], B12[MAX][MAX], B21[MAX][MAX], B22[MAX][MAX];
    int C11[MAX][MAX], C12[MAX][MAX], C21[MAX][MAX], C22[MAX][MAX];
    int M1[MAX][MAX], M2[MAX][MAX], M3[MAX][MAX], M4[MAX][MAX];
    int M5[MAX][MAX], M6[MAX][MAX], M7[MAX][MAX];
    int temp1[MAX][MAX], temp2[MAX][MAX];

    // Initialize C
    int i, j;
    for (i = 0; i < MAX; i++)
        for (j = 0; j < MAX; j++)
            C[i][j] = 0;

    // Divide
    for (i = 0; i < newSize; i++)
        for (j = 0; j < newSize; j++) {
            A11[i][j] = A[i][j];
            A12[i][j] = A[i][j + newSize];
            A21[i][j] = A[i + newSize][j];
            A22[i][j] = A[i + newSize][j + newSize];

            B11[i][j] = B[i][j];
            B12[i][j] = B[i][j + newSize];
            B21[i][j] = B[i + newSize][j];
            B22[i][j] = B[i + newSize][j + newSize];
        }

    // 7 Strassen multiplications
    addMatrix(newSize, A11, A22, temp1);
    addMatrix(newSize, B11, B22, temp2);
    strassenMultiply(newSize, temp1, temp2, M1);

    addMatrix(newSize, A21, A22, temp1);
    strassenMultiply(newSize, temp1, B11, M2);

    subtractMatrix(newSize, B12, B22, temp2);
    strassenMultiply(newSize, A11, temp2, M3);

    subtractMatrix(newSize, B21, B11, temp2);
    strassenMultiply(newSize, A22, temp2, M4);

    addMatrix(newSize, A11, A12, temp1);
    strassenMultiply(newSize, temp1, B22, M5);

    subtractMatrix(newSize, A21, A11, temp1);
    addMatrix(newSize, B11, B12, temp2);
    strassenMultiply(newSize, temp1, temp2, M6);

    subtractMatrix(newSize, A12, A22, temp1);
    addMatrix(newSize, B21, B22, temp2);
    strassenMultiply(newSize, temp1, temp2, M7);

    // Combine results
    addMatrix(newSize, M1, M4, temp1);
    subtractMatrix(newSize, temp1, M5, temp2);
    addMatrix(newSize, temp2, M7, C11);

    addMatrix(newSize, M3, M5, C12);
    addMatrix(newSize, M2, M4, C21);

    subtractMatrix(newSize, M1, M2, temp1);
    addMatrix(newSize, temp1, M3, temp2);
    addMatrix(newSize, temp2, M6, C22);

    // Final assembly
    for (i = 0; i < newSize; i++)
        for (j = 0; j < newSize; j++) {
            C[i][j] = C11[i][j];
            C[i][j + newSize] = C12[i][j];
            C[i + newSize][j] = C21[i][j];
            C[i + newSize][j + newSize] = C22[i][j];
        }
}

// Print matrix
void printMatrix(int size, int M[MAX][MAX]) {
    int i, j;
	for (i = 0; i < size; i++) {
        for(j = 0; j < size; j++)
            printf("%4d ", M[i][j]);
        printf("\n");
    }
}
void normalMultiply(int size, int A[MAX][MAX], int B[MAX][MAX], int C[MAX][MAX]){
	int i, j, k;
	for(i=0; i<size;i++){
		for(j=0; j<size;j++){
			C[i][j]=0;
			for(k=0; k<size;k++){
				C[i][j]+= A[i][k]*B[k][j];
			}
		}
	}
}

int main() {
    int A[MAX][MAX], B[MAX][MAX], C1[MAX][MAX], C2[MAX][MAX], C3[MAX][MAX];
    srand(time(NULL));

    // Generate random matrices A and B
    int i, j;
    for (i = 0; i < MAX; i++)
        for (j = 0; j < MAX; j++) {
            A[i][j] = rand() % 10;
            B[i][j] = rand() % 10;
        }

    printf("Matrix A:\n");
    printMatrix(MAX, A);
    printf("\nMatrix B:\n");
    printMatrix(MAX, B);

    clock_t start, end;
    double time_strassen, time_divide, time_normal;

    // Strassen's execution time
    start = clock();
    strassenMultiply(MAX, A, B, C1);
    end = clock();
    time_strassen = ((double)(end - start)) / CLOCKS_PER_SEC;

    // Divide & Conquer execution time
    start = clock();
    divideConquerMultiply(MAX, A, B, C2);
    end = clock();
    time_divide = ((double)(end - start)) / CLOCKS_PER_SEC;
    
    start = clock();
    normalMultiply(MAX, A, B, C3);
    end = clock();
    time_normal = ((double)(end - start)) / CLOCKS_PER_SEC;

    printf("\nStrassen's Result:\n");
    printMatrix(MAX, C1);

    printf("\nDivide and Conquer Result:\n");
    printMatrix(MAX, C2);
    
    printf("\nNormal Matrix Result:\n");
    printMatrix(MAX, C3);

    printf("\nExecution Time:\n");
    printf("Strassen's Algorithm: %.6f seconds\n", time_strassen);
    printf("Divide and Conquer Algorithm: %.6f seconds\n", time_divide);
    printf("Normal Matrix Multiplication Algorithm: %.6f seconds\n", time_normal);

    return 0;
}
