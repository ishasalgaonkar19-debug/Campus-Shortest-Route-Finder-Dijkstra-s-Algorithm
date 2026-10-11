# Campus-Shortest-Route-Finder-Dijkstra-s-Algorithm
A college campus has several locations such as the Main Gate, Library, Computer Department, Laboratory and  Auditorium.. Develop a menu-driven C program that allows a user to enter the campus road network, select a source location,  and find the shortest distance from the selected source to every other location using Dijkstra's Algorithm

#include <stdio.h>
#include <string.h>

#define MAX 20
#define INF 9999

int graph[MAX][MAX];
char name[MAX][50];
int dist[MAX], visited[MAX], parent[MAX];
int n, source;

void printPath(int v)
{
    if (parent[v] == -1)
    {
        printf("%s", name[v]);
        return;
    }

    printPath(parent[v]);
    printf(" -> %s", name[v]);
}

void dijkstra()
{
    int i, j, count;
    int min, u;

    for (i = 0; i < n; i++)
    {
        dist[i] = INF;
        visited[i] = 0;
        parent[i] = -1;
    }

    dist[source] = 0;

    /* Find shortest paths */
    for (count = 0; count < n - 1; count++)
    {
        min = INF;
        u = -1;

        for (i = 0; i < n; i++)
        {
            if (visited[i] == 0 && dist[i] < min)
            {
                min = dist[i];
                u = i;
            }
        }

        if (u == -1)
            break;

        visited[u] = 1;

        for (j = 0; j < n; j++)
        {
            if (visited[j] == 0 &&
                graph[u][j] != INF &&
                dist[u] + graph[u][j] < dist[j])
            {
                dist[j] = dist[u] + graph[u][j];
                parent[j] = u;
            }
        }
    }
}

void enterGraph()
{
    int i, j;

    printf("\nEnter number of locations: ");
    scanf("%d", &n);

    printf("\nEnter location names:\n");

    for (i = 0; i < n; i++)
    {
        printf("Location %d: ", i + 1);
        scanf(" %[^\n]", name[i]);
    }

    printf("\nEnter adjacency matrix:\n");
    printf("Enter %d for no direct connection.\n\n", INF);

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
        {
            scanf("%d", &graph[i][j]);

            if (i == j)
                graph[i][j] = 0;
        }
    }

    printf("\nGraph entered successfully.\n");
}

void displayMatrix()
{
    int i, j;

    printf("\nAdjacency Matrix:\n\n");

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
        {
            if (graph[i][j] == INF)
                printf("INF\t");
            else
                printf("%d\t", graph[i][j]);
        }
        printf("\n");
    }
}

void selectSource()
{
    int i, choice;

    printf("\nLocations:\n");

    for (i = 0; i < n; i++)
        printf("%d. %s\n", i + 1, name[i]);

    printf("\nEnter source location: ");
    scanf("%d", &choice);

    if (choice < 1 || choice > n)
    {
        printf("Invalid location!\n");
        return;
    }

    source = choice - 1;

    printf("\nSource selected: %s\n", name[source]);
}

void displayPaths()
{
    int i;

    printf("\nShortest Paths from %s:\n\n", name[source]);

    printf("%-25s %-15s %s\n",
           "Destination", "Distance", "Shortest Path");

    printf("---------------------------------------------------------------\n");

    for (i = 0; i < n; i++)
    {
        if (i == source)
            continue;

        printf("%-25s ", name[i]);

        if (dist[i] == INF)
        {
            printf("%-15s No path\n", "INF");
        }
        else
        {
            printf("%-15d ", dist[i]);
            printPath(i);
            printf("\n");
        }
    }
}

void displayDistances()
{
    int i;

    printf("\nDistance from %s:\n\n", name[source]);

    for (i = 0; i < n; i++)
    {
        if (dist[i] == INF)
            printf("%s : INF\n", name[i]);
        else
            printf("%s : %d\n", name[i], dist[i]);
    }
}

int main()
{
    int choice;

    do
    {
        printf("\n====================================\n");
        printf(" CAMPUS SHORTEST ROUTE FINDER\n");
        printf("====================================\n");
        printf("1. Enter Campus Graph\n");
        printf("2. Display Adjacency Matrix\n");
        printf("3. Select Source Location\n");
        printf("4. Find Shortest Distance\n");
        printf("5. Display Shortest Paths\n");
        printf("6. Display Distance from Source\n");
        printf("7. Exit\n");

        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                enterGraph();
                break;

            case 2:
                displayMatrix();
                break;

            case 3:
                selectSource();
                break;

            case 4:
                dijkstra();
                printf("\nShortest distances calculated successfully.\n");
                break;

            case 5:
                displayPaths();
                break;

            case 6:
                displayDistances();
                break;

            case 7:
                printf("\nProgram terminated.\n");
                break;

            default:
                printf("\nInvalid choice!\n");
        }

    } while (choice != 7);

    return 0;
}
