# EInterfaz
Resumen del libro de practicas
Capitulo 2
Tipos de datos y sentencias de alto nivel
2.1.1. Modos de direccionamiento del ARM
En la arquitectura ARM los accesos a memoria se hacen mediante instrucciones
específicas ldr y str (luego veremos las variantes ldm, stm y las preprocesadas push
y pop). El resto de instrucciones toman operandos desde registros o valores inmediatos, sin excepciones. 
# Direccionamiento inmediato. 
El operando fuente es una constante, formando parte de la instrucción.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/f110e9c9-0d2f-48fe-8b82-89e1e9a025cc)

# Direccionamiento inmediato con desplazamiento o rotación.
Es una variante del anterior en la cual se permiten operaciones intermedias sobre los registros.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/4c412fc6-eada-402a-a90f-47de3ba1e5f8)


# Direccionamiento a memoria, sin actualizar registro puntero. 
Es la forma más sencilla y admite 4 variantes. Después del acceso a memoria ningún registro implicado en el cálculo de la dirección se modifica.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/d32f004d-6d4f-4893-af61-67d351f06061)


Simplemente añade (o sustrae) un valor inmediato al registro dado para
calcular la dirección. Es muy útil para acceder a elementos fijos de un
array, ya que el desplazamiento es constante. Por ejemplo si tenemos r1
apuntando a un array de enteros de 32 bits int a[] y queremos poner
a 1 el elemento a[3], lo hacemos así:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/98eafec3-35e2-41a8-98f4-e10317f92290)


# Direccionamiento a memoria, actualizando registro puntero.
En este modo de direccionamiento, el registro que genera la dirección se actualiza con la propia dirección. De esta forma podemos recorrer un array con un sólo registro
sin necesidad de hacer el incremento del puntero en una instrucción aparte.
Hay dos métodos de actualizar dicho registro, antes de ejecutar la instrucción
(preindexado) o después de la misma (postindexado). Los tres siguientes tipos
son los postindexados.



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/4a344eaa-f36e-4846-9510-e78ba1aa4972)


Una notación muy parecida a la versión que no actualiza registro, la
única diferencia es que la constante de desplazamiento queda fuera de
los corchetes. Presenta el mismo límite de hasta 4095. Este ejemplo pone
a cero los 3 primeros elementos a[0], a[1], a[2] del array:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/fd49a632-be1b-407c-9eed-871b578772c0)


# 2.1.2. Tipos de datos
Tipos de datos básicos. En la siguiente tabla se recogen los diferentes tipos de datos básicos que podrán aparecer en los ejemplos, así como su tamaño y rango de representación.



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/23ff85a8-cd72-42c0-9695-fec5511867b4)

Vectores. Todos los elementos de un vector se almacenan en un único bloque de
memoria a partir de una dirección determinada. Los diferentes elementos se almacenan en posiciones consecutivas, de manera que el elemento i está entre los i-1 e
i+1 

Matrices bidimensionales. Una matriz bidimensional de N×M elementos se almacena en un único bloque de memoria. Interpretaremos una matriz de N×M como
una matriz con N filas de M elementos cada una. 

# 2.1.3. Instrucciones de salto
Las instrucciones de salto pueden producir saltos incondicionales (b y bx) o
saltos condicionales. Cuando saltamos a una etiqueta empleamos b, mientras que
si queremos saltar a un registro lo hacemos con bx. La variante de registro bx la
solemos usar como instrucción de retorno de subrutina, raramente tiene otros usos.
En los saltos condicionales añadimos dos o tres letras a la (b/bx), mediante las
cuales condicionamos si se salta o no dependiendo del estado de los flags.

# 2.1.4. Estructuras de control de alto nivel
En este punto veremos cómo se traducen a ensamblador las estructuras de control
de alto nivel que definen un bucle (for, while, . . . ), así como las condicionales
(if-else).
Las estructuras for y while se pueden ejecutar un mínimo de 0 iteraciones (si
la primera vez no se cumple la condición). La traducción de las estructuras for y
while se puede ver en los listados 2.1 y 2.2.
Para programar en ensamblador estas estructuras se utilizan instrucciones de
salto condicional. Previo a la instrucción de salto es necesario evaluar la condición
del bucle o de la sentencia if, mediante instrucciones aritméticas o lógicas, con el
fin de actualizar los flags de estado.

Listado 2.1: Estructura del for y while en C (tipos1.c)


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/584fe0da-166c-4ae3-b84c-3afa177e0308)


Listado 2.2: Traducción de las estructuras for y while. Hemos supuesto que el valor
inicial está en la variable vi y el valor final en la variable vf y se ha utilizado el
registro r1 como índice de las iteraciones i.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/cfb50ebe-eee6-485c-ac2c-739c9f184dd4)


# 2.1.5. Compilación a ensamblador
Para acabar la teoría veamos cómo trabaja un compilador de C real. Normalmente los compiladores crean código compilado (archivos .o) en un único paso. En
el caso de gcc este proceso se hace en dos fases: en una primera se pasa de C a
ensamblador, y en una segunda de ensambladador a código compilado (código máquina). Lo interesante es que podemos interrumpir justo después de la compilación
y ver con un editor el aspecto que tiene el código ensamblador generado a partir del
código fuente en C.
Veámoslo con un ejemplo.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/e939bfd0-e5ec-41aa-855d-92f807767901)


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/ed0fc91d-a1e2-488f-8da1-30f8ab75e80c)

