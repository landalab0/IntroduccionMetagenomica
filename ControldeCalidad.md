# Control de calidad 

En este paso evaluaremos la calidad de las lecturas de secuencias contenidas en nuestros archivos FASTQ.

## ¿FASTQ QUÉ? ¿🙀?
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



## FASTQ👌

Es un programa que nos permite visualizar la calidad de nuestras lecturas (nuestros archivos FASTQ). FastQc ve la calidad de manera colectiva a través de todos los reads de una muestra.



Puedes instalarlo desde este link: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

* Nota: Para poder utilizarlo debes de tener un ambiente Java.

Podemos obtener gráficos de la calidad de nuestras lecturas con FastQC. En la siguiente imagen vemos un gráfico que indica una muestra de muy alta calidad:

![image](https://github.com/landalab0/IntroduccionMetagenomica/assets/160524982/eea1648a-2d36-42cf-a8e3-7ceda990fbee)


En el eje de las x se ve la posición de la base en la lectura y en el eje de las y se muestra las puntuaciones de calidad. En este ejemplo, los reads tienen 40 pb de longitud.

Para poder correr FastQC, nos ubicaremos en el directorio:

$ cd ~/dc_workshop/data/untrimmed_fastq/ 

Podemos utilizar múltiples archivos como input, ya sea comprimidos o descomprimidos. Para esto podríamos utilizar * \*.fastq* *para correr FastQC en todos los archivos FASTQ que se encuentren en este directorio.

  **`Code`** 
  
 $ fastqc *.fastq* 

 Veremos un mensaje como este:

 Started analysis of JC1A_R1.fastq.gz
Approx 5% complete for JC1A_R1.fastq.gz
Approx 10% complete for JC1A_R1.fastq.gz
Approx 15% complete for JC1A_R1.fastq.gz
Approx 20% complete for JC1A_R1.fastq.gz
Approx 25% complete for JC1A_R1.fastq.gz

Esperaremos a que FastQc corra nuestros cuatro archivos FASTQ, y cuando el análisis termine, el prompt regresará.

 **`Output`** 
 
Approx 95% complete for JC1A_R1.fastq.gz
Analysis complete for JC1A_R1.fastq.gz

FastQc habrá creado nuevos archivps en nuestro directorio data/untrimmed_fastq/

JC1A_R1_fastqc.html  JC1A_R2_fastqc.html  JP4D_R1.fastq        JP4D_R2_fastqc.html  TruSeq3-PE.fa
JC1A_R1_fastqc.zip   JC1A_R2_fastqc.zip   JP4D_R1_fastqc.html  JP4D_R2_fastqc.zip
JC1A_R1.fastq.gz     JC1A_R2.fastq.gz     JP4D_R1_fastqc.zip   JP4D_R2.fastq.gz 


Por cada archivo FASTQ que ingresamos, FastQC creó un archivo .zip (un archivo comprimido que contiene varios archivos de salida para un único archivo FASTQ de entrada) y uno .html (muestra una página web con un resumen del reporte de cada una de nuestras muestras).

Como nos gustaría mantener los archivos de datos y los archivos de resultados separados, moveremos los archivos de output en un nuevo directorio en nuestro directorio /results.

 **`Code`** 
 
$ mkdir -p ~/dc_workshop/results/fastqc_untrimmed_reads 
$ mv *.zip ~/dc_workshop/results/fastqc_untrimmed_reads/ 
$ mv *.html ~/dc_workshop/results/fastqc_untrimmed_reads/ 

Ahora, iremos al directorio de resultados y veremos que hay en estos archivos de output.

$ cd ~/dc_workshop/results/fastqc_untrimmed_reads/ 

## Outputs de FastQC ✨

Además del gráfico "Calidad de secuencia por base", existen otros nueve gráficos que podemos analizar:

-> Calidad de secuencia por mosaico:
-> Puntuaciones de calidad por secuencia:
-> Por contenido de secuencia de bases:
-> Contido de GC por secuencia:
-> Por contenido de N base:
-> Distribución de longitud de secuencia:
-> Niveles de duplicación de secuencias:
-> Secuencias sobrerepresentadas:
-> Contenido del adaptador:
-> Contenido de K-mer:

## 

Nos situaremos de nuevo en el subdirectorio de resultados:

 **`Code`**
 
$ cd ~/dc_workshop/results/fastqc_untrimmed_reads/ 
$ ls 

 **`Output`** 
JC1A_R1_fastqc.html           JP4D_R1_fastqc.html                  
JC1A_R1_fastqc.zip            JP4D_R1_fastqc.zip                   
JC1A_R2_fastqc.html           JP4D_R2_fastqc.html                  
JC1A_R2_fastqc.zip            JP4D_R2_fastqc.zip 

Para ver el contenido de un archivo .zip, podemos usar **unzip** para descomprimirlos. Como tenemos que descomprimir más de un archivo, usaremos **for** bucle para correr todos los archivos .zip. 

 **`Code`** 
 
$ for filename in *.zip
> do
> unzip $filename
> done

 **`Output`** 

Archive:  JC1A_R1_fastqc.zip                                            
creating: JC1A_R1_fastqc/                                            
creating: JC1A_R1_fastqc/Icons/                                      
creating: JC1A_R1_fastqc/Images/                                    
inflating: JC1A_R1_fastqc/Icons/fastqc_icon.png                      
inflating: JC1A_R1_fastqc/Icons/warning.png                          
inflating: JC1A_R1_fastqc/Icons/error.png                            
inflating: JC1A_R1_fastqc/Icons/tick.png                             
inflating: JC1A_R1_fastqc/summary.txt                                
inflating: JC1A_R1_fastqc/Images/per_base_quality.png                
inflating: JC1A_R1_fastqc/Images/per_tile_quality.png                
inflating: JC1A_R1_fastqc/Images/per_sequence_quality.png            
inflating: JC1A_R1_fastqc/Images/per_base_sequence_content.png       
inflating: JC1A_R1_fastqc/Images/per_sequence_gc_content.png         
inflating: JC1A_R1_fastqc/Images/per_base_n_content.png              
inflating: JC1A_R1_fastqc/Images/sequence_length_distribution.png 
inflating: JC1A_R1_fastqc/Images/duplication_levels.png              
inflating: JC1A_R1_fastqc/Images/adapter_content.png                 
inflating: JC1A_R1_fastqc/fastqc_report.html                         
inflating: JC1A_R1_fastqc/fastqc_data.txt                            
inflating: JC1A_R1_fastqc/fastqc.fo  

**Unzip** descomprime los archivos .zip y crea un nuevo directorio (con subdirectorios) para cada una de las muestras. Ahora tendremos tres tipos de archivos por cada muestra. 

Veamos que hay en uno de los nuevos directorios de salida.

  **`Code`** 
  
$ ls -F JC1A_R1_fastqc/

 **`Output`** 
 
fastqc_data.txt  fastqc.fo  fastqc_report.html	Icons/	Images/  summary.txt

Usaremos **less** para ver una vista previa del archivo sumary.txt de esta muestra:

 **`Code`** 
 
$ less JC1A_R1_fastqc/summary.txt

 **`Output`** 
 
PASS    Basic Statistics        JC1A_R1.fastq.gz                     
FAIL    Per base sequence quality       JC1A_R1.fastq.gz             
PASS    Per tile sequence quality       JC1A_R1.fastq.gz             
PASS    Per sequence quality scores     JC1A_R1.fastq.gz             
WARN    Per base sequence content       JC1A_R1.fastq.gz             
FAIL    Per sequence GC content JC1A_R1.fastq.gz                     
PASS    Per base N content      JC1A_R1.fastq.gz                     
PASS    Sequence Length Distribution    JC1A_R1.fastq.gz             
FAIL    Sequence Duplication Levels     JC1A_R1.fastq.gz             
PASS    Overrepresented sequences       JC1A_R1.fastq.gz             
FAIL    Adapter Content JC1A_R1.fastq.gz  

El archivo summary nos muestra una lista de los pruebas que FASTQC hizo y nos dice si la muestra pasó o reprobó, o esta en el límite (WARN).

Podemos registrar los resultados de todas las muestras concatenando todos los summary.txt en un solo archivo usando **cat**. Se llamará fastqc_summaries.txt y se almacenará en ~/dc_workshop/docs.

  **`Code`** 
$ mkdir -p ~/dc_workshop/docs
$ cat */summary.txt > ~/dc_workshop/docs/fastqc_summaries.txt



