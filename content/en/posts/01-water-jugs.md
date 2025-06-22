+++
title = 'Graph theory and the water jugs problem'
date = 2023-08-18T19:35:28-03:00
draft = false
+++

![water-jugs](/water-jugs.png)

Recently, I came across a very interesting puzzle that dates back to ancient times and can be solved using graph theory algorithms, such as breadth-first search, for example. In this post, I describe the approach I took to solve this problem.

## The problem

Imagine that you have two jugs, A and B, at your disposal.

Jug A can hold at most a liters of water.<br>Jug B can hold at most b liters of water.

Initially, both jugs are empty.

A "step" is defined as any of the following operations:<br>
1. Empty one of the jugs;<br>
2. Completely fill one of the jugs;<br>
3. Transfer water from one jug to the other, without wasting water, until either one jug is full or the other is empty.

Problem input:

- a (positive integer representing the capacity of jug A);
- b (positive integer representing the capacity of jug B);
- c (any positive integer).

**The goal is to find the minimum number of steps needed to obtain exactly c liters of water in either of the two jugs, or return -1 if it is not possible.**

## Some useless but curious observations

There are a few implicit facts in the problem. They have no direct impact on the solution, but I thought they were at least worth mentioning.

The first is that we need to assume there is some infinite water source — a river, pool, faucet, waterfall, spout or cascade—from which we can fill the jugs as many times as we want. Similarly, there is also an infinite storage location where we can pour water whenever we want to empty one of the jugs.

Also, notice that we are most likely dealing with jugs with quite irregular shapes and without any volume markings (unlike a beaker, for example), so the only volume measurements we can make are two: when the jug is empty — 0 liters — or when the jug is full—a or b liters. Any other volume measurement different from these two was certainly obtained through sequential executions involving the 3 basic allowed operations: emptying, filling, or transferring water from one jug to the other.

But none of this really matters for solving the problem. So, let’s move on.

## Defining variables

To better handle the problem, it’s convenient to define some variables:<br>
x → amount of water currently in jug A;<br>
y → amount of water currently in jug B;<br>
w → amount of water needed to fill jug A;<br>
z → amount of water needed to fill jug B;<br>

## Some examples

Let’s look at a few examples with the variables x and y, indicating the amount of water (in liters) stored in jugs a and b at any given moment, respectively.

### Example 1

For the inputs a = 3, b = 5, and c = 4, the answer is 6 steps, because, starting from both jugs empty (x = 0, y = 0), we can perform the following sequence of operations:

1. Fill jug B (x=0, y=5);
2. Transfer from B to A (x=3, y=2);
3. Empty jug A (x=0, y=2);
4. Transfer from B to A (x=2, y=0);
5. Fill jug B (x = 2, y = 5);
6. Transfer from B to A (x=3, y=4);

### Example 2

For the inputs a=8, b=56, and c=46, the answer is -1, because it’s impossible to reach a state where one of the jugs contains exactly 46 liters of water.

## Vendo o problema como um grafo

Looking at Example 2, the immediate question that comes up is how we were able to know that, for these input values, there is no sequence of operations that will get us to the desired c liters of water in either jug.

Obviously, there are cases where the answer is immediate. For the inputs a = 3, b = 5, and c = 100, it’s pretty obvious that it’s impossible for either jug A or B to ever contain 100 liters of water. But for non-trivial cases, this answer isn’t always so obvious.

The key here is to visualize the problem as a graph, where the vertices of this graph represent the states of the problem, and the edges represent the possible transitions from one state to another through the allowed jug operations.

In simpler terms, a state is like a snapshot showing the amount of water contained in the two jugs at any given moment. So, we can imagine a state as an ordered pair (x, y) — note that we’re talking about a directed graph — following the variable conventions adopted earlier.

Thus, for a generic state (x, y), we have the following sets of neighbors:

![conjunto-vizinhos](/conjunto-vizinhos.png)

Note: The water transfer cases from one jug to another are omitted here because they require a preliminary check to determine if the transfer will result in one jug becoming empty or the other becoming full. The legend below illustrates what needs to be done in these situations.

![legenda-vizinhos](/legenda-vizinhos.png)

## Solving Using Breadth-First Search (BFS)

The basic idea of the Breadth-First Search algorithm is as follows:

