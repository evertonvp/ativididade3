# ativididade3
#include <stdio.h>
#include <stdlib.h>

int comparar(const void *a, const void *b);

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Uso: %s <nome_do_arquivo>\n", argv[0]);
        return 1;
    }

    FILE *entrada = fopen(argv[1], "r");
    if (entrada == NULL) {
        perror("Erro ao abrir o arquivo de entrada");
        return 1;
    }

    int contador = 0;
    int numero;
    while (fscanf(entrada, "%d", &numero) == 1) {
        contador++;
    }

    rewind(entrada);

    int *numeros = (int *)malloc(contador * sizeof(int));
    if (numeros == NULL) {
        perror("Erro ao alocar memória");
        fclose(entrada);
        return 1;
    }

    for (int i = 0; i < contador; i++) {
        if (fscanf(entrada, "%d", &numeros[i]) != 1) {
            perror("Erro ao ler o número do arquivo");
            fclose(entrada);
            free(numeros);
            return 1;
        }
    }

    fclose(entrada);

    qsort(numeros, contador, sizeof(int), comparar);

    FILE *saida = fopen("saida.txt", "w");
    if (saida == NULL) {
        perror("Erro ao abrir o arquivo de saída");
        free(numeros);
        return 1;
    }

    for (int i = 0; i < contador; i++) {
        fprintf(saida, "%d\n", numeros[i]);
    }

    fclose(saida);

    free(numeros);

    printf("Números ordenados foram salvos em saida.txt.\n");

    return 0;
}

int comparar(const void *a, const void *b) {
    return (*(int *)a - *(int *)b);
}
