+++
title = 'Teoria dos grafos e o problema dos jarros de água'
date = 2023-08-18T19:35:28-03:00
draft = false
+++

![water-jugs](/water-jugs.png)

Recentemente, me deparei com um puzzle bem interessante que remonta a antiguidade e que pode ser resolvido utilizando algoritmos bem conhecidos da área de teoria dos grafos, como busca em largura, por exemplo. Nesse post descrevo a maneira que tentei resolver esse problema.

## O problema

Imagine que você tenha à sua disposição dois jarros A e B.

O jarro A consegue armazenar, no máximo, a litros de água.<br>O jarro B consegue armazenar, no máximo, b litros de água.

Inicialmente, ambos os jarros estão vazios.

É chamado de 'passo' qualquer uma das operações abaixo:<br>
1. Esvaziar um dos jarros;<br>
2. Encher, por completo, um dos jarros;<br>
3. Transferir água de um jarro para o outro, sem desperdício, até que um dos jarros esteja cheio ou vazio.

Dados de entrada do problema:

- a (inteiro positivo representando a capacidade do jarro A);
- b (inteiro positivo representando a capacidade do jarro B);
- c (inteiro positivo qualquer).

**O objetivo é determinar o número mínimo de passos para se obter exatamente c litros de água em qualquer um dos dois jarros ou responder -1 quando isso não for possível.**

## Algumas observações inúteis, porém curiosas

Existem alguns fatos que estão implícitos no problema, por não terem qualquer influência direta na resolução, mas que achei válido, pelo menos, mencioná-los.

O primeiro deles é que precisamos supor que há alguma fonte de água infinita - um rio, piscina, torneira, cachoeira, bica, igarapé, cacimba ou cascata - por meio da qual nós podemos encher os jarros quantas vezes julgarmos necessário. Analogamente, também existe um local de armazenamento infinito em que podemos despejar água sempre que queremos esvaziar um dos jarros.

Perceba também que, muito provavelmente, estamos lidando com jarros com formatos bastante irregulares e que não possuem nenhuma marcação indicadora de volume(diferente de um béquer, por exemplo), de tal modo que as únicas medições de volume que conseguimos fazer são duas: quando o jarro está vazio - 0 litros - ou quando o jarro está cheio - a ou b litros. Note que qualquer outra indicação de volume diferente dessas duas, certamente foi obtida por meio de execuções sequenciais envolvendo as 3 operações básicas que são permitidas: esvaziar, encher ou transferir de um jarro a outro.

Mas nada disso é importante para resolver o problema. Então, avancemos.

## Definindo variáveis

Para melhor lidar com o problema, é conveniente definirmos algumas variáveis:<br>
x -> quantidade de água existente em A;<br>
y -> quantidade de água existente em B;<br>
w -> quantidade de água que falta para encher A;<br>
z -> quantidade de água que falta para encher B;<br>

## Alguns exemplos

Vejamos alguns exemplos com as variáveis x e y, indicando a quantidade de litros de água armazenada pelos jarros a e b naquele exato instante, respectivamente.

### Exemplo 1

Para as entradas a=3, b=5 e c=4, temos que a resposta é 6 passos, pois, partindo de ambos os jarros vazios (x=0, y=0), podemos executar a seguinte sequência de operações:

1. Encher jarro B (x=0, y=5);
2. Transferir de B para A (x=3, y=2);
3. Esvaziar jarro A (x=0, y=2);
4. Transferir de B para A (x=2, y=0);
5. Encher jarro B (x=2, y=5);
6. Transferir de B para A (x=3, y=4);

### Exemplo 2

Para as entradas a=8, b=56 e c=46, temos que a resposta é -1, pois é impossível se chegar em uma configuração onde um dos copos fique com 46 litros d'água.

## Vendo o problema como um grafo

Ao observarmos o Exemplo 2, a dúvida imediata que surge é em relação a como foi possível saber que, para esses valores de entrada, não existe nenhuma sequência de operações que fará com que eu obtenha os mencionados c litros de água em quaisquer um dos copos.

