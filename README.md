# Pruebas-Procesador

# Caso 1
## Descripcion:Realizar las Cuatro Operaciones basicas entre dos registros
## Instrucciones: XOR,LUI,ORI,ADD,SUB,MUL,DIV

## Precondiciones:
Previamente antes de sumar los registros tengo que cargarles algun valor para eso vamos a usar las instruciones LUI y ORI (osea un registro tendra un valor significativamente mas grande que el otro)
Además antes del LUI y ORI haremos un xor de los registros con si mismos para asegurarnos que no tengan basura.
Los registros que usarmeos para guardar los valores seran  4 y 5 para guardar los valores que utilizaremos(estos valores los volveremos a utilizar en proximos casos),y usaremos 8 para el resultado de las funciones y ver si andan.

## Code:
XOR 4 4 4 (00000 00100 00100 00100 00000 0 001010) 0x0108400A
XOR 5 5 5 (00000 00101 00101 00101 00000 0 001010) 0x014A500A
LUI 4 10000.. (11001 00000 00100 0 1000 0000 0000 0000) 0xC8088000 
ORI 5 5 100.. (00101 00101 00101 0 1000 0000 0000 0000) 0x294A8000
ADD 4 5 8 (00000 00100 00101 01000 00000 0 011100) 0x010A801C
SUB 4 5 8 (00000 00100 00101 01000 00000 0 011101) 0x010A801D

## PostCondiciones
Para ir controlando si el codigo esta funcionando vamos poniendo r a ver si los registros se actualizaron ademas vemos en la consola del procesador si tomo bien la instruccion entre otras cosas

## Conclusiones
Vemos que las instrucciones anduvieron de 10 y que en SUB al producirse un overflow el procesador maneja este overflow apagando el bit de signo (el MSB) por lo cual el numero es interpretado en forma erronea culpa del desbordamiento.