- Start from an initial vertex in the graph, which in our case will be the state (0, 0) where both jugs are empty;
- Visit all neighbors of this initial vertex;
- As neighbors are visited, store them in a queue data structure;
- Once visiting all neighbors is done, dequeue an element q;
- Repeat the neighbor visitation process for this vertex q until we find a state that is (c, y) or (x, c).

The guarantee that the solution found (if it exists) will be the one with the minimum number of steps comes simply from the fact that we are using a queue. This ensures we are performing a level-order traversal, making sure that no state at distance k+1 from the start state (0,0) is visited before all states at distance k are visited. Therefore, at the moment we find a solution, it will certainly be the closest one to the initial state compared to any other existing solutions.

Also, it’s worth mentioning that, if no solution exists, we will end up traversing the entire state space reachable from (0,0) until our queue is empty, at which point we will then return -1.

## Source code

My C implementation of the solution to this problem is shown below. Although the variables a, b, and c in the code are fixed, the implementation is generic and adaptable for any input values less than 4000.

Example output for the values a = 3, b = 5, and c = 4.

![output](/output.png)



```c
#include<stdio.h>
#include <stdlib.h>

int a = 3, b = 5, c = 4;

typedef struct State {
    int x;
    int y;
    int distance;
    struct State *pred;
    struct State *next;
} state;

state *head, *tail;

int visited[4000][4000];
int queue_size;

void *initQueue() {
    head = (state *) malloc(sizeof(state));
    head->next = NULL;
    queue_size = 0;
}

void insert(state *s) {
    if (queue_size == 0) {
        s->next = NULL;
        head = s;
    } else if (queue_size == 1) {
        tail = s;
        head->next = tail;
    } else {
        s->next = NULL;
        tail->next = s;
        tail = s;
    }
    queue_size++;
}

state *dequeue() {
    state *removed;
    if (queue_size == 0) {
        return NULL;
    } else if (queue_size == 1) {
        removed = head;
        head = NULL;
        queue_size--;
        return removed;
    } else {
        removed = head;
        head = head->next;
        queue_size--;
        return removed;
    }
}

state *getNeighbors(state s) {
    state *neighbors = malloc(sizeof(state)*6);

    state n;
    n = s;
    n.distance = s.distance + 1;

    n.x = 0;
    neighbors[0] = n; // Empty jug A

    n = s;
    n.distance = s.distance + 1;
    n.y = 0;
    neighbors[1] = n; // Empty jug B

    n = s;
    n.distance = s.distance + 1;
    n.x = a;
    neighbors[2] = n; // Empty jug A

    n = s;
    n.distance = s.distance + 1;
    n.y = b;
    neighbors[3] = n; // Fill jug B

    n = s;
    n.distance = s.distance + 1;
    if (s.x >= (b-s.y)) {
        n.x = s.x - (b-s.y);
        n.y = b;
    } else {
        n.x = 0;
        n.y = s.y + s.x;
    }
    neighbors[4] = n; // Transfer from A to B

    n = s;
    n.distance = s.distance + 1;
    if (s.y >= (a-s.x)) {
        n.x = a;
        n.y = (s.y-(a-s.x));
    } else {
        n.x = s.x + s.y;
        n.y = 0;
    }
    neighbors[5] = n; // Transfer from B to A

    return neighbors;
}

void printSolution(state s) {
    int size = s.distance;
    state solution[size];
    while(s.x!=0 || s.y!=0) {
        solution[s.distance-1] = s;
        s = *s.pred;
    }
    printf("(0 , 0)\n");
    for(int i=0; i<size; i++) {
        printf("(%d , %d)\n", solution[i].x, solution[i].y);
    }
}

int bfs(state initial) {
    state *n, *current;

    initQueue();
    insert(&initial);

    while(queue_size != 0) {
        current = dequeue();
        visited[current->x][current->y] = 1;

        n = getNeighbors(*current);

        for(int i=0; i<6; i++) {
            if (!visited[n[i].x][n[i].y]) {
                visited[n[i].x][n[i].y] = 1;
                n[i].pred = current;
                insert(&n[i]);
                if (n[i].x==c || n[i].y==c) {
                    printSolution(n[i]);
                    return n[i].distance;
                }
            }
        }
    }
    return -1;
}

int main() {
    state inicio = {0,0,0};
    int answer = bfs(inicio);
    printf("Numero minimo de passos: %d\n", answer);
    return 0;
}

```