Obviamente que há casos em que essa resposta é imediata. Para os dados de entrada a=3, b=5 e c=100 é mais do que notório que é impossível quaisquer um dos jarros A e B conter, em algum momento, 100 litros de água. Mas, para casos não-triviais, essa resposta pode não ser tão imediata.

O segredo aqui é visualizar esse problema como um grafo, em que os vértices desse grafo são os estados do problema e as arestas representam as possibilidades de se transitar de um estado para outro através das operações que são permitidas de se fazer com os jarros.

Trocando em miúdos, um estado é como se fosse uma fotografia demonstrativa da quantidade de água contida nos dois jarros d'água em um instante qualquer. Assim, podemos imaginar um estado como sendo um par ordenado (x,y) - observe que estamos falando de um grafo direcionado - obedecendo a convenção de variáveis que foi adotada anteriormente.

Logo, para um estado genérico (x,y) temos os seguintes conjuntos de vizinhos

![conjunto-vizinhos](/conjunto-vizinhos.png)

Perceba que os casos de transferência de água de um jarro para outro estão omitidos, pois esses estados necessitam de uma verificação prévia para sabermos se a transferência em questão resultará em um dos jarros vazios ou em dos jarros cheios. A legenda abaixo ilustra o que deve ser feito nessas situações

![legenda-vizinhos](/legenda-vizinhos.png)

## Resolvendo através da busca em largura

A ideia básica do algoritmo de busca em largura é a seguinte:

- Partir de um vértice inicial do grafo que, no nosso caso, será o estado (0,0) em que ambos os jarros estão vazios;
- Visitar todos os vizinhos desse vértice inicial;
- À medida que os vizinhos são visitados, armazená-los em uma estrutura de dados do tipo fila.
- Quando a visita dos vizinhos for concluída, desenfileirar um elemento q da fila;
- Repetir o processo de visita dos vizinhos para esse vértice q até que algum estado seja (c,y) ou (x,c).

A garantia de que a solução a ser encontrada (caso ela exista) será a com o número mínimo de passos possível, se dá pelo simples fato de estarmos usando uma fila. Dessa forma, estamos fazendo uma visita organizada por níveis, de tal forma que nenhum estado que se encontra a uma distância k+1 do estado (0,0) será visitado antes de todos os estados que se encontram a uma distância k serem visitados. Logo, no momento em que encontrarmos uma solução, ela certamente será a que está mais próxima do estado inicial em comparação às outras eventuais soluções existentes.

Além disso, é válido ser dito que, caso a solução não exista, iremos percorrer todo o espaço de estados alcançáveis por (0,0), até que nossa fila esteja vazia para, então, respondermos -1 ao final.

## Código-fonte

Minha implementação em C, da resolução desse problema, segue abaixo. Apesar das variáveis a,b e c no código estarem fixadas, a implementação foi genérica e adaptável para quaisquer valores de entrada inferiores a 4000.

Exemplo da saída para os valores a = 3, b = 5 e c = 4.

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
    neighbors[0] = n; // Esvaziar vaso A

    n = s;
    n.distance = s.distance + 1;
    n.y = 0;
    neighbors[1] = n; // Esvaziar vaso B

    n = s;
    n.distance = s.distance + 1;
    n.x = a;
    neighbors[2] = n; // Encher vaso A

    n = s;
    n.distance = s.distance + 1;
    n.y = b;
    neighbors[3] = n; //Encher vaso B

    n = s;
    n.distance = s.distance + 1;
    if (s.x >= (b-s.y)) {
        n.x = s.x - (b-s.y);
        n.y = b;
    } else {
        n.x = 0;
        n.y = s.y + s.x;
    }
    neighbors[4] = n; // Transferir de A para B

    n = s;
    n.distance = s.distance + 1;
    if (s.y >= (a-s.x)) {
        n.x = a;
        n.y = (s.y-(a-s.x));
    } else {
        n.x = s.x + s.y;
        n.y = 0;
    }
    neighbors[5] = n; // Trasnferir de B para A

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

