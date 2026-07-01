# Pruebas-Procesador

# Caso 1
## Descripcion:
Realizar las Cuatro Operaciones basicas entre dos registros y diferenciar los casos unsigned y signed
## Instrucciones: XOR,LUI,ORI,ADD,SUB,MUL,DIV

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
