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
LUI 4 10000.. (11001 00000 00100 0 1000 0000 0000 0000) 0xC8088000
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
ADDI 4 4 0xFF00 (11000 00100 00100 1 1111111100000000) 0xC109FF00
ADDI 5 5 0x0068 (11000 00101 00101 0 0000000001000100) 0xC14A0068
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
Otra conclusion que vemos es que el signo funciona muy bien ya que detecto que el registro que empieza en 1 es negativo por lo que es < que el que empieza en 0.

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
Ver si dos Registros son menores y como cambia en signed y unsigned

## Instrucciones: SLT,SLTU

## Precondiciones
Utilizaremos los registros del caso 1, que los tomaremos como seteados previamente

## Code
SLT 4 5 8 (00000 00100 00101 01000 000000 001100) 0x010A800C
SLTU 4 5 8 (00000 00100 00101 01001 000000 001101) 0x010A900D

## Postcondiciones
Vemos en los registors que valor se pone si 1 o 0

## Conclusiones
Vemos que anda bien ya que en el caso de SLT devuelve un 1 que esta bien porque 4 es menor pero en el caso del unsigned devuelve 0 porque el registro 4 se vuelve positivo y es mayor