Después de crear el fichero tipos3.s, lo compilamos con este comando.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/3582c05a-be20-4ec9-97a6-3afe77c2e2ad)

Con el parámetro -S forzamos la generación del .s en lugar del .o y con -Os le
indicamos al compilador que queremos optimizar en tamaño, es decir que queremos
código ensamblador lo más pequeño posible, sin importar el rendimiento del mismo.
El código ensamblador resultante está un poco sucio, lleno de directivas superfluas, con punteros a variables e instrucciones no simplificadas por el preprocesador.
Tras limpiarlo quedaría así.
Listado 2.6: Código del programa tipos3a.s


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/1292a917-a576-4359-94f2-a2e6c513e15a)

El carácter \n se ha transformado en octal \012 puesto que el ensamblador no
entiende de secuencias de escape. La instrucciones push y pop son la versión simple
de stmfd y ldmfd que veremos más adelante. Nótese que la función no acaba con el
típico bx lr. Se trata de una optimización que consigue reducir de dos instrucciones
a una. Es decir, estas dos instrucciones:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/d631d99d-da14-451b-8079-e47e09c7e7ba)

Se simplifican a:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/1e757de9-74a9-450f-a1f2-808c647c4244)

# Capitulo 3
Subrutinas y paso de parámetros
3.1. Lectura previa
3.1.1. La pila y las instrucciones ldm y stm
Se denomina pila de programa a aquella zona de memoria, organizada de forma
LIFO (Last In, First Out), que el programa emplea principalmente para el almacenamiento temporal de datos. Esta pila, definida en memoria, es fundamental para
el funcionamiento de las rutinas1 , aspecto que se desarrollará en esta práctica.
El puntero de pila es r13 aunque por convención nunca se emplea esa nomeclatura, sino que lo llamamos sp (stack pointer o puntero de pila). Dicho registro
apunta siempre a la palabra de memoria que corresponde a la cima de la pila (última
palabra introducida en ella).
De esta forma, la instrucción push realmente implementa las dos siguientes instrucciones:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/d2bb57f4-7580-4851-9986-c57741cbf7c2)


Para sacar elementos de la pila tenemos la operación pop, que primero extrae el
elemento de la pila y luego incrementa el puntero (la pila decrece hacia arriba). Por
tanto, la instrucción pop es equivalente a:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/013aa4f0-8214-4cc0-8c7e-70909b2b7a26)


A continuación explicamos cada uno de los argumentos de ldm/stm
1. modo_direc
ia: Incrementa dirección después (increment after) de cada transferencia.
Es el modo por defecto en caso de omitirlo.
ib: Incrementa dirección antes (increment before) de cada transferencia.
da: Decrementa después (decrement after) de cada transferencia.
db: Decrementa antes (decrement before) de cada transferencia.
2. cond. Es opcional, son las mismas condiciones de los flags que vimos en la
sección 2.1.3 del capítulo anterior (página 38), que permiten ejecutar o no
dicha instrucción.
3. rn. Es el registro base, el cual apunta a la dirección inicial de memoria donde se
hará la transferencia. El registro más común es sp (r13), pero puede emplearse
cualquier otro.
4. !. Es un sufijo opcional. Si está presente, actualizamos rn con la dirección
calculada al final de la operación.
5. lista_reg. Es una lista de uno o más registros, que serán leídos o escritos en
memoria. La lista va encerrada entre llaves y separada por comas. También
podemos usar un rango de registros. En este ejemplo se almacenan los registros
r3, r4, r5, r6, r10 y r12. Si inicialmente r1 contiene el valor 24, después
de ejecutar la instrucción siguiente r3 se almacenará en la dirección 20, r4 en
16, r5 en 12, r6 en 8, r10 en 4 y r12 en 0.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/8c5e20b2-fc2f-4f16-906e-f6a5f441dc65)

Si tenemos en cuenta que push predecrementa, que pop postincrementa y que
ambas actualizan el registro base (que sería sp), la traducción de las pseudoinstrucciones push {r4, r6} y pop {r4, r6} serían respectivamente


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/96fc31e6-f981-4440-bcf0-e480a26ec3d3)


# 3.1.2. Convención AAPCS
Podemos seguir nuestras propias reglas, pero si queremos interactuar con las
librerías del sistema, tanto para llamar a funciones como para crear nuestras propias
funciones y que éstas sean invocadas desde un lenguaje de alto nivel, tenemos que
seguir una serie de pautas, lo que se denominamos AAPCS (Procedure Call Standard
for the ARM Architecture).
1. Podemos usar hasta cuatro registros (desde r0 hasta r3) para pasar parámetros y hasta dos (r0 y r1) para devolver el resultado.
2. No estamos obligados a usarlos todos, si por ejemplo la función sólo usa dos
parámetros de tipo int con r0 y r1 nos basta. Lo mismo pasa con el resultado,
podemos no devolver nada (tipo void), devolver sólo r0 (tipo int ó un puntero
a una estructura más compleja), o bien devolver r1:r0 cuando necesitemos
enteros de 64 bits (tipo long long).
3. Los valores están alineados a 32 bits (tamaño de un registro), salvo en el caso
de que algún parámetro sea más grande, en cuyo caso alinearemos a 64 bits.
Un ejemplo de esto lo hemos visto en el Ejercicio 2.5, donde necesitábamos
pasar dos parámetros: una cadena (puntero de 32 bits) y un entero tipo long
long. El puntero a cadena lo almacenábamos en r0 y el entero de 64 bits debe
empezar en un registro par (r1 no vale) para que esté alineado a 64 bits, serían
los registros r2 y r3. En estos casos se emplea little endian, la parte menos
significativa sería r2 y la de mayor peso, por tanto, r3.
4. El resto de parámetros se pasan por pila. En la pila se aplican las mismas
reglas de alineamiento que en los registros. La unidad mínima son 32 bits,
por ejemplo si queremos pasar un char por valor, extendemos de byte a word
rellenando con ceros los 3 bytes más significativos. Lo mismo ocurre con los
enteros de 64 bits, pero en el momento en que haya un sólo parámetro de este
tipo, todos los demás se alinean a 64 bits.
5. Es muy importante preservar el resto de registros (de r4 en adelante incluyendo lr). La única excepción es el registro r12 que podemos cambiar a nuestro
antojo. Normalmente se emplea la pila para almacenarlos al comienzo de la
función y restaurarlos a la salida de ésta. Puedes usar como registros temporales (no necesitan ser preservados) los registros desde r0 hasta r3 que no se
hayan empleado para pasar parámetros.
6. La pila debe estar alineada a 8 bytes, esto quiere decir que de usarla para preservar registros, debemos reservar un número par de ellos. Si sólo necesitamos
preservar un número impar de ellos, añadimos un registro más a la lista dentro
del push, aunque no necesite ser preservado.
7. Aparte de para pasar parámetros y preservar registros, también podemos usar
la pila para almacenar variables locales, siempre y cuando cumplamos la regla
de alinear a 8 bytes y equilibremos la pila antes de salir de la función.

# 3.2.3. Funciones recursivas
El siguiente paso es implementar una función recursiva en ensamblador. Vamos
a escoger la secuencia de Fibonacci por su sencillez. Trataremos de imprimir los diez
primeros números de la secuencia. Se trata de una sucesión de números naturales en
las que los dos primeros elementos valen uno y los siguientes se calculan sumando
los dos elementos anteriores. Los diez primeros números serían los siguientes.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/01fd79ba-3d4a-4c39-b66b-30c9fd07c77a)

Este es el código en un lenguaje de alto nivel como C que imprime la anterior
secuencia

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/775b801c-1140-41d0-bd31-87fa055d2c75)


Lo que vamos a explicar ahora es cómo crear variables locales dentro de una
función. Aunque en C no necesitemos variables locales para la función fibonacci,
sí nos hará falta en ensamblador, en concreto dos variables: una para acumular la
suma y otra para mantener el parámetro de entrada.
Para ello vamos a emplear la pila, que hasta ahora sólo la dedicábamos para
salvaguardar los registros a partir de r4 en la función. La pila tendría un tercer uso
que no hemos visto todavía. Sirve para que el llamador pase el resto de parámetros
en caso de que haya más de 4. Los primeros 4 parámetros (dos en caso de parámetros
de 64 bits) se pasan por los registros desde r0 hasta r3. A partir de aquí si hay más
parámetros éstos se pasan por pila.
Las variables locales se alojan debajo del área de salvaguarda de registros, para
ello hay que hacer espacio decrementando el puntero de pila una cierta cantidad de bytes, e incrementando sp en esa misma cantidad justo antes de salir de la función.
En la figura 3.1 vemos el uso de la pila de una función genérica.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/2ddc146c-7b4a-44da-8989-f01f7322ce91)

# 3.2.4. Funciones con muchos parámetros de entrada
Lo último que nos falta por ver es cómo acceder a los parámetros de una función
por pila, para lo cual necesitamos una función de al menos cinco parámetros. Lo
más sencillo que se nos ocurre es un algoritmo que evalue cualquier polinomio de
grado 3 en el dominio de los enteros.
f(x) = ax3 + bx2 + cx + d (3.1)
Nuestra función tendría 5 entradas, una para cada coeficiente, más el valor de la
x que sería el quinto parámetro que pasamos por pila. Como siempre, comenzamos
escribiendo el código en C:
Listado 3.11: Evaluador de polinomios subrut5.c


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/eedb727e-2dc7-4d33-ae69-08c736930c88)


Cuya salida es la siguiente.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/f38cf9fd-23a1-4e91-8d4c-cb9fb21e88a7)


El código completo en ensamblador se muestra en el listado 3.12.
Listado 3.12: Evaluador de polinomios subrut5.s

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/7e362dc4-c5c5-4ed2-90c4-7a80ce7eb200)


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/1612d65f-174a-4267-bf92-7d9ca77af440)

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/98453330-f6f9-4061-93f1-27f0d35da8ce)


# 3.2.5. Pasos detallados de llamadas a funciones
Como ya hemos visto todos los casos posibles, hacemos un resumen de todo en
una serie de puntos desde que pasamos los parámetros en el llamador hasta que
restauramos la pila desde el llamador, pasando por la llamada a la función y la
ejecución de la misma

1. Usando los registros r0-r3 como almacén temporal, el llamador pasa por pila
los parámetros quinto, sexto, etc... hasta el último. Cuidado con el orden,
especialmente si se emplea un push múltiple. Este paso es opcional y sólo
necesario si nuestra función tiene más de 4 parámetros.
2. El llamador escribe los primeros 4 parámetros en r0-r3. Este paso es opcional,
ya que nos lo podemos saltar si nuestra función no tiene parámetros.
3. El llamador invoca a la función con bl. Este paso es obligatorio.
4. Ya dentro de la función, lo primero que hace esta es salvaguardar los registros
desde r4 que se empleen más adelante como registros temporales. En caso de
no necesitar ningún registro temporal nos podemos saltar este paso.
5. Decrementar la pila para hacer hueco a las variables locales. La suma de bytes
entre paso de parámetros por pila, salvaguarda y variables locales debe ser
múltiplo de 8, rellenar aquí hasta completar. Como este paso es opcional, en
caso de no hacerlo aquí el alineamiento se debe hacer en el paso 4.
6. La función realiza las operaciones que necesite para completar su objetivo,
accediendo a parámetros y variables locales mediante constantes .equ para
aportar mayor claridad al código. Se devuelve el valor resultado en r0 (ó en
r1:r0 si es doble palabra).
7. Incrementar la pila para revertir el alojamiento de variables locales.
8. Recuperar con pop la lista de registros salvaguardados.
9. Retornar la función con bx lr volviendo al código llamador, exactamente a la
instrucción que hay tras el bl.
10. El llamador equilibra la pila en caso de haber pasado parámetros por ella.

# Capitulo 4
E/S a bajo nivel

4.1. Lectura previa
# 4.1.1. Librerías y Kernel, las dos capas que queremos saltarnos
Anteriormente hemos utilizado funciones específicas para comunicarnos con los
periféricos. Si por ejemplo necesitamos escribir en pantalla, llamamos a la función
printf. Pues bien, entre la llamada a la función y lo que vemos en pantalla hay 2
capas software de por medio.
Una primera capa se encuentra en la librería runtime que acompaña al ejecutable,
la cual incluye sólamente el fragmento de código de la función que necesitemos, en
este caso en printf. El resto de funciones de la librería (stdio), si no las invocamos
no aparecen en el ejecutable. El enlazador se encarga de todo esto, tanto de ubicar
las funciones que llamemos desde ensamblador, como de poner la dirección numérica
correcta que corresponda en la instrucción bl printf.
Este fragmento de código perteneciente a la primera capa sí que podemos depurarlo mediante gdb. Lo que hace es, a parte del formateo que realiza la propia función,
trasladar al sistema operativo una determinada cadena para que éste lo muestre por
pantalla. Es una especie de traductor intermedio que nos facilita las cosas. Nosotros
desde ensamblador también podemos hacer llamadas al sistema directamente como
veremos posteriormente.
La segunda capa va desde que hacemos la llamada al sistema (System Call o
Syscall) hasta que se produce la transferencia de datos al periférico, retornando
desde la llamada al sistema y volviendo a la primera capa, que a su vez retornará el
control a la llamada a librería que hicimos en nuestro programa inicialmente.
En esta segunda capa se ejecuta código del kernel, el cual no podemos depurar.
Además el procesador entra en un modo privilegiado, ya que en modo usuario (el
que se ejecuta en nuestro programa ensamblador y dentro de la librería) no tenemos privilegios suficientes como para acceder a la zona de memoria que mapea los
periféricos.
La función printf es una función de la librería del lenguaje C. Como vemos en
la figura, esta función internamente llama a la System Call (rutina del Kernel del
SO) write que es la que se ejecuta en modo supervisor y termina accediendo a los periféricos (en este caso al terminal o pantalla donde aparece el mensaje). En la
figura 4.1 podemos ver el código llamador junto con las dos capas.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/77844c1a-119f-44ca-8b4f-4b47fade643a)


Ahora veremos un ejemplo en el cual nos saltamos la capa intermedia para comunicarnos directamente con el kernel vía llamada al sistema. En este ejemplo vamos
a escribir una simple cadena por pantalla, en concreto "Hola Mundo!".
Listado 4.1: esbn1.s

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/9cf168f8-5faa-4023-b7cf-5f5b17ca4eeb)


# 4.1.2. Ejecutar código en Bare Metal
El ciclo de ensamblado y enlazado es distinto en un programa Bare Metal. Hasta
ahora hemos creado ejecutables, que tienen una estructura más compleja, con cabecera y distintas secciones en formato ELF [8]. Toda esta información le viene muy
bien al sistema operativo, pero en un entorno Bare Metal no disponemos de él. Lo
que se carga en kernel.img es un binario sencillo, sin cabecera, que contiene directamente el código máquina de nuestro programa y que se cargará en la dirección de
RAM 0x8000.
Lo que para un ejecutable hacíamos con esta secuencia

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/e9d6f450-c793-4276-b87b-174f5f4241a0)

En caso de un programa Bare Metal tenemos que cambiarla por esta otra


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/17d52271-1db7-40ac-a709-a20e91111c81)


Otra característica de Bare Metal es que sólo tenemos una sección de código
(la sección .text), y no estamos obligados a crear la función main. Al no ejecutar
ninguna función no tenemos la posibilidad de salir del programa con bx lr, al fin y
al cabo no hay ningún sistema operativo detrás al que regresar. Nuestro programa
debe trabajar en bucle cerrado. En caso de tener una tarea simple que queramos
terminar, es preferible dejar el sistema colgado con un bucle infinito como última
instrucción.
El proceso de arranque de la Raspberry Pi es el siguiente:
Cuando la encendemos, el núcleo ARM está desactivado. Lo primero que se
activa es el núcleo GPU, que es un procesador totalmente distinto e independiente al ARM. En este momento la SDRAM está desactivada.
El procesador GPU empieza a ejecutar la primera etapa del bootloader (son
3 etapas), que está almacenada en ROM dentro del mismo chip que comparten ARM y GPU. Esta primera etapa accede a la tarjeta SD y lee el fichero
bootcode.bin en caché L2 y lo ejecuta, siendo el código de bootcode.bin la
segunda etapa del bootloader.
En la segunda etapa se activa la SDRAM y se carga la tercera parte del bootloader, cuyo código está repartido entre loader.bin (opcional) y start.elf.
En tercera y última etapa del bootloader se accede opcionalmente a dos archivos ASCII de configuración llamados config.txt y cmdline.txt. Lo más
relevante de esta etapa es que cargamos en RAM (en concreto en la dirección
0x8000) el archivo kernel.img con código ARM, para luego ejecutarlo y acabar con el bootloader, pasando el control desde la GPU hacia la CPU. Este
último archivo es el que nos interesa modificar para nuestros propósitos, ya
que es lo primero que la CPU ejecuta y lo hace en modo privilegiado, es decir,
con acceso total al hardware.


El proceso completo que debemos repetir cada vez que desarrollemos un programa nuevo en Bare Metal es el siguiente:
Apagamos la Raspberry.
Extraemos la tarjeta SD.
Introducimos la SD en el lector de nuestro ordenador de desarrollo.
Montamos la unidad y copiamos (sobreescribimos) el kernel.img que acabamos
de desarrollar.
Desmontamos y extraemos la SD.
Insertamos de nuevo la SD en la Raspberry y la encendemos.

# 4.2. Acceso a periféricos
Los periféricos se controlan leyendo y escribiendo datos a los registros asociados
o puertos de E/S. No confundir estos registros con los registros de la CPU. Un
puerto asociado a un periférico es un ente, normalmente del mismo tamaño que el
ancho del bus de datos, que sirve para configurar diferentes aspectos del mismo. No
se trata de RAM, por lo que no se garantiza que al leer de un puerto obtengamos
el último valor que escribimos. Es más, incluso hay puertos que sólo admiten ser
leídos y otros que sólo admiten escrituras. La funcionalidad de los puertos también
es muy variable, incluso dentro de un mismo puerto los diferentes bits del mismo
tienen distinto comportamiento.
Como cada periférico se controla de una forma diferente, no hay más remedio
que leerse el datasheet del mismo si queremos trabajar con él. De ahora en adelante
usaremos una placa auxiliar, descrita en el apéndice B, y que conectaremos a la fila
inferior del conector GPIO según la figura 4.2. En esta sección explicaremos cómo
encender un LED de esta placa auxiliar.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/273cf8ea-8e98-4c81-8f64-7f4359cfdceb)

# 4.2.1. GPIO (General-Purpose Input/Output)
El GPIO es un conjunto de señales mediante las cuales la CPU se comunica con
distintas partes de la Rasberry tanto internamente (audio analógico, tarjeta SD o
LEDs internos) como externamente a través de los conectores P1 y P5. Como la
mayor parte de las señales se encuentran en el conector P1 (ver figura 4.3), normalmente este conector se denomina GPIO. Nosotros no vamos a trabajar con señales
GPIO que no pertenezcan a dicho conector, por lo que no habrá confusiones.
El GPIO contiene en total 54 señales, de las cuales 17 están disponibles a través
del conector GPIO (26 en los modelos A+/B+). Como nuestra placa auxiliar emplea
la fila inferior del conector, sólo dispondremos de 9 señales.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/e939da15-d4cb-4e44-88ba-163b2db0cadc)


GPFSELn
Las 54 señales/pines las separamos en 6 grupos funcionales de 10 señales/pines
cada uno (excepto el último que es de 4) para programarlas mediante GPFSELn.
El LED que queremos controlar se corresponde con la señal número 9 del puerto
GPIO. Se nombran con GPIO más el número correspondiente, en nuestro caso sería
GPIO 9. Nótese que la numeración empieza en 0, desde GPIO 0 hasta GPIO 53.
Así que la funcionalidad desde GPIO 0 hasta GPIO 9 se controla con GPFSEL0,
desde GPIO 10 hasta GPIO 19 se hace con GPFSEL1 y así sucesivamente. 


GPSETn y GPCLRn
Los 54 pines se reparten entre dos puertos GPSET0/GPCLR0, que contienen los
32 primeros, y en GPSET1/GPCLR1 están los 22 restantes, quedando libres los 10
bits más significativos de GPSET1/GPCLR1.
Una vez configurado GPIO 9 como salida, ya sólo queda saber cómo poner un cero o un uno en la señal GPIO 9, para apagar y encender el primer LED de la placa
auxiliar respectivamente (un cero apaga y un uno enciende el LED).


Otros puertos
Ya hemos explicado los puertos que vamos a usar en este capítulo, pero el dispositivo GPIO tiene más puertos.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/7657fdb5-015e-4d0c-b96c-ca624096f9c8)



En la figura 4.6 tenemos los siguientes:
GPLEVn. Estos puertos devuelven el valor del pin respectivo. Si dicho pin está
en torno a 0V devolverá un cero, si está en torno a 3.3V devolverá un 1.
GPEDSn. Sirven para detectar qué pin ha provocado una interrupción en caso
de usarlo como lectura. Al escribir en ellos también podemos notificar que ya
hemos procesado la interrupción y que por tanto estamos listos para que nos
vuelvan a interrumpir sobre los pines que indiquemos.
GPRENn. Con estos puertos enmascaramos los pines que queremos que provoquen una interrupción en flanco de subida, esto es cuando hay una transición
de 0 a 1 en el pin de entrada.
GPFENn. Lo mismo que el anterior pero en flanco de bajada.
El resto de puertos GPIO se muestran en la figura 4.7.
Estos registros son los siguientes:
GPHENn. Enmascaramos los pines que provocarán una interrupción al detectar
un nivel alto (3.3V) por dicho pin.



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/887659ef-1db2-44bc-bcbe-238fcc494c90)


GPLENn. Lo mismo que el anterior pero para un nivel bajo (0V).
GPARENn y GPAFENn. Tienen funciones idénticas a GPRENn y GPFENn, pero
permiten detectar flancos en pulsos de poca duración.
GPPUD y GPPUDCLKn. Conectan resistencias de pull-up y de pull-down sobre los
pines que deseemos. Para más información ver el último ejemplo del siguiente
capítulo.

# 4.2.2. Temporizador del sistema
El temporizador del sistema es un reloj que funciona a 1MHz y en cada paso
incrementa un contador de 64bits. Este contador viene muy bien para implementar
retardos o esperas porque cada paso del contador se corresponde con un microsegundo. Los puertos asociados al temporizador son los de la figura 4.8. Básicamente
encontramos un contador de 64 bits y cuatro comparadores. El contador está dividido en dos partes, la parte baja CLO y la parte alta CHI. La parte alta no nos resulta interesante, porque tarda poco más de una hora (2 32 µs) en incrementarse y no va asociado a ningún comparador

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/f308f20b-46b6-45fb-9fca-06525df8d1d4)

Los comparadores son puertos que se pueden modificar y se comparan con CLO.
En el momento que uno de los 4 comparadores coincida y estén habilitadas las
interrupciones para dicho comparador, se produce una interrupción y se activa el
correspondiente bit Mx asociado al puerto CS (para que en la rutina de tratamiento
de interrupción o RTI sepamos qué comparador ha provocado la interrupción). Los
comparadores C0 y C2 los emplea la GPU internamente, por lo que nosotros nos
ceñiremos a los comparadores C1 y C3.

# Capitulo 5
# 5.1. Lectura previa
El microprocesador se encuentra en un entorno donde existen otros componentes.
La forma de comunicación más usual entre estos componentes y el microprocesador
se denomina interrupción. Básicamente, una interrupción es una petición que se hace
a la CPU para que detenga temporalmente el trabajo que esté realizando y ejecute
una rutina determinada.
# 5.1.1. El sistema de interrupciones del ARM
Decimos que las interrupciones del ARM son autovectorizadas. Cada tipo de
interrupción lleva asociado un número (que llamamos número de interrupción,
NI) que identifica el tipo de servicio a realizar. En total hay 8 tipos de interrupciones. A partir de dicho número se calcula la dirección a la que salta la CPU para
atender dicha interrupción. A diferencia de otras arquitecturas donde los vectores
contienen las direcciones de las rutinas de tratamiento, en ARM no tenemos direcciones sino instrucciones. Cada vector contiene normalmente un salto a la rutina de
tratamiento correspondiente. Dicha rutina se suele llamar RTI (Rutina de Tratamiento de Interrupción). En la arquitectura ARMv6 todos los vectores de
interrupción se almacenan en una zona de memoria llamada tabla de vectores de
interrupción. Esta tabla comienza en la dirección física 0x00000000 (aunque puede
cambiarse por 0xffff0000) y acaba en 0x0000001f y contiene en total 8 vectores de
interrupción. Cuando termina de ejecutarse una RTI, el procesador continúa ejecutando la instrucción siguiente a la que se estaba ejecutando cuando se produjo la
interrupción.

La lista del vector de interrupciones es la siguiente.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/50b7bac5-0100-4d91-96c0-1dcbddbf9146)

La última columna se refiere al Modo de operación que comentamos en el primer
capítulo y que forma parte del registro cpsr (ver figura 5.1). Es un estado en el que
se encuentra el procesador con una serie de privilegios con respecto a otros modos
y que gracias a ellos podemos construir un sistema operativo con diferentes capas.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/ca2fa6a5-1546-452e-b6ad-0437f87f0a4f)

# 5.1.2. Rutina de tratamiento de interrupción
Es el segmento de código que se ejecuta para atender a una interrupción. Una vez
se haya ejecutado dicha rutina, retomamos la ejecución normal de nuestro programa,
justo después de la instrucción donde lo habíamos interrumpido. Cada rutina de tratamiento debe atender a todas las posibles fuentes de interrupción de su mismo tipo,
con lo que al comienzo de la interrupción se suelen acceder a los puertos asociados
para detectar qué periférico ha causado la interrupción y actuar en consecuencia.
Si nos interesan IRQ y FIQ, a lo sumo tendremos que escribir dos rutinas de
tratamiento distintas. Si se produce una IRQ, se ejecutará el código que se encuentre
en la dirección 0x0018, mientras que si lo que salta es una FIQ, la dirección a
ejecutar será 0x001C. La diferencia entre una IRQ y una FIQ es que esta última
tiene sus propios registros desde r8 hasta r12 asociados al modo de operación, con
lo que podemos prescindir del salvado y recuperación de estos registros en la RTI,
ahorrando un tiempo que en determinadas aplicaciones de tiempo real puede ser
decisivo.
El esqueleto de una RTI es el siguiente.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/fc042d1f-ef69-47ad-b171-80191a54c8aa)


Vemos que a diferencia de las subrutinas donde salíamos con lr, en una RTI
salimos con lr-4 (si es un error en datos sería lr-8), a ello se debe que la última
instrucción sea subs en lugar de movs. ¿Y porqué hay un sufijo s al final de la
instrucción sub? Pues porque se trata de instrucción especial que sirve para restaurar
el registro cpsr que había antes de la interrupción (copia spsr_irq o spsr_fiq en
cpsr).


# 5.1.3. Pasos para configurar las interrupciones
Nosotros vamos a tratar un caso sencillo de programa principal en el cual hacemos
las inicializaciones correspondientes para luego meternos en un bucle infinito y que
las interrupciones hagan su trabajo. Las cosas se pueden complicar metiendo código
en el programa principal concurrente con las interrupciones. Un ejemplo de esto sería
una rutina que dibuja la pantalla en el programa principal, mientras que se aceptan
interrupciones para registrar las pulsaciones del teclado.
Sin embargo nuestro programa principal tras la inicialización será una instrucción
que salta a sí misma continuamente, bucle: b bucle.
El orden recomendado es el siguiente, aunque se puede cambiar el mismo salvo
el último punto.
1. Escribimos en el vector de interrupciones la instrucción de salto necesaria
a nuestra RTI. Nosotros emplearemos una macro llamada ADDEXC que tiene
2 parámetros, vector y dirección de la RTI. La macro genera y escribe el
código de operación del salto, para ver los detalles consultar apéndice A. En
nuestros ejemplos tendremos IRQs (0x18) y FIQs (0x1c), por lo que como
mucho haremos dos invocaciones a dicha macro (para dos RTIs distintas).


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/aa659cdb-d631-4364-945f-11f60d56d1e2)


2. Inicializamos el puntero de pila (registro sp) en todos los modos de operación.
Al cambiar el modo de operación hay que tener cuidado de no modificar la
máscara global de interrupciones, ya que comparten el mismo byte bajo de
cpsr. Como sabemos que al comienzo estaban deshabilitadas, las mantenemos
igual (bits I y F a 1. Los punteros tienen que alojar la pila en zonas distintas
donde sepamos que no habrá conflictos con la memoria de programa. En los ejemplos en los que usemos FIQ e IRQ inicializamos la pila de FIQ a 0x4000,
la de IRQ a 0x8000 y la del modo Supervisor a 0x8000000. Como la memoria
de programa empieza en 0x8000 y la pila crece hacia abajo, tendremos 16K
de pila en modo IRQ, otros 16K en modo FIQ y 128Mb a compartir entre
programa principal y pila de programa. El mapa de memoria sería el indicado
en la figura 5.4

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/cdf6674c-e2dc-4c01-8f30-d3f679d46909)

3. Escribimos código de inicialización ajeno al proceso de interrupción, como por
ejemplo configurar los GPIOs a salidas donde queramos que actúe un LED.
4. Ahora viene la inicialización de las interrupciones. Aquí le decimos al sistema
qué fuentes pueden provocar interrupciones, escribiendo en los puertos asociados.
5. El último paso es habilitar las interrupciones globalmente escribiendo en el
registro cpsr. Lo hacemos indirectamente vía otro registro, y la instrucción
tiene otro nombre pero hace lo mismo que un mov. En concreto se llama msr,
y también hay otra equivalente mrs si lo que queremos es leer de cpsr a un
registro.
6. Después de esto se acaba la inicialización y tendríamos el bucle infinito del
que consta nuestro programa principal. Si todo ha ido bien las rutinas de tratamiento de interrupción se encargarán de hacer funcionar nuestro programa
como queramos.


