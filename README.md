# Lab04 - Memória

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
