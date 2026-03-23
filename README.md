LINEAR SEARCH
#include <stdio.h>

int main() {
    int arr[100], n, key, i, found = 0;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter elements:\n");
    for(i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("Enter element to search: ");
    scanf("%d", &key);

    for(i = 0; i < n; i++) {
        if(arr[i] == key) {
            printf("Element found at position %d", i + 1);
            found = 1;
            break;
        }
    }

    if(found == 0) {
        printf("Element not found");
    }

    return 0;
}
BREADTH FIRST SEARCH
#include <stdio.h>
#include <stdlib.h>

#define MAX 100

int queue[MAX];
int front = -1, rear = -1;

void enqueue(int value) {
    if (rear == MAX - 1)
        printf("Queue Overflow\n");
    else {
        if (front == -1)
            front = 0;
        queue[++rear] = value;
    }
}

int dequeue() {
    if (front == -1 || front > rear) {
        printf("Queue Underflow\n");
        return -1;
    }
    return queue[front++];
}

int isEmpty() {
    return (front == -1 || front > rear);
}

void BFS(int graph[MAX][MAX], int n, int start) {
    int visited[MAX] = {0};

    enqueue(start);
    visited[start] = 1;

    while (!isEmpty()) {
        int node = dequeue();
        printf("%d ", node);

        for (int i = 0; i < n; i++) {
            if (graph[node][i] == 1 && !visited[i]) {
                enqueue(i);
                visited[i] = 1;
            }
        }
    }
}

int main() {
    int n, start;
    int graph[MAX][MAX];

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &graph[i][j]);
        }
    }

    printf("Enter starting node: ");
    scanf("%d", &start);

    printf("BFS Traversal: ");
    BFS(graph, n, start);

    return 0;
}
DFS USING RECURSION
#include <stdio.h>

#define MAX 100

int visited[MAX];

void DFS(int graph[MAX][MAX], int n, int node) {
    printf("%d ", node);
    visited[node] = 1;

    for (int i = 0; i < n; i++) {
        if (graph[node][i] == 1 && !visited[i]) {
            DFS(graph, n, i);
        }
    }
}

int main() {
    int n, start;
    int graph[MAX][MAX];

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &graph[i][j]);
        }
    }

    printf("Enter starting node: ");
    scanf("%d", &start);

    for (int i = 0; i < n; i++)
        visited[i] = 0;

    printf("DFS Traversal: ");
    DFS(graph, n, start);

    return 0;
}
DEPTH FIRST SEARCH USING ADJACENCY MATRIX
#include <stdio.h>

#define MAX 10

int adj[MAX][MAX], visited[MAX], n;

void dfs(int v) {
    int i;
    printf("%d ", v);
    visited[v] = 1;

    for(i = 1; i <= n; i++) {
        if(adj[v][i] == 1 && visited[i] == 0) {
            dfs(i);
        }
    }
}

int main() {
    int i, j, start;

    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for(i = 1; i <= n; i++) {
        for(j = 1; j <= n; j++) {
            scanf("%d", &adj[i][j]);
        }
    }

    for(i = 1; i <= n; i++)
        visited[i] = 0;

    printf("Enter starting vertex: ");
    scanf("%d", &start);

    printf("DFS traversal: ");
    dfs(start);

    return 0;
}
0/1 Knapsack Dynamic Programming
#include <stdio.h>

// Function to return maximum of two numbers
int max(int a, int b) {
    return (a > b) ? a : b;
}

int main() {
    int n, W;

    printf("Enter number of items: ");
    scanf("%d", &n);

    int wt[n], val[n];

    printf("Enter weights of items:\n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &wt[i]);
    }

    printf("Enter values of items:\n");
    for(int i = 0; i < n; i++) {
        scanf("%d", &val[i]);
    }

    printf("Enter capacity of knapsack: ");
    scanf("%d", &W);

    // DP table
    int K[n+1][W+1];

    // Build table K[][] in bottom-up manner
    for(int i = 0; i <= n; i++) {
        for(int w = 0; w <= W; w++) {
            if(i == 0 || w == 0)
                K[i][w] = 0;
            else if(wt[i-1] <= w)
                K[i][w] = max(val[i-1] + K[i-1][w - wt[i-1]], K[i-1][w]);
            else
                K[i][w] = K[i-1][w];
        }
    }

    printf("Maximum value in Knapsack = %d", K[n][W]);

    return 0;
}
RECURSIVE VERSION OF 0/1 KNAPSACK
#include <stdio.h>

// Function to return maximum of two numbers
int max(int a, int b) {
    return (a > b) ? a : b;
}

// Recursive Knapsack function
int knapsack(int W, int wt[], int val[], int n) {
    // Base case
    if (n == 0 || W == 0)
        return 0;

    // If weight of nth item is more than capacity, skip it
    if (wt[n-1] > W)
        return knapsack(W, wt, val, n-1);

    // Return max of including or excluding the item
    else
        return max(
            val[n-1] + knapsack(W - wt[n-1], wt, val, n-1), // include
            knapsack(W, wt, val, n-1)                       // exclude
        );
}

