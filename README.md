# Lab04 - Memória

## Grupo:
- Felipe do Nascimento Fonseca - 10409389
- Giovanni Alves Lavia - 10409448

<hr>

### 1. Considerando a estrutura de dados celula, crie três instâncias do objeto célula (três valores na lista).
```c
celula c1, c2, c3;
```

### 2. Construa uma função que imprima todos os valores da lista.
```c
void imprimirLista(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        printf("%d ", atual->conteudo);
        atual = atual->prox;
    }
    printf("\n");
}
```
### Resultado esperado:

![image](https://github.com/Giovannilavia11/Memoria/assets/89709011/cf1794cf-f575-482f-9671-d772755e29fd)

### 3. Calcule a quantidade de memória gasta por cada instância da célula.
**Resposta:** A memória ocupada por uma instância da célula pode variar dependendo da arquitetura do sistema. Normalmente, incluirá o tamanho do conteúdo (neste caso, um inteiro) e o tamanho do ponteiro para a próxima célula. Para uma estimativa, podemos usar sizeof(celula):
```c
size_t tamanhoCelula = sizeof(celula);
```

### 4. Construa uma função que remove os elementos da lista.
```c
void removerElementos(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        celula *temp = atual;
        atual = atual->prox;
        temp->prox = NULL; // Desvincula a célula da lista sem liberar memória
    }
}
```
### Resultado esperado:

![image](https://github.com/Giovannilavia11/Memoria/assets/89709011/07a878a9-79ef-4d92-b9d9-a0ef831548ea)

### 5. Incremente sua função liberando a memória quando um elemento é liberado.
```c
void removerElementosComLiberacaoMemoria(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        celula *temp = atual;
        atual = atual->prox;
        free(temp); // Liberando a memória da célula removida
    }
}
```
### Resultado esperado:

![image](https://github.com/Giovannilavia11/Memoria/assets/89709011/8108e735-09df-41c4-9ef5-a2abed808ea1)

### 6. Calcule o máximo de elementos possíveis na fila, considerando a memória disponível no computador.
**Resposta:** Para calcular o máximo de elementos possíveis na lista encadeada, considerando um computador com 8 GB de memória, precisamos levar em conta o tamanho de cada célula e o espaço ocupado pelo sistema operacional e outros programas em execução. Vamos supor que o sistema operacional e outros programas estejam utilizando cerca de 2 GB de memória, deixando 6 GB disponíveis para a nossa lista encadeada.

Vamos supor também que uma célula na lista encadeada ocupa 16 bytes (um inteiro de 4 bytes e um ponteiro para a próxima célula de 8 bytes em sistemas de 64 bits).

Com essas informações, podemos calcular o número máximo de células que podemos alocar:
```c
#define MEMORIA_DISPONIVEL_BYTES 6 * 1024 * 1024 * 1024 // 6 GB em bytes
#define TAMANHO_CELULA_BYTES 16 // Tamanho de uma célula em bytes

int calcularMaximoElementosPossiveis() {
    int numeroMaximoCelulas = MEMORIA_DISPONIVEL_BYTES / TAMANHO_CELULA_BYTES;
    return numeroMaximoCelulas;
}
```
A função calcularMaximoElementosPossiveis() calculará o número máximo de células que podemos alocar com base na memória disponível e no tamanho de cada célula. Em seguida, podemos adaptar esse número para considerar o tamanho do conteúdo da célula, se necessário, ou fazer outras modificações dependendo dos requisitos específicos do sistema. 

### Resultado esperado:

![image](https://github.com/Giovannilavia11/Memoria/assets/89709011/f5fa05d7-904a-4817-a4bb-b52f16765a01)

<hr>

### Código final do programa:
```c
#include <stdio.h>
#include <stdlib.h>

struct reg {
    int conteudo;
    struct reg *prox;
};
typedef struct reg celula;

// Função para imprimir os valores da lista
void imprimirLista(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        printf("%d ", atual->conteudo);
        atual = atual->prox;
    }
    printf("\n");
}

// Função para remover elementos da lista sem liberar memória
void removerElementos(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        celula *temp = atual;
        atual = atual->prox;
        temp->prox = NULL; // Desvincula a célula da lista sem liberar memória
    }
}

// Função para remover elementos da lista com liberação de memória
void removerElementosComLiberacaoMemoria(celula *inicio) {
    celula *atual = inicio;
    while (atual != NULL) {
        celula *temp = atual;
        atual = atual->prox;
        free(temp); // Liberando a memória da célula removida
    }
}

// Função para calcular o número máximo de elementos possíveis na lista
int calcularMaximoElementosPossiveis() {
    #define MEMORIA_DISPONIVEL_BYTES (6L * 1024 * 1024 * 1024) // 6 GB em bytes
    #define TAMANHO_CELULA_BYTES sizeof(celula) // Tamanho de uma célula em bytes

    return MEMORIA_DISPONIVEL_BYTES / TAMANHO_CELULA_BYTES;
}

int main() {
    // 1. Criando três células
    celula *c1 = (celula *)malloc(sizeof(celula));
    celula *c2 = (celula *)malloc(sizeof(celula));
    celula *c3 = (celula *)malloc(sizeof(celula));

    // Verificar se a memória foi alocada corretamente
    if (c1 == NULL || c2 == NULL || c3 == NULL) {
        printf("Erro ao alocar memória.\n");
        return 1;
    }

    // Preenchendo as células com valores e encadeando
    c1->conteudo = 1;
    c1->prox = c2;
    c2->conteudo = 2;
    c2->prox = c3;
    c3->conteudo = 3;
    c3->prox = NULL;

    // 2. Imprimir os valores da lista
    printf("Lista inicial: ");
    imprimirLista(c1);

    // 4. Remover elementos da lista sem liberar memória
    removerElementos(c1);
    printf("Lista após remover elementos sem liberar memória: ");
    imprimirLista(c1); // Não deverá imprimir nada, pois os nós foram desvinculados

    // Reconstituir a lista para a próxima operação
    c1->prox = c2;
    c2->prox = c3;
    c3->prox = NULL;

    // 5. Remover elementos da lista com liberação de memória
    removerElementosComLiberacaoMemoria(c1);
    printf("Lista após remover elementos com liberação de memória: ");
    // imprimirLista(c1); // Não pode ser chamada pois a memória foi liberada

    // 6. Calcular o número máximo de elementos possíveis na lista
    int maxElementos = calcularMaximoElementosPossiveis();
    printf("Número máximo de elementos possíveis na lista com 6GB disponíveis: %d\n", maxElementos);

    return 0;
}
```

### Print do resultado final do programa:

![image](https://github.com/Giovannilavia11/Memoria/assets/89709011/e8a3f84a-7b1f-4d56-bb3f-ba8dcf5ba910)

