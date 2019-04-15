# TP 1
1- Escribir hello2.c, que es una variante de hello.c:

```
#include <stdio.h>

int/*medio*/main(void){
  int i=42;
  prontf("La respuesta es %d\n");
```
2- Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su
contenido:

comando: gcc -E hello2.c > hello2.i

resultado: Al principio del archivo hello2.i aparece todo el contenido de "stdio.h". Al final de hello2.i aparece el codigo escrito de hello2.c sin el comentario. 

3- Escribir hello3.c, una nueva variante:

```
int printf(const char *s, ...);

int main(void){
  int i=42;
  prontf("La respuesta es %d\n");
```
4- Investigar la semántica de la primera línea:


