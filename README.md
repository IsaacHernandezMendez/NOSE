#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int proceso;
    int tiempo_llegada;
    int rafaga;
    int prioridad;
    int tiempo_espera;
    int tiempo_retorno;
} Proceso;

void calcularPrioridad(Proceso p[], int n) {
    int tiempo_actual = 0;
    int completado = 0;
    int minimo, i;

    while (completado < n) {
        minimo = -1;

        for (i = 0; i < n; i++) {
            if (p[i].tiempo_llegada <= tiempo_actual && p[i].tiempo_espera == -1) {
                if (minimo == -1 || p[i].prioridad < p[minimo].prioridad) {
                    minimo = i;
                }
            }
        }

        if (minimo != -1) {
            p[minimo].tiempo_espera = tiempo_actual - p[minimo].tiempo_llegada;
            tiempo_actual += p[minimo].rafaga;
            p[minimo].tiempo_retorno = tiempo_actual - p[minimo].tiempo_llegada;
            completado++;
        } else {
            tiempo_actual++;
        }
    }
}

void mostrarResultados(Proceso p[], int n) {
    int suma_espera = 0, suma_retorno = 0;

    printf("\nProceso\tT.Llegada\tR%cfaga\tPrioridad\tT.Espera\tT.Retorno\n", 160);
    for (int i = 0; i < n; i++) {
        printf("%d\t%d\t\t%d\t%d\t\t%d\t\t%d\n", p[i].proceso, p[i].tiempo_llegada,
               p[i].rafaga, p[i].prioridad, p[i].tiempo_espera, p[i].tiempo_retorno);
        suma_espera += p[i].tiempo_espera;
        suma_retorno += p[i].tiempo_retorno;
    }
    printf("\nTiempo de espera promedio: %.2f", (float)suma_espera / n);
    printf("\nTiempo de retorno promedio: %.2f\n", (float)suma_retorno / n);
}

int main() {
    int n;

    printf("Ingrese el n%cmero de procesos: ", 163);
    scanf("%d", &n);

    Proceso procesos[n];

    for (int i = 0; i < n; i++) {
        printf("Proceso %d:\n", i + 1);
        procesos[i].proceso = i + 1;
        printf("  Tiempo de llegada: ");
        scanf("%d", &procesos[i].tiempo_llegada);
        printf("  R%cfaga de CPU: ", 160);
        scanf("%d", &procesos[i].rafaga);
        printf("  Prioridad: ");
        scanf("%d", &procesos[i].prioridad);
        procesos[i].tiempo_espera = -1;
    }

    calcularPrioridad(procesos, n);

    printf("\n--- Resultados del algoritmo de Prioridad ---\n");
    mostrarResultados(procesos, n);

    return 0;
}

