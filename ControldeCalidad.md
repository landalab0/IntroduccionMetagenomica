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

   **`Code`** 

*      $ cd ~/dc_workshop/data/untrimmed_fastq/

*      $ gunzip JP4D_R1.fastq.gz

*      $ head -n 4 JP4D_R1.fastq

   
   **`Output`** 
   
* @MISEQ-LAB244-W7:156:000000000-A80CV:1:1101:12622:2006 1:N:0:CTCAGA
CCCGTTCCTCGGGCGTGCAGTCGGGCTTGCGGTCTGCCATGTCGTGTTCGGCGTCGGTGGTGCCGATCAGGGTGAAATCCGTCTCGTAGGGGATCGCGAAGATGATCCGCCCGTCCGTGCCCTGAAAGAAATAGCACTTGTCAGATCGGAAGAGCACACGTCTGAACTCCAGTCACCTCAGAATCTCGTATGCCGTCTTCTGCTTGAAAAAAAAAAAAGCAAACCTCTCACTCCCTCTACTCTACTCCCTT

+                                                                                                
A>>1AFC>DD111A0E0001BGEC0AEGCCGEGGFHGHHGHGHHGGHHHGGGGGGGGGGGGGHHGEGGGHHHHGHHGHHHGGHHHHGGGGGGGGGGGGGGGGHHHHHHHGGGGGGGGHGGHHHHHHHHGFHHFFGHHHHHGGGGGGGGGGGGGGGGGGGGGGGGGGGGFFFFFFFFFFFFFFFFFFFFFBFFFF@F@FFFFFFFFFFBBFF?@;@####################################

La línea 4 muestra la calidad de cada nucleótido. La calidad es interpetada como la probabilidad de que en esa posición exista una base errónea (ej. 1 en 10) o, la precisión de esa base (ej. 90%). El valor de la puntuaación numérica de cada nucleótido se convierte en un código de caracterees donde cada uno representa una puntuación de calidad para un nucleótido individual. . Esta conversión permite el alineamiento de cada nucleótido individual con su nivel de calidad. Por ejemplo, en la línea anterior, la línea de puntuación de calidad es:

  **`Output`** 


A>>1AFC>DD111A0E0001BGEC0AEGCCGEGGFHGHHGHGHHGGHHHGGGGGGGGGGGGGHHGEGGGHHHHGHHGHHHGGHHHHGGGGGGGGGGGGGGGGHHHHHHHGGGGGGGGHGGHHHHHHHHGFHHFFGHHHHHGGGGGGGGGGGGGGGGGGGGGGGGGGGGFFFFFFFFFFFFFFFFFFFFFBFFFF@F@FFFFFFFFFFBBFF?@;@#################################### 

 La máquina de secuenciación utilizada para generar nuestros datos utiliza la codificación de puntuación PHRED de calidad estándar de Sanger, utilizando Illumina versión 1.8 en adelante. A cada personaje se le asigna una puntuación de calidad entre 0 y 41, como se muestra en el cuadro siguiente.

  **`Output`** 

  
  Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJ
                   |         |         |         |         |
Quality score:    01........11........21........31........41


## FASTQ

Es un programa que nos permite visualizar la calidad de nuestras lecturas (nuestros archivos FASTQ).

Puedes instalarlo desde este link: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

* Nota: Para poder utilizarlo debes de tener un ambiente Java.

  
