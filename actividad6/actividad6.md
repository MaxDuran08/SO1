# Actividad 6

## Problema 1

### Pregunta:

**¿Incluyendo el proceso inicial, cuántos procesos son creados por el siguiente programa? Razone su respuesta.**


### Respuesta:

En este programa, la función `fork()` es llamada tres veces, y cada vez que se ejecuta, el proceso actual se duplica. Analizando paso por paso:

1. **Primera llamada a `fork()`**: Se crea un proceso hijo, resultando en **2 procesos** (el padre y el hijo).
2. **Segunda llamada a `fork()`**: Ambos procesos (el original y el hijo) ejecutan esta llamada a `fork()`, resultando en **4 procesos**.
3. **Tercera llamada a `fork()`**: Ahora, los 4 procesos existentes ejecutan la tercera llamada a `fork()`, resultando en **8 procesos**.

**Total de procesos**: Incluyendo el proceso original, se crean **8 procesos**.

---

## Problema 2

### Pregunta:

**Escriba un programa en C que cree un proceso hijo (`fork`) que finalmente se convierta en un proceso zombie. Este proceso zombie debe permanecer en el sistema durante al menos 60 segundos. Los estados del proceso se pueden obtener con el comando `ps -l`.**

### Código:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid;

    // Crear un proceso hijo
    pid = fork();

    if (pid < 0) {
        // Si `fork()` falla
        perror("fork failed");
        exit(1);
    }

    if (pid == 0) {
        // Proceso hijo
        printf("Soy el proceso hijo, mi PID es: %d\n", getpid());
        printf("Voy a terminar, pero me convertiré en un proceso zombie...\n");
        exit(0);  // Termina el proceso hijo (se convierte en zombie)
    } else {
        // Proceso padre
        printf("Soy el proceso padre, mi PID es: %d\n", getpid());
        printf("Mi hijo tiene el PID: %d\n", pid);
    
        // Dormir por 60 segundos para permitir que el proceso hijo esté en estado zombie
        sleep(60);
    
        // Eliminar al proceso zombie con wait()
        wait(NULL);
        printf("El proceso zombie ha sido eliminado\n");
    }

    return 0;
}
```

### Explicación:

1. El proceso hijo se convierte en un zombie después de llamar a `exit(0)`, ya que su estado no es recogido por el padre inmediatamente.
2. El proceso padre duerme durante 60 segundos, permitiendo que el hijo permanezca en estado zombie.
3. Después de dormir, el padre llama a `wait(NULL)` para eliminar el proceso zombie.

### Comando para verificar el estado del proceso:

En otra terminal, puedes usar el siguiente comando para verificar el estado del proceso zombie:

```bash
ps -l
```

El estado del proceso zombie se verá como `Z` en la columna `STAT`.

### Compilación:

```bash
gcc -o zombie_process zombie_process.c
./zombie_process
```

---

## Problema 3

### Pregunta:

**Usando el siguiente código como referencia, completar el programa para que sea ejecutable y responder las siguientes preguntas:**

- **¿Cuántos procesos únicos son creados?**
- **¿Cuántos hilos únicos son creados?**


### Código Completo:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

void *thread_function(void *arg) {
    printf("Nuevo hilo creado en el proceso con PID: %d\n", getpid());
    return NULL;
}

int main() {
    pid_t pid;

    // Primera llamada a fork
    pid = fork();

    if (pid < 0) {
        perror("fork failed");
        exit(1);
    }

    if (pid == 0) {
        // Proceso hijo
        printf("Soy el proceso hijo con PID: %d\n", getpid());

        // Segunda llamada a fork dentro del proceso hijo
        fork();

        // Crear un hilo en el proceso hijo
        pthread_t thread_id;
        pthread_create(&thread_id, NULL, thread_function, NULL);

        // Esperar a que el hilo termine
        pthread_join(thread_id, NULL);
    }

    // Tercera llamada a fork desde el proceso original
    fork();

    // El proceso principal duerme un momento
    sleep(1);

    return 0;
}
```

### Respuestas:

#### Número de procesos creados:

1. **Primera llamada a `fork()`**: Se crean 2 procesos (padre e hijo).
2. **Segunda llamada a `fork()`**: Se crea un nuevo proceso hijo solo dentro del primer proceso hijo, resultando en 3 procesos.
3. **Tercera llamada a `fork()`**: Esta llamada se ejecuta en todos los procesos existentes (3), creando 3 procesos adicionales.

**Total de procesos**: Se crean **6 procesos únicos**.

#### Número de hilos creados:

Se crea **1 hilo** dentro del primer proceso hijo después de la segunda llamada a `fork()`.

### Compilación:

```bash
gcc -o fork_thread_example fork_thread_example.c -pthread
./fork_thread_example
```

