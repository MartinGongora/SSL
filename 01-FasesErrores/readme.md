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

5- Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias
entre hello3.c y hello3.i.

comando: gcc -E hello3.c > hello3.c

resultado: 

```
# 1 "hello3.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "hello3.c"
int printf(const char *s, ...);

int main(void){
 int i=42;
 prontf("La respuesta es %d\n");
```
La diferencia son las primeras cuatro lineas, luego son iguales. 

6- Compilar el resultado y generar hello3.s, no ensamblar:

comando: gcc -S hello3.c

resultado:
```
hello3.c: In function ‘main’:
hello3.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
hello3.c:5:2: error: expected declaration or statement at end of input
```
Nos da una advertencia por no estar declarada prontf y un error que indica un problema de sintaxis. Se crea el archivo hello3.c pero con solo esto en su interior:
```
	.file	"hello3.c"
 ```
 7- Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar
hello4.s, no ensamblar:

comando: gcc -S hello4.c

resultado: 
```
hello4.c: In function ‘main’:
hello4.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
```
Vuelve a darnos la misma advertencia pero igualmente genera el archivo hello.s con codigo assembler.

8- Investigar hello4.s:
```
	.file	"hello4.c"
	.def	__main;	.scl	2;	.type	32;	.endef
	.section .rdata,"dr"
.LC0:
	.ascii "La respuesta es %d\12\0"
	.text
	.globl	main
	.def	main;	.scl	2;	.type	32;	.endef
	.seh_proc	main
main:
	pushq	%rbp
	.seh_pushreg	%rbp
	movq	%rsp, %rbp
	.seh_setframe	%rbp, 0
	subq	$48, %rsp
	.seh_stackalloc	48
	.seh_endprologue
	call	__main
	movl	$42, -4(%rbp)
	leaq	.LC0(%rip), %rcx
	call	prontf
	movl	$0, %eax
	addq	$48, %rsp
	popq	%rbp
	ret
	.seh_endproc
	.ident	"GCC: (Rev2, Built by MSYS2 project) 6.2.0"
	.def	prontf;	.scl	2;	.type	32;	.endef
```
Hay un codigo assembler donde se puede observar la llamada a prontf en la linea "call prontf"

9- Ensamblar hello4.s en hello4.o, no vincular:

comando: as -o hello4.o hello4.s

resultado: se crea un archivo hello4.o 

10- Vincular hello4.o con la biblioteca estándar y generar el ejecutable:


