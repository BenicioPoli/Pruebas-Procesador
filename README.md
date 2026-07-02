# Pruebas-Procesador

# Caso 1
## Descripcion:
Realizar las Cuatro Operaciones basicas entre dos registros y diferenciar los casos unsigned y signed

## Instrucciones: XOR,LUI,ORI,ADD,SUB,MUL,MULH,MULHU,DIV,DIVU,REST,RESTU

## Precondiciones:
Previamente antes de sumar los registros tengo que cargarles algun valor para eso vamos a usar las instruciones LUI y ORI (osea un registro tendra un valor significativamente mas grande que el otro)
Además antes del LUI y ORI haremos un xor de los registros con si mismos para asegurarnos que no tengan basura.
Los registros que usarmeos para guardar los valores seran  4 y 5 para guardar los valores que utilizaremos(estos valores los volveremos a utilizar en proximos casos),y usaremos 8 para el resultado de las funciones y ver si andan.(y tambien usaremos 9 en las operaciones en las que necesiemos de dos registros)
(nosotros pondremos siempre el resultado de las operaciones en el mismo registro para simplificar lo que va a ocasionar que se vaya sobre escribiendo por lo que habra que ir viendo r al poner cada instrucción)
## Code:
```
XOR 4 4 4 (00000 00100 00100 00100 00000 0 001010) 0x0108400A
XOR 5 5 5 (00000 00101 00101 00101 00000 0 001010) 0x014A500A
LUI 4 10000.. (00111 00000 00100 0 1000 0000 0000 0000) 0x38088000
ORI 5 5 100.. (00101 00101 00101 0 1000 0000 0000 0000) 0x294A8000
ADD 4 5 8 (00000 00100 00101 01000 00000 0 011100) 0x010A801C
SUB 4 5 8 (00000 00100 00101 01000 00000 0 011101) 0x010A801D
MUL 4 5 8 (00000 00100 00101 01000 00000 0 010101) 0x010A8015
MULH 4 5 9 (00000 00100 00101 01001 00000 0 010110) 0x010A9016
MULHU 4 5 9 (00000 00100 00101 01001 00000 0 010111) 0x010A9017
DIV 4 5 8 (00000 00100 00101 01000 00000 0 011000) 0x010A8018
REST 4 5 9 (00000 00100 00101 01001 00000 0 011010) 0x010A901A
DIVU 4 5 8 (00000 00100 00101 01000 00000 0 011001) 0x010A8019
RESTU 4 5 9 (00000 00100 00101 01001 00000 0 011011) 0x010A901B
```

## PostCondiciones
Para ir controlando si el codigo esta funcionando vamos poniendo r a ver si los registros se actualizaron ademas vemos en la consola del procesador si tomo bien la instruccion entre otras cosas

## Conclusiones
Vemos que las instrucciones anduvieron de 10 y que en SUB al producirse un overflow el procesador maneja este overflow apagando el bit de signo (el MSB) por lo cual el numero es interpretado en forma erronea culpa del desbordamiento.
Vemos tambien que por como se calcula la multiplicacion binaria el procesador se puede ahorrar tener un MULU ya que la parte baja no variara entre signed y unsigned.
Vemos el funcionamiento de unsigned y signed por ejemplo en la misma multiplicación donde al poner unsigned la parte mayor da 0x00004000 contra los 0xFFFFC000 queda al usar signed,esto tambien se ve en la division donde nos da 0xFFFF0000 en el signed y 0x0001000 en el unsigned.
Vimos que el resto no da basura da 0 como debe dar en nuestro caso

# Caso 2

## Descripcion
En este caso vamos a intentar mandar una letra h a la pantalla.

## Instrucciones: ADDI,SB

## Precondiciones
Otra vez haremos que los registros esten en 0 antes de aplicarles cualquier cosa y cargaremos la h en un registro y la direccion del serial de la pantalla 0xFFFFFF00 en otro registro.
Para variar ingresaremos con ADDI los valores en vez de usar ORI y LUI.
Para ADDI necesitamos mas que nunca que el registro este vacio para hacer 0 + el valor que queremos.
ADDI sirve para 16 bits que para h los alcanzan,pero para la direccion del puerto el problema se soluciona facil debido a que el bit 17 (el de significancia posterior a imm) se pone como 1 o 0 significando este si es negativo o positivo el registro para lo cual va a rellenar con 1 si se poene como 1 el bit y con 0 si se pone como 0.

Usaremos los registros 4 y 5 para guardar la dirección del serie y la h.

## Code:
```
XOR 4 4 4 (00000 00100 00100 00100 00000 0 001010) 0x0108400A
XOR 5 5 5 (00000 00101 00101 00101 00000 0 001010) 0x014A500A
ADDI 4 4 0xFF00 (00001 00100 00100 1 1111111100000000) 0x0909FF00
ADDI 5 5 0x0068 (00001 00101 00101 0 0000000001000100) 0x094A0068
SB 4 5 0 (01011 00100 00101 0 00000000..00) 0x590A0000
```
## Postcondiciones
Para ir controlando otra vez vemos los registros y para ver la h se mando correctamente vemos el picocom.

## Conclusiones
Vimos la comunicación con la interface serie y vemos que funciona bien.

# Caso 3

## Descripcion
En este caso veremos todos los branch if a ver si salta cuando corresponde utilizando los registros del caso 1

## Instrucciones: BEQ,BNE,BLT,BGT,BLE,BGE

## Precondiciones
Utilizaremos los registros del Caso 1 los cuales asumimos como cargados previamente.
Para que se note el salto utilizaremos un salto de 0x000F

## Code:
```
BEQ 4 5 0x000F (10000 00100 00101 0 000...1111) 0x810A000F
BNE 4 5 0x000F (10001 00100 00101 0 000...1111) 0x890A000F
BLT 4 5 0x000F (10010 00100 00101 0 000...1111) 0x910A000F
BGT 4 5 0x000F (10011 00100 00101 0 000...1111) 0x990A000F
BLE 4 5 0x000F (10100 00100 00101 0 000...1111) 0xA10A000F
BGE 4 5 0x000F (10101 00100 00101 0 000...1111) 0xA90A000F
```

## Postcondiciones:
Si funciono en este caso se ve en la consola porque nos va a decir que saltamos al pc tanto y si vemos que avanzamos un pc nada más es que no aplicaba el salto.

## Conclusiones
Vemos como se calcula el salto,vemos que esta saltando 40 cada vez que debe,salta 40 debido a como el procesador calcula el salto,ya que este lo calculo agregandole a la imm 13 bits a la izquierda y 2 bits a la derecha (en este caso todos 0) entonces nuestra F queda como 111100. Otra cosa que se nota es que esta F aporta 0x40 porque si en vez de poner 0x000F de offset ponemos 0x0008 el PC salta 0x0C entonces ese es el salto minimo que permite este RTM32 (que es el salto de toda la vida cuando escribimos en el RTM)
Otra conclusion que vemos es que el signo funciona muy bien ya que detecto que el registro que empieza con MSB en 1  es negativo por lo que es < que el que empieza en 0.

# Caso 4:
## Descripcion:
Hacer las operaciones booleanas entre los registros

## Instrucciones: AND,OR,NOR

## Precondiciones:
Vamos a usar los mismos registros 4 y 5 del caso 1 pero le vamos a añadir al registro 4 un 1 en el mismo bit que tiene un uno el 5 esto para poder probar el add mejor,para esto vamos a usar un ORI en el que usamos el LUI.

## Code:
```
ORI 4 4 100.. (00101 00100 00100 0 1000 0000 0000 0000) 0x29088000
AND 4 5 8 (00000 00100 00101 01000  00000 0 001000) 0x010A8008
OR 4 5 8  (00000 00100 00101 01000  00000 0 001001) 0x010A8009
NOR 4 5 8 (00000 00100 00101 01000  00000 0 001011) 0x010A800B
```

## Postcondiciones:
Vemos que el codigo funciono el and devuelve lo que esta almacenado en el registro 5 el or devuelve el registro 4 completo y el nor devuelve 0x7FFF7FFF que es lo que corresponde que mande

## Conclusiones:
Se pueden realizar correctamente operaciones booleanas en el procesador

# Caso 5
## Descripcion: 
Ver si dos Registros son menores y como cambia en signed y unsigned en estas comparaciones,tambien probar lo mismo con imms

## Instrucciones: SLT,SLTU,SLTI,SLTIU

## Precondiciones
Utilizaremos los registros del caso 1, que los tomaremos como seteados previamente,y para hacer la comparacion con los imm utilizaremos el registro 4 por tener un contenido negativo
Utilizaremos para guardar los valores los registros 8 y 9 utilizamos dos para poder comparar en cada caso el signed con el unsigned.
Vamos a comparar es las tipo I con una imagen cero para hacer más facil el analisis.

## Code
```
SLT 4 5 8 (00000 00100 00101 01000 000000 001100) 0x010A800C
SLTU 4 5 9 (00000 00100 00101 01001 000000 001101) 0x010A900D
SLTI 4 8 0 (10110 00100 01000 0 00000..000) 0xB1100000 
SLTIU 4 8 0  (10111 00100 01001 0 00000..000) 0xB9120000
```

## Postcondiciones
Vemos con el comando r al ejecutar cada instruccion si el registro  8 o 9 se pusieron en 1 o 0

## Conclusiones
Vemos que anda bien ya que en el caso de SLT devuelve un 1 que esta bien porque 4 es menor pero en el caso del unsigned devuelve 0 porque el registro 4 se vuelve positivo y es mayor.
Lo mismo con las tipo I al comparar con la imagen 0 el registro 4 en signed devuelve 1 porque R4 tiene un valor negativo ahora si hacemos unsigned el R4 pasa a tener un valor positivo gigante por lo que se devuelve 0.

# Caso 6
## Descripcion:
Vamos a probar si las cuatro instrucciones de JUMP andan correctamente y el pc salta donde debe saltar.

## Instrucciones: J,JAL,JR,JALR

## Precondiciones
En este caso no hay ninguna precondición exigida sin embargo se prefiere empezar con el pc en 0,en las tipo J,para una mejor visualización
Para los J tipo R nosotros usaremos el registro 0 que se encuentra vacio (estos saltos de las tipo R si se recomienda empezarlas en pc 0x08 o en otro numero que no sea 0)

Aclaración: ej JALR podemos guardar donde queramos el salto que se iba a realizar naturalmente nosotros lo guardamos en 31 para tener igual analisis que con JAL.
## Code
```
J 0x02 (00010 0...0010) 0x10000002
JAL 0x02 (00011 0...0010) 0x18000002
JR 0 (00000 00000 00000 00000 0 00000 001110) 0x0000000E
JALR 0 (00000 00000 00000 11111 0 00000 001111) 0x0001F00F
```

## Postcondiciones
Para ver si las tipo J funcionaron hay que ver si salta adonde debe,con ese salto que pusimos el pc debe saltar 8 posiciones (esto debido a que el salto se forma con el valor ingresado + en los 3 MSB los del pc actual + en la 2 LSB 0) entonces 1000 es 8 por lo que empezando en 0 el pc debera saltar a 0x8 en vez de a 0x4 como hace habitualmente.
Y además al ejecutar los JAL se debe poder ver con r que el R[31] guarde en este caso un 4 que es el pc adonde iria sin el salto.
Y en las tipo R hay que ver si salta a la dirección definida en el registro en este caso 0 y si en R[31]  se guarda el pc + 4 en el caso del JALR

## Conclusiones
Vemos que todo salta adonde tiene que saltar asi que las cuatro instrucciones de JUMP andan bien.

# Caso 7
## Descripcion:
En este caso vamos a ver las funciones que tienen que ver con el desplazamiento de bits

## Instrucciones: SLL,SRL,SRA,SLLR,SRLR,SRAR

## Precondiciones
Para desplazar vamos a necesitar un valor que desplazar nosotros vamos a usar el valor de R4 que seteamos en el caso 4 es decir 0x80008000 sabiendo que en bianario el 8 es 1000. Es importante usar este valor para ver como
se trata  a los negativos.
Además debemos guardar en un registro algun valor para podes hacer el desplazamiento,como para desplazar toma los bits de menos relevancia y para simplificar el movimiento vamos a cargar en un registro un simple 2 utilizando ORI. (lo haremos en el 6 y vamos a limpiar el registro previamente para no tener basura)
Guardemos los resultados en los registros 8,9 y 10

## Code
```
XOR 6 6 6  (00000 00110 00110 00110 00000 0 001010) 0x018C600A
ORI 6 6 2  (00101 00110 00110 0 0000 0000 0000 0010) 0x298C0002
SLL 0 4 8 aux:2  (00000 00000 00100 01000 00010 0 000000) 0x00088100  
SRL 0 4 9 aux:2  (00000 00000 00100 01001 00010 0 000001) 0x00089101
SRA 0 4 10 aux:2 (00000 00000 00100 01010 00010 0 000010) 0x0008A102
SRA 0 5 10 aux:2 (00000 00000 00101 01010 00010 0 000010) 0x000AA102
SLLR 6 4 8  (00000 00110 00100 01000 00000 0 000011) 0x01888003
SRLR 6 4 9  (00000 00110 00100 01001 00000 0 000100) 0x01889004
SRAR 6 4 10 (00000 00110 00100 01010 00000 0 000101) 0x0188A005
SRAR 6 5 10 (00000 00110 00101 01010 00000 0 000101) 0x018AA005
```

## Postcondiciones
Al ejecutar cada paquete de SLL ver en r si el valor del registro coincide con lo que se busca.

## Conclusiones
Al hacer SLL del valor nos dio 0x00020000,lo cual esta bien coincide con mover el bit del 8 dos lugares,lo que si el numero se volvio positivo en el mundo de los signed.

Luego al hacer SRL del valor nos dio 0x20002000 lo cual tambien esta bien coincide con mover dos bits los bits del 8,lo que si otra vez el numero se volvio positivo en el mundo de los signed.

Ahora al hacer SRA del valor nos dio 0xE0002000 lo cual coincide con mover los bits del 8 dos lugarse Y autocompletar con 0 los bits de MSB dependiendo cuanto nos movemos.

Por lo cual entre SRA y SLL nos damos cuenta de una muy importante diferencia mientras SLL rellena los MSB nuevos con 0,SRA lo hace con 1 lo cual nos  permite mantener el signo en nuestro caso del numero negativo.
Para ver si los 1 eran por ser negativo o era siempre probamos con el registro 5 en el cual teniamos 0x00008000 y vemos que dio 0x00002000 por lo cual esto es algo importante y nos hace concluir que el SRA siempre mantiene el signo.

Podemos concluir que SRL se usaria para unsigneds y SRA para signeds

Haciendo las SLLR obtuvimos los mismos resultados (esto porque usamos de R lo mismo que habiamos usado aux) asi que concluimos que los Shift funcionan bien en el procesador.

# Caso 8
## Descripcion: Vamos a probar subir cosas a memoria y cargarlas en otro registro

## Instrucciones: SW,LW,SH,LH,LHU,SB,LB,LBU

## Precondiciones:
Vamos a usar la direccion de memoria 0x010 y vamos a asumir que estamos en una estructura de 4kb
Vamos a usar para cargar un registro que tiene un 1 en el MSB un 1 en el LSB y un 1 a la mitad,este registro lo vamos a formar con el Lui del caso 1 y con un ORI esto en el registro 4,este registro lo cargaremos en memoria de distintas formas y luego para sacarlo usaremos los registros 8 y 9,y vamos a usar el registro source 0 para que la direccion sea unicamente el offset (asumimos 0 como limpio)

## Code
```
LUI 4 10000.. (00111 00000 00100 0 1000 0000 0000 0000) 0x38088000
ORI 4 4 100..01 (00101 00100 00100 0 1000 0000 0000 0001) 0x29088001
SW 0 4 0x10 (01001 00000 00100 0 00000...010000) 0x48080010
LW 0 8 0x10 (01000 00000 01000 0 00000...010000) 0x40100010
SH 0 4 0x10  (01010 00000 00100 0 0000...010000) 0x50080010
LH 0 8 0x10  (01100 00000 01000 0 0000...010000) 0x60100010
LHU 0 9 0x10 (01101 00000 01001 0 0000...010000) 0x68120010
SB 0 4 0x10 (01011 00000 00100 0 0000....010000) 0x58080010
LB 0 8 0x10 (01110 00000 01000 0 0000....010000) 0x70100010
LBU 0 9 0x10 (01111 00000 01001 0 0000...010000) 0x78120010
```

## Postcondiciones
Verificar en los registros si se guardo el valor correctamente.Y tambien ver en la consolita si se ejecutaron los comandos correctos


## Conclusiones
Vemos SW y LW funcionan porque luego de su ejecución R8 quedo igual que R4, no existe SWU porque es innecesario se carga todo el numero asi que no puede haber confusion en los signos.

Vemos que LH nos rellena con 1 los bits de mayor valor,esto pasa porque nuestro numero tiene en la parte mas baja de bit de mayor valor un 1 entonces rellena con 1 para mantener el signo,esto con LHU no pasa nos rellana con 0 de valor esto porque LHU toma como que todo es positivo.

Vemos que en LB,LBU carga el mismo valor en los registros 0x01 esto es porque si tomamos solo los ultimos 8 bits de nuestro numero es positivo entonces en tanto signed como unsigned es igual.

# Caso 9

## Descripcion:
En este caso vamos a seguir probando la descarga en memoria pero a traves de los Loade indexed.

## Instrucciones: LHX,LHUX,LBX,LBUX,LWX

## Precondiciones
Aca la direccion de memoria va a depender de la suma de dos registros entonces lo que vamos a hacer es cargar en un registro el numero 0x010 utilizando ORI y vamos a sumarlo con el registro 0,este 0x010 lo subiremos en el registro 5.
Vamos a utilizar el mismo registro 4 que utilizamos en el caso anterior y los mimos comandos de store.
Y para los load vamos a utilizar los registros 8 y 9.

## Code
```
LUI 4 10000.. (00111 00000 00100 0 1000 0000 0000 0000) 0x38088000
ORI 4 4 100..01 (00101 00100 00100 0 1000 0000 0000 0001) 0x29088001
ORI 5 5 000.10000 (00101 00101 00101 0 0000 0000 0001 0000) 0x294A0010
SW 0 4 0x10 (01001 00000 00100 0 00000...010000) 0x48080010
LWX 0 8 5 (00000 00000 01000 00101 00000 0 010100) 0x00105014
SH 0 4 0x10  (01010 00000 00100 0 0000...010000) 0x50080010
LHX 0 8 5  (00000 00000 01000 00101 00000 0 010000) 0x00105010
LHUX 0 9 5 (00000 00000 01001 00101 00000 0 010001) 0x00125011
SB 0 4 0x10 (01011 00000 00100 0 0000....010000) 0x58080010
LBX 0 8 5  (00000 00000 01000 00101 00000 0 010010) 0x00105012
LBUX 0 9 5 (00000 00000 01001 00101 00000 0 010011) 0x00125013
```

## Postcondiciones
Verificar en los registros si se guardo el valor correctamente. Y tambien ver en la consolita si se ejecutaron los comandos correctos

## Conclusiones
Al ejecutar LWX nos queda R8 = R4 por lo que funciona bien.
Vemos que LHX nos rellena con 1 los bits MSB debido a que el bit MSB de los 16 cargados es 1 por lo que el numero es negativo,y vemos que LHUX nos carga con 0 porque toma que el numero es positivo,este es el funcionamiento esperado asi que esta bien
Al ejecutar LBX y LBUX nos carga el mismo valor que es un simple 0x01 en hexa por lo que andan bien.

# Caso 10

## Descripcion: 
En este caso vamos a hacer una prueba adicional sobre LBX y LB debido a que antes lo probamos unicamente con positivos vamos a ver si a los negativos los trata bien

## Instrucciones: LB,LBX

## Precondiciones
Para esta prueba vamos a usar el mismo R5 del caso anterior y vamos a cargar en R4 con un ORI un 0x80 (esto sobre el valor que ya tiene R4 es decir no lo limpiamos previamente)

## Code
```
ORI 4 4 0x80 (00101 00100 00100 0 0000 0000 1000 0000) 0x29088080
SB 0 4 0x10 (01011 00000 00100 0 0000....010000) 0x58080010
LB 0 8 0x10 (01110 00000 01000 0 0000...010000) 0x70100010
LBX 0 9 5 (00000 00000 01001 00101 00000 0 010010) 0x00125012
```

## Postcondiciones
Verificamos valores de los registros

## Conclusiones
Vemos que ambas instrucciones devuelven lo mismo 0x81 autocompletado con F lo que seria el valor de los 8 LSB de R4 autocompletado con 1 por ser negativo,por lo que confirmamos que LB y LBX detectan bien ambos signos.