int main() {
    int n, W;

    printf("Enter number of items: ");
    scanf("%d", &n);

    int wt[n], val[n];

    printf("Enter weights:\n");
    for(int i = 0; i < n; i++)
        scanf("%d", &wt[i]);

    printf("Enter values:\n");
    for(int i = 0; i < n; i++)
        scanf("%d", &val[i]);

    printf("Enter capacity: ");
    scanf("%d", &W);

    int result = knapsack(W, wt, val, n);

    printf("Maximum value = %d", result);

    return 0;
}
BREADTH FIRST SEARCH USING RECURSION
#include <stdio.h>
#include <stdlib.h>

#define MAX 100

int graph[MAX][MAX];
int visited[MAX];
int queue[MAX];
int front = 0, rear = 0;
int n;

// Enqueue
void enqueue(int v) {
    if (rear < MAX) {
        queue[rear++] = v;
    }
}

// Recursive BFS function
void bfs_recursive() {
    if (front == rear)  // queue empty
        return;

    int current = queue[front++];
    printf("%d ", current);

    for (int i = 0; i < n; i++) {
        if (graph[current][i] == 1 && !visited[i]) {
            visited[i] = 1;
            enqueue(i);
        }
    }

    bfs_recursive();  // recursive call
}

int main() {
    int start;

    printf("Enter number of vertices: ");
    scanf("%d", &n);

    printf("Enter adjacency matrix:\n");
    for (int i = 0; i < n; i++) {
        visited[i] = 0;
        for (int j = 0; j < n; j++) {
            scanf("%d", &graph[i][j]);
        }
    }

    printf("Enter starting vertex: ");
    scanf("%d", &start);

    visited[start] = 1;
    enqueue(start);

    printf("BFS Traversal: ");
    bfs_recursive();

    return 0;
}
INPUT:
Enter number of vertices: 4
Enter adjacency matrix:
0 1 1 0
1 0 1 1
1 1 0 0
0 1 0 0
Enter starting vertex: 0
OUTPUT:
BFS Traversal: 0 1 2 3
QUICK SORT
#include <stdio.h>

// Function to swap two elements
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Partition function
int partition(int arr[], int low, int high) {
    int pivot = arr[high];   // pivot element
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

// Quick Sort function
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);   // left sub-array
        quickSort(arr, pi + 1, high);  // right sub-array
    }
}

// Function to print array
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

// Main function
int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original array:\n");
    printArray(arr, n);

    quickSort(arr, 0, n - 1);

    printf("Sorted array:\n");
    printArray(arr, n);

    return 0;
}
OUTPUT:
Original array:
10 7 8 9 1 5 
Sorted array:
1 5 7 8 9 10
MERGE SORT:
#include <stdio.h>

// Function to merge two halves
void merge(int arr[], int left, int mid, int right) {
    int i, j, k;

    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Temporary arrays
    int L[n1], R[n2];

    // Copy data
    for (i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    // Merge arrays
    i = 0; 
    j = 0; 
    k = left;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy remaining elements
    while (i < n1) {
        arr[k] = L[i];
        i++; 
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++; 
        k++;
    }
}

// Merge Sort function
void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Merge them
        merge(arr, left, mid, right);
    }
}

// Print array
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

// Main function
int main() {
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original array:\n");
    printArray(arr, n);

    mergeSort(arr, 0, n - 1);

    printf("Sorted array:\n");
    printArray(arr, n);

    return 0;
}
OUTPUT:
Original array:
38 27 43 3 9 82 10

Sorted array:
3 9 10 27 38 43 82
TRAVELLING SALESMAN PROBLEM:
#include <stdio.h>
#include <limits.h>

#define MAX 10

int n;
int cost[MAX][MAX];
int dp[1 << MAX][MAX];

// Function to find minimum cost
int tsp(int mask, int pos) {
    // If all cities are visited, return cost to go back to start
    if (mask == (1 << n) - 1)
        return cost[pos][0];

    // If already computed
    if (dp[mask][pos] != -1)
        return dp[mask][pos];

    int ans = INT_MAX;

    // Try visiting all unvisited cities
    for (int city = 0; city < n; city++) {
        if ((mask & (1 << city)) == 0) {
            int newAns = cost[pos][city] +
                         tsp(mask | (1 << city), city);

            if (newAns < ans)
                ans = newAns;
        }
    }

    return dp[mask][pos] = ans;
}

int main() {
    printf("Enter number of cities: ");
    scanf("%d", &n);

    printf("Enter cost matrix:\n");
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &cost[i][j]);

    // Initialize dp table
    for (int i = 0; i < (1 << n); i++)
        for (int j = 0; j < n; j++)
            dp[i][j] = -1;

    int result = tsp(1, 0); // Start from city 0

    printf("Minimum travelling cost: %d\n", result);

    return 0;
}
EXAMPLE INPUT:
Enter number of cities: 4
Enter cost matrix:
0 10 15 20
10 0 35 25
15 35 0 30
20 25 30 0
OUTPUT:
Minimum travelling cost: 80