# 5.1.4. El controlador de interrupciones
Los puertos que componen el controlador de interrupciones son los siguientes.

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/cf529f05-d7dc-4994-830a-148fa7502ab0)

Las FIQs sólo tienen un puerto de control asociado, quedando todo el detalle en
las IRQs. Hay tres grupos de tres puertos cada uno. El primer grupo (Pending) sirve
para indicar que hay una interrupción pendiente, el segundo (Enable) es para habilitar las interrupciones y el tercero (Disable) para deshabilitarlas. Dentro de cada
grupo tenemos un puerto básico que tiene un resumen sobre el mapa de interrupciones y otros dos puertos que indican con más detalle la fuente de la interrupción.
En el puerto básico hay fuentes individuales GPU IRQ x y bits que engloban a varias
fuentes Bits in PR1, que por ejemplo indica que el origen hay que buscarlo en el
puerto 1. En el puerto 1 están las primeras 32 posiciones del mapa de interrupciones,
mientras que en el puerto 2 están las 32 últimas.
La documentación oficial sobre el mapa de interrupciones está incompleta, pero
buscando un poco por internet se puede encontrar que las interrupciones asociadas
al System Timer se controlan con los 4 primeros bits de la tabla (uno para cada
comparador).

# 5.1.5. Ejemplo. Encender LED rojo a los 4 segundos
Se trata de programar el comparador y las interrupciones para que transcurrido
un tiempo determinado se produzca una interrupción, dentro de la cual se encienda
el LED. Es un caso muy sencillo porque sólo se va a producir una interrupción que
viene de una sola fuente, por lo que en la RTI lo único que haremos es encender el
LED.
El diagrama que vamos a usar es el siguiente



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/bb1363cc-8e0e-4b76-a16e-24e57de2dac6)


1. Escribimos en el vector de interrupciones
Invocamos la macro para una IRQ, pasándole la etiqueta de nuestra RTI irq_handler.



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/30283a04-960b-496c-b16b-e3718693c003)


2. Inicializamos punteros de pila
La única forma de acceder a los registros sp_irq y sp_fiq es cambiando de
modo y modificando el registro sp correspondiente.
El modo viene indicado en la parte más baja del registro cpsr, el cual modificaremos con la instrucción especial msr. En la figura 5.1 vemos el contenido completo
del registro cpsr. Como cpsr es un registro muy heterogéneo, usamos sufijos para
acceder a partes concretas de él. En nuestro caso sólo nos interesa cambiar el byte
bajo del registro, añadimos el sufijo _c llamándolo cpsr_c, para no alterar el resto
del registro. Esta parte comprende el modo de operación y las máscaras globales de
las interrupciones. Otra referencia útil es cpsr_f que modifica únicamente la parte
de flags (byte alto). Las otras 3 referencias restantes apenas se usan y son cpsr_s
(Status) para el tercer byte, cpsr_x (eXtended) para el segundo byte y cpsr_csxf
para modificar los 4 bytes a la vez.
En la siguiente tabla vemos cómo se codifica el modo de operación.


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/25dbdb97-8e4a-406b-87dc-1b07dcdff3dc)


Como las interrupciones globales de IRQ y FIQ están desactivadas (estado por
defecto tras el reset), mantenemos a 1 dichos bits.
El código que inicializa los punteros de pila es el siguiente:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/2ac568bf-9c44-489d-a96a-9be18b48004e)


3. Código de inicialización ajeno a interrupciones
En el ejemplo que tenemos entre manos se trata de configurar los puertos GPIO
de entrada y de salida, inicializar temporizadores. En casos más complejos tendríamos que inicializar estructuras de datos, rellenar las tablas que sean precalculadas
y en general cualquier tarea de inicialización requerida para hacer funcionar nuestro
programa.

El código para asignar el sentido al pin GPIO 9 es el siguiente:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/9e8dd82b-a4fb-4b40-a8ff-550dc59ef983)


Luego programamos el comparador para que salte la interrupción a los 4,19
segundos:



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/d5ea499f-30d5-435e-b7a0-e17e19edcac5)

4. Inicializamos interrupciones localmente
Consiste en escribir en los puertos asociados dependiendo de las fuentes que
querramos activar. En este primer ejemplo habilitamos el comparador C1 del temporizador como fuente de interrupción:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/d4d155da-c5a7-4c46-80c7-f5a9804a924b)


5. Habilitamos interrupciones globalmente
Se trata de poner a cero el bit correspondiente en cpsr. El siguiente código
habilita interrupciones del tipo IRQ:

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/84ebf93a-49a7-435d-944e-4d29ec6098b0)


6. Resto del programa principal
Como hemos adelantado, en todos nuestros ejemplos será un bucle infinito:

![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/0f4363e8-f46b-4143-8419-5dfdd06d80d4)


A continuación mostramos el listado del ejemplo completo:


![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/f763481f-f0ed-4945-a10a-4b72243a3f4e)



![image](https://github.com/Cesar21212033/EInterfaz/assets/143043308/aca73d99-7bb3-49ff-b3a6-a89038ba636c)
