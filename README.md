# MAX-FLOW-FACTORY-TO-VILLAGE
#include <stdio.h>

#define A 0
#define B 1
#define C 2
#define MAX_NODES 1000
#define O 1000000000

int n;
int e;
int capacity[MAX_NODES][MAX_NODES];
int flow[MAX_NODES][MAX_NODES];
int color[MAX_NODES];
int pred[MAX_NODES];

int min(int x, int y)
{
  return x < y ? x : y;
}

int head, tail;
int q[MAX_NODES + 2];

void enqueue(int x)
{
  q[tail] = x;
  tail++;
  color[x] = B;
}

int dequeue()
{
  int x = q[head];
  head++;
  color[x] = C;
  return x;
}

int bfs(int start, int target)
{
  int u, v;
  for (u = 0; u < n; u++)
  {
    color[u] = A;
  }
  head = tail = 0;
  enqueue(start);
  pred[start] = -1;
  while (head != tail)
  {
    u = dequeue();
    for (v = 0; v < n; v++)
    {
      if (color[v] == A && capacity[u][v] - flow[u][v] > 0)
      {
        enqueue(v);
        pred[v] = u;
      }
    }
  }
  return color[target] == C;
}

int max_flow(int source, int sink)
{
  int i, j, u;
  int max_flow = 0;
  for (i = 0; i < n; i++)
  {
    for (j = 0; j < n; j++)
    {
      flow[i][j] = 0;
    }
  }

  while (bfs(source, sink))
  {
    int increment = O;
    for (u = n - 1; pred[u] >= 0; u = pred[u])
    {
      increment = min(increment, capacity[pred[u]][u] - flow[pred[u]][u]);
    }
    for (u = n - 1; pred[u] >= 0; u = pred[u])
    {
      flow[pred[u]][u] += increment;
      flow[u][pred[u]] -= increment;
    }
    max_flow += increment;
  }
  return max_flow;
}

int main()
{

  for (int i = 0; i < n; i++)
  {
    for (int j = 0; j < n; j++)
    {
      capacity[i][j] = 0;
    }
  }
  int z;
  n = 6;
  e = 7;
  int s = 0;
  int t;
  int x;
  printf("Factory Node Index=%d\n",s);
  printf("Enter Village Node Index:");
  scanf("%d",&t);
  printf("Enter Village Demand:");
  scanf("%d",&x);

  for(int i=0;i<=t;i++){
    for(int j=0;j<=t;j++){
        printf("Enter Path Value at node [%d][%d]",i,j);
        capacity[i][j]=scanf("%d \n",&z);
    }
  }



  printf("Max Flow: %d\n", max_flow(s, t));
  int l=max_flow(s,t);
  if (l>=x){
    printf("Village Demand can be met in 1 cycle");
  }
  else{
    printf("Village Demand can't be met in 1 cycle");

  }
}
