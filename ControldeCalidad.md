# Control de calidad 

En este paso evaluaremos la calidad de las lecturas de secuencias contenidas en nuestros archivos FASTQ.

## ¿FASTQ? ¿🙀?
Tranquiii. FASTQ es un formato de archivo utilizado en bioinformática para almacenar secuencias de AND y sus correspondientes calidades de base.
Este formato tiene algunas características: 


|      Línea      |      Descripción      |
| --------------- | --------------------- |
|  1              | Comienza con un carácter '@' seguido de un identificador único para la secuencia |
|  2              | La secuencia de nucleótidos |
|  3              | Un separador, comunmente un '+' que separa la secuencia de ADN de la línea de calidad |
|  4              | Una línea de caracteres que representan la calidad de cada base de la secuencia de ADN, debe de tener el mismo número de caracteres que la línea 2 | 

Este tipo de archivos vienen comprimidos: fastqc.gz. Primero hay que descomprimirlos usando el comando **gunzip**.

Si usamos el comando **head** podremos ver las primeras cuatro líneas del archivo.

*Code*

   $ cd ~/dc_workshop/data/untrimmed_fastq/

   $ gunzip JP4D_R1.fastq.gz

   $ head -n 4 JP4D_R1.fastq

*Output*

      @MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:12622:2006 1:N:0:CTCAGA
CCCGTTCCTCGGGCGTGCAGTCGGGCTTGCGGTCTGCCATGTCGTGTTCGGCGTCGGTGGTGCCGATCAGGGTGAAATCCGTCTCGTAGGGGATCGCGAAGATGATCCGCCCGTCCGTGCCCTGAAAGAAATAGCACTTGTCAGATCGGAAGAGCACACGTCTGAACTCCAGTCACCTCAGAATCTCGTATGCCGTCTTCTGCTTGAAAAAAAAAAAAGCAAACCTCTCACTCCCTCTACTCTACTCCCTT                                        
+                                                                                                
A>>1AFC>DD111A0E0001BGEC0AEGCCGEGGFHGHHGHGHHGGHHHGGGGGGGGGGGGGHHGEGGGHHHHGHHGHHHGGHHHHGGGGGGGGGGGGGGGGHHHHHHHGGGGGGGGHGGHHHHHHHHGFHHFFGHHHHHGGGGGGGGGGGGGGGGGGGGGGGGGGGGFFFFFFFFFFFFFFFFFFFFFBFFFF@F@FFFFFFFFFFBBFF?@;@####################################


