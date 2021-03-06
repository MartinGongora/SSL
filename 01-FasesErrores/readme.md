# TP 1 "Fases de la Traducción y Errores"
_1- Escribir hello2.c, que es una variante de hello.c:_

```
#include <stdio.h>

int/*medio*/main(void){
  int i=42;
  prontf("La respuesta es %d\n");
```
_2- Preprocesar hello2.c, no compilar, y generar hello2.i. Analizar su
contenido:_

comando: gcc -E hello2.c > hello2.i

resultado: Al principio del archivo hello2.i aparece todo el contenido de "stdio.h". Al final de hello2.i aparece el codigo escrito de hello2.c sin el comentario. 

_3- Escribir hello3.c, una nueva variante:_

```
int printf(const char *s, ...);

int main(void){
  int i=42;
  prontf("La respuesta es %d\n");
```
_4- Investigar la semántica de la primera línea:_

La primera linea indica el prototipado de printf, el primer argumento indica el tipo de argumento que recibe, y el "..." indica que la cantidad de argumentos de ese tipo puede variar. 

_5- Preprocesar hello3.c, no compilar, y generar hello3.i. Buscar diferencias
entre hello3.c y hello3.i._

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

_6- Compilar el resultado y generar hello3.s, no ensamblar:_

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
 _7- Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar
hello4.s, no ensamblar:_

comando: gcc -S hello4.c

resultado: 
```
hello4.c: In function ‘main’:
hello4.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
```
Vuelve a darnos la misma advertencia pero igualmente genera el archivo hello.s con codigo assembler.

_8- Investigar hello4.s:_
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

_9- Ensamblar hello4.s en hello4.o, no vincular:_

comando: as -o hello4.o hello4.s

resultado: se crea un archivo hello4.o 

_10- Vincular hello4.o con la biblioteca estándar y generar el ejecutable:_

comando: gcc -o hello4 hello4.o

respuesta:
```
hello4.o:hello4.c:(.text+0x1c): referencia a `prontf' sin definir
collect2.exe: error: ld returned 1 exit status
```
No encuentra la definicion de prontf y no crea el ejecutable. 

_11- Corregir en hello5.c y generar el ejecutable:_

comando: gcc -o hello5 hello5.c 

resultado: se crea el ejecutable. 

_12- Ejecutar y analizar el resultado:_

comando: hello5.exe

resultado: 
```
La respuesta es 3306720
```
Debido a que no recibe el int para mostrar, muestra un int cualquiera. 

_13- Corregir en hello6.c y empezar de nuevo:_

comando: gcc -o hello6 hello6.c

resultado: se crea el ejecutable.

comando: hello6.exe

resultado: 
```
La respuesta es 42
```
Esta vez imprime 42 que es el int guardado en la variable i. 

_14- Escribir hello7.c, una nueva variante:_
```
int main(void){
 int i=42;
   printf("La respuesta es %d\n", i);
}
```
comando: gcc -o hello7 hello7.c

resultado: 
```
hello7.c: In function ‘main’:
hello7.c:3:2: warning: implicit declaration of function ‘printf’ [-Wimplicit-function-declaration]
printf("La respuesta es %d\n",i);
^
hello7.c:3:2: warning: incompatible implicit declaration of built-in function ‘printf’
hello7.c:3:2: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’
```
Se crea el ejecutable pero nos lanza dos advertencias. 

comando: hello7.exe

resultado:
```
La respuesta es 42
```
_15- Explicar porqué funciona._

Las advertencias nos dicen que printf esta declarada implicitamente. Esto funciona ya que C permite hacer declaraciones implicitas, y el linker enlaza por defecto con la biblioteca standar. 




